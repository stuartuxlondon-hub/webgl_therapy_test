# Deployment Guide: 3D Social & Therapy Sanctuary

This document outlines how to host and deploy the **3D Social & Therapy Sanctuary Prototype**. Because the application is contained entirely within a single standalone HTML file (`index.html`), deployment is straightforward and works across any static web host or WebRTC-compatible infrastructure.

---

## 1. Fast Static Hosting Options

Since all Three.js, PeerJS, and Tailwind CSS libraries are loaded via CDN, you can host this application on any free static web hosting service.

### Option A: GitHub Pages (Recommended)
1. **Create a Repository:**
   * Go to [GitHub](https://github.com) and create a new public or private repository (e.g., `therapy-sanctuary-3d`).
2. **Push Your Code:**
   * Rename your single HTML file to `index.html` if it isn't already.
   * Commit and push `index.html` (and this `DEPLOYMENT.md`) to the `main` or `master` branch.
3. **Enable GitHub Pages:**
   * Navigate to **Settings** > **Pages** in your repository.
   * Under **Source**, select `Deploy from a branch`.
   * Select `main` (or `master`) branch and root `/` folder, then click **Save**.
4. **Access Your Application:**
   * Within 1–2 minutes, your live site will be accessible at `https://<your-username>.github.io/<repository-name>/`.

---

### Option B: Vercel
1. Install the Vercel CLI via terminal: `npm i -g vercel`
2. Run `vercel` inside the folder containing `index.html`.
3. Follow the terminal prompts to deploy.
4. Alternatively, link your GitHub repository directly at [Vercel Dashboard](https://vercel.com).

---

### Option C: Netlify
1. Log in to [Netlify](https://www.netlify.com).
2. Drag and drop the directory containing `index.html` into the Netlify Web App Deploy zone.
3. Netlify will instantly provide a live HTTPS web URL.

---

## 2. HTTPS & WebRTC Requirements

> ⚠️ **Important Requirement for Voice Chat & Cross-Device Multiplayer:**
>
> WebRTC (`getUserMedia` for microphone access and peer-to-peer data channels) **requires a secure context (HTTPS)** in all modern browsers.

* When running locally for development, `http://localhost:8000` or `http://127.0.0.1:8000` is treated as a secure context.
* When hosting on the web, ensure your deployment server serves the page over **`https://`**. Services like GitHub Pages, Vercel, and Netlify provide free SSL/HTTPS out of the box.

---

## 3. WebRTC & PeerJS Infrastructure for Production

The prototype uses **PeerJS** (`peerjs@1.5.2`) over free public STUN servers provided by Google for NAT traversal. While this works well across local networks and standard home connections, corporate or strict cellular networks may require TURN relay servers.

### Adding Custom TURN Servers (Optional for Enterprise/Cellular Networks)

If users report inability to connect or share audio across strict mobile firewalls, update the `Peer` setup in `index.html`:

```javascript
peerInstance = new Peer(peerId, {
  debug: 1,
  config: {
    iceServers: [
      { urls: 'stun:stun.l.google.com:19302' },
      { urls: 'stun:stun1.l.google.com:19302' },
      // Add your TURN Server here:
      {
        urls: 'turn:your-turn-server.com:3478',
        username: 'your-username',
        credential: 'your-password'
      }
    ]
  }
});
```

* Free or low-cost TURN server providers include **Metered.ca**, **Twilio Network Traversal**, or hosting your own **Coturn** instance on AWS/DigitalOcean.

---

## 4. Local Testing Across Multiple Devices

To test multiplayer and 3D positional voice chat across your phone and laptop on the same Wi-Fi network:

1. **Serve locally:**
   ```bash
   # Using Python 3:
   python3 -m http.server 8000
   ```
2. **Find your local IP address:**
   * **macOS/Linux:** `ifconfig | grep inet`
   * **Windows:** `ipconfig`
3. **Open on phone browser:**
   * Navigate to `http://<YOUR_LOCAL_IP>:8000`
4. **Select the same Room** (e.g., `Main Deck`) on both devices to test real-time multiplayer movement, emotes, and 3D positional voice.