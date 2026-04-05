# 🔥 NFT CARD GENERATOR PRO

> Animated on-chain ready NFT card generator with flame black interface. Generate, customize, mint on Monad Mainnet, download PNG & video — all from one HTML file. No dependencies, no build tools, open directly in browser.

[![Monad](https://img.shields.io/badge/Network-Monad%20Mainnet-ff4400?style=flat-square)](https://monad.xyz)
[![HTML](https://img.shields.io/badge/Stack-Pure%20HTML%2FJS-ff6600?style=flat-square)](#)
[![License](https://img.shields.io/badge/License-MIT-ffaa00?style=flat-square)](LICENSE)

---

## 🌐 Live Demo

**[→ Open NFT Card Generator](https://00impera.github.io/NFT-CARD-GENERATOR-PRO/)**

---

## ✨ Features

- 🎴 Animated NFT card with canvas rendering at 30 FPS
- 🖼️ Drag & drop image upload (PNG, JPG, GIF, WEBP)
- 🎨 12 neon lightning colors — toggle individually or use palette presets
- ⚡ 4 palette presets: NEON / MATRIX (green/black) / MONO (white/grey/black) / FIRE
- 💪 4 customizable power stats with live sliders (Attack, Defense, Speed, Magic)
- ⭐ 6 rarity grades: SSS+ Mythic / SSS Legendary / SS Epic / S Rare / A Uncommon / B Common
- 💾 Download as PNG (full resolution 800×1200)
- 🎬 Download as animated WebM/MP4 video (5 seconds at 30 FPS)
- 🌿 Mint on Monad Mainnet — sends 100 MON to owner wallet
- 🔥 Flame black interface with rising ember particles
- ⚡ Every button vibrates on click
- 📱 Fully responsive — works on mobile

---

## 🗂 File Structure

```
NFT-CARD-GENERATOR-PRO/
├── index.html     # Complete app — single file, no dependencies
└── README.md
```

---

## ⚙️ All Functions

### 🖼️ Image & Upload

| Function | Description |
|---|---|
| `loadImage(file)` | Reads image via FileReader, sets `img` variable, auto-triggers `generateCard()` |
| Drag & drop | `dragover` / `dragleave` / `drop` events on `.upload-zone` |
| File input | `change` event on `#imageInput` — accepts `image/*` |

---

### 🎨 Card Generation & Animation

| Function | Description |
|---|---|
| `generateCard()` | Shows canvas, enables download buttons, starts animation loop |
| `animate(ts)` | `requestAnimationFrame` loop — increments `time`, calls `drawCard()`, tracks FPS |
| `drawCard()` | Full canvas render — background, orbs, image, floating icons, rarity badge, name panel, stat bars, watermark |
| `drawStatBar(stat, y, cols, t)` | Draws one animated stat bar with shimmer effect |
| `drawCornerAccents(col)` | Draws 4 glowing corner bracket decorations |
| `roundRect(ctx, x, y, w, h, r)` | Canvas helper — draws rounded rectangle path |

---

### 🎨 Colors & Palettes

| Function | Description |
|---|---|
| `setPalette(name)` | Sets active colors from preset — `'neon'` / `'matrix'` / `'mono'` / `'fire'` |
| Color button toggle | Click `.color-btn` to add/remove color from `activeColors` array |

**Palette presets:**

| Name | Colors |
|---|---|
| `neon` | Green, Cyan, Magenta, Yellow, Pink, Purple, Red, Blue |
| `matrix` | Green, Lime, Dark green, White shades |
| `mono` | White, Grey, Black shades |
| `fire` | Red, Orange, Yellow, Gold shades |

---

### 💾 Download PNG

| Function | Description |
|---|---|
| `downloadCard()` | Pauses animation → renders final frame → `canvas.toBlob()` → triggers download as `{name}.png` |

**Output:** 800×1200px PNG, named from NFT name field.

---

### 🎬 Download Video

| Function | Description |
|---|---|
| `downloadVideo()` | Records 5-second canvas stream using `MediaRecorder` API |

**Details:**
- Tries MIME types in order: `video/webm;codecs=vp9` → `video/webm;codecs=vp8` → `video/webm` → `video/mp4`
- Records at 8 Mbps, 30 FPS
- Shows red REC indicator with countdown timer
- Shows progress bar during recording
- Auto-downloads as `{name}-animated.webm` or `.mp4`
- **Requires Chrome or Firefox** (Safari has limited MediaRecorder support)

---

### 🌿 Mint On-Chain — Monad Mainnet

| Function | Description |
|---|---|
| `mintOnChain()` | Connects MetaMask → switches to Monad → sends 100 MON to owner → polls for receipt → auto-downloads PNG on success |

**Config:**
```javascript
const OWNER_ADDRESS = '0x592B35c8917eD36c39Ef73D0F5e92B0173560b2e';
const MONAD_CHAIN_ID = '0x8F'; // 143
const MINT_PRICE_WEI = '0x' + BigInt('100000000000000000000').toString(16); // 100 MON
```

**Flow:**
1. `eth_requestAccounts` — connect wallet
2. `wallet_switchEthereumChain` — switch to Monad (chain ID 143)
3. `wallet_addEthereumChain` — adds Monad if not present
4. `eth_sendTransaction` — sends 100 MON to owner address (gas: 21000)
5. Polls `eth_getTransactionReceipt` every 2 seconds (max 60 attempts = 2 min)
6. On success → auto-downloads card PNG

---

### ⚡ Vibrate On Click

```javascript
document.addEventListener('click', e => {
  const btn = e.target.closest('button, .color-btn, .upload-zone');
  if (!btn || btn.disabled) return;
  btn.classList.remove('vibrating');
  void btn.offsetWidth; // reflow to restart animation
  btn.classList.add('vibrating');
  btn.addEventListener('animationend', () => btn.classList.remove('vibrating'), { once: true });
});
```

Every button on the page vibrates when clicked — 10-step shake with translate + rotate + scale.

---

### 📊 Stats Display

| Function | Description |
|---|---|
| `updatePreviewSize()` | Updates preview info bar — size (800×1200) and LIVE/PAUSED status. Runs every 500ms via `setInterval` |

---

## 🎴 Card Layout (800×1200px)

```
┌─────────────────────────┐  ← Animated flame border + corner accents
│                         │
│   [IMAGE AREA 720×560]  │  ← User image, shimmer overlay, floating icons
│         ⭐ SS ⭐         │  ← Rarity badge with pulse glow
│                         │
│  NAME              #001 │  ← Name panel (black bg)
│       Edition: 1/100    │
│                         │
│  ⚔️ ATTACK   ████░  85  │  ← Animated stat bars with shimmer
│  🛡️ DEFENSE  ███░░  70  │
│  ⚡ SPEED    █████  90  │
│  ✨ MAGIC    ████░  75  │
│                         │
│    imperamonad.xyz      │  ← Watermark
└─────────────────────────┘
```

---

## 🔧 How to Deploy

```bash
# Clone repo
git clone https://github.com/00impera/NFT-CARD-GENERATOR-PRO
cd NFT-CARD-GENERATOR-PRO

# No build needed — open directly
open index.html

# OR deploy to GitHub Pages
# Settings → Pages → Branch: main → / (root) → Save
# Live at: https://nftgeneratornft.nelutz2you.workers.dev/
```

---

## 🌿 Mint Configuration

To change mint price or owner address, edit these lines in `index.html`:

```javascript
const OWNER_ADDRESS = '0x592B35c8917eD36c39Ef73D0F5e92B0173560b2e'; // ← your wallet
const MONAD_CHAIN_ID = '0x8F';                                        // ← 143 = Monad
const MINT_PRICE_WEI = '0x' + BigInt('100000000000000000000').toString(16); // ← 100 MON
```

To change price to 50 MON:
```javascript
const MINT_PRICE_WEI = '0x' + BigInt('50000000000000000000').toString(16);
```

---

## 📱 Browser Support

| Browser | Generate | PNG | Video | Mint |
|---|---|---|---|---|
| Chrome | ✅ | ✅ | ✅ | ✅ |
| Firefox | ✅ | ✅ | ✅ | ✅ |
| Safari | ✅ | ✅ | ⚠️ Limited | ✅ |
| Mobile Chrome | ✅ | ✅ | ✅ | ✅ |
| Mobile Safari | ✅ | ✅ | ❌ | ✅ |




## 📄 License

MIT — see [LICENSE](LICENSE)

---

*Built on [Monad Mainnet](https://monad.xyz) · Pure HTML/Canvas/JS · No dependencies*
