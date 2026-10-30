# Recipes

You finish this chapter with five
routines worth stealing. Each says
plainly which parts the lab verified
and which parts wait on your hardware —
the same honesty as the rest of the
guide, kept in miniature.

## 1. Back up the whole network in one copy

*Verified: every file named here exists
and is written by the bridge; the lab's
own `data/` folder is the evidence.*

Stop the bridge, copy its `data/`
folder, restart it. That folder holds
`configuration.yaml` — which, on a
running network, also holds the
generated network key and the two PAN
IDs — the device database and the state
file. That copy *is* the network: with
it and the same coordinator, a rebuilt
bridge wakes up knowing every device by
name. Without it, a dead computer means
re-pairing the house. Put the copy
where your other backups go, and mind
that the network key inside it is a
key: whoever holds the file can join
your mesh.

## 2. A watchdog that notices a dead bridge

*Verified in its parts: the broker
refusals and message flow of chapter 5,
and the retained `bridge/state` topic
is the bridge's documented behaviour.*

Chapter 8's `bridge/state` topic is
retained and last-will driven, which
makes a one-line watchdog possible from
any machine that can reach the broker:

```
mosquitto_sub -h <broker> -t 'zigbee2mqtt/bridge/state' \
    --retained-only -W 5
```

The retained value prints immediately:
`online`, or `offline` if the bridge
died ungracefully. Scheduled hourly
from a second machine, that is the
difference between noticing a dead
bridge in an hour and noticing it from
a cold house.

## 3. The evening walk

*Documented: needs your devices.*

Once a few devices are paired, leave
chapter 8's `mosquitto_sub -v -t
'zigbee2mqtt/#'` running and walk the
house using things: doors, switches,
the tap, the letterbox. You are
checking three things at once — every
device you use reports, the names read
the way you think they read, and the
link quality numbers are sane for the
distances involved. Rename and re-pair
the stragglers now, while the roster is
short enough to hold in your head.

## 4. A light that must never fail

*Documented: bindings are chapter 10.*

For the one switch in the house that
must work during any outage — the
hallway, the stairs — bind the physical
dimmer directly to the bulb's group
(chapter 10), *and* leave the ordinary
automation in place for the fancy
behaviour. The binding carries the
essential job with every computer in
the house switched off; the automation
adds scenes and schedules when the
computers are alive. Essential first,
clever second, in that order.

## 5. Moving the bridge to a new machine

*Documented, on the strength of recipe
1's file list.*

Install on the new machine (chapter 4),
stop both bridges, move the `data/`
folder across, plug the coordinator
into the new machine, start. The
devices do not notice the house's brain
moved — the network key, the names and
the pairings all live in that folder
and in the coordinator itself. The two
traps are mundane: the new machine's
serial path will differ (chapter 5's
by-id path exists for this), and the
broker's address in the configuration
may need to follow the bridge if the
two are separating.
