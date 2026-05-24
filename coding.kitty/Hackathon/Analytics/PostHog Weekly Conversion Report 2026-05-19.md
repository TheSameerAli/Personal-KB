---
title: PostHog Weekly Conversion Report — May 12–18, 2026
date: 2026-05-19
tags: [analytics, conversions, growth, posthog, coding-kitty]
type: project
status: active
related: ["[[coding.kitty/Hackathon/2026/overview]]", "[[coding.kitty/Hackathon/2026/social-media-strategy]]", "[[Sameer Ali]]"]
---

# PostHog Weekly Conversion Report — May 12–18, 2026

> [!INFO] Data Source
> Pulled live from [[PostHog]] on 2026-05-19. Project: `eu.posthog.com/project/180150`. Covers the 7-day window ending May 18, 2026.

---

## Summary

The site went from near-zero traffic to a meaningful spike over 3 days — almost certainly driven by a launch, social post, or hackathon announcement. The core problem is clear: **traffic is arriving but not converting**. Only 13.9% of homepage visitors reach the login page, and just 6.45% make it to `/ideas`. Retention is also very low — most visitors don't come back the next day.

---

## Traffic Overview

| Metric | Value |
|---|---|
| Unique Visitors (7d) | 469 |
| Total Sessions | 590 |
| Total Pageviews | 1,556 |
| Bounce Rate | 25.4% |
| Avg. Session Duration | 7m 7s |

> [!TIP] Positive Signal
> A 25.4% bounce rate is actually quite good — it means 3 in 4 visitors explore beyond the landing page. The 7-minute average session duration suggests real engagement, not bots.

### Daily Traffic Breakdown

| Date | Unique Visitors | Sessions |
|---|---|---|
| May 11–14 | 0 | 0 |
| May 15 | 6 | 8 |
| May 16 | 175 | 195 |
| May 17 | 237 | 281 |
| May 18 | 84 | 109 |

> [!WARNING] Traffic Cliff
> Traffic dropped ~65% from May 17 to May 18. This is a classic post-launch decay pattern. Without a re-engagement strategy, the audience will evaporate within days.

---

## Conversion Funnel

The funnel from homepage → login → ideas reveals a severe drop-off problem.

| Step | Users | Conversion Rate | Drop-off |
|---|---|---|---|
| Homepage (`/`) | 403 | 100% | — |
| Login Page (`/login`) | 56 | **13.9%** | 86.1% |
| Ideas Page (`/ideas`) | 26 | **6.45%** | 93.55% |

> [!WARNING] Critical Drop-off
> 86 out of 100 visitors who land on the homepage never reach the login page. This is the single biggest conversion lever available right now.

### What This Means

- The homepage is not compelling enough visitors to take action
- The path from homepage → login is unclear or not prominent
- Users who do reach `/login` convert to `/ideas` at ~46% — meaning the login page itself is not the problem; it's getting people *to* it

---

## Top Pages

| Page | Visitors |
|---|---|
| `/` (Homepage) | 403 |
| `/theme` | 189 |
| `/ideas` | 103 |
| `/login` | 74 |
| `/find-teammate` | 50 |

> [!NOTE] Insight
> `/theme` is the second most visited page with 189 visitors — nearly as many as `/ideas`. This suggests users are highly interested in customization or browsing themes. This page could be a conversion hook if it has a CTA to sign up.

---

## Traffic Sources

| Source | Visitors | Notes |
|---|---|---|
| Direct | 197 | Likely from shared links, email, or bookmarks |
| hackthekitty.com | 123 | Internal referral — own site/domain |
| Instagram (`l.instagram.com`) | 113 | Strong social channel |
| Google | 33 | Organic search — low but growing |
| Google Android Search | 10 | Mobile search |
| TikTok | ~7 | Emerging channel |
| LinkedIn | ~3 | Low but present |
| Discord | ~1 | Community referral |

### Source Trend (Daily Unique Visitors)

| Date | Direct | hackthekitty.com | Instagram | Google |
|---|---|---|---|---|
| May 15 | 5 | 0 | 1 | 1 |
| May 16 | 84 | 14 | 60 | 14 |
| May 17 | 120 | 53 | 53 | 20 |
| May 18 | 13 | 81 | 0 | 0 |

> [!INFO] Channel Shift
> Instagram drove 60 visitors on May 16 but dropped to zero by May 18. hackthekitty.com referrals *increased* on May 18 while everything else fell — suggesting a delayed internal link or newsletter send.

---

## Retention

Day-1 retention (users who return the next day) is critically low.

| Cohort Date | Day 0 | Day 1 | Day 2 |
|---|---|---|---|
| May 15 | 6 | 2 (33%) | 2 (33%) |
| May 16 | 173 | 14 (8%) | 4 (2.3%) |
| May 17 | 221 | 10 (4.5%) | — |
| May 18 | 69 | — | — |

> [!WARNING] Retention Crisis
> Only 4–8% of new visitors return the next day. For a product trying to build a community (hackathon, find-teammate, ideas), this is the core growth problem. Users are curious but not finding a reason to come back.

---

## Errors

| Error | Occurrences | Sessions Affected | Users Affected |
|---|---|---|---|
| Script error (web) | 6 | 3 | 3 |

> [!NOTE]
> A generic "Script error" is a cross-origin JS error — likely a third-party script or an error in a script loaded from a different domain. Low volume but worth investigating since it affects real users. [View in PostHog →](https://eu.posthog.com/project/180150/error_tracking/019e3228-f4e9-7930-b73c-193535468614)

---

## Conversion Recommendations

### 🔴 High Priority

1. **Fix the homepage → login drop-off (86.1% loss)**
   - Add a clear, prominent CTA above the fold on `/` — "Sign up free" or "Join the hackathon"
   - The current flow loses 347 out of 403 visitors before they even see the login page
   - A/B test CTA copy and placement

2. **Add a CTA to `/theme`**
   - 189 visitors browse themes but there's no evidence they convert
   - A "Sign up to save your theme" or "Join to submit a theme" prompt could capture this engaged audience

3. **Improve Day-1 retention**
   - Send a follow-up email or push notification within 24 hours of signup
   - Add a reason to return: daily challenge, new ideas feed, teammate match notification

### 🟡 Medium Priority

4. **Double down on Instagram**
   - Instagram drove 113 visitors — the second largest source
   - The May 16 spike (60 visitors in one day) shows the channel works
   - Post consistently; the drop to zero on May 18 suggests a single post drove all traffic

5. **Capitalize on hackthekitty.com referrals**
   - 123 visitors came from the own domain — likely a blog post or announcement
   - Add more internal links from that content to the signup/ideas flow

6. **Investigate the `/find-teammate` page**
   - 50 visitors reached this page — it's a high-intent feature
   - If it requires login, make sure the login redirect is smooth and returns users to this page after auth

### 🟢 Lower Priority

7. **Grow Google organic**
   - Only 33 visitors from Google — significant upside
   - Add meta descriptions and title tags optimized for hackathon-related searches

8. **Fix the cross-origin script error**
   - 6 occurrences across 3 users — small but real
   - Check for third-party scripts loaded without `crossorigin` attribute

---

## Key Numbers to Watch Next Week

| Metric | Current | Target |
|---|---|---|
| Homepage → Login conversion | 13.9% | 20%+ |
| Day-1 retention | 4–8% | 15%+ |
| Instagram visitors | 113 | 150+ |
| Total unique visitors | 469 | 600+ |

---

## See Also

- [[coding.kitty/Hackathon/2026/social-media-strategy]] — social channel strategy
- [[coding.kitty/Hackathon/2026/overview]] — hackathon context
- [[coding.kitty/Hackathon/2026/sponsors/sponsor-tracker]] — sponsor pipeline
