<div align='center'>HT-baileys</div>

<div align="center">
  <img src="https://files.catbox.moe/vi3npg.png" alt="HT-baileys Banner" width="100%" />
  <br/><br/>

  <a href="https://heavstal-tech.vercel.app">
    <img src="https://img.shields.io/badge/Maintainer-Heavstal_Tech-007acc?style=for-the-badge&logo=vercel&logoColor=white" alt="Heavstal Tech" />
  </a>
  <a href="https://www.npmjs.com/package/@heavstaltech/baileys">
    <img src="https://img.shields.io/badge/NPM-@heavstaltech/baileys-cb3837?style=for-the-badge&logo=npm&logoColor=white" alt="NPM" />
  </a>
  <a href="https://github.com/Promise818/HT-baileys">
    <img src="https://img.shields.io/badge/Version-1.0.1-2ea44f?style=for-the-badge&logo=github&logoColor=white" alt="Version" />
  </a>
  <a href="https://github.com/Promise818/HT-baileys/blob/master/LICENSE">
    <img src="https://img.shields.io/badge/License-MIT-fab005?style=for-the-badge" alt="License" />
  </a>
</div>

---

## Introduction

**HT-baileys** is an advanced fork of the WhatsApp Web API library, engineered and maintained by **Heavstal Tech**.

This library builds upon the stability of the original Baileys architecture while integrating extended features required for modern, production-grade automation. It provides a robust solution for enterprise bots, featuring optimized connection headers, custom pairing code logic, and native support for WhatsApp Channels (Newsletters).

## Features Overview

| Feature | Description |
| :--- | :--- |
| 💬 **Send Messages to Channels** | Full support for sending text and media messages to WhatsApp Channels (Newsletters). |
| 🔘 **Button & Interactive Messages** | Native support for buttons, lists, and interactive messages on both Messenger and Business API. |
| 🤖 **AI Message Icon** | Customize message appearances with an optional AI icon (`ai: true`), adding a modern touch to automated responses. |
| 🖼️ **Full-Size Profile Pictures** | Allows uploading profile pictures in their original resolution without aggressive cropping. |
| 🔑 **Custom Pairing Codes** | **Heavstal Exclusive:** Define custom alphanumeric pairing codes (e.g., `HEAVSTAL`) for branded authentication flows. |
| 🛠️ **Libsignal Fixes** | Enhanced stability with refined logs, providing cleaner output and fewer connection drops. |

---

## Installation

### Stable Release
```bash
npm install @heavstaltech/baileys
```

### Edge Release
```bash
npm install @heavstaltech/baileys@latest
```

---

## Usage

### Basic Connection

The following example demonstrates how to initialize a socket connection using `HT-baileys`.

```ts
import makeWASocket, { 
    useMultiFileAuthState, 
    DisconnectReason, 
    Browsers 
} from '@heavstaltech/baileys'

async function connectToWhatsApp() {
    const { state, saveCreds } = await useMultiFileAuthState('auth_info')

    const sock = makeWASocket({
        auth: state,
        printQRInTerminal: false, // Set to true if using QR scanning
        browser: Browsers.macOS("Desktop"),
        syncFullHistory: true
    })

    sock.ev.on('connection.update', (update) => {
        const { connection, lastDisconnect } = update
        
        if(connection === 'close') {
            const shouldReconnect = (lastDisconnect?.error as any)?.output?.statusCode !== DisconnectReason.loggedOut
            console.log('Connection closed. Reconnecting:', shouldReconnect)
            
            if(shouldReconnect) {
                connectToWhatsApp()
            }
        } else if(connection === 'open') {
            console.log('Connection opened successfully')
        }
    })

    sock.ev.on('creds.update', saveCreds)
}

connectToWhatsApp()
```

---

## Feature Implementation

### 1. Custom Pairing Code
HT-baileys supports the definition of custom alphanumeric pairing codes, allowing for branded connection flows.

```ts
if (!sock.authState.creds.registered) {
    // Ensure the number format is correct (Country Code + Number)
    const phoneNumber = "2349000000000"
    
    // Define your branded pairing code
    const customCode = "HEAVSTAL" 
    
    setTimeout(async () => {
        const code = await sock.requestPairingCode(phoneNumber, customCode)
        console.log(`Pairing Code: ${code}`)
    }, 3000)
}
```

### 2. Newsletter Management
Provides a full suite of tools for managing WhatsApp Channels (Newsletters).

**Create a Channel**
```ts
const channel = await sock.newsletterCreate("Heavstal Updates", "Official Channel Description")
console.log("Channel Created:", channel.id)
```

**Update Channel Metadata**
```ts
await sock.newsletterUpdateName(channel.id, "Heavstal Tech")
await sock.newsletterUpdateDescription(channel.id, "Official Updates")
```

### 3. Interactive Messages (Buttons & Lists)
Native support for interactive message types, including single-select lists and call-to-action buttons.

```ts
const interactiveMessage = {
    body: { text: "Select an option below" },
    footer: { text: "Heavstal Tech" },
    header: { title: "Main Menu", hasMediaAttachment: false },
    nativeFlowMessage: {
        buttons: [
            {
                name: "single_select",
                buttonParamsJson: JSON.stringify({
                    title: "Tap to open",
                    sections: [{
                        title: "Services",
                        rows: [
                            { header: "STATUS", title: "System Status", id: "status_check" }
                        ]
                    }]
                })
            }
        ]
    }
}

await sock.sendMessage(jid, { 
    viewOnceMessage: { 
        message: { interactiveMessage } 
    } 
})
```

### 4. Album Messages
Allows sending multiple media assets (Images/Videos) grouped as a single album.

```ts
const mediaContent = [
    { image: { url: "https://example.com/image1.jpg" } },
    { image: { url: "https://example.com/image2.jpg" } }
]

await sock.sendMessage(jid, { 
    album: mediaContent, 
    caption: "Media Album" 
})
```

---

## About Heavstal Tech

**Heavstal Tech** is a forward-thinking technology organization dedicated to building robust tools and ecosystems for the modern web. From automation libraries to full-stack applications, we prioritize performance, scalability, and developer experience.

🌐 **Official Website:** [https://heavstal-tech.vercel.app](https://heavstal-tech.vercel.app)

## Disclaimer

**HT-baileys** is an independent project maintained by **Heavstal Tech**. It is not affiliated with, authorized, maintained, sponsored, or endorsed by WhatsApp Inc. or Meta Platforms, Inc.

This software is provided "as is", without warranty of any kind. Users are responsible for ensuring their usage complies with WhatsApp's Terms of Service.

---

<div align="center">
  <sub>© 2025 Heavstal Tech. All rights reserved.</sub>
</div>
