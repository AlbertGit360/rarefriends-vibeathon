# Friendpad

Every token is launched, drawn and held by a Rare Friend.

![Friendpad launch preview](https://raw.githubusercontent.com/AlbertGit360/friendpad/d18ac2f4dc81ffc23a8e645eb2797c1695a6b2b5/docs/01-launch.png)

**Builder:** AlbertGit360 · Telegram @albertos360 · X https://x.com/AlbertErgart · **Category:** Token Activity · **Stack:** no FriendSDK runtime — vanilla TypeScript/ES modules + viem 2.21.54, static site (holder world reuses FriendSDK v0.1.2 world renderer + movement modules, vendored)

Friendpad is a memepad where an activated Rare Friend's own ERC-6551 wallet launches each meme token for a 100 RF fee (50 RF burned, 50 RF seeded into RF dividends), the Friend's generation decides how sharp the meme is drawn, and only activated Friend wallets holding the token earn $RAREFRIENDS dividends. [Source code](https://github.com/AlbertGit360/friendpad/tree/d18ac2f4dc81ffc23a8e645eb2797c1695a6b2b5) · [Full README](https://github.com/AlbertGit360/friendpad/blob/d18ac2f4dc81ffc23a8e645eb2797c1695a6b2b5/README.md)

## Demo

**Working demo:** https://albertgit360.github.io/friendpad/

Works without a wallet in view-only mode (browse tokens, charts, holder worlds, economy, look up any Friend). To trade and launch, connect any injected EIP-1193 wallet on Robinhood Chain mainnet (4663). Friendpad only reads your address: **no transactions, no signatures, no RF needed**. Launching requires an **activated** Friend (hardwired Generations NFT gen ≥ 1, or Genesis); `?dev=1` lets you launch from any looked-up Friend for testing.

## Run it

No build step and no `npm install`:

```sh
git clone https://github.com/AlbertGit360/friendpad.git
cd friendpad
git checkout d18ac2f4dc81ffc23a8e645eb2797c1695a6b2b5
python -m http.server 8000   # or: npx serve -l 8000; on Windows double-click start.bat
```

Open `http://localhost:8000`. viem loads from esm.sh, so internet access is required. Source is TypeScript in `src/`; compiled ES modules in `js/` are committed (`npx -p typescript@5 tsc -p .` to rebuild).

## How to use it

1. Browse the seeded tokens (marked `seeded demo`) — chart, trades, tax and the holder world.
2. **Connect wallet** — your Friends are read from chain (generation, tier, reward weight, activation, Friend wallet). New users get 1 simulated WETH.
3. **Launch** — pick an activated Friend, upload an image. The preview opens on your Friend's generation (Gen-6 26×26 … Gen-1 96×96, Genesis 128×128 in colour); set tax, allocation and an optional creator buy/lock; launch for 100 RF (simulated).
4. **Buy** into your wallet or straight into a Friend wallet — your holding appears in the holder world; activated Friends walk as their on-chain sprite, other wallets float.
5. **Your positions** — Move to Friend, Sell from Friend, Claim: each shows the real `TokenBoundAccount.execute(...)` calldata that would be sent in production, then applies the change to the simulation.
6. **Economy** — RF burned, RF paid to Friends, creator fees, per-token tax, Friends leaderboard.

## Rules, costs and rewards

**Everything economic is simulated and labelled `SIMULATED`.** Friends, generations, tiers, reward weights, activation and Friend wallets are real (read live).

| | Rule |
|---|---|
| Launch fee | 100 RF: 50 RF burned (Proof of Burn), 50 RF to the token's Friend-dividend pool (mirrors ActivationManager's 50/50) |
| Trade tax | Creator-set at launch, fixed: 0–5% buy / 0–5% sell; default 1% / 1% split 40% Friend dividends (RF) / 30% creator Friend (WETH) / 30% RF buyback & burn; dividends + burn ≥ 25% |
| Platform fee | 0.1% per trade on top |
| Dividends | Only tokens inside an **activated** Friend's wallet (reward weight > 0) above the token's minimum balance; weight = balance × 1–2× boost from on-chain reward weight |
| RF price | 400,000 RF per WETH (simulated) |
| Creator buy | Optional, ≤ 20% of supply, first trade, untaxed, WETH only, into the creator Friend's wallet, optional 1/7/30-day lock |

No random outcomes, odds or consumables.

## Checks, credits and limitations

Checks: `tsc` strict compile clean. Pixelizer checked by eye on four real meme photos — sharpness increases monotonically Gen-6 → Gen-1 → Genesis ([comparison](https://github.com/AlbertGit360/friendpad/blob/d18ac2f4dc81ffc23a8e645eb2797c1695a6b2b5/docs/pixelizer-comparison.png)). Headless Chrome checks of the GitHub Pages demo in a fresh profile without a wallet: 5 seeded tokens render, reload on `#/token/…` keeps the route, `tokenBoundAccount` verification and Friend lookup read from chain, no JS exceptions; viem (esm.sh) and Blockscout API (CORS) confirmed from the Pages origin. Buy flow checked with a read-only mock wallet (address only); a real 210-Friend wallet (address only) lists all 10 activated Friends with sprites and deployed wallets. No unit-test suite; the Move to Friend → Claim flow with a real wallet holding an activated Friend is not covered by these checks.

Known limitations: the simulation lives in browser `localStorage`; seeded tokens use real Friends as demo creators/holders; temporary Friends have no deployed wallet, so Move/Sell/Claim are disabled for them; a seller can empty a Friend wallet right before selling the NFT (buyers can check `state()`); a staked NFT makes the staking contract the wallet owner; uploaded images are not moderated. No real contracts, trading or payouts. Token Activity metrics are not claimed.

Credits: viem (MIT); FriendSDK v0.1.2 world presets, renderer and movement (Apache-2.0) with Rare Friends Isometric World Assets; canonical Rare Friends sprites read on-chain; JetBrains Mono and Silkscreen (OFL). [Full credits](https://github.com/AlbertGit360/friendpad/blob/d18ac2f4dc81ffc23a8e645eb2797c1695a6b2b5/README.md#credits).
