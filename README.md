<div align="center">
  <h1>Gifted-Baileys</h1>
  <p>A WebSocket-based JavaScript library for interacting with the WhatsApp Web API</p>
  
  [![npm version](https://img.shields.io/npm/v/gifted-baileys.svg)](https://www.npmjs.com/package/gifted-baileys)
  [![npm downloads](https://img.shields.io/npm/dm/gifted-baileys.svg)](https://www.npmjs.com/package/gifted-baileys)
  [![License](https://img.shields.io/npm/l/gifted-baileys.svg)](https://github.com/mauricegift/gifted-baileys/blob/main/LICENSE)
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
