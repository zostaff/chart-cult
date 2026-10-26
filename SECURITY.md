<div align="center">

# Security

**Your wallet, your keys, your signature. Chartcult reads the chain, it never holds your money.**

[Back to the README](README.md) · [chartcult.fun](https://chartcult.fun)

</div>

Connecting a wallet to a trading site is a real risk, so this page says exactly what Chartcult can and cannot do. Everything below describes how the live site at [chartcult.fun](https://chartcult.fun) works today.

## At a glance

| | How Chartcult handles it |
|---|---|
| Custody | **None.** We hold no funds and cannot move yours |
| Seed phrase or private key | **Never asked for.** Anything claiming to be Chartcult that asks is a scam |
| Account | **Not needed.** Signing in is optional and is a text message, not a transaction |
| Signatures | Only for a real trade, a launch, a premium payment or signing in, always shown first |
| Fee on your trades | **0%** on top of the venue |
| Where your lists, layout and journal live | Your browser's local storage |
| What we know about your Telegram | A chat id, nothing else |
| Downloads | **None.** Chartcult is a website. There is no app, extension, installer or `.exe` |

## Connecting a wallet

- **Solana**: the Wallet Standard `connect` call. Your wallet shares your public address. Nothing else.
- **EVM** (MetaMask, Rabby, Coinbase Wallet and other EIP-6963 wallets): `eth_requestAccounts`, which shares your public address. Coming back on a later visit uses `eth_accounts` only, so the site never pops up a request by itself.
- No delegation, no session keys, no unlimited approvals. Charts, alerts, replays, paper trading and the AI helper need no wallet at all.

## Every signature, explained

Your wallet signs only in the cases below, and only after you click and the wallet shows you the request.

### Signing in (optional)

A plain text message that reads *"Sign in to Chartcult. This signature proves you own this wallet. It is not a transaction and costs nothing."* with your address, a one-time nonce, the time and the domain. The nonce expires in 5 minutes and works once. The session that follows lives in an `HttpOnly`, `Secure`, `SameSite=Lax` cookie the page's scripts cannot read. Signing in only carries your settings across devices and your premium tier.

### Trading on Solana

Swaps are built by Jupiter for the exact amount you confirmed. Before your wallet is asked, the page shows what you pay, about what you get and the minimum after slippage, and **refuses any transaction whose fee payer is not your own wallet**. Your wallet signs it, the page sends it through our relay, and you get the Solscan link. Quick buys from the feed need a second click on "confirm" before the wallet is asked.

### Trading on Robinhood Chain

Buys and sells go straight to the coin's pons bonding curve, with your address as the recipient and a minimum out. A sell first asks your wallet to approve **exactly the amount being sold** for that curve, never an unlimited allowance. Limit orders live in your browser and still need your wallet to confirm when they trigger.

### Launching a coin

- **pump.fun**: the coin's mint key is generated in your browser with WebCrypto and **never leaves it**. Our server builds the create transaction, your wallet signs first, the page adds the mint's signature, and the server checks the payer, the signatures and the create instruction before it sends the transaction. It creates your coin and, if you chose one, makes your dev buy. Nothing else.
- **Robinhood Chain**: your wallet sends one transaction to the pons factory (or pons' router for a launch with a dev buy), with the launch fee and the dev buy shown before you approve.

Chartcult adds no fee on top of the launchpad's.

### Paying for premium

One transaction that **burns** the quoted amount of the Chartcult token from your own account, with a memo naming the quote. Nothing is sent to anyone. The server confirms the burn on chain before it turns the tier on, and the tier is never taken from what the browser claims.

## What the server can and cannot see

**It keeps**
- the public chain data it decodes: launches, trades, holders, pools;
- for signed-in wallets: your address, your synced settings and your premium tier;
- for Telegram alerts: your chat id, your alerts and a SHA-256 hash of the link secret. Linking never asks for a phone number or a Telegram login.

**It never has or keeps**
- your seed phrase or private keys;
- the pump.fun mint key of a coin you launch;
- your followed wallets, KOL list, layout, paper accounts or journal, unless you sign in and sync settings;
- your AI questions: each one goes to the AI model to be answered and is not stored by Chartcult. The helper sees public on-chain data and what you type, nothing from your wallet.

**It protects its own keys.** RPC, X and AI keys live only on the server and are never sent to the page. The RPC proxy answers our own site only, refuses other origins, limits each IP, and forwards a short list of read methods. The one write it relays is `sendTransaction` for a transaction your wallet already signed in the browser.

## Rules the product follows

- **Safety stays free.** Dev, bundle and sniper shares, mint and freeze checks and every red flag are free on every tier.
- **The AI never gives trading advice.** It explains risk from the numbers on screen and never tells anyone to buy or sell or promises a return.
- **Every label has a reason.** A flag shows the slot, the sell or the wallet behind it, so you can check it yourself.

## Stay safe

- The only official site is **[chartcult.fun](https://chartcult.fun)**. The only official alerts bot is **[@chartcult_alerts_bot](https://t.me/chartcult_alerts_bot)**.
- We never ask for a seed phrase and never ask you to install anything or turn off your antivirus.
- Read every transaction in your wallet before you approve it. If the wallet shows something the page did not, reject it.

> Memecoins are high risk and most of them go to zero. Red flags in Chartcult are readings of public data, not guarantees, and nothing on the site is financial advice.

## Reporting a vulnerability

Found a security issue? Please report it privately: open a **private security advisory** on this repository (Security tab, "Report a vulnerability") rather than a public issue. Tell us what you found, how to reproduce it and what an attacker could do with it. We read every report and will tell you when it is fixed.
