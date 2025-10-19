# Evidence-Based +EV NFL Betting: DraftKings-First (Las Vegas Constraints)

## Executive Summary
- **Core edge**: Price. Consistently beat the closing line (positive CLV) via fast info, accurate priors, and aggressive line shopping.
- **Primary sportsbook**: DraftKings. Execute bets on DraftKings where legal; in Nevada, use DraftKings as your price benchmark and execute at local books when you can match or improve DK price.
- **Risk control**: Small unit sizes and fractional Kelly to survive variance and avoid ruin.
- **Target markets**: Efficient on sides/totals; more opportunity in props/derivatives and select teasers (only with fair pricing).
- **Vegas logistics**: In-person app signup, geofencing, cage cash handling; register at multiple operators to shop lines.

---

## DraftKings Availability & Primary Workflow
- As of Oct 2025, DraftKings Sportsbook is not live with a mobile app in Nevada. When you are in a DraftKings-legal state, make DraftKings your primary book for execution, pricing, CLV tracking, and promos/features.
- When you are physically in Las Vegas (NV), use DraftKings as your primary price reference and attempt to replicate or beat the DK price at local sportsbooks (retail or NV mobile apps). If you need to execute on DraftKings, you must be physically located within a DraftKings-legal state’s geofence.
- Official legal states (DraftKings): https://sportsbook.draftkings.com/help/sports-betting/where-is-sports-betting-legal

## Setup & Nevada Logistics
- **In-person registration**: Required for NV apps. Bring ID to the sportsbook counter/cage.  
  - DraftKings Sportsbook is not live in Nevada as of Oct 2025; for on-the-ground execution in Las Vegas, register at local operators: Circa Sports NV, Westgate SuperBook NV, Caesars/William Hill NV, BetMGM NV, STN Sports (Station), South Point, Boyd Sports.
- **Deposits/withdrawals**: Cash at casino cage; some apps add remote options after signup. Apps are geofenced to NV.
- **Line shopping**: Maintain accounts at several books. Small price improvements compound EV.

References:
- DraftKings legal states: https://sportsbook.draftkings.com/help/sports-betting/where-is-sports-betting-legal  
- LegalSportsReport (NV overview): https://www.legalsportsreport.com/sports-betting-states/nevada/  
- PlayNevada (apps): https://www.playnevada.com/sports-betting-apps/  
- William Hill NV FAQ: https://www.williamhill.us/faq-new-account/  
- Nevada Gaming Control Board: https://gaming.nv.gov

---

## Bankroll Management
- **Units**: 0.5–1.0% bankroll per standard wager; smaller for higher variance or uncertain edges.
- **Kelly (fractional)**: Use 25–50% Kelly to balance growth vs drawdowns.
```text
Kelly fraction (decimal odds) ≈ (bp - q) / b
where b = odds - 1, p = win probability estimate, q = 1 - p
```
- **Risk of ruin**: Even +EV strategies can bust without discipline. Avoid “chasing.”

Reference: The Logic of Sports Betting (Miller & Davidow).

---

## Price Edge & Closing Line Value (CLV)
- **CLV definition**: Your bet price vs the market’s final (closing) price. Positive CLV predicts skill and future profitability.
- **Practice**: Use DraftKings as your primary book and price benchmark where legal. In Nevada, shop Circa, Westgate, Caesars/WH, BetMGM, STN, South Point, Boyd and compare against the DK price—execute locally only when the local price matches or improves your DK benchmark.
- **Reduced juice**: Prefer -108/-105 vs -110 when possible; lowers the hold you must overcome.
- **Tracking**: Log your bet price and the close; aim to beat the close consistently.

References:
- Pinnacle (Buchdahl on CLV): https://www.pinnacle.com/betting-resources/en/educational/what-is-closing-line-value-clv-in-sports-betting  
- Unabated (measuring CLV): https://unabated.com/articles/getting-precise-about-closing-line-value

---

## Market Structure & Timing
- **Market-making vs retail**: Sharper books originate/take sharp action at higher limits; followers copy/move slower.
- **When to bet**:  
  - Early (openers): Potentially more mispricing; lower limits.  
  - Late (near close): Highest limits; most efficient.  
  - News windows: Injury reports and inactives (−90 min) can create edges.
- **Steam**: Don’t chase screen moves. If you weren’t early, you’re probably late.

---

## Bet Selection
- **Sides/totals**: Most efficient; require real modeling/info edge to profit consistently.
- **Player props/derivatives**: Often less efficient; focus on injury/role/weather sensitivity. Accept lower limits and variance.
- **Teasers (Wong criteria)**: Two-team 6-pt legs that each cross 3 and 7 can be +EV only at fair pricing (historically ~−120). Modern −130/−140 often neutralizes edge. Prefer lower-total games; check push rules.
- **Alt lines/SGPs**: Only if verified correlation value survives pricing/house rules; NV often restricts or prices out edges.

References:
- Unabated (teaser math/pricing): https://unabated.com/articles/profitable-nfl-teaser-bets  
- Unabated (NFL wager selection): https://unabated.com/articles/are-you-placing-the-right-wagers-when-betting-the-nfl  
- Stanford Wong, Sharp Sports Betting (book)  
- The Logic of Sports Betting (book)

---

## Promos/Bonuses (NV Reality)
- **NV has fewer promos**: In-person signup and regulatory environment limit bonus EV. Don’t rely on promos as core strategy.
- **If available**: Compute true EV and conversion; avoid overexposure to turnover requirements.

---

## Live Betting: Edges & Pitfalls
- **Latency**: Streaming delays (often 20–45+ sec) put you behind book feeds. Markets suspend on timeouts/injuries/reviews.
- **Limits**: Lower in-play limits; sharp detection and stake throttling are common.
- **Best practice**: Bet during natural breaks (timeouts/quarters/halftime). Don’t chase suspensions/steam.

References:
- Unabated (live betting): https://unabated.com/articles/peek-into-the-future-of-live-betting  
- Pinnacle (live middling principles): https://www.pinnacle.com/betting-resources/en/educational/mastering-live-betting-the-art-of-middling

---

## Arbitrage & Middling in Vegas
- **Feasible in theory**: Differences across shops can create arbs/middles.
- **Practical frictions**: Travel time, geofencing, quick moves, low limits, and tax record-keeping reduce viability.
- **Tax**: US gambling winnings taxable—maintain meticulous records.

IRS: https://www.irs.gov/taxtopics/tc419

---

## Modeling & Data
- **Approach**: Simple Elo/market-anchored models with injury/weather adjustments; validate out-of-sample.
- **Validation**: Track CLV; it’s a better skill signal than short-term ROI. Avoid overfitting.
- **Exploit lags**: Offensive line injuries, secondary changes, wind/weather, role shifts in props.

Studies on efficiency:
- Spinosa (Claremont): https://scholarship.claremont.edu/cgi/viewcontent.cgi?params=%2Fcontext%2Fcmc_theses%2Farticle%2F2102%2F&path_info=Charles_Spinosa_Thesis.pdf  
- Shank (SSRN): https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3022567, https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3385529  
- Kuper (Berkeley): https://econ.berkeley.edu/sites/default/files/Kuper.pdf

---

## Step-By-Step Vegas Playbook
1. DraftKings-first: Create and fund a DraftKings Sportsbook account in a DK-legal state (see DK legal states page). When in Las Vegas, also create/fund local NV apps (Circa, Westgate, Caesars/WH, BetMGM, STN, South Point, Boyd) for on-the-ground execution.  
2. Define bankroll and unit size; choose fractional Kelly for strongest edges.  
3. Build an odds screen with DraftKings as your primary price. When in NV, replicate or improve the DK price at local books before executing.  
4. Early week: Only bet openers with clear model/info edge; prefer DK execution when geolocated in a DK state; otherwise execute locally in NV at equal/better price.  
5. Mid-week: React to injury reports; prioritize props/derivatives where books lag. Use DK where legal; in NV, mirror DK value locally.  
6. Gameday −90 min: Move fast on inactives; hit stale props/totals/sides before market catches up (prefer DK if in a DK state, otherwise NV local books).  
7. Teasers: Only classic Wong legs crossing 3 & 7 and only at fair pricing (near −120 and favorable push rules). Shop DK and NV locals; skip if modern pricing removes edge.  
8. Live: Limit to breaks; accept small limits; avoid chasing suspended/steaming markets. Prefer DK live when within a DK-legal state; otherwise use NV locals.  
9. Track CLV for every bet; aim to beat the close consistently (use DK close as your primary benchmark when available).  
10. Record deposits/withdrawals, stakes, prices, CLV deltas, results for audit/tax and process improvement.  
11. Weekly review: Reallocate toward markets where your CLV is positive; trim what you can’t beat.

---

## Handy Math
```text
Break-even win rate (American odds):
-110 → 52.38%,  -120 → 54.55%,  -130 → 56.52%

Bet EV (per $1 staked, decimal odds d):
EV = p*(d - 1) - (1 - p)
```

---

## References & Further Reading
- **CLV & Skill**  
  - Pinnacle (Buchdahl): https://www.pinnacle.com/betting-resources/en/educational/what-is-closing-line-value-clv-in-sports-betting  
  - Unabated (CLV): https://unabated.com/articles/getting-precise-about-closing-line-value
- **Teasers**  
  - Unabated (profitable NFL teasers): https://unabated.com/articles/profitable-nfl-teaser-bets  
  - Unabated (NFL wager selection): https://unabated.com/articles/are-you-placing-the-right-wagers-when-betting-the-nfl  
  - Stanford Wong, Sharp Sports Betting (book)  
  - The Logic of Sports Betting (book)
- **Live/In-Play**  
  - Unabated (live betting): https://unabated.com/articles/peek-into-the-future-of-live-betting  
  - Pinnacle (live middling): https://www.pinnacle.com/betting-resources/en/educational/mastering-live-betting-the-art-of-middling
- **Nevada**  
  - DraftKings legal states: https://sportsbook.draftkings.com/help/sports-betting/where-is-sports-betting-legal  
  - LegalSportsReport NV: https://www.legalsportsreport.com/sports-betting-states/nevada/  
  - PlayNevada apps: https://www.playnevada.com/sports-betting-apps/  
  - William Hill NV FAQ: https://www.williamhill.us/faq-new-account/  
  - NGCB: https://gaming.nv.gov
- **Market Efficiency (Academic)**  
  - Spinosa (Claremont): https://scholarship.claremont.edu/cgi/viewcontent.cgi?params=%2Fcontext%2Fcmc_theses%2Farticle%2F2102%2F&path_info=Charles_Spinosa_Thesis.pdf  
  - Shank (SSRN): https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3022567, https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3385529  
  - Kuper (Berkeley): https://econ.berkeley.edu/sites/default/files/Kuper.pdf

---

# Status
- Completed: Markdown guide saved to project root as `README.md`.
