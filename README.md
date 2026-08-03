<div align='center'>HT-baileys</div>

 <a><img src='https://i.imgur.com/LyHic3i.gif'/></a><a><img src='https://i.imgur.com/LyHic3i.gif'/></a>
 
<div align="center">
  <img src="https://files.catbox.moe/vi3npg.png" alt="HT-baileys Banner" width="100%" />
  <br/><br/>

  <a href="https://heavstal-tech.vercel.app">
    <img src="https://img.shields.io/badge/Maintainer-Heavstal_Tech-007acc?style=for-the-badge&logo=vercel&logoColor=white" alt="Heavstal Tech" />
  </a>
  <a href="https://www.npmjs.com/package/@heavstaltech/baileys">
    <img src="https://img.shields.io/badge/NPM-@heavstaltech/baileys-cb3837?style=for-the-badge&logo=npm&logoColor=white" alt="NPM" />
  </a>
  <a href="https://github.com/HeavstalTech/HT-baileys">
    <img src="https://img.shields.io/badge/Version-4.4.2-2ea44f?style=for-the-badge&logo=github&logoColor=white" alt="Version" />
  </a>
  <a href="https://github.com/HeavstalTech/HT-baileys/blob/master/LICENSE">
    <img src="https://img.shields.io/badge/License-MIT-fab005?style=for-the-badge" alt="License" />
  </a>
</div>


## Introduction

**HT-baileys** is an advanced fork of the WhatsApp Web API library, engineered and maintained by **Heavstal Tech**.

This library builds upon the stability of the original Baileys architecture while integrating extended features required for modern, production-grade automation. It provides a robust solution for enterprise bots, featuring optimized connection headers, custom pairing code logic, and native support for WhatsApp Channels (Newsletters), Interactive Messages, and more.

WhatsApp Baileys is an open-source library designed to help developers build automation solutions and integrations with WhatsApp efficiently and directly. Using websocket technology without the need for a browser, this library supports a wide range of features such as message management, chat handling, group administration, as well as interactive messages and action buttons for a more dynamic user experience.

Actively developed and maintained, baileys continuously receives updates to enhance stability and performance. One of the main focuses is to improve the pairing and authentication processes to be more stable and secure. Pairing features can be customized with your own codes, making the process more reliable and less prone to interruptions.

This library is highly suitable for building business bots, chat automation systems, customer service solutions, and various other communication automation applications that require high stability and comprehensive features. With a lightweight and modular design, baileys is easy to integrate into different systems and platforms.

## Disclaimer

This project is not affiliated, associated, authorized, endorsed by, or in any way officially connected with WhatsApp or any of its subsidiaries or affiliates. Use at your own discretion. Do not spam people with this. We discourage any stalkerware, bulk or automated messaging usage.

## Installation

```bash
npm install @heavstaltech/baileys
```

Or using yarn:
```bash
yarn add @heavstaltech/baileys
```

## Quick Start

### CommonJS (Recommended)
```javascript
const { default: makeWASocket, useMultiFileAuthState, Browsers } = require('@heavstaltech/baileys')
// use same code below
```

### ES Modules / TypeScript
```javascript
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

### Important Note for ES Module (`"type": "module"`) Users

Because `@heavstaltech/baileys` is compiled as a CommonJS module, Node.js cannot detect its named exports during the import phase. Attempting to use standard named imports will throw a `SyntaxError: Named export not found`.

To fix this, you must import the entire package as a default object and destructure the variables/functions from it:

**Incorrect (Will throw an error):**
```javascript
import { proto, delay, getContentType, areJidsSameUser, generateWAMessage } from "@heavstaltech/baileys";
```

**Correct (Destructuring the default export):**
```javascript
import pkg from "@heavstaltech/baileys";
const { proto, delay, getContentType, areJidsSameUser, generateWAMessage } = pkg;
```

*(Note: If you need to import the main socket function, which is exported as `default` under the hood, you can alias it during the destructuring like this: `const { default: makeWASocket } = pkg;`)*


## Features

- Full WhatsApp Web API support
- Multi-device support with QR code and pairing code authentication
- LID (Link ID) addressing support for both personal chats and groups
- Group status/story sending functionality
- Session management and restoration
- Message sending, receiving, and manipulation
- Group management
- Privacy settings
- Profile management
- And much more!

> **Note:** This version doesn't support buttons, conider using other moded libraries for sending of buttons



## About Heavstal Tech

**Heavstal Tech** is a forward-thinking technology organization dedicated to building robust tools and ecosystems for the modern web. From automation libraries to full-stack applications, we prioritize performance, scalability, and developer experience.

🌐 **Official Website:** [https://heavstal.com.ng](https://heavstal.com.ng)

## Disclaimer

**HT-baileys** is an independent project maintained by **Heavstal Tech**. It is not affiliated with, authorized, maintained, sponsored, or endorsed by WhatsApp Inc. or Meta Platforms, Inc.

This software is provided "as is", without warranty of any kind. Users are responsible for ensuring their usage complies with WhatsApp's Terms of Service.

---

<div align="center">
  <sub>© 2025 - 2026 Heavstal Tech. All rights reserved.</sub>
</div>

 <a><img src='https://i.imgur.com/LyHic3i.gif'/></a><a><img src='https://i.imgur.com/LyHic3i.gif'/></a>
