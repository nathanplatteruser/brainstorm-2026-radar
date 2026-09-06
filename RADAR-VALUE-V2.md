# SettleUp Radar — Valuation Model V2

Shipped 2026-09-06 CT on `nathanplatteruser/brainstorm-2026-radar` (GitHub Pages).

Live SKUs (product truth, not valuation ceilings): **Pilot $499 / Firm $1,299 / Letter Risk Audit $2,500**.

Founding offer closes **Oct 2, 2026 (America/Chicago)**. After that date founding pricing ends.

localStorage key: `settleup-radar-value-v2` (v1 key intentionally unused so old edits cannot corrupt V2).

## The 9 structural fixes

### 1. Bar-licensed hard rule
Anyone bar-licensed (counsel / attorney / partner at a law firm / Member of a law firm / GC) **must never** carry referral $ or commission-style `influence_ev` as personal money.

- Route only to `advisory` / `paid_review` / `audit` (plus qualitative `credibility` when needed).
- Influence for attorneys is **Multiplier** (qualitative), not $ referrals.
- Ari Derman's old referral-commission `influence_ev` (5994) is cleared.
- Forced seeds: Ari Derman, Regina Slowey, Joann Needleman, Heath Morgan, Dennis Barton, Abby Hogan, Jacqueline Wilson, plus title/org detection for counsel/attorney/lawyer/partner-at-law.

### 2. Multi-segment
`segments: [{id, ceiling, label}]` array. Personal ceiling = **sum** of segment ceilings.

Edit drawer supports adding/removing multiple segments. Ari seeded with `advisory + credibility` (no referral $). Slowey seeded with `advisory` (+ multiplier).

### 3. Multiplier flag
`is_multiplier: true` + short `multiplier_note`.

Seeded: Regina Slowey, Ari Derman, Joann Needleman (attorney-reviewed unlock). Badge shown. Totals footnote: true value is not only their own weighted number.

### 4. Silence / stage override
`stage_override: true` keeps a higher **manual** probability despite not-contacted / sent silence.

Ralph Hall: OEM path, manual ceiling, `stage_override` with 20% manual prob, Manual override badge. Silence does not auto-kill him.

### 5. Long-cycle enterprise
`cycle: "enterprise"` uses a separate probability curve: **max 8%** in the 90-day window even if stage would be higher.

Detected: Ally, Atlanticus, InDebted, U.S. Bank counsel, Equifax-type, other large enterprise org/title hits.

### 6. Strategic optionality
`optionality: true` for peer founders / vendor CEOs with $0 direct.

Filter chip **Optionality**. They stay visible when sorting by $ (sorted after positive EV, before null/unquantified).

### 7. Capacity-adjusted total
Footer shows **both**:
- Raw pipeline sum (weighted EV)
- Capacity-adjusted realizable: greedily take highest weighted until caps
  - max **2** advisory/retainer engagements
  - max **3** pilot-ish (pilot / pilot_sub / audit / paid_review)

Caps documented in the UI.

### 8. Priority Score
`priority = (effective_prob/100) * velocity` (0–1 scale).

Velocity defaults: audit/pilot/pilot_sub/paid_review = 0.9; advisory/retainer = 0.45; enterprise cycle = 0.2; peer/optionality = 0.35; oem = 0.4. Multi-segment uses **max** segment velocity unless manually set.

Sort chips: **Fit | Weighted EV | Ceiling | Priority**.

### 9. Oct 2 founding-window decay
`effective_prob = stage_or_manual_prob * decay`

`decay = max(0.35, days_left_to_Oct2 / days_from_Sep6_to_Oct2)`

On 2026-09-06 decay = 1.0. At/after Oct 2 decay floors at 0.35. Shown as **Founding window decay** in totals.

## Also shipped
- **Export CSV** / **Export JSON** (full roster + valuation fields)
- **Copy totals** clipboard summary
- Message-copy buttons retained
- **Unquantified**: null ceiling + not optionality → badge "Could not quantify"; ceiling/weighted show **—** (never fake $0)
- Sticky chips, 44px taps, search `font-size: 16px`
- Metrics: Fit, Ceiling (sum), Prob (effective), Weighted EV, Priority
- Badges: Estimated | Confirmed | Multiplier | Stage override | Enterprise | Optionality | Unquantified | Bar-licensed

## First-pass seed overrides
| Person | Notes |
|---|---|
| Ari Derman | bar, advisory+credibility, multiplier, no referral $, stage sent |
| Regina Slowey | bar, advisory, multiplier |
| Joann Needleman | bar, advisory+audit, multiplier |
| Olga Mironova | pilot_sub, high velocity |
| Ralph Hall | oem + stage_override + 20% manual |
| Michael Koczwara, Dennis Barton, Heath Morgan, Dan Medina | fast-win pilots/audits, high velocity |
| Enterprise orgs | cycle=enterprise |

Do not wait on Whova attendee expansion; V2 ships on the current roster. Parent merges attendees later.
