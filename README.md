# Bloom

**Nearby file sharing and private video rooms, right in your browser.**

Bloom is a lightweight, browser-based peer-to-peer sharing app. Send files directly to other devices, discover people who are online, and now meet in private rooms with voice, video, screen sharing and room chat.

No account. No installer. No traditional backend to run. Bloom is designed to work as a self-contained HTML app.

---

## What's new in version 2.0.0

Bloom now includes **private Rooms** alongside its existing file-sharing features.

- **Create a private room** and invite people with a short, six-character room code.
- **Join a room** by entering its code.
- **Voice calls** with an individual microphone on/off control.
- **Camera video** with an individual camera on/off control.
- **Live screen sharing** so participants can show a screen, application window or browser tab.
- **Room chat** for messages during a session.
- **Participant list** showing who is in the room.
- **Copy room code** to quickly invite others.
- **Leave room** when the session is finished.

### Room media controls

The room toolbar has three independent controls:

| Control | What it does |
| --- | --- |
| **Mic** | Turns your microphone on or off. |
| **Camera** | Starts or stops your camera video. |
| **Share screen** | Opens the browser's screen/window/tab picker. Click it again to stop sharing. You can also stop sharing from the browser's own sharing indicator. |

The controls show their active state. Microphone, camera and screen sharing can be switched independently while you remain in the room.

> Screen capture is subject to browser support and permission. Use a modern browser and open Bloom from a secure context, such as an HTTPS website or localhost. Your browser will ask you to choose what to share.

---

## Core features

### Peer-to-peer file sharing

- Discover other Bloom devices that are online.
- Send one or multiple files directly to another person.
- The recipient can accept or decline an incoming transfer.
- Track queued, active and completed transfers.
- Resume supported interrupted downloads.
- Share files through **Open downloads** so other people can request them.
- Download from multiple peers at once with piece verification.
- Seed and relay files you have received while Bloom is open.

### Private Rooms

- Create a room or join one using a six-character code.
- Invite others by sharing the room code.
- Talk using your microphone and camera.
- Share your screen, a window or a browser tab.
- Send messages in the room chat.
- See the people currently in the room.
- Toggle your microphone, camera and screen share without leaving the room.

### Discovery, chat and presence

- Go online or offline whenever you choose.
- See other Bloom devices that are currently online.
- Choose the display name shown to other devices.
- Use the public network chat, or restrict activity with Private session.
- See device verification status and manage trusted devices.

### Identity and privacy tools

- Each browser profile has its own cryptographic identity.
- Verify another device using a pairing code or safety number.
- Restrict supported interactions to devices you have verified with **Private session**.
- Your private key stays in browser storage and is not included in settings exports.

### More features

- Adaptive file-transfer chunking and configurable transfer limits.
- Local usage charts and transfer statistics.
- A bandwidth Power meter, from Eco to Max.
- Dark interface with customizable accent colour.
- Multiple browser profiles for testing or separating identities.
- Update checks and device-to-device update sharing.

---

## Quick start

### Option 1: Open the HTML file

1. Download the latest Bloom HTML file from the [Releases](https://github.com/VXZYSSRR/Bloom/releases) page, or from the repository.
2. Open the HTML file in a modern browser. Chrome or Edge is recommended.
3. Press **Go online** to appear in the people list and use the file-sharing features.
4. Open **Rooms** in the sidebar to create or join a private room.

Bloom does not require an installation or build step.

### Option 2: Host Bloom on a website

Bloom is a static HTML app, so it can be hosted by a static web host such as GitHub Pages.

1. Add the HTML file to your repository.
2. If you want Bloom to open at the site root, name the file `index.html`.
3. Enable GitHub Pages (or use another static host).
4. Open the HTTPS website in your browser.

Hosting over HTTPS is recommended. Browser media features such as camera, microphone and screen capture generally require a secure context.

---

## Using Rooms

### Create a room

1. Open Bloom.
2. Select **Rooms** from the sidebar.
3. Choose **Create a private room**.
4. Bloom will create a room and display its six-character code.
5. Copy the code and send it to the people you want to invite.

### Join a room

1. Open **Rooms**.
2. Enter the six-character code you received.
3. Press **Join**.
4. Once connected, you can use the room controls and chat.

**Important:** Anyone who has the room code may be able to join. Only share it with people you intend to invite.

### Start voice or video

- Select **Mic** to enable or disable your microphone.
- Select **Camera** to start or stop camera video.
- Your browser may ask for permission the first time you use either device.
- You can change these controls while staying in the room.

### Share your screen

1. Select **Share screen**.
2. Choose a screen, application window or browser tab in the browser picker.
3. Confirm the selection to begin sharing.
4. Select **Share screen** again to stop, or use the browser's built-in stop-sharing control.

If the screen-share option is unavailable, check that you are using a supported browser and that Bloom is open over HTTPS or localhost.

### Chat and participants

The room's side panel shows the current participants and contains the room chat. Enter a message and send it to communicate with everyone in that room.

Choose **Leave** when you want to exit the room.

---

## How Bloom works

Bloom uses separate mechanisms for discovery/signalling and peer-to-peer data.

- **Discovery and signalling:** small messages are exchanged through public MQTT-over-WebSocket relays so Bloom devices can find one another and establish connections.
- **File transfers:** file data is sent directly between connected devices using WebRTC data channels.
- **Room media:** room voice, camera video and screen sharing use browser media capture and WebRTC peer connections.
- **Room invitations:** a short room code is used to identify the room and help invited participants find it.

The relays help devices discover and connect. They are not intended to host your file transfers or room media.

A direct peer-to-peer connection is not guaranteed on every network. Strict firewalls, NAT configurations or networks that block WebRTC connectivity can prevent a connection from being established.

---

## Browser support and permissions

Use an up-to-date browser with support for:

- WebRTC
- Web Crypto
- WebSocket
- `getUserMedia` for microphone and camera
- `getDisplayMedia` for screen capture

Chrome and Edge are recommended.

For voice and video, allow Bloom to use your microphone and camera when prompted. For screen sharing, choose the screen, window or tab in the browser's own picker.

Screen sharing requires a browser that supports `getDisplayMedia` and a secure context (normally HTTPS or localhost). The browser controls which display surfaces can be selected.

Keep Bloom open while you are using online features. Discovery, room connections, seeding and relaying depend on the page remaining active.

---

## Security and privacy notes

Bloom is designed to keep file transfers and room media peer-to-peer when a direct connection succeeds. However, peer-to-peer does not mean anonymous:

- Public discovery relays can see the small messages used for presence, listings, chat and connection signalling.
- Other peers you connect to may be able to see your IP address as part of establishing a peer-to-peer connection.
- Anyone with a private room's code may be able to join it.
- The public network chat is visible to people using the same relay network.
- Browser storage contains your local identity, trusted devices, settings and supported transfer data.

Use **Private session** and verify devices when you need to limit interactions to people you trust. Do not use public prototypes for sensitive information.

---

## Troubleshooting

### I cannot see anyone online

- Make sure you pressed **Go online**.
- Ask the other person to do the same.
- Check that both devices have at least one discovery relay in common.
- Some networks block particular WebSocket relays.

### I cannot join a room

- Check the six-character code carefully.
- Ask the room creator to confirm that the room is still active.
- Check your internet connection and browser console for connection errors.
- Some network configurations prevent direct WebRTC connections.

### My microphone or camera does not work

- Check the browser's site permissions.
- Make sure another application is not exclusively using the device.
- Check that the correct microphone or camera is selected in browser/system settings.
- Toggle the control off and on again after changing permissions.

### Screen sharing does not start

- Use a current version of Chrome, Edge or another browser that supports `getDisplayMedia`.
- Open Bloom from an HTTPS page or localhost.
- Allow the browser to open the screen-sharing picker.
- Choose a screen, window or tab.
- If you previously denied permission, check the browser's site settings.

### A peer-to-peer connection will not establish

Some restrictive networks, firewalls and NAT configurations prevent direct connections. Try another network. Bloom's current peer-to-peer approach does not guarantee connectivity through every network.

---

## For developers

Bloom is intentionally distributed as a self-contained HTML file.

- No package installation or compilation is required to open the app.
- The interface and application logic are contained in the HTML file.
- The application uses browser APIs for local storage, cryptography, WebRTC and media capture.
- Discovery/signalling uses MQTT over WebSocket relays.

### Local development

Open the HTML file in a modern browser. For APIs that require a secure context (including camera, microphone and screen capture), use HTTPS or localhost.

### Release checklist

1. Update the `APP_VERSION` constant in the HTML file.
2. Update the version shown in the app's About/settings area if it is maintained separately.
3. Name the release file clearly (for example, `Bloom-2.0.0.html`).
4. Update this README if features or requirements change.
5. Publish the HTML file and README to the repository.

---

## Contributing and support

Found a bug or have an idea? Open an issue in the [GitHub repository](https://github.com/VXZYSSRR/Bloom). Pull requests and suggestions are welcome.

---

## License

Bloom is distributed under the [MIT License](LICENSE).

---

**Bloom — share files, talk, collaborate and stay connected.**
