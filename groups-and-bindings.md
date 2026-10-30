# Groups, scenes and bindings

*Taught from the project's documentation;
documented-only in `criteria.md`, for the
same reason as chapters 7 and 9.*

You finish this chapter knowing the
three ways Zigbee2MQTT makes devices act
together — a group you address as one
thing, a scene that recalls a mood, and
a binding that lets two devices talk
with the bridge out of the room — and
which of the three survives the bridge
being switched off. That last fact is
the one worth the chapter.

## Groups: one name, many devices

A **group** is a list of devices (or
parts of devices — one gang of a double
switch) given its own friendly name.
From then on the group is a citizen of
the same topic system as any device:

```
zigbee2mqtt/living_room_lights/set  {"state": "ON"}
```

The bridge fans the order out to every
member. Groups are defined in the
frontend (a page of their own) or in
the configuration file's `groups:`
section, they appear in Home Assistant
as a single light, and they fix the
classic pop-corning problem two ways at
once: the radio sends fewer, better
aimed messages, and the bulbs change
together rather than one by one down
the ceiling.

Membership has one Zigbee-flavoured
rule: every member must be a device
that can *receive* — bulbs, plugs,
dimmers. Battery sensors observe the
world; they cannot join a group because
there is nothing to tell them.

## Scenes: remembered arrangements

A **scene** is a snapshot of states
inside a group — this bulb at 40%, that
one warm, the third off — stored under
a number, recalled with
`{"scene_recall": 1}` on the group's
`/set` topic, and saved in the first
place by sending the arrangement you
want followed by `{"scene_store": 1}`.
Scenes live in the devices themselves,
which is why they recall crisply: one
radio message, and every member steps
to its remembered mark.

## Bindings: devices that talk directly

A **binding** is a Zigbee-level
instruction: *you, remote, speak
directly to that bulb (or that group)
when your button is pressed.* The bridge
sets the binding up — from the
frontend's bind tab, choosing the source,
the destination, and which clusters
(on/off, level, colour) the conversation
covers — and then steps out of the way.

That stepping-out is the point. A bound
remote keeps working **with the bridge
down, the broker down and Home
Assistant off**, because the conversation
never leaves the radio mesh. Bindings
are the right tool for the switch by
the door that must always work, and the
wrong tool for anything an automation
needs to know about, since a bound
conversation is one the bridge only
partly overhears.

One caution with a real edge: a bound
device reports its state changes to the
bridge as usual, but a device bound to
a *group* changes every member, so let
the bridge stay the one source of truth
for state — do not also command the
same bulbs individually from automations
unless you enjoy small mysteries.

## The check

Decide for three jobs, from the
descriptions alone: the ceiling bulbs
that should fade together every evening
(a group, driven by an automation); the
exact low warm arrangement for reading
(a scene in that group); the physical
dimmer by the sofa that must work during
a server outage (a binding, remote to
group). If the third answer came out
"binding" for the outage reason, the
chapter has done its work.
