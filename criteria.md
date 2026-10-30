# Writing criteria for this guide

Every "for Mere Mortals" guide is written to the same nine-point
standard, so a reader who trusts one can trust the rest. Here it
is, with the verification notes for this particular guide below.

1. **Fix it first.** Reader outcomes lead, not action items: each
   chapter opens by naming the outcome it delivers, shows a check
   the reader can run to confirm they got it, and a reasonable
   reader finishes able to state what was done, what it means,
   and what to try next.
2. **Plain language.** Short sentences, everyday vocabulary,
   technical terms defined on first use. No slang, and no
   idioms that only make sense in one country — the reader may
   be translating as they go.
3. **No dead ends.** A guide never tells the reader to do
   something it cannot show them how to verify. Every claim a
   reader relies on — an install, a command, an output — is
   something this guide demonstrates or clearly labels
   documented-only below.
4. **Live-verified where it matters.** Procedures are run for
   real in the guide's lab wherever the build machine allows it,
   and the transcripts kept in the repository are the evidence.
   What could not be run is listed honestly below, never
   presented as if it had been.
5. **General audiences.** Nothing in this guide — examples,
   fixtures, names, addresses — is modelled on any particular
   reader or author's real life, network, or organisations.
   The lab's devices are invented rooms and invented sensors.
6. **Standalone.** A reader needs this guide, its lab, and
   nothing else. Where another guide would genuinely help, it
   is pointed to once, by name, and the chapter still completes
   without it.
7. **Honest about limits.** The Limits chapter names what the
   tool cannot do: it cannot start without a coordinator, one
   stick serves exactly one network, the mesh obeys physics
   rather than settings, device support is per-model, sleeping
   devices answer late by design, and running your own cloud
   means keeping your own keys.
8. **The ethics rule is in the text, early.** This bridge's
   whole purpose is that a house's device traffic stays in the
   house; chapter 1 states that before any procedure depends on
   it, and the duties that come with it are stated where they
   bite — a password on the broker (chapter 5) and never
   leaving permit join open (chapter 7) — because a bridge that
   admits or exposes a stranger's devices has failed at its one
   job.
9. **Builds clean.** The site builds with MkDocs in strict mode
   with no warnings, and a fresh reader following the chapters
   in order reaches the stated outcome of each.

## Verification notes for this guide

The build machine is a 2-CPU, 7.7 GB Ubuntu 24.04 sandbox,
shared with other work while the lab ran (timings are therefore
ranges, and the guide says so). It has **no Zigbee radio and no
USB serial devices at all**, which shaped this guide's evidence
in a way the text states openly: everything up to the radio is
run and transcribed; everything past it is documented-only.

Zigbee2MQTT **2.14.2** — the current official release — was
installed from the project's own GitHub release archive
(SHA-256 checked, transcript t01) with its pinned pnpm
(11.26.0, run through `npx` because this Node has no corepack
on its path) and compiled with its own build script (t02).
One optional native module, `unix-dgram` (syslog transport
only), failed to compile in the sandbox; pnpm carried on, the
default logging never touches it, and both bridge starts below
ran on this exact install. Mosquitto **2.0.18** was unpacked
from Ubuntu's .deb packages and run from the folder (t03)
because the sandbox has no usable package service; the
binaries are the distribution's own, unmodified. The exact
configuration key names and the adapter list quoted in
chapters 5 and 6 were read out of the installed release's own
settings schema (`dist/util/settings.schema.json`), not from
memory.

Run for real, with transcripts as evidence:

- Environment and versions: Node 24.22.1, npm 10.9.4, Ubuntu
  24.04, and the release archive's SHA-256 (t00, t01).
- The dependency install (207 MB of `node_modules`) and the
  TypeScript build producing `dist/` (t02).
- The broker started with password authentication on and
  anonymous access off; its startup log, including the
  sandbox-specific privilege warnings the text explains (t03).
- Publish/subscribe against that broker with the exact
  credentials the bridge configuration uses, and the two
  refusals — wrong password and no credentials — both
  `Connection Refused: not authorised`, exit code 5 (t04).
- The complete `configuration.yaml` the bridge then read,
  plus what the first start wrote back into it: the
  `version: 5` stamp and the configuration backups (t05).
- First start with the configured port absent: exit code 1
  in under three seconds, ending `No such file or directory,
  cannot open /dev/ttyACM0`, with zigbee-herdsman 10.10.0
  named in the log (t06). The broker's log confirms the
  bridge never reached it — herdsman starts first.
- First start against a pseudo-terminal standing in for a
  serial port: the port opened, herdsman wrote its bootloader
  payload, and the start died at `Inappropriate ioctl for
  device, cannot set` — the second failure mode chapter 6
  teaches (t07).

Documented only — described from the project's documentation,
not run, and the text does not claim otherwise:

- Everything requiring a coordinator or a device: pairing and
  the interview (chapter 7), live device topics and `/set`
  traffic (chapter 8's conventions are documented; its
  publish/subscribe mechanics are the ones verified in t04),
  Home Assistant discovery (chapter 9), groups, scenes and
  bindings (chapter 10), the network map, channel changes and
  over-the-air updates (chapter 11), and the healthy startup
  log chapter 6 describes after its two verified failures.
- Choosing and flashing a coordinator (chapter 3): assembled
  from the project's supported-adapters documentation.
- The container and Home Assistant add-on install routes
  (chapter 4): the sandbox cannot run containers.
- The neighbour tools of chapter 13 (ZHA, deCONZ, vendor
  hubs, Thread and Matter) — characterised, not run.

Fixtures are this guide's own and deliberately generic: a
hallway sensor, a kitchen bulb, a landing motion sensor, a
living-room light group, the username `z2m-bridge` and a
password (`lab-password-123`) that belongs to a thrown-away
lab and is printed in the guide precisely so no reader copies
it into a house. No real network, house or person is depicted.
