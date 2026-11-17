# Prompt for Google AI Studio (Build)

Attach `index.html` and the 6 files in `img/` to the chat, then paste this:

---

Rebuild the attached landing page as a production app. It is the landing page for paid Google and Meta ads for Southwest Roofing Solutions, a roofer in Miami-Dade, and it is already approved by the client. Your job is to make it work, not to redesign it.

**Keep exactly as they are**
- Every word of copy, every heading, the order of the sections, the colors, fonts and layout of the attached `index.html`. Do not add, remove or rewrite any claim, number, review, offer term or legal line. Do not add stock photos, icons of fake badges, testimonials, counters or "limited time" banners.
- The 6 images in `img/` (logo.jpg and five job-*.webp photos), with their captions and alt text.
- The phone number (786) 600-4721 and its `tel:+17866004721` links, the license line "Lic. CCC1332456", and the footer offer terms.
- `<meta name="robots" content="noindex, nofollow">`.
- The mobile layout: single column under 860px and the sticky "Call / Get my price" bar at the bottom.

**Make the form work**
- Fields: Name (required), Phone (required, US format), ZIP (required, 5 digits), Current roof (select). Validate on the client with plain error text under each field.
- On submit, POST JSON `{name, phone, zip, roof, source: "re-roof-landing", utm_source, utm_medium, utm_campaign, utm_term, gclid, fbclid, page_url, submitted_at}` to `LEAD_WEBHOOK_URL` (an environment variable; I will paste our CRM webhook). Read the UTM, gclid and fbclid values from the page URL on load.
- Disable the button while sending. On success, replace the form with: "Thanks, [first name]. Alejandro's team will call you to book your inspection." On failure, show: "Something went wrong. Please call (786) 600-4721." and keep what they typed.
- The "Claim my $1,500 off" buttons lower on the page scroll to the form and focus the Name field.

**Track conversions**
- Load the Google tag with `GOOGLE_ADS_ID` and the Meta Pixel with `META_PIXEL_ID` (both environment variables; skip loading either one if it is empty).
- On a successful form submit: fire Google Ads conversion `GOOGLE_ADS_LEAD_LABEL` and Meta `Lead`.
- On any click of a `tel:` link: fire Google Ads conversion `GOOGLE_ADS_CALL_LABEL` and Meta `Contact`.
- PageView on load for both.

**Quality bar**
- Mobile first. Lighthouse 90+ on mobile for performance and accessibility. Lazy-load the photos below the fold, keep the hero text visible without waiting for images.
- No console errors. Works without JavaScript enough that the phone links still call.

**Deliver**
- Deploy it (Cloud Run), give me the public URL, and list the four environment variables I need to fill in.

---

## Values to have ready

| Variable | Where to get it |
|---|---|
| `LEAD_WEBHOOK_URL` | Inbound webhook from the client's CRM |
| `GOOGLE_ADS_ID` | Google Ads › Goals › Conversions › tag setup (AW-XXXXXXXXX) |
| `GOOGLE_ADS_LEAD_LABEL`, `GOOGLE_ADS_CALL_LABEL` | One conversion action each: "Form lead" and "Call click" |
| `META_PIXEL_ID` | Meta Events Manager › the client's dataset |

After it is live, point `southwestrs.com/re-roof` at it (or use the Cloud Run URL as the ads' final URL), send one test lead, and check that it reaches the CRM and shows in both ad platforms before turning the ads on.
