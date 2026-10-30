# Troubleshooting

You finish this chapter with a method,
not a pile of anecdotes: every entry
starts from the line or the silence you
actually see, and the first two entries
are the lab's own runs, quoted in
chapter 6 and kept as transcripts t06
and t07.

## "No such file or directory, cannot open …"

**Seen in the lab (t06).** Herdsman
tried the path in `serial.port` and the
operating system has no such device.
The stick is unplugged, or the path is
wrong, or it changed: USB serial
devices take `ttyACM0`, `ttyACM1`, … in
plug order, so yesterday's path can be
a different device today. Compare with
`ls /dev/serial/by-id/` and put the
permanent by-id path in the
configuration. On Linux the other
classic is permissions — your user
needs access to serial devices (the
`dialout` group on Debian and Ubuntu).

## The port opens, then "Inappropriate ioctl" or a failed handshake

**Seen in the lab (t07).** Something is
at that path but it is not the
coordinator the configuration claims:
a different USB gadget, a stick of
another family than `adapter:` says,
or a port already claimed by another
program (a second bridge, ZHA, a
modem manager). Herdsman's log lines
before the error tell you how far the
conversation got — no open at all is
chapter 6's first failure; dying in
the first exchange is this one.

## It starts, then cannot connect to MQTT

The radio side is fine and the post
office is shut or refusing you. Check,
in order: the broker is running; the
`mqtt.server` address and port reach it
(`mosquitto_pub` from chapter 5 with
the same credentials is the test); the
user and password in
`configuration.yaml` are the ones
`mosquitto_passwd` set. That last one
is the champion — a password changed
in one file and not the other produces
a broker log full of polite refusals
and a bridge that retries forever.

## The frontend will not load

The frontend serves only once the
bridge has started, so a dead bridge
(chapter 6) means a dead page — check
the log first, the browser second.
When the bridge is up: the port
(default 8080) may be taken by another
service, `frontend.enabled` may be
false, and if you set an `auth_token`
the page will ask for it — that is the
token working, not the page broken.

## A device pairs but shows as unsupported

The interview completed; the bridge
lacks this model's converter. Check
its exact model string (on the
device's page in the frontend) against
the supported-devices list. If the
model is there, an out-of-date bridge
is the usual gap. If it is genuinely
absent, the project's documentation
describes adding support, and many
devices still expose basic features
honestly in the meantime.

## A device fell silent

Battery first — always battery first.
Then distance: has furniture moved,
has its parent router been unplugged
(a "smart" plug on a switched socket
is a classic)? `last_seen` and
`linkquality` on its topic (chapter 8's
wildcard subscription) separate "dead"
from "stranded". A device that must be
re-paired after every outage is
usually a device whose parent changed;
re-pair it *in its final position*,
through the parent it will actually
use.

## The whole network is sluggish

Look for noise before blame: a new
Wi-Fi access point on an overlapping
channel, a USB 3 hub beside the stick
(chapter 3's extension cable exists
for this), or one malfunctioning device
flooding the mesh — the map and the
log's chatter point at it. Channel
changes are chapter 11's planned
operation, never a first resort.
