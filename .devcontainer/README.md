# lighting-ops devcontainer

Scope: this container is for editing this repo (and, if you clone
[haconfiguration](https://github.com/richardthorek/haconfiguration) alongside
it, that repo too), calling the Home Assistant REST API, and talking to
FPP/MQTT once the Pi is on the network. It is **not** for flashing the SD
card or running xLights — see "What stays native" below.

## First-time setup

1. Copy `.env.example` to `.env` in this folder and fill in `HA_URL` /
   `HA_TOKEN` (the same pair `haconfiguration`'s own CI uses — see that
   repo's `CLAUDE.md`, "Source of Truth & Deployment"). A placeholder file
   is fine for the first build; the container just needs the file to exist.
2. VS Code → **Dev Containers: Reopen in Container**. First build pulls the
   Python 3.11 image and installs Claude Code's native CLI; after that it's
   cached and reopens in seconds.
3. Run `claude` once inside the container terminal to authenticate (opens
   a browser login flow) — this is Anthropic's [native installer](https://code.claude.com/docs/en/setup),
   not the npm package, so there's no Node.js dependency and it auto-updates
   itself in the background.

To also work on `haconfiguration` in the same window: clone it as a sibling
folder next to this repo and use VS Code's **File → Add Folder to
Workspace** once attached, or copy this `.devcontainer/` folder into it —
it's small and self-contained.

## Verify it talks to your stack

Once the Pi guide's Phase H/I (`docs/pi-setup-guide.html`) is done:

```bash
curl -s -H "Authorization: Bearer $HA_TOKEN" "$HA_URL/api/config" | python3 -m json.tool
mosquitto_sub -h <ha-ip> -u fpp -P <fpp's mqtt password> -t 'falcon/#' -v
ssh fpp@<pi-ip>          # not fpp.local — see the mDNS note below
```

## The one networking gotcha: mDNS doesn't reach the container

Docker Desktop's WSL2-backed network NATs the container, and multicast DNS
(`.local` names) doesn't reliably cross that NAT. `ping fpp.local` or
`ssh fpp@fpp.local` from **inside** the container will often just hang,
even though the same command works fine from the host or a browser. Use
the DHCP reservation / static IP set up in Phase E of the Pi guide (for the
Pi) and whatever fixed address the Home Assistant host has, and use those
IPs for every `ssh`, `curl`, and `mosquitto_sub` run from inside the
container. Reserve `fpp.local` for the browser.

## What stays native (not in this container)

- **Flashing the SD card** — Raspberry Pi Imager or balenaEtcher, run
  directly on the host OS. Both need raw access to the USB SD reader as a
  block device; passing USB mass storage through a container is possible
  (e.g. `usbipd-win` on Windows) but fragile, and not worth it for
  something done rarely.
- **xLights** — native GUI app, talks to FPP over the network (FPP
  Connect), not through anything a container would help with.
- **The FPP web UI and the Home Assistant dashboard** — just a browser.
