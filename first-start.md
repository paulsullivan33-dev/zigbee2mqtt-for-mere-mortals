# First start — and the missing radio

You finish this chapter able to read a
Zigbee2MQTT startup log the way a
mechanic listens to an engine: which
sounds are normal, which line names the
broken part. The lab's bridge never got
a healthy start — it had no radio — so
this chapter teaches the reading skill
on two real failures, and describes the
healthy start it can honestly point to
but could not perform.

## Failure one: the stick that is not there

With chapter 5's configuration in place
— broker running, password proved — the
lab started the bridge (transcript t06).
The whole run took under three seconds:

```
info:  z2m: Starting Zigbee2MQTT version 2.14.2 (commit #unknown)
info:  z2m: Starting zigbee-herdsman (10.10.0)
info:  zh:zstack:znp: Opening SerialPort with {"path":"/dev/ttyACM0","baudRate":115200,"rtscts":false,"autoOpen":false}
error: z2m: Error while starting zigbee-herdsman
error: z2m: Failed to start zigbee-herdsman
error: z2m: Exiting...
error: z2m: Error: Error: No such file or directory, cannot open /dev/ttyACM0
```

Read it as a story in order. The bridge
announces itself and its herdsman
version. Herdsman tries the serial port
*named in your configuration* — the log
quotes the path back at you, which is
the first thing to compare against
reality (`ls /dev/serial/by-id/` shows
what is actually plugged in). The
operating system answers *no such file
or directory*, and the bridge stops:
no coordinator, no network, no point
pretending otherwise. It exits with
code 1 and a clear conscience.

Notice what is *not* in the log: the
broker. Zigbee2MQTT starts the radio
side first and connects to MQTT only
once herdsman is happy — the lab
confirmed this in the broker's own log,
which shows no bridge connection during
either failed start. So a startup that
dies before any mention of MQTT is a
radio-side problem, and one that gets
past herdsman and then complains is a
broker problem (wrong password is the
usual suspect, and chapter 5's check
exists precisely to clear it in advance).

## Failure two: something answers, but it is not a radio

For the second lesson the lab pointed
the configuration at a **pseudo-terminal**
— a software stand-in for a serial port
(transcript t07). The port *opened*. The
log advanced two lines further:

```
info:  zh:zstack:znp: Serialport opened
info:  zh:zstack:znp: Writing CC2530/CC2531 skip bootloader payload
info:  zh:zstack:znp: Skip bootloader for CC2652/CC1352
error: z2m: Error: Error: Inappropriate ioctl for device, cannot set
```

Herdsman got as far as poking the port
the way it pokes a fresh TI stick, and
the pretend device could not perform a
real serial port's control tricks
(*ioctl* is the operating system's word
for them), so the start died there
instead. The lesson generalises: **how
far the log gets tells you what is
wrong.** No open at all — the path is
wrong or the stick is absent. Opens but
dies in the first exchange — the device
at that path is not the coordinator you
think it is (a modem, a different
adapter family than `adapter:` claims,
a stick still claimed by another
program). The guide's lab explored this
dead end so a reader who lands in it
recognises the neighbourhood.

There is no software trick past this
point. The current release has no dummy
or virtual adapter among its `adapter`
choices (the full list — `zstack`,
`ember`, `deconz`, `zigate`, `ezsp`,
`zboss`, `zoh` — comes from the release's
own settings schema, which the lab
read). A bridge without a coordinator
is a translator with nobody to translate
for, and everything after this chapter
assumes the stick from chapter 3 is in
your hand.

## The healthy start, as documented

With a real coordinator the same log
continues instead of stopping: herdsman
reports the adapter's firmware version
and the network's channel, you will see
the generated `network_key`, `pan_id`
and `ext_pan_id` written into
`configuration.yaml`, then a line saying
the bridge has **connected to MQTT**,
then the frontend's address
(`http://localhost:8080` by default).
Devices already paired in a previous
life rejoin and announce themselves;
on a brand-new network, silence after
that point is success — the bridge is
waiting for chapter 7.

Some files appear in `data/` even on a
failed start, and the lab's runs
produced them: the configuration
backups and migration notes from
chapter 5, and a `log/` folder holding
`log.log` — the same lines the console
showed, and the file to attach when you
ask anyone for help. What a failed
start does *not* leave behind is just
as telling: no `state.json`, no device
database. Those are written by a bridge
that has actually run a network, so
their absence is one more confirmation
of how far yours got.

## The check

Start your bridge once and find, in
order: the version line, the herdsman
line, and the serial-port line quoting
*your* path. If the run fails, the last
error line names either a path, a port
that is not a radio, or the broker.
Whichever it names is the chapter you
go back to — 5 for the broker and the
file, 3 for the stick.
