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
    <img src="https://img.shields.io/badge/Version-4.2.0-2ea44f?style=for-the-badge&logo=github&logoColor=white" alt="Version" />
  </a>
  <a href="https://github.com/HeavstalTech/HT-baileys/blob/master/LICENSE">
    <img src="https://img.shields.io/badge/License-MIT-fab005?style=for-the-badge" alt="License" />
  </a>
</div>

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
