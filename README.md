# Ethos Annuities — Monorepo

Static HTML prototypes for the Ethos annuities product.

---

## Structure

```
Annuities/
├── lp-preview/           ← Landing page with interactive phone shell (self-contained)
│   └── index.html
└── funnel/               ← Mobile onboarding funnel (375×812 viewport)
    ├── components.css
    ├── components.js
    ├── fonts/
    ├── v1/               ← V1: keyboard slides up with footer
    │   ├── goals-step/
    │   ├── state-step/
    │   ├── zip-step/
    │   ├── birthdate-step/
    │   ├── name-step/
    │   ├── email-step/
    │   ├── phone-step/
    │   └── otp-step/
    └── v2/               ← V2: keyboard slides independently, CTA stays pinned
        ├── goals-step/
        ├── state-step/
        ├── zip-step/
        ├── birthdate-step/
        ├── name-step/
        ├── email-step/
        ├── phone-step/
        └── otp-step/
```

---

## LP Preview (`lp-preview/`)

Landing page with animated float cards across three product categories (Instant Income, Safe Growth, Market Growth). Self-contained — open `lp-preview/index.html` directly in a browser.

**Features:** Interactive phone shell, three product tabs, mobile-responsive tab bar, "How it works" section, trust & social proof sections.

---

## Funnel (`funnel/`)

Mobile onboarding funnel prototype. Requires a local static server for font loading:

```bash
cd funnel
python3 -m http.server 3000
# or: npx serve .
```

Then open:
- `http://localhost:3000/v1/goals-step/` — V1 funnel
- `http://localhost:3000/v2/goals-step/` — V2 funnel

### Screen Flow

```
goals-step → state-step → zip-step → birthdate-step → name-step → email-step → phone-step → otp-step
```

### V1 vs V2

| | V1 | V2 |
|---|---|---|
| **Keyboard** | Slides up with footer as one unit | Slides up independently, over the CTA |
| **CTA** | Moves up with keyboard | Always pinned at bottom |
| **Keyboard container** | `.bottom-wrapper` (inside `slide-inner`) | `.keyboard-wrapper` (outside `slide-container`) |
| **JS init** | `EDS.initV1Keyboard(...)` | `EDS.initV2Keyboard(...)` |
| **Auto-focus** | Manual — `EDS.autoFocus(input)` | Built into `initV2Keyboard` |

### Per-Screen Reference

| Screen | Keyboard | Progress | Notes |
|---|---|---|---|
| goals-step | None (card selection) | 10% | 3 selectable goal cards, auto-advances on tap |
| state-step | None (dropdown) | 2% | `EDS.initDropdown` for 50 US states |
| zip-step | Numeric (V1) / iOS (V2) | 4% | 5-digit validation |
| birthdate-step | iOS phone pad | 6% | `EDS.formatDate`, heading + subtext |
| name-step | QWERTY | 5% | `top-group` + security row |
| email-step | QWERTY | 10% | `top-group` + security row + legal text |
| phone-step | iOS phone pad | 20% | `EDS.formatPhone`, 10-digit validation, legal text |
| otp-step | iOS phone pad | 30% | 6-digit OTP boxes, 30s resend timer, "Get my report" CTA |
