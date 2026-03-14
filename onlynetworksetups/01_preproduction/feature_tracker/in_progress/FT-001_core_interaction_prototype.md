# FT-001: Core Interaction Prototype

**Feature ID:** FT-001
**Feature Name:** Core Interaction Prototype
**Status:** in_progress
**Target Version:** V0.2.0
**Owner:** Lead Software Architect + Senior Game Developer
**Date Created:** 2026-03-14

---

## Purpose

Build the minimum playable interaction layer that proves the core game loop works:
a player can pick up hardware, inspect it, connect cables, and bring a modem + router
online in a single room. This prototype is the V0.2.0 exit gate.

---

## Core Loop / Learning Impact

- Player physically handles real-device analogues (modem, router, coax cable, Ethernet cable, power adapter).
- Player learns correct port identification and connection sequence through trial and error.
- Pass/fail logic provides immediate feedback without hand-holding.
- Teaches: coax-to-modem, Ethernet modem-to-router, power sequencing, and LED status reading.

---

## Dependencies

- None (greenfield prototype room)
- Unity project scaffold (or target engine scene file)
- Basic first-person camera rig

---

## Design Notes

### Interaction Model (First-Person)
- Raycast-based pickup at a configurable reach distance (default 1.5 m).
- Held object follows camera-forward offset with physics interpolation.
- Right-click or secondary button toggles inspect mode (free-rotate around object center).
- Snap-to-port system: when a cable tip enters a port collider, highlight + confirm prompt appears.
- Connection confirmed on button press if cable type matches port type.

### Objects in Prototype Room
| Object | Ports | Notes |
|---|---|---|
| Cable Modem | Coax-IN, Ethernet-OUT x1, Power-IN | LED strip: Power, DS, US, Online |
| Wi-Fi Router | WAN-IN (Ethernet), LAN x4, Power-IN | LED strip: Power, WAN, Wi-Fi 2.4, Wi-Fi 5 |
| Coax Cable | Coax plug x2 | Connects wall-outlet to modem |
| Ethernet Cable | RJ-45 x2 | Connects modem to router WAN |
| Power Adapter (Modem) | Barrel plug | Connects power strip to modem |
| Power Adapter (Router) | Barrel plug | Connects power strip to router |
| Wall Coax Outlet | Coax socket | Fixed in scene, not pickupable |
| Power Strip | 4x outlets | Fixed, provides power source |

### Pass / Fail Logic (Success Path)
1. Coax cable: wall outlet → modem Coax-IN.
2. Ethernet cable: modem Ethernet-OUT → router WAN-IN.
3. Power adapter: power strip → modem Power-IN.
4. Power adapter: power strip → router Power-IN.
5. Modem LEDs cycle: Power → DS/US sync → Online (green).
6. Router LEDs cycle: Power → WAN (green) → Wi-Fi (green).
7. Success screen triggered.

Any cable connected to the wrong port returns an error haptic/audio cue and does not latch.

---

## Implementation Notes

### Cable Connection Logic (Coax + Ethernet)

**State Machine — Cable Object**

```
IDLE
  └─[player picks up]──► HELD
       ├─[cable tip enters wrong port collider]──► HELD (error feedback, no state change)
       └─[cable tip enters correct port collider + confirm input]──► CONNECTED
            └─[player yanks / disconnect action]──► IDLE
```

**CableConnector Component (pseudocode)**

```csharp
public class CableConnector : MonoBehaviour
{
    public CableType cableType;          // Coax | Ethernet | Power
    public CableEnd endA;                // plug A
    public CableEnd endB;                // plug B

    private CableState stateA = CableState.Idle;
    private CableState stateB = CableState.Idle;

    public bool IsFullyConnected => stateA == CableState.Connected
                                 && stateB == CableState.Connected;

    void OnEndEnterPort(CableEnd end, PortReceiver port)
    {
        if (port.AcceptsType(cableType))
            port.ShowHighlight();
        else
            FeedbackManager.PlayError();
    }

    void OnConnectConfirmed(CableEnd end, PortReceiver port)
    {
        if (!port.AcceptsType(cableType)) return;

        end.State = CableState.Connected;
        end.AttachTo(port);
        port.Register(this);
        FeedbackManager.PlaySnap();
        EventBus.Publish(new CableConnectedEvent(this, end, port));
    }
}
```

**PortReceiver Component (pseudocode)**

```csharp
public class PortReceiver : MonoBehaviour
{
    public CableType acceptedType;
    public PortID portID;               // e.g. MODEM_COAX_IN
    public bool IsOccupied { get; private set; }

    public bool AcceptsType(CableType t) => t == acceptedType && !IsOccupied;

    public void Register(CableConnector cable)
    {
        IsOccupied = true;
        connectedCable = cable;
        GetComponentInParent<DeviceStateMachine>().NotifyPortConnected(portID);
    }
}
```

---

### Modem / Router State Machine

**Modem States**

```
OFF
 └─[Power-IN connected]──► BOOTING (3 s timer)
      └─[timer complete + Coax-IN connected]──► DS_US_SYNC (5 s timer)
           └─[timer complete]──► ONLINE
                └─[Coax-IN or Power-IN disconnected]──► OFF (reset)
```

**Router States**

```
OFF
 └─[Power-IN connected]──► BOOTING (2 s timer)
      └─[timer complete + WAN-IN connected + modem.State == ONLINE]──► WAN_ACTIVE
           └─[always after WAN_ACTIVE]──► WIFI_BROADCASTING
                └─[WAN-IN or Power-IN disconnected]──► OFF (reset)
```

**DeviceStateMachine Component (pseudocode)**

```csharp
public class DeviceStateMachine : MonoBehaviour
{
    public DeviceType deviceType;       // Modem | Router
    public DeviceState CurrentState { get; private set; } = DeviceState.Off;

    private Dictionary<PortID, bool> portStatus = new();
    private Coroutine bootRoutine;

    public void NotifyPortConnected(PortID port)
    {
        portStatus[port] = true;
        EvaluateTransitions();
    }

    public void NotifyPortDisconnected(PortID port)
    {
        portStatus[port] = false;
        TransitionTo(DeviceState.Off);
    }

    private void EvaluateTransitions()
    {
        switch (CurrentState)
        {
            case DeviceState.Off:
                if (portStatus.GetValueOrDefault(PortID.PowerIn))
                    StartBoot();
                break;

            case DeviceState.Booting:
                // handled by coroutine
                break;

            case DeviceState.Online when deviceType == DeviceType.Modem:
                // terminal state until disconnected
                break;

            case DeviceState.WanActive when deviceType == DeviceType.Router:
                TransitionTo(DeviceState.WifiBroadcasting);
                break;
        }
    }

    private IEnumerator BootSequence()
    {
        TransitionTo(DeviceState.Booting);
        yield return new WaitForSeconds(deviceType == DeviceType.Modem ? 3f : 2f);

        if (deviceType == DeviceType.Modem)
        {
            if (portStatus.GetValueOrDefault(PortID.CoaxIn))
            {
                TransitionTo(DeviceState.DsUsSync);
                yield return new WaitForSeconds(5f);
                TransitionTo(DeviceState.Online);
            }
        }
        else // Router
        {
            var modem = FindObjectOfType<DeviceStateMachine>(
                d => d.deviceType == DeviceType.Modem);
            if (portStatus.GetValueOrDefault(PortID.WanIn)
                && modem?.CurrentState == DeviceState.Online)
                TransitionTo(DeviceState.WanActive);
        }
    }

    private void TransitionTo(DeviceState next)
    {
        CurrentState = next;
        LEDController.UpdateLEDs(deviceType, next);
        EventBus.Publish(new DeviceStateChangedEvent(deviceType, next));
        MissionValidator.Check();
    }
}
```

**LED Mapping**

| Device | State | LEDs |
|---|---|---|
| Modem | Off | All off |
| Modem | Booting | Power: amber blink |
| Modem | DS/US Sync | Power: solid, DS+US: amber blink |
| Modem | Online | Power+DS+US+Online: solid green |
| Router | Off | All off |
| Router | Booting | Power: amber blink |
| Router | WAN Active | Power: solid, WAN: solid green |
| Router | Wi-Fi Broadcasting | All: solid green |

---

## QA Notes

- [ ] Coax cable connects to Ethernet port → error feedback, no state change.
- [ ] Power cable connects before coax → modem enters Booting but not Online until coax added.
- [ ] Router powers on before modem reaches Online → router stays in Booting/WAN-inactive.
- [ ] Disconnecting power mid-boot → device returns to Off and resets timer.
- [ ] Success screen fires exactly once when router reaches Wi-Fi Broadcasting.
- [ ] 60 FPS maintained throughout interaction on mid-range mobile target device.

---

## Done Definition

A player with no developer guidance can:
1. Enter the prototype room.
2. Connect all cables correctly.
3. Watch modem and router boot in sequence with correct LED transitions.
4. Reach the success screen.

Failure paths (wrong port, wrong order) give audio/haptic feedback without crashing.

---

## Folder Destinations

| Artifact | Location |
|---|---|
| This feature file | `01_preproduction/feature_tracker/in_progress/` |
| Cable/Port C# scripts | `02_production/game_build/interaction/` |
| Device state machine scripts | `02_production/game_build/networking_logic/` |
| LED controller | `02_production/game_build/networking_logic/` |
| Prototype room scene | `02_production/levels/mission_01_first_signal/` |
| Mission validator | `02_production/game_build/mission_system/` |

---

## Changelog Reference

- V0.2.0 changelog entry pending upon implementation completion.

---

## Open Risks

| Risk | Mitigation |
|---|---|
| Port collider overlap causing double-registration | Add IsOccupied guard + physics layer mask |
| Modem/router boot race condition if both powered simultaneously | Event-driven state re-evaluation on every port change |
| 60 FPS drop on mobile due to raycast frequency | Throttle raycast to 20/s when no object is held |

---

## Decision History

| Date | Decision | Reason |
|---|---|---|
| 2026-03-14 | Use event bus (publish/subscribe) over direct method calls between devices | Decouples modem from router; mission validator can observe without coupling to device scripts |
| 2026-03-14 | Snap-to-port on confirm input, not on overlap entry | Prevents accidental connections when carrying cable past a port |
| 2026-03-14 | State machine reset to Off on any required port disconnect | Ensures players cannot cheat a partial online state |
