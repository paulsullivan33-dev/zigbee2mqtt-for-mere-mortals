# Pairing your first device

*This chapter needs a running network,
so the lab could not perform it: it is
taught from the project's documentation
and labelled documented-only in
`criteria.md`. The checks in it are
checks you run on your own system.*

You finish this chapter with one device
joined, interviewed, renamed to
something human, and visible in the
frontend — and you will know the three
places a pairing can stall, so a quiet
device reads as information, not
failure.

## Open the door, briefly

A Zigbee network is closed by default.
To admit a device you open joining for
a short window — **permit join** — in
one of two ways:

- In the frontend, the button at the top
  of the devices page, which starts a
  countdown (four minutes by default)
  and closes the door behind you.
- Over MQTT, by publishing
  `{"value": true, "time": 254}` to
  `zigbee2mqtt/bridge/request/permit_join`
  — the same switch, thrown from a
  script or an automation.

Leaving joining open permanently is the
one genuinely bad habit in this guide:
an open network will admit a stranger's
device as happily as yours. Open it,
pair, watch it close.

## Put the device into pairing mode

Every device has its own little ritual —
hold a button for five seconds, power
-cycle a bulb five times, press the
recessed pin with a paperclip. The
ritual is in the device's booklet and,
better, on its page in the project's
supported-devices list, which also names
any quirks (some devices only join a
network whose channel they like; some
must be kept awake by tapping their
button during the interview). Do the
ritual near a **router** — a mains
powered device — rather than next to
the coordinator: the new device joins
through whichever parent hears it best.

## The interview

Joining is only the door. What follows
is the **interview**: the bridge asks
the device what it is, which clusters
(the Zigbee word for feature bundles) it
speaks, and what it can report. In the
frontend the device appears first as a
raw IEEE address — `0x00158d0001…` — and
grows a name, a picture and controls as
the interview completes. In the log you
will see the interview start and,
moments later, `Successfully interviewed`.

Three stalls, three meanings:

- **Nothing appears at all.** The device
  never reached the network: ritual not
  done, battery dead or still wrapped,
  or joining was not actually open.
- **It appears but the interview
  fails or never finishes.** Usually a
  sleepy battery device drifting off
  mid-conversation — keep it awake — or
  a device too far from its parent.
- **It interviews but is "unsupported".**
  The bridge knows Zigbee but not this
  model's quirks. Rare for mainstream
  gear; the project's documentation has
  a path for adding support, and in the
  meantime the device may still expose
  basic features.

## Give it a name you will say out loud

The moment the interview completes,
rename the device: the frontend's pencil
icon, or the configuration file's
`devices:` section, turns
`0x00158d0001…` into a **friendly name**
like `landing_motion`. This name is not
decoration — it *is* the device's MQTT
address (`zigbee2mqtt/landing_motion`),
the name automations will use, and the
name you will say across a room. Choose
names by place and job, not by brand:
brands get replaced; the landing still
needs its motion sensor.

Renaming later is allowed and the
bridge rewrites its records, but every
automation that used the old topic needs
the same edit — another reason to name
early, name once.

## The check

In the frontend, the device row shows
its friendly name, a recent **last seen**
time and a link quality number. Poke
the device — wave at the motion sensor,
click the switch — and the last-seen
time moves and a message lands on its
topic (chapter 8 shows how to watch it
arrive). A device that interviewed, took
a name, and answers when poked is fully
paired; everything else in this guide
is refinement.
