# Pickr — Product Roadmap

> Owner: Kavih AI Technologies Pvt. Ltd. (transfer in progress)
> Live: https://pickrswipe.vercel.app

---

## ✅ Done

- Single-file HTML/CSS/JS app with Tinder-style swipe UX
- Local folder support (Chrome/Edge: direct disk save; Firefox/Safari: ZIP download)
- Touch swipe gestures for mobile
- Google Drive + Google Photos integration (disabled pending OAuth verification)
- Warm light theme, all-browser support
- Deployed on Vercel with auto-deploy from GitHub
- Privacy Policy and Terms of Use pages
- Vercel Analytics integrated

---

## 🔜 Phase 1 — Legal & Business Foundation

- [ ] **Purchase custom domain** (`pickrswipe.com` or `pickr.app`)
  - Register under Kavih AI Technologies Pvt. Ltd. as registrant
  - Connect to Vercel (CNAME record)
  - Unlocks: Google consent screen URL fields, professional branding

- [ ] **Transfer project ownership to Kavih AI Technologies**
  - Create GitHub org `kavih-ai` (or `kavihaitechnologies`)
  - Transfer `pickr` repo from personal account to org
  - Create Vercel team for Kavih AI → transfer project
  - Update copyright notice in license + footer to "Kavih AI Technologies Pvt. Ltd."
  - GCP project → IAM → add company account as Owner

- [ ] **Complete Google OAuth verification**
  - Fill in homepage / privacy policy / TOS URLs in GCP consent screen (needs custom domain first)
  - Submit app for Google review (sensitive scopes: drive.readonly, drive.file, photoslibrary.readonly)
  - Re-enable Google Drive and Google Photos buttons in index.html once verified

---

## 🔜 Phase 2 — User Behaviour Analytics

- [ ] **Opt-in consent banner**
  - On first visit, show a non-intrusive banner: "Help improve Pickr by sharing anonymous photo selection data"
  - Store consent decision in localStorage
  - Required before any behavioural data is logged

- [ ] **Backend for event logging**
  - Vercel serverless function (`/api/log-event`) to receive selection events
  - Database: Supabase (free tier, Postgres) or Firebase Firestore
  - Schema per event:
    ```
    sessionId (anonymous UUID), photoIndex, totalPhotos,
    decision (keep|skip), timeSpentMs, timestamp
    ```
  - Never log filenames, actual images, or any personally identifiable information

- [ ] **Dashboard to view aggregated data**
  - What % of photos do users keep vs skip?
  - Average time spent per photo
  - At what position in a folder do users show "swipe fatigue"?

---

## 🔜 Phase 3 — AI / Smart Features

- [ ] **Photo preference model**
  - Use collected keep/skip labels as training signal
  - Extract image embeddings using pretrained CLIP or EfficientNet (no training from scratch)
  - Train binary classifier (keep/skip predictor) on top of embeddings
  - Goal: predict score for each photo before user sees it

- [ ] **"Smart Sort" feature**
  - Pre-rank photos in a folder by predicted preference score
  - User sees best photos first → faster workflow
  - Shown as an opt-in toggle on the landing screen

- [ ] **"Moments" / highlights generation** (long-term)
  - Cluster selected photos by visual similarity + timestamp proximity
  - Surface as auto-generated highlight sets (similar to Google Photos Memories / Apple Highlights)
  - Requires: preference model + face/scene detection + temporal clustering

---

## 🔜 Phase 4 — Monetisation

See monetisation strategy below. Priority order:

- [ ] Google AdSense — display ads (passive, low-friction)
- [ ] Affiliate partnerships — camera gear, cloud storage, photo editing apps
- [ ] Pickr Pro — one-time purchase or subscription for premium features
- [ ] B2B / white-label licensing to photography businesses

---

## Monetisation Strategy

### Option 1 — Google AdSense (Start Here)
- Add a single non-intrusive banner ad (e.g., bottom of landing screen, below Done screen)
- Requires: Google AdSense account, site must have real traffic + original content
- Revenue: ~$1–5 CPM (per 1,000 page views) — scales with traffic
- No upfront cost, passive income

### Option 2 — Affiliate Links
- Recommend products relevant to photographers:
  - Google One / iCloud storage upgrades
  - Lightroom, Capture One subscriptions
  - Camera gear on Amazon (4–8% commission)
- Place naturally in the Done screen ("Love your picks? Edit them in Lightroom →")
- No friction for users, no ads blocker impact

### Option 3 — Pickr Pro (Freemium)
Free tier (always): local folder, basic swipe UX
Pro tier ($3–5/mo or one-time $15):
- Smart Sort (AI pre-ranking)
- Batch duplicate detection
- Cloud sync (Google Drive / Photos — once OAuth verified)
- Unlimited session history

Payment: **Stripe** or **Razorpay** (India-friendly, supports INR)
Requires a backend (can extend the Phase 2 serverless setup)

### Option 4 — B2B / White-label
- Photography studios, real estate photographers, wedding photographers all deal with
  hundreds of RAW photos per shoot → exactly Pickr's use case
- Offer a white-labeled version or API under Kavih AI brand
- Pricing: SaaS per-seat or per-project

### Recommended order
1. AdSense first (zero effort, start earning with any traffic)
2. Affiliate links alongside (complementary, no conflict)
3. Pro tier once Smart Sort (AI feature) is built — gives a compelling reason to pay
4. B2B later once Pro is proven

---

## Notes

- Google Photos API quota: 10,000 requests/day shared across all users — monitor once re-enabled
- All AI/ML work should be done server-side; never send user photos to any external model without explicit consent
- GDPR/PDPB (India's data protection law) compliance required before collecting any behavioural data
