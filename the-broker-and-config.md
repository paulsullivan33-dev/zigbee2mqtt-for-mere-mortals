# The broker and the configuration file

You finish this chapter with a running
MQTT broker that refuses strangers, a
complete `configuration.yaml` for the
bridge, and a check — done against the
lab's own broker, with the bridge's own
password — that the two will be able to
talk when the radio arrives.

## The broker: Mosquitto, with a password

On a normal machine the install is one
line (`apt install mosquitto
mosquitto-clients` on Debian and Ubuntu;
the project also publishes its own
packages and a container). The lab's
machine had no package service it could
use, so it unpacked Ubuntu's Mosquitto
**2.0.18** .deb files into a folder and
ran the same binaries from there —
transcript t03 in the lab folder. The
broker does not care which way it
arrived.

Out of the box, Mosquitto listens only
on the same machine and allows anonymous
local connections. For a bridge that will
hold your house's device traffic, give
it a password file and one named user
for the bridge:

```
mosquitto_passwd -c /etc/mosquitto/passwd z2m-bridge
```

(`-c` creates the file; you are prompted
for the password. Choose your own — the
lab's was `lab-password-123`, which is
exactly as good as it sounds and is
changed by nobody because the lab is
thrown away.)

The broker's own configuration then
needs four ideas, in the lab's order:

```
listener 1883 127.0.0.1
allow_anonymous false
password_file /etc/mosquitto/passwd
persistence false
```

Listen on the loopback address for now —
the bridge and broker live on the same
machine in this guide, and a broker
nobody else can reach needs no firewall
story. (`persistence false` suits a lab;
a house broker that should remember
messages across restarts wants it on,
with a directory it may write to.)

## Proving the broker before the bridge

Start Mosquitto, then test it the way
chapter 2 promised — a message in, the
same message out, with the bridge's
credentials (transcript t04):

```
mosquitto_sub -h 127.0.0.1 -u z2m-bridge -P '…' -t 'lab/check' -v
mosquitto_pub -h 127.0.0.1 -u z2m-bridge -P '…' -t 'lab/check' \
    -m 'broker and credentials work'
```

The subscriber prints
`lab/check broker and credentials work`.
Then the two refusals, which matter as
much: publish with a **wrong** password,
and again with **no** credentials. Both
come back `Connection Refused: not
authorised`, and the broker's log says
the same. If a stranger can publish to
your broker, they can switch your lights
— this ten-second check is the fence.

## The bridge's configuration file

The bridge reads `data/configuration.yaml`
in its folder. Here is the lab's real
file (transcript t05), the one the next
chapter starts with:

```yaml
homeassistant:
  enabled: false

frontend:
  enabled: true
  port: 8080

mqtt:
  base_topic: zigbee2mqtt
  server: mqtt://localhost:1883
  user: z2m-bridge
  password: lab-password-123

serial:
  port: /dev/ttyACM0
  adapter: zstack

advanced:
  log_level: info
  channel: 11
```

Line by line, the decisions:

- **`homeassistant.enabled: false`** —
  discovery stays off until chapter 9, so
  the bridge's own behaviour is visible
  first. Turning it on later is one line.
- **`frontend`** — the bridge's own web
  dashboard, on port 8080. You will use
  it constantly. (Give it an
  `auth_token` before you ever expose it
  beyond the machine itself.)
- **`mqtt`** — where the broker lives and
  the name the bridge answers to. Every
  topic in this guide starts with
  `base_topic`. The user and password are
  the pair you just proved.
- **`serial`** — where the coordinator
  is plugged in, and which dialect it
  speaks (chapter 3). On a real machine,
  name the stick by its permanent
  `/dev/serial/by-id/…` path rather than
  `/dev/ttyACM0`, which can change places
  with other USB devices at reboot. For a
  network coordinator this becomes
  `port: tcp://192.168.1.12:6638`.
- **`advanced.channel`** — the radio
  channel from chapter 3's discussion;
  11 is the project's default.

Three settings are absent on purpose.
`network_key`, `pan_id` and `ext_pan_id`
default to `GENERATE` in the example
file the release ships: on the first
successful start the bridge invents them
and writes them into this file, and from
then on they *are* your network's
identity — which is why the first thing
to back up, once a network exists, is
this file.

One change you can watch happen: when
2.14.2 first read the lab's file it
stamped `version: 5` at the top and kept
backups of the steps (`configuration_
backup_v*.yaml`, plus short migration
notes in the data folder). That is the
configuration format's own version
number, and the bridge manages it
itself.

## The check

The broker check above is the chapter's
real test. For the file, the check is
dumber and worth doing: it must be valid
YAML, tabs nowhere, and the password in
it must be the one `mosquitto_passwd`
just set — the single most common reason
a first start fails at the *broker* is a
password that was changed in one file
and not the other.
