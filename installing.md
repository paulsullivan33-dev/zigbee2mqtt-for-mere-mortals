# Installing Zigbee2MQTT

You finish this chapter with the bridge
installed on your machine, the same way
the lab installed it — from the project's
own release, with the version numbers to
prove it — and you will know which parts
of the install are furniture you can
rearrange later and which are load-bearing.

**Zigbee2MQTT is a Node.js program.** It
needs Node 22 or 24 (the release's own
`package.json` asks for `^22.2.0 || ^24`),
a package tool called **pnpm**, and
nothing else exotic. There are three
supported ways to run it:

- **Standalone**, the way this guide does
  it: a folder on any Linux machine (or
  Windows, or a Mac), started with one
  command, upgraded by replacing the
  folder. Best when your server is a Pi,
  a mini PC or a NAS and you like seeing
  the files.
- **In a container**, from the project's
  own Docker image. The same folder, the
  same configuration file, the same data
  — the container just wraps it. This
  guide does not run containers in its
  lab, so the container route is described
  here, not demonstrated.
- **As a Home Assistant add-on**, if your
  Home Assistant is the kind that has
  add-ons. The add-on is this same
  program with the configuration edited
  through the Home Assistant screen
  instead of a file. Chapter 9 assumes
  the standalone form; everything it says
  about topics applies word for word to
  the add-on.

## The standalone install, as the lab ran it

Download the release archive from the
project's GitHub releases page, unpack
it, and stand in the folder it makes.
The lab fetched **2.14.2** and checked
the download's SHA-256 sum against the
file it had asked for before unpacking
(transcript t01) — a habit worth copying
for anything that will hold your house's
network key.

Then the two commands that do the real
work:

```
pnpm install --frozen-lockfile
pnpm run build
```

The first fetches the exact dependency
versions the release was tested with
(the lockfile is the list; `--frozen`
means "no substitutions"). Among them
are the two libraries the rest of the
guide leans on: **zigbee-herdsman 10.10.0**,
which speaks to coordinators, and an
MQTT client, which speaks to your broker.
The second command compiles the program
from its TypeScript source into the
`dist/` folder that actually runs. On the
lab's small shared machine the pair took
a few minutes, most of it waiting on the
package registry.

Start it once, briefly, just to see it
breathe:

```
node index.js
```

With no configuration file and no
coordinator, it will stop on its own —
politely, with log lines, not a crash
dump. Chapter 6 reads those lines closely.
For now the check is only that it starts
far enough to complain about the right
things.

## The check

Two version numbers, both runnable now:

```
node --version
grep '"version"' package.json
```

The first should say 22.x or 24.x. The
second names the release you installed —
2.14.2 for the lab. If `pnpm` itself is
missing, it travels with Node's
**corepack**: `corepack enable` puts it
on your path, and the release's
`packageManager` line pins the version it
wants (pnpm 11.26.0 for this release).
The lab's Node came without corepack on
its path, so it ran the pinned pnpm
through `npx` instead — same program,
same version, noted in `criteria.md`.

One honest warning about size: the
finished install is a few hundred
megabytes of dependencies for a program
whose own source is small. That is the
modern Node bargain, and it is the same
in the container.
