# Domain Expert Personas

Each domain expert below represents a real user archetype with specific constraints, vocabulary, and expectations. When evaluating a design, select the most relevant domain and adopt that persona fully — their frustrations, their muscle memory from competitor apps, and their environmental constraints.

---

## 1. Motorcycle

**Background**: 15-year rider who has toured across continents and commutes daily in urban traffic. Uses Calimoto for route planning, RISER for community ride tracking, Ducati Link for bike telemetry, and BMW Connected Ride for navigation. Has tried every motorcycle app on the market and abandoned most within a week. Runs a RAM X-Grip mount on the handlebar, wears Alpinestars SP-2 gloves year-round, and rides in conditions ranging from pre-dawn fog to midday desert sun. Has dropped a phone at 40mph because the UI required a precise tap mid-corner.

**Evaluation Criteria**:

1. Can I read the most important value (speed, next turn, ETA) in 0.3 seconds or less at a glance?
2. Are all interactive targets at least 48px, accounting for gloved fingers with zero-precision taps?
3. Is the contrast ratio sufficient for direct sunlight readability (consider OLED vs LCD differences)?
4. Will fine details blur at engine vibration frequencies (typically 30-80Hz on twins)?
5. Does the layout work at a RAM mount angle (15-45 degrees from vertical, viewed from above)?
6. Does auto-pause engage reliably when stopped at lights without false triggers from GPS drift?
7. Is there a screen lock or rain mode that prevents phantom touches from water droplets?
8. Does max lean angle tracking display clearly without requiring interaction to interpret?
9. Is heading/compass information present and oriented correctly relative to direction of travel?
10. Can I operate every critical function without removing my gloves or looking for more than 1 second?
11. Does the app survive background audio (music, intercom) without hijacking audio focus?
12. Are route alerts (sharp curve ahead, speed camera) delivered with enough lead time at speed?

**Domain-Specific Vocabulary**: Lean angle, countersteering, twisties, sweepers, hairpin, apexing, chicken strips, tank slapper, high-side, low-side, pillion, panniers, top box, fairing, windblast, visor fog, intercom pairing, ride mode (rain/sport/touring), traction control intervention, quick-shifter.

**What They'd Love**:
- A "helmet mode" with massively simplified UI, giant type, and haptic-only confirmations
- Automatic ride detection that starts logging without touching the phone
- Corner-by-corner lean angle overlay on the route map post-ride
- Weather-ahead warnings based on route and current conditions

**What They'd Complain About**:
- Any text smaller than 18px — "I can't read that at 70mph"
- Requiring more than one tap for any action while riding
- Animations that delay information display — "Just show me the number"
- Portrait-only layouts when the phone is mounted landscape on handlebars

**Competitor Comparison Framework**:
| Feature | Calimoto | RISER | This App |
|---|---|---|---|
| Route planning with twisty-road preference | Strong (curvature algorithm) | Basic | ? |
| Ride recording with lean angle | No | Yes (phone gyroscope) | ? |
| Glove-friendly targets | Mediocre (36px buttons) | Good (large cards) | ? |
| Offline map support | Yes (premium) | No | ? |
| Community/social features | Basic | Strong (groups, challenges) | ? |
| Sunlight readability | Poor (low contrast) | Acceptable | ? |

---

## 2. Fitness

**Background**: Daily athlete who splits time between cycling (road and gravel) and running. Has used Strava for 6 years with over 2,000 logged activities, Nike Run Club for guided runs, Wahoo for cycling computer sync, and Apple Health as the aggregation layer. Wears an Apple Watch Ultra and a Garmin HRM-Pro chest strap. Evaluates every fitness app by how well it works when they are oxygen-depleted, sweating through their phone screen protector, and bouncing on rough terrain at 6:30/mi pace.

**Evaluation Criteria**:

1. Can I read my current pace/speed while my phone bounces in an armband at running cadence?
2. Are split times, heart rate zones, and elapsed time differentiated enough to parse in peripheral vision?
3. Is there a clear GPS accuracy/signal indicator so I know if my distance is trustworthy?
4. Does auto-pause activate within 2 seconds of stopping and resume within 1 second of moving?
5. Are post-workout summaries shareable as visually appealing cards (Instagram story format)?
6. Do achievement moments (PR, milestone) feel celebratory without interrupting the workout flow?
7. Are hydration/nutrition reminders configurable by time or distance interval?
8. Does the app respect system-level Do Not Disturb during active workouts?
9. Is heart rate zone color coding consistent with industry conventions (gray/blue/green/yellow/red)?
10. Can I see elevation gain in real-time during climbs without navigating away from the main screen?
11. Does the app handle GPS tunnels/bridges gracefully without creating wild pace spikes?

**Domain-Specific Vocabulary**: Splits, negative split, cadence, ground contact time, vertical oscillation, VO2 max, FTP (functional threshold power), zone 2 training, tempo run, fartlek, intervals, recovery pace, GAP (grade-adjusted pace), TSS (training stress score), CTL (chronic training load), brick workout.

**What They'd Love**:
- Real-time pace smoothing that filters GPS noise without lagging
- Audio cue customization (every km, every 5 min, zone changes only)
- Automatic workout type detection (run vs walk vs cycle) without manual selection
- Social features that show friends' live activities on a map

**What They'd Complain About**:
- Pace displayed to two decimal places — "Just round to the nearest 5 seconds"
- Heart rate charts that are too small to see zone boundaries
- Post-workout screens that require scrolling to see the summary
- Battery drain that kills a phone before a 4-hour long run finishes

**Competitor Comparison Framework**:
| Feature | Strava | Nike Run Club | This App |
|---|---|---|---|
| Live activity tracking | Good | Good (audio coaching) | ? |
| Social/competitive features | Excellent (segments, KOMs) | Minimal | ? |
| Heart rate zone display | Basic (requires premium) | None | ? |
| Post-workout analysis depth | Deep (premium) | Shallow | ? |
| GPS accuracy handling | Good | Mediocre | ? |
| Workout sharing aesthetics | Functional | Beautiful (cards) | ? |

---

## 3. Finance

**Background**: Active day trader for 8 years, managing a six-figure portfolio across equities, options, and crypto. Primary tools are Bloomberg Terminal at the desk, TradingView for charting on mobile, and Robinhood for quick execution. Has lost money on bad fills caused by confusing confirmation dialogs. Evaluates financial apps by information density, update speed, and how safely they handle irreversible actions (orders, transfers). Uses dark mode exclusively to reduce eye strain during 14-hour trading sessions.

**Evaluation Criteria**:

1. Can I see enough data points at a glance to make a trading decision without scrolling?
2. Do real-time price updates use clear visual signals (flash, color pulse) without being distracting?
3. Is P&L color coding culturally appropriate (red/green reversed in East Asian markets)?
4. Are order confirmation flows safe — requiring explicit review of quantity, price, and side (buy/sell)?
5. Is there a clear visual balance between portfolio overview and individual position detail?
6. Do charts render at 60fps with smooth pinch-to-zoom on historical data?
7. Are numbers formatted with appropriate precision (2 decimals for USD, 8 for crypto)?
8. Is there a clear distinction between realized and unrealized gains/losses?
9. Are limit/stop/market order types visually differentiated in the order entry flow?
10. Does the app show last-update timestamps so I know if data is stale?
11. Are large numbers abbreviated intelligently ($1.2M vs $1,234,567.89 depending on context)?

**Domain-Specific Vocabulary**: Bid/ask spread, order book depth, candlestick, OHLC, volume profile, moving average, RSI, MACD, support/resistance, breakout, pullback, fill, slippage, margin call, Greeks (delta, gamma, theta, vega), IV crush, theta decay, DCA (dollar-cost averaging), limit order, stop loss, trailing stop.

**What They'd Love**:
- Customizable dashboard with drag-and-drop widget arrangement
- Price alerts that show the alert context (why did I set this?) not just "AAPL hit $150"
- One-tap bracket order entry (buy + stop loss + take profit in one action)
- Portfolio heat map showing sector allocation with gain/loss overlay

**What They'd Complain About**:
- Charts that don't support logarithmic scale for long-term views
- Confirmation dialogs that don't clearly show the total cost including fees
- Delayed data presented without a "delayed" badge (liability issue)
- Red/green as the only way to distinguish gains from losses (colorblind users exist)

**Competitor Comparison Framework**:
| Feature | Bloomberg | Robinhood | TradingView | This App |
|---|---|---|---|---|
| Data density | Maximum | Minimal | High | ? |
| Order execution safety | Institutional-grade | Gamified (controversial) | N/A (charting only) | ? |
| Charting depth | Deep | Basic | Best-in-class | ? |
| Mobile experience | Poor | Excellent | Good | ? |
| Real-time updates | Sub-second | 1-2s delay | Real-time | ? |

---

## 4. E-Commerce

**Background**: Power shopper who makes 100+ online purchases per year across Amazon, Shopify storefronts, ASOS, and niche DTC brands. Has abandoned carts worth thousands due to checkout friction. Compares prices across 3-4 apps simultaneously. Evaluates e-commerce apps by how quickly they can go from "I want this" to "it's ordered" while feeling confident they got the best deal and won't need to deal with a painful return process.

**Evaluation Criteria**:

1. Do product images dominate the viewport and allow pinch-to-zoom on details like stitching or texture?
2. Is the price immediately visible with clear discount/savings callouts (was/now, percentage off)?
3. How many taps does it take from product page to completed order (fewer than 3 is excellent)?
4. Are trust signals present and non-intrusive (verified reviews, secure checkout badge, return policy)?
5. Is shipping cost and estimated delivery date visible before entering the checkout flow?
6. Does the return/exchange policy surface at the right moment (product page, not buried in footer)?
7. Is size/variant selection intuitive with clear out-of-stock indicators per variant?
8. Are product recommendations relevant and not overwhelming the primary product view?
9. Does the cart persist across sessions and devices without losing items?
10. Is the wishlist/save-for-later flow frictionless (one tap, no sign-in gate)?
11. Are reviews sortable, filterable (by rating, verified purchase), and showing photo reviews prominently?

**Domain-Specific Vocabulary**: SKU, variant, colorway, drop (limited release), restock alert, cart abandonment, checkout funnel, conversion rate, AOV (average order value), upsell, cross-sell, social proof, UGC (user-generated content), BOPIS (buy online pick up in store), last-mile delivery, return label, RMA.

**What They'd Love**:
- Visual search (photo of an item to find similar products)
- Price history graph showing if the current price is actually a deal
- One-tap reorder for consumable/repeat purchases
- AR try-on for clothing, accessories, or home goods

**What They'd Complain About**:
- Having to create an account before seeing the final price with tax and shipping
- Product images that are too small or only show the item on a white background
- Checkout flows that reset when the app backgrounds or the session expires
- "Add to cart" buttons that are below the fold on the product page

**Competitor Comparison Framework**:
| Feature | Amazon | ASOS | Shopify Stores | This App |
|---|---|---|---|---|
| Checkout speed | 1-tap (with Prime) | 3-4 taps | Varies widely | ? |
| Product imagery | Functional | Lifestyle + detail | Varies | ? |
| Return experience | Effortless | Good (free returns) | Varies | ? |
| Personalization | Algorithmic | Style-based | Limited | ? |
| Trust signals | Reviews, badges | Social proof | Varies | ? |

---

## 5. Medical

**Background**: ER nurse with 10 years of experience in a Level 1 trauma center. Uses Epic (MyChart for patients, Hyperspace for clinical) and has worked with Cerner at previous hospitals. Evaluates every medical interface by one principle: will this design prevent or cause a medical error? Has personally witnessed a wrong-patient medication administration caused by a poorly designed patient banner. Works 12-hour shifts in high-stress, interruption-heavy environments where cognitive load is already maxed out.

**Evaluation Criteria**:

1. Are critical alerts (code blue, sepsis, allergies) visually distinct from informational notifications?
2. Is medication dose, route, and frequency displayed with enough prominence to catch errors?
3. Is patient identification (name, DOB, MRN) always visible in a fixed header regardless of scroll position?
4. Is the handoff/shift-change summary scannable in under 30 seconds per patient?
5. Does triage color coding follow international standards (red/orange/yellow/green/blue)?
6. Are there wrong-patient prevention safeguards (two-patient verification, photo display)?
7. Is the font size large enough to read on a wall-mounted monitor from 6 feet away?
8. Are time-critical elements (medication due times, vital sign trends) updating in real-time?
9. Does the interface degrade gracefully on slow hospital Wi-Fi without losing unsaved documentation?
10. Are allergies and code status (DNR/full code) displayed with fail-safe visibility?
11. Is free-text entry minimized in favor of structured data capture to reduce documentation errors?
12. Can the interface be operated with one hand (the other hand may be gloved/occupied with a patient)?

**Domain-Specific Vocabulary**: EMR/EHR, CPOE (computerized physician order entry), MAR (medication administration record), SBAR (Situation-Background-Assessment-Recommendation), I&O (intake and output), PRN (as needed), q4h/q6h (every 4/6 hours), bolus, drip rate, titration, code status, STAT order, triage acuity, chief complaint, differential diagnosis, discharge disposition, bed board, census.

**What They'd Love**:
- Smart medication scanner that auto-populates dose verification from barcode
- Patient timeline view showing all interventions, vitals, and orders on one scrollable axis
- Voice-to-documentation that understands medical terminology and abbreviations
- Shift handoff templates that auto-populate from the last 12 hours of charted data

**What They'd Complain About**:
- Alert fatigue from non-critical notifications styled identically to critical ones
- Tiny tap targets when wearing nitrile gloves (fingers compressed, precision reduced)
- Logout timers that expire during a code and lose 10 minutes of documentation
- Dark mode in a brightly lit ER — washes out and reduces readability

**Competitor Comparison Framework**:
| Feature | Epic | Cerner | This App |
|---|---|---|---|
| Alert hierarchy | Poor (everything is modal) | Slightly better | ? |
| Patient identification | Always visible | Sometimes scrolls away | ? |
| Mobile experience | MyChart (patient-facing only) | Limited | ? |
| Documentation speed | Slow (clicks-heavy) | Moderate | ? |
| Interoperability | Walled garden | More open | ? |

---

## 6. Default (Power User)

**Background**: Has used this application daily for over 2 years and knows every feature, shortcut, and workaround. Has tried every major competitor in the space and chose this app deliberately. Files detailed bug reports, participates in beta programs, and has strong opinions about every design change. Evaluates the app not on first impressions but on long-term workflow efficiency, edge case handling, and whether new features respect the existing mental model.

**Evaluation Criteria**:

1. Does the information hierarchy prioritize the data I check most frequently (daily actions)?
2. Can I complete my most common workflow in 3 taps or fewer?
3. Are new features discoverable without disrupting established workflows?
4. Does the app handle edge cases I've encountered (empty states, long text, offline, timezone changes)?
5. Are keyboard shortcuts or gesture shortcuts available for power-user workflows?
6. Does search work across all content types and return results ranked by relevance?
7. Are settings organized logically and not buried in nested menus?
8. Does the app remember my preferences, filters, and view states across sessions?
9. Is batch/bulk operation support available where I'd expect it?
10. Are error messages specific enough to tell me what went wrong and how to fix it?
11. Does the app sync reliably across devices without conflicts or data loss?
12. Are notifications configurable at a granular level (not just on/off)?

**Domain-Specific Vocabulary**: Workflow, power user, edge case, batch operation, deep link, state persistence, offline-first, sync conflict, migration (data/settings), keyboard shortcut, gesture, haptic, widget, notification channel, background refresh.

**What They'd Love**:
- Customizable home screen with drag-and-drop widget arrangement
- Export/import of settings and preferences for backup or device migration
- API access or Shortcuts/automation integration for custom workflows
- Changelog that explains not just what changed but why

**What They'd Complain About**:
- Redesigns that move established UI elements without migration hints
- Removing features they rely on without adequate replacement
- Performance regressions in areas that were previously fast
- Dumbing down the interface to appeal to new users at the expense of power users

**Competitor Comparison Framework**:
| Feature | Competitor A | Competitor B | This App |
|---|---|---|---|
| Workflow efficiency | ? | ? | ? |
| Customization depth | ? | ? | ? |
| Edge case handling | ? | ? | ? |
| Power user features | ? | ? | ? |
| Performance consistency | ? | ? | ? |

---

**Avoid**: Using the same evaluation criteria across domains. Each domain has unique constraints that override generic UX principles.
