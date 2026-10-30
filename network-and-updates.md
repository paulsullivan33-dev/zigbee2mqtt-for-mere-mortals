# The map, the channel and over-the-air updates

*Taught from the project's documentation;
documented-only in `criteria.md`.*

You finish this chapter able to read
your network's health at a glance, to
understand the two kinds of update
asking for your attention — the devices'
own firmware and the coordinator's —
and to know which of the three jobs in
this chapter is safe to do casually and
which two deserve a quiet ten minutes.

## The map and the link quality number

Every device message carries
`linkquality` — a number up to 255 that
grades the last radio hop it used. It is
a weather report, not a verdict: a
sensor at 60 that reports faithfully
every time is healthy; a bulb at 180
that drops commands is not, whatever
the number says. Watch for *changes* —
a device whose link quality halves the
week the new metal fridge arrives is
telling you something true.

The frontend's **map** draws the whole
mesh: coordinator in the middle, routers
as the scaffolding they are, battery
devices hanging off whichever parent
adopted them. Read it for structure,
not decoration. A house whose devices
all hang directly off the coordinator
is a house that wants a couple of smart
plugs in the middle rooms; a device
hanging off a distant, weak parent when
a strong router sits closer is a device
to re-pair near the parent you want it
to choose (devices pick a parent when
they join, and battery devices are
conservative about changing their mind).

## Changing the channel

Chapter 3 chose a channel; changing it
later is possible (`advanced.channel`,
then a restart) and the bridge warns
you, correctly, about what follows:
every device must find the network
again on its new frequency. Mains
devices generally do. Sleeping battery
devices wake on their own schedule and
can need a button press — a walk around
the house — and a few old devices need
full re-pairing. Nothing about the
operation is dangerous, but it is the
opposite of casual: plan it like a
power cut, not like a setting.

## Device firmware, over the air

Many makers ship firmware fixes and —
more often in practice — support for
newer bridge features, and Zigbee2MQTT
can deliver them **over the air**: the
frontend's OTA page lists which of your
devices have an update available, and
one click starts a slow, careful
transfer. Slow is literal: a bulb can
take tens of minutes, stays usable
meanwhile, and should not be power
cycled mid-transfer. Do these one at a
time, on devices whose behaviour you
will notice, and read the device's page
first — occasionally an update changes
a quirk the bridge was compensating
for, and the pair of release notes
knows more than either alone.

## The coordinator's own firmware

The stick from chapter 3 runs firmware
too, and the project's adapter pages
name the versions current releases are
tested with. Updating it is the
flashing job chapter 3 mentioned:
a small tool, the project's firmware
file, a few minutes with the bridge
stopped. It is the one update in this
guide that takes the whole network
down while it happens — every device
waits, nothing is lost, and everything
rejoins when the bridge returns — so it
belongs in the same planned-quiet-ten
minutes as a channel change, and not in
the same week as one.

## The check

Three questions, answerable from this
chapter: your map shows twelve devices
hanging off the coordinator and none
off the two smart plugs — what do the
plugs' positions suggest? A battery
sensor's link quality dropped from 140
to 70 the day a metal cabinet arrived
beside it — first move, change the
channel or move the cabinet? An update
is offered for the coordinator and for
a bulb on the same evening — which one
gets the quiet ten minutes? (Plugs that
route nothing are placed where they
help nobody; move the cabinet, not the
network; the coordinator.)
