# OPWork — Web3 Freelance Marketplace on Bitcoin

> Decentralized freelance platform powered by **OP_NET** Bitcoin smart contracts. Hire and get hired — paid instantly in BTC and OP_20 tokens, no middlemen.

![Version](https://img.shields.io/badge/version-1.0.0-orange) ![Bitcoin](https://img.shields.io/badge/chain-Bitcoin%20L1-f7931a) ![OP_NET](https://img.shields.io/badge/protocol-OP__NET-orange) ![License](https://img.shields.io/badge/license-MIT-green)

---

## Overview

OPWork adalah platform freelance berbasis Web3 yang berjalan di atas Bitcoin Layer 1 menggunakan protokol **OP_NET**. Terinspirasi dari Upwork, namun sepenuhnya terdesentralisasi — semua pembayaran dikunci dalam smart contract escrow di Bitcoin, dan dirilis otomatis setelah delivery dikonfirmasi.

Tidak ada akun terpusat. Tidak ada fee platform tersembunyi. Tidak ada pihak ketiga yang memegang dana. Cukup connect wallet Bitcoin kamu dan mulai bekerja atau merekrut.

---

## Features

### Wallet Connection
- **OP_WALLET** (native OP_NET) — deteksi otomatis via `window.opnet`
- **UniSat Wallet** — via `window.unisat`
- **Xverse Wallet** — via `window.XverseProviders`
- **OKX Wallet** — via `window.okxwallet.bitcoin`
- Input manual alamat Bitcoin / Taproot (read-only mode)
- Auto-reconnect saat halaman di-reload tanpa popup ulang
- Real-time account change listener (`accountsChanged` event)
- Badge **"✓ Installed"** otomatis jika ekstensi terdeteksi di browser

### Job Marketplace
- Browse lowongan dengan filter kategori: Development, Design, Marketing, Writing, Data & AI
- Search realtime berdasarkan judul, deskripsi, dan skill tag
- Pembayaran ditampilkan dalam **BTC**, **OP_20**, **PILL**, dan **sats** lengkap estimasi USD
- Live feed transaksi animasi di hero section
- Detail modal per job dengan info client, smart contract address, dan escrow info

### OP_NET Escrow
- Setiap job terhubung ke smart contract address di Bitcoin L1
- Dana dikunci otomatis saat job diposting
- Rilis hanya setelah client konfirmasi delivery
- Tidak ada dispute — logika escrow dijalankan on-chain

### Post a Job
- Form posting: judul, kategori, deskripsi, budget, token pilihan
- Pilihan token: BTC, OP_20, PILL, sats
- Tipe kontrak: Fixed Price, Hourly, Milestone
- Lock escrow otomatis saat submit (requires wallet connected)

### Dashboard
- Total earnings, active contracts, in-escrow balance, reputation score
- Terkunci jika wallet belum terhubung

---

## Tech Stack

| Layer | Teknologi |
|---|---|
| Frontend | Vanilla HTML, CSS, JavaScript (single file) |
| Fonts | Space Grotesk, JetBrains Mono |
| Wallet API | OP_WALLET / UniSat (`window.unisat`), Xverse, OKX |
| Smart Contract | OP_NET Bitcoin L1 Protocol |
| Token Standard | OP_20 (Bitcoin-native fungible token) |
| Escrow | OP_NET Smart Contract (AssemblyScript) |

---

## Getting Started

### Prerequisites

Install salah satu wallet Bitcoin yang kompatibel:

- **OP_WALLET** (Recommended) → [Chrome Web Store](https://chromewebstore.google.com/detail/opwallet/pmbjpcmaaladnfpacpmhmnfmpklgbdjb)
- **UniSat** → [unisat.io](https://unisat.io)
- **Xverse** → [xverse.app](https://www.xverse.app)
- **OKX Wallet** → [okx.com/web3](https://www.okx.com/web3)

### Run Locally

```bash
# Clone atau download file
git clone https://github.com/yourname/opwork-web3.git
cd opwork-web3

# Buka langsung di browser (penting: jangan pakai iframe/preview)
open opwork-web3.html
```

> **Penting:** File harus dibuka langsung di browser (bukan via iframe embed atau preview tools) agar ekstensi wallet dapat inject provider ke halaman (`window.opnet`, `window.unisat`, dll).

---

## Wallet Integration

### OP_WALLET / UniSat

OP_WALLET adalah fork dari UniSat dan menggunakan API yang sama:

```javascript
// Cek apakah OP_WALLET terinstall
const provider = window.opnet || window.unisat;

// Request connect (memunculkan popup ekstensi)
const accounts = await provider.requestAccounts();

// Ambil saldo (dalam satoshi)
const balance = await provider.getBalance();
// { confirmed: 100000, unconfirmed: 0, total: 100000 }

// Ambil public key
const pubkey = await provider.getPublicKey();

// Listen account changes
provider.on('accountsChanged', (accounts) => {
  console.log('Account changed:', accounts[0]);
});

// Auto-reconnect tanpa popup (cek sesi sebelumnya)
const existing = await provider.getAccounts();
```

### Xverse

```javascript
const provider = window.XverseProviders?.BitcoinProvider;

const response = await provider.request({
  method: 'getAccounts',
  params: { purposes: ['payment', 'ordinals', 'stacks'] }
});
const address = response.result.addresses[0].address;
```

### OKX Wallet

```javascript
const provider = window.okxwallet?.bitcoin;
const accounts = await provider.requestAccounts();
const balance  = await provider.getBalance();
```

---

## OP_NET MCP Protocol

OPWork mengintegrasikan [OP_NET MCP](https://ai.opnet.org/mcp) untuk:

- Query on-chain data (token balances, contract state)
- Broadcast transaksi smart contract
- Verifikasi escrow status
- Membaca event log dari OP_20 token transfers

```javascript
// Contoh query via OP_NET MCP endpoint
const res = await fetch('https://api.opnet.org/v1/query', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    method: 'getBalance',
    params: { address: 'bc1p...' }
  })
});
```

---

## Project Structure

```
opwork-web3/
├── opwork-web3.html       # Main app (single-file)
└── README.md              # Dokumentasi ini
```

Karena ini adalah single-file app, semua HTML, CSS, dan JavaScript ada dalam satu file untuk kemudahan deployment dan portabilitas.

---

## Roadmap

- [ ] Integrasi OP_NET smart contract escrow nyata (AssemblyScript)
- [ ] On-chain reputation system (review tersimpan di Bitcoin L1)
- [ ] OP_20 token payment gateway
- [ ] Notifikasi real-time via OP_NET event listener
- [ ] Profile page dengan riwayat kerja on-chain
- [ ] Multi-milestone contract support
- [ ] Mobile app (React Native + OP_WALLET SDK)
- [ ] Dispute resolution via DAO voting

---

## Contributing

Pull requests welcome. Untuk perubahan besar, silakan buka issue terlebih dahulu untuk diskusi.

---

## License

MIT © 2025 OPWork

---

> Built on Bitcoin. Powered by OP_NET. No banks. No borders. No middlemen.
