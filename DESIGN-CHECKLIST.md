# mstonks, design inventory

Every screen, modal, card and component the PRD (v1.3, Web MVP + X Beta) implies.
Derived from §5 (journeys), §6 (IA), §7 (X agent), §16 (Beta), §18 (share engine)
and §19 (screen references).

Legend, `[x]` designed in this repo , `[ ]` not yet designed , `(B)` Beta , `(V3)` deferred

---

## 1. Marketing surface

- [x] **Landing page** `landing/v1.html` (theme: Ignition, 5 chapters, self-contained)
  - [x] Nav, single line, mark sized to the brand lockup proportion
  - [x] Hero, ember field canvas plus the live scanner panel
  - [x] Scout, ignition cascade canvas (dormant field, spark, spread front, cooling)
  - [x] Launch, draggable entry on the narrative burn curve with live economics
  - [x] Predict, daily up-or-down market with selectable side, size and payout
  - [x] Close, ember field and single CTA
  - [x] Footer with non-endorsement and geofence disclosure
  - [x] Chapter rail with scroll spy
- [x] **Landing page v2** `landing/v2.html` (theme: Misfits, 7 chapters, hero shader, card fan)
  - [x] Design source on canvas, `landing/v2-misfits.dc.html`
- [x] **Landing page v3** `landing/v3.html` (v2 plus launch countdown, tightened hero, card hover lift)
- [x] **Docs page** `docs/v1.html` (engine, metrics, launching, reservations, rewards,
      Beta surfaces, status table, risk and FAQ)
- [ ] Legal, Terms, Privacy, Risk disclosures, Non-endorsement policy
- [ ] 404 / error page
- [ ] Geo-restricted state (issuer/venue ineligible jurisdiction)

---

## 2. Product pages, MVP (PRD §6)

| # | Route | Screen | Status |
|---|---|---|---|
| 1 | `/` | **Narrative Home**, top Emerging/Accelerating/Breakout, filters, compact active-market strip | [ ] |
| 2 | `/narratives` | **Narrative Board**, sortable table + card grid, momentum, sparkline, attached markets | [x] `terminal/v1.html` |
| 3 | `/narratives/{id}` | **Narrative Detail**, why-now, trend chart, evidence timeline, X sample, attached memestocks | [ ] |
| 4 | `/launch?narrative=` | **Launch Wizard**, narrative locked, chain, stock quote, metadata, fee tier, routing preview | [x] as the pre-launch reservation flow in `terminal/v1.html`, agent-driven |
| 5 | `/t/[chain]/[token]` | **Token Terminal**, chart, tape, buy/sell, parent narrative strip, rewards, trust facts | [ ] |
| 6 | `/rewards` | **Holder Rewards**, epochs, TWAB weight, claimable, history, contributing markets | [ ] |
| 7 | `/me` | **Wallet / Settings**, connected wallet, balances, preferred chain, slippage, preferences | [ ] |
| 8 | `/me/launcher` | **Launcher Dashboard**, launches, narrative health, fee earnings, reward pools, share clicks | [ ] |
| 9 | `/api` | **Developer**, API keys, endpoints, quickstart, rate limits | [ ] |
| 10 | `/agents` | **Agent docs**, machine-readable instructions + `llms.txt` | [ ] |

*(placeholder route `/launchpad` in this repo stands in for the terminal entry point)*

---

## 3. Modals, sheets and overlays

### Wallet & session
- [ ] Connect wallet (RainbowKit connector list)
- [ ] Wrong-network → switch chain prompt
- [ ] Wallet account sheet (address, balances, disconnect)
- [ ] Trading preferences (default slippage, preferred chain, quick-trade amounts)

### Launch flow
- [ ] Stock quote asset picker (canonical registry, by chain, searchable)
- [ ] Token image upload → crop → safety check
- [ ] Duplicate symbol / name warning (informational, non-blocking)
- [ ] Metadata validation errors
- [ ] Launch preview & confirm, narrative, pair, chain, fee tier, immutable 30/50/20
- [ ] Launch signing → pending → confirmed (with `Launched` event)
- [ ] Launch failed / reverted

### Trade flow
- [ ] Buy / Sell confirm, quote, fee tier, expected output, slippage, price impact
- [ ] High price-impact warning (hard confirm)
- [ ] Slippage settings popover
- [ ] Permit2 / approval step
- [ ] Trade pending → filled (with PnL context) → failed
- [ ] Post-trade share prompt

### Rewards
- [ ] Epoch detail sheet (manifest hash, Merkle root, funded vs claimed)
- [ ] Claim confirm → sign → success
- [ ] "Accruing, distribution not yet finalized" fallback state
- [ ] Pending meme→stock conversion notice (guard deferred)

### Evidence & narrative
- [ ] Evidence drawer, full news source list with timestamps
- [ ] Sampled X post viewer (likes/reposts/replies/impressions, author, text)
- [ ] Narrative methodology / "how momentum is scored" explainer
- [ ] Risk-flag detail (unsupported claim, impersonation, false endorsement)

### Share
- [ ] Share card composer, pick event type, preview, copy link / download / post
- [ ] Deep-link + OG preview

### Compliance & developer
- [ ] Non-endorsement + risk acknowledgement (first launch / first trade)
- [ ] Geofence / eligibility block
- [ ] API key create → reveal once → revoke
- [ ] Rate-limit exceeded

### Beta
- [ ] (B) Prediction-market unlock announcement
- [ ] (B) UP/DOWN position confirm (Limitless deep-link or embedded)
- [ ] (B) Settlement result
- [ ] (B) Watchlist / alert setup
- [ ] (B) X account link + bot opt-out
- [ ] (B) Delegated-execution consent & policy screen (max notional, daily cap, cooldown)

---

## 4. Cards & repeated components

### Narrative
- [x] Lifecycle badge, Emerging / Accelerating / Breakout / Peaking / Fading
- [x] Momentum sparkline
- [x] Narrative row (board table density)
- [ ] Narrative card (grid density)
- [ ] Narrative hero header (detail page)
- [ ] Trend chart, bucketed X counts, ignition marker, peak marker
- [ ] Metric tiles, current rate, velocity 1h/3h, acceleration, baseline multiple, narrative share, current-vs-peak
- [ ] News evidence timeline item
- [ ] Sampled X post card
- [ ] Attached-market table row (rank by volume/liquidity/traders)
- [ ] Narrative-market share bar (which expression is winning)
- [ ] "Launch this narrative" CTA block
- [ ] Risk-flag chip

### Market
- [x] Chain chip (Base / Robinhood)
- [ ] Stock pair chip (canonical token address, issuer, verified)
- [ ] Market card (compact strip on home)
- [ ] Price chart (lightweight-charts)
- [ ] Trade tape row
- [ ] Trade panel (buy/sell, amount, quote, impact)
- [ ] Holder / trust facts panel
- [ ] Launch proof panel (locked LP, factory tx, beneficiaries)
- [ ] Parent-narrative context strip (persists after purchase)

### Economics & rewards
- [x] Fee tier selector card (1% LOW / 3% DEGEN)
- [x] Fee routing split bar (30/50/20)
- [ ] Reward pool card (asset, accrued, claimable)
- [ ] Epoch claim card
- [ ] Reward history row
- [ ] TWAB weight explainer
- [ ] Per-token reward contribution breakdown

### Launcher
- [ ] Launch row with narrative-health warning (peaking / fading)
- [ ] Fee earnings summary
- [ ] Share-click attribution panel

### System
- [ ] Empty states, no narratives, no markets, no rewards, no launches
- [ ] Loading skeletons (board, detail, terminal)
- [ ] Error / retry states
- [ ] Toast / transaction notification stack
- [ ] Live-update indicator (SSE connected / reconnecting)

---

## 5. Share cards, PRD §18 (each renders deterministically, server-side)

- [x] Narrative breakout
- [x] Launch
- [x] PnL
- [x] Rewards
- [ ] Trade (entry MC + narrative momentum)
- [ ] Reward-pool milestone
- [ ] Narrative + market (attention vs valuation)
- [ ] (B) Prediction-market unlock (odds + settlement)

---

## 6. X Distribution Agent, Beta (PRD §7)

Reply templates, one per intent, deterministic, one reply per interaction:

- [ ] (B) `narratives <stock>`, top emerging/accelerating with state + metrics + links
- [ ] (B) `<token>` info, price / MC / vol / rewards + linked narrative state
- [ ] (B) `launch`, resolve narrative, confirm fields, deep-link to web confirmation
- [ ] (B) `buy` / `sell`, quote + deep-link to wallet-connected web confirmation
- [ ] (B) `quote`, read-only
- [ ] (B) `rewards <token>`, claimable / estimated
- [ ] (B) `claim`, deep-link to web claim
- [ ] (B) Error / unsupported / ambiguous-ticker reply
- [ ] (B) Opt-out confirmation

---

## 7. Beta screen extensions (PRD §16, §19)

- [ ] (B) Prediction eligibility badge, eligible / locked / live
- [ ] (B) Daily UP/DOWN market card (reference price, settlement time, YES/NO odds)
- [ ] (B) Prediction module on Narrative Detail
- [ ] (B) Prediction module on Token Terminal
- [ ] (B) Venue adapter state (Limitless on Base , Robinhood partner TBD)
- [ ] (B) Watchlists & alerts
- [ ] (B) Trader profile / public calls / theses
- [ ] (B) Leaderboards & "who found it early" reputation
- [ ] (B) Attention-vs-capital gap view
- [ ] (B) Channel attribution dashboard

## 8. V3 / deferred, intentionally not designed

- (V3) Perpetuals, isolated margin, funding, liquidations
- (V3) Meme-vs-stock relative-value markets
- (V3) Leverage eligibility / perp-unlock progress

---

## Next deliverable

**Launchpad terminal**, items 1-5 in §2 above, plus the wallet, launch and trade
modals in §3. The landing page CTA already points at `/launchpad`.
