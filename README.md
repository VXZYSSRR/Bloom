<div align="center">

<!-- Put your logo at assets/logo.png (the flower PNG works great here) -->

<img src="assets/logo.png" alt="Bloom logo" width="140" height="140">

# Bloom

**Nearby file sharing, right in your browser.**
No account. No upload. No install. Files go straight from one device to another.


<!-- Screenshot / GIF of the Home screen goes here: -->

<!-- <img src="assets/screenshot.png" alt="Bloom home screen" width="820"> -->

</div>

\---

## Table of contents

* [What is Bloom?](#what-is-bloom)
* [Highlights](#highlights)
* [Quick start](#quick-start)
* [How it works](#how-it-works)
* [Features in detail](#features-in-detail)
* [Use cases](#use-cases)
* [Settings reference](#settings-reference)
* [Security and privacy](#security-and-privacy)
* [Browser support and requirements](#browser-support-and-requirements)
* [Troubleshooting and FAQ](#troubleshooting-and-faq)
* [For developers](#for-developers)
* [Suggest a feature](#suggest-a-feature)
* [License](#license)

\---

## What is Bloom?

Bloom is a peer-to-peer file sharing app that lives in **a single HTML file**. Open it in a browser, press **Go online**, and you will see other Bloom users who are online. Pick a person, press **Send**, and the file travels directly from your device to theirs over WebRTC. It is never uploaded to a server.

There is no sign-up, no installer, no app store and no backend to run. Bloom is the whole app in one file: the interface, the discovery client, the transfer engine and the security layer.

Bloom also goes beyond one-to-one sending. You can put files into **Open downloads** so anyone can request them, download one file from **several people at once**, **seed** files you have received so others can get them faster, chat in a **public room**, and keep a **trusted list** of devices you have verified with cryptographic fingerprints.

\---

## Highlights

|||
|-|-|
|**Direct device-to-device transfer**|Files go over WebRTC data channels when a direct connection succeeds. Nothing is uploaded to a server.|
|**Zero setup**|One HTML file. No account, no install, no server of your own.|
|**Automatic discovery**|Finds other Bloom users through public MQTT relays. Only small signed notes pass through the relays, never your files.|
|**Swarm downloads**|Downloads pull different pieces from several people at once, and every piece is checked against a signed hash.|
|**Seeding and relaying**|Files you receive can be offered onward while Bloom is open, so popular files get faster for everyone.|
|**Open downloads, Public and Live**|Share files with everyone, or see who is sending something right now.|
|**Identity you can verify**|Every message is signed. Compare safety numbers or swap pairing codes to verify a device.|
|**Private session**|One switch limits Bloom to devices you have verified.|
|**Resumable**|Unfinished downloads continue after a reload or a sleeping tab.|
|**Power meter**|Cap Bloom's speed (Eco to Max) so games and streaming keep their bandwidth.|
|**Usage charts**|See who was online, busiest hours, traffic and your own transfers. Stored only in your browser.|
|**Self-updating without an app store**|Newer versions pass from device to device. Bloom never installs anything on its own.|
|**Dark, accent-coloured UI**|Pick your own accent colour, with optional background blur and dimming when settings open.|

\---

## Quick start

### Option 1: just open the file

1. Download `Bloom-1\\\\\\\_0\\\\\\\_2.html` from this repository.
2. Open it in a modern browser (Chrome or Edge recommended, see [Browser support](#browser-support-and-requirements)).
3. Press **Go online** at the top of the Home screen.
4. Ask the person you want to share with to do the same.

That is all. Bloom shows a short tour the first time you open it.

### Option 2: host it (for example on GitHub Pages)

Because Bloom is one static file, you can host it anywhere that serves static files.

1. Put the file in your repository (rename it to `index.html` if you want it at the site root).
2. Turn on GitHub Pages for the repository.
3. Share the link.

> Hosting over `https://` works with the encrypted `wss://` relays. The unencrypted `ws://` relays only work when the file is opened from your computer or from an `http://` page, so they act as a last resort.

### Sending your first file

1. Drop files onto **Home**, or press **Choose files**.
2. Under **People Online**, find the person and press **Send**.
3. They get a request, and they decide whether to accept.
4. The file transfers directly between you. Progress shows under **Transfers**.

\---

## How it works

Bloom has two separate channels.

* **Discovery and signalling** use small signed messages over public MQTT brokers. Bloom speaks MQTT 3.1.1 over WebSocket with its own built-in client, so there is no external library. It connects to several brokers at once, and two devices only need one broker in common to find each other.
* **File data** travels peer to peer over WebRTC data channels. STUN is used to help two devices connect directly.

```mermaid
sequenceDiagram
    participant A as Your device
    participant R as Public MQTT relays
    participant B as Their device

    A->>R: signed "I'm online" note
    B->>R: signed "I'm online" note
    R-->>A: sees B in People Online
    A->>R: signed connection offer (signal)
    R-->>B: offer
    B->>R: signed answer
    A-->>B: direct WebRTC data channel
    Note over A,B: Your files travel here, never through the relays
```

What the relays can see: small signed presence, chat, listing and signalling messages. What they cannot see: your files.

\---

## Features in detail

### Discovery and presence

* **Go online / Go offline.** You become visible only while you are online, and you can disappear again at any time.
* **People Online** lists active Bloom devices with their display name, verification status and online time. Sort by **Oldest**, **Newest** or **A to Z**, and expand the list to a larger view.
* **Display name.** Choose the name other people see. A name is only a claim: the key fingerprint is the real identity.
* **Several relays at once.** Bloom connects to every relay in its list and uses whichever your network lets through. Edit the list under Advanced.
* **Presence is signed.** Bloom shares small signed notes such as your state, display name and app version.

### Sending files

* **Drag and drop** files onto Home, or use **Choose files**. Multiple files at once are supported.
* **Request and accept.** The other person sees an incoming request, checks who it is from and accepts or declines.
* **Direct transfer** over WebRTC data channels.
* **Adaptive chunking.** Bloom adjusts chunk size and backpressure to the browser's negotiated limits. You can also pin a size from 64 KB to 512 KB.
* **Queue.** Files from the same person queue in your chosen order, and you can reorder waiting files on the Downloads tab.
* **Transfers panel** shows queued, active and finished transfers. Use **Clear finished** to tidy up.
* **Interrupted transfers resume** where they can.
* **Received files** appear in their own panel, saved or ready to download.

### Open downloads, Public and Live

* **Share my files / Add to Open downloads.** Publish files so others can request them. A listing stays open until you stop sharing, and files stay available for as long as you are online.
* **Public** shows what other people are sharing.
* **Live now** shows hosts that are sending a file right now. Ask a host for a file and you can join in.
* **Requests to you** shows who has asked for your files. Accepting lists the file in Open downloads.
* **Downloads tab.** Tick several files to download together, or **Shift-click** to select a range. Sort by **Oldest**, **Newest** or **A to Z**, and use **Request** to start.
* **Downloaders and queue.** See who is downloading from you. Upload speed is shared between downloaders, and a slot frees after 60 seconds without progress.
* **Visibility control.** Choose who can see your listings: **Verified devices** or **Everyone**.

### Swarm downloads (many-to-one)

* **Pieces from several people at once.** A download can pull different pieces from up to 16 peers (default 8), so more sharers means faster downloads.
* **Signed manifests and verified pieces.** Every piece is checked against a signed hash and a bad copy is thrown away.
* **Pause, resume and recover.** Unfinished downloads continue after a reload or a sleeping tab.
* **Keep the screen awake while transferring.** Bloom asks the browser to stay awake only while something is actually moving (where the browser supports it).

### Seeding and relaying

* **Seeding.** Files you receive stay on offer to others who want the same file, only while Bloom is open, and are marked **Seeding** in Downloads. Turn it off any time.
* **Add your own files to seed** with the **+** button in the Seeding panel.
* **List my seeded files in Open downloads** so people can still find a file when the original sharer is offline.
* **Relay files I download to other people.** A finished download is kept in browser storage and offered onward, and pieces you are still fetching are shared too. Choose whether to relay to **Verified devices** or **Everyone**.

### Public chat

* A **public room** for everyone on the network.
* Messages are signed. **Clear** wipes your view of the room.
* Anyone on the same relay can read the room and see your device name, so do not share anything private there.

### Identity and trust

* **Cryptographic identity.** Each browser profile makes its own key pair. The key fingerprint is the identity, and the private key never leaves your browser.
* **Pairing codes.** Copy your code and share it over a channel you trust. Paste someone else's code to trust them.
* **Safety numbers.** Compare a safety number with the other person to verify a device without sharing codes.
* **Trusted devices.** Verified devices get a mark, and you see all of them in one place. Trust is one-way: verify them and have them verify you.
* **Warnings.** Bloom flags unverified devices and name conflicts, because anyone can type any name.
* **Only accept requests from verified devices** is available as a setting.

### Private session

Switch on **Private session** from the account menu (bottom left) when you only want to deal with people you trust.

* People Online, Live and Open downloads list only verified devices.
* Requests and relayed pieces from unverified devices are ignored.
* Your own listings go only to verified devices.
* It lasts for the session only and turns itself off when you reload.

### Power meter

Playing a game or streaming while Bloom is open? Turn the **Power meter** down in the account menu. It limits how fast Bloom sends files and pulls pieces, and nothing else.

|Level|Approximate speed|
|-|-|
|Eco|about 2 Mbit/s|
|Light|about 8 Mbit/s|
|Balanced|about 25 Mbit/s|
|Fast|about 80 Mbit/s|
|Max|no limit|

Lower levels make transfers slower but never stop them. Your choice is saved.

### Charts

A local analytics page with time ranges of **1h, 12h, 24h, 7d and 30d**:

* **People online** over time, for people visible to you.
* **Busiest hours** across the last 30 days.
* **Unique people**, counted by device key.
* **Bloom Traffic**: data moved by everyone you can see.
* **Open downloads**: public listings you can see, plus your own.
* **Your transfers**: exact figures measured on your device.

Charts show how much of the range has data, pause while you are offline, and can be cleared with **Clear data**. They are stored only in your browser.

### Updates without an app store

* When Bloom meets someone online with a newer version, it can fetch it straight from them, and your copy passes the newest version it has to people with older ones.
* Bloom never installs anything itself. You get an **Update ready** card, save the file and open it yourself.
* Accept updates only from devices you have verified, in **Settings > About**.
* **Check for updates automatically** and **Check now** are available, plus an option to get and share updates with nearby Bloom devices.

### Appearance and quality of life

* **Accent colour** picker with reset.
* **Blur** and **darken** the background when settings open, each with its own slider.
* **People list refresh** rate of 5, 10 or 15 seconds.
* **Collapsible sidebar** with a keyboard shortcut (<kbd>Ctrl</kbd>/<kbd>Cmd</kbd> + <kbd>\\</kbd>).
* **Built-in tutorial** that plays on first open, can be skipped and can be replayed from Settings.
* **Multiple profiles.** Add `#profile=name` to the page address to run a separate identity in another tab, which is great for testing with two devices on one computer.
* **Export and import settings.** Back up your trusted devices and settings to a file. Exports never include your private key.

\---

## Use cases

* **Send a file to the next desk or the next room** without email, cloud drives or USB sticks.
* **Move photos and videos between your own phone and laptop** with no cable and no cloud.
* **Share a folder of files with a group** by adding them to Open downloads and letting everyone pull from several sources at once.
* **LAN party or game night.** Swap mods, maps, installers and clips, and cap Bloom with the Power meter so it does not hurt your game.
* **Classrooms, workshops and meetups.** Hand out slides, datasets and starter files to a whole room at once. The more people who finish, the faster it gets for everyone.
* **Team hand-offs** for large files that are awkward to upload.
* **Trusted-circle sharing.** Verify your friends once, switch on Private session, and keep everything inside a closed circle.
* **Keep a file alive** by seeding, so people can still get it after the original sender goes offline.
* **Quick public chat** with people who are online.
* **Learning and tinkering.** A small, readable example of WebRTC, MQTT signalling, signed messages and piece-verified swarm downloads in plain JavaScript.

\---

## Settings reference

Open **Settings** from the sidebar.

|Page|What you can change|
|-|-|
|**Transfers**|Download folder, default download order (smallest first, largest first, order I picked, name A to Z).|
|**Sharing**|Max simultaneous downloads (1 to 50), who can see listings (verified devices or everyone), start next in line automatically.|
|**Seeding and relaying**|Keep sharing files I have received, list seeded files in Open downloads, relay files I download.|
|**Security**|Only accept requests from verified devices.|
|**Privacy and backup**|Export and import settings (trusted devices, labels and settings, never your private key).|
|**People list**|How often the People list refreshes (5, 10 or 15 seconds).|
|**Appearance**|Accent colour, background blur and amount, background darkness and amount.|
|**Help and tutorial**|Replay the tour.|
|**About**|Version, automatic update checks, **Check now**, and sharing updates with nearby devices.|
|**Advanced**|Transfer chunk size (adaptive or 64 to 512 KB), max peers per download (2 to 16), relay to (verified or everyone), keep the screen awake during transfers, discovery relay list, reset to recommended.|

The defaults work for almost everyone. Advanced options are tuning knobs for networks where something is not working.

\---

## Security and privacy

Bloom is built around one idea: **a name is only a claim, the key is the identity.**

**What Bloom does**

* Every message is signed, and identities are key fingerprints.
* Files travel over encrypted WebRTC data channels, and do not pass through the discovery relays.
* Downloaded pieces are verified against signed hashes before they are accepted.
* You can verify a person with a safety number or a pairing code, and only accept requests from verified devices.
* Private session restricts everything to devices you have verified.
* Your private key stays in your browser and is never included in exports.
* Bloom sends nothing from your device when you press **Suggest a feature**. It only opens GitHub in a new tab.

**What others can see**

* The public relays see small signed discovery messages: presence, chat, listings and connection signalling.
* Anyone on the same relay can read the public chat and see your device name.
* **People you connect to can see your IP address.** This applies to everyone you serve and every peer you connect to, which includes seeding and relaying.

**What is stored on your device**

* Your identity key, trusted devices and settings, in browser storage.
* Files kept for seeding or relaying, and resume data for unfinished downloads.
* Local usage stats for the Charts page.
* Clearing your browser storage makes you a new device.

> \\\\\\\*\\\\\\\*Please keep sensitive files off public prototypes.\\\\\\\*\\\\\\\* Bloom uses public MQTT relays for discovery, and anyone you connect to can see your IP address. Verify devices and use Private session for anything that matters.

\---

## Browser support and requirements

* A modern, up-to-date desktop or mobile browser with **WebRTC**, **Web Crypto** and **WebSocket** support.
* **Chromium-based browsers (Chrome, Edge and similar) work best.** Choosing a download folder uses the File System Access API where supported, which lets files be written to disk progressively instead of being held in memory.
* **Screen wake lock** is used when the browser supports it.
* An internet connection is needed for discovery. Bloom uses public STUN servers (Google and Cloudflare) to help devices connect.
* Bloom does not use a TURN relay, so on networks that block direct peer-to-peer connections (some strict firewalls or NATs), a connection may not succeed.
* Keep the tab open. Discovery, seeding and relaying only work while Bloom is open.

\---

## Troubleshooting and FAQ

**I do not see anyone in People Online.**
Make sure you pressed **Go online**, and that the other person did too. Both of you need at least one relay in common, so check Settings > Advanced > Discovery relays. Some networks block certain relays, and Bloom tries all of them at once.

**A transfer will not start.**
A direct connection may be blocked by your network. Try another network, or ask the other person to try. Also check that they accepted the request.

**Downloads are slow.**
Check the Power meter, and see whether other people are sharing the same file. More sharers means faster downloads. If you set the chunk size manually, try **Adaptive**.

**A transfer stopped when my laptop or phone went to sleep.**
Turn on **Keep the screen awake while a transfer is running** in Advanced. Unfinished downloads pick up where they stopped when the tab wakes or reloads.

**Is anything uploaded to a server?**
No. Files go straight between devices. Only small signed discovery messages pass through public relays.

**Do I need an account?**
No.

**Why does it say a device is not verified?**
Because anyone can type any name. Compare the safety number or swap pairing codes with the person, and Bloom will mark them verified.

**I lost my trusted devices.**
Clearing your browser storage creates a new identity. Use **Export settings** to keep a backup, and **Import settings** to restore it. Only import files you exported yourself.

**Can I use two identities on one computer?**
Yes. Open the page with `#profile=b` (or any name) in another tab to get a separate identity.

**Why did Bloom warn me that another tab is using this identity?**
Two tabs cannot share one profile. Open the second tab with a different `#profile=` name.

\---

## For developers

Bloom is intentionally one self-contained file with no build step and no dependencies.

### Running locally

Open the HTML file in a browser, or serve it with any static file server. There is nothing to install or compile.

### Structure

The script is organised into labelled sections:

* **Discovery relays**: a built-in MQTT 3.1.1 over WebSocket client that connects to several brokers at once.
* **Swarm core**: pure helpers for manifests, bitfields and piece picking, with no DOM and no network.
* **Swarm glue**: storage, signed manifests, sources, policy, connections and serving; download jobs, piece scheduling, verification, pause/resume and persistence; relay announcements, the sweeper and the settings UI.
* **Resume**: recovery after a reload or a sleeping tab.
* **Charts**: local usage statistics, stored only in the browser.
* **Update check**: compares `APP\\\\\\\_VERSION` with a small `version.json` that you host.
* **Tutorial**: first-run tour, replayable from Settings.

### Cutting a release

1. Bump `APP\\\\\\\_VERSION` in the file (it is marked "bump this in every release").
2. Optionally set `UPDATE\\\\\\\_URL` to a hosted `version.json` so Bloom can check for new versions automatically.
3. Publish the new file. Devices that already have Bloom can also pick up the new version from each other.

### Discovery relays

The default list contains several public MQTT brokers. To use your own, add a `wss://` address in Settings > Advanced, for example `wss://user:password@host:port/path`. Changes apply the next time you go online.

### Testing with two peers

Open the page in two tabs with different profiles, for example `Bloom.html#profile=a` and `Bloom.html#profile=b`, press **Go online** in both, and send a file from one to the other.

\---

## Suggest a feature

Found a bug or have an idea? Post it on the community board:

[**Open a discussion**](https://github.com/Bloom37892/Bloom/discussions/new?category=ideas)

Pull requests are welcome too. Keep Bloom a single, dependency-free file where you can.

\---

## License

<!-- Choose a license and add a LICENSE file, for example MIT. -->

Add your license here.

\---

<div align="center">
<sub>Made with care. Files go straight to the people you choose.</sub>
</div>

