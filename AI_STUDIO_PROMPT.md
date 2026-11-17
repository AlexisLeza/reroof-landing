# Prompt for Google AI Studio (Build)

Paste the text between the lines. It points AI Studio to this GitHub repo. If AI Studio says it can't open the links, download the repo (Code › Download ZIP), attach `index.html` and the 4 files in `img/`, and send the same prompt.

---

The source of this page is in the public GitHub repo https://github.com/AlexisLeza/reroof-landing:
- Page: https://raw.githubusercontent.com/AlexisLeza/reroof-landing/main/index.html
- Images: https://raw.githubusercontent.com/AlexisLeza/reroof-landing/main/img/logo.jpg, and roof-gray.webp, roof-valley.webp, roof-ridge.webp in the same `img/` folder.

Copy the images into the app's own assets; don't hotlink them. If you can't read these links, tell me and I'll attach the files.

Rebuild this landing page as a production app. It is the landing page for paid Google and Meta ads for Southwest Roofing Solutions, a roofer in Miami-Dade, and it is already approved by the client. Your job is to make it work, not to redesign it.

**Scope: this one page only**
- Build a separate, standalone app that contains only this landing page. It is a new page that sits beside the client's existing website; it does not replace it.
- Do not touch, rebuild, restyle, crawl, import from or link into the existing website (southwestrs.com) or any of its pages, menus, files, hosting or DNS. Do not add other pages, a navigation menu, a blog or a sitemap.
- The only links that leave this page are the `tel:` links. The logo is not a link.
- If something seems to need a change on the main website, stop and tell me instead of doing it.

**Keep exactly as they are**
- every word of copy, every heading, the order of the sections, the colors, fonts and layout of `index.html`. Do not add, remove or rewrite any claim, number, review, offer term or legal line. Do not add stock photos, icons of fake badges, testimonials, counters or "limited time" banners.
- The 4 images (logo.jpg and three roof-*.webp photos), with their captions and alt text.
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
- Deploy it (Cloud Run), give me the public URL, and list the five environment variables I need to fill in.

---

## Values to have ready

| Variable | Where to get it |
|---|---|
| `LEAD_WEBHOOK_URL` | Inbound webhook from the client's CRM |
| `GOOGLE_ADS_ID` | Google Ads › Goals › Conversions › tag setup (AW-XXXXXXXXX) |
| `GOOGLE_ADS_LEAD_LABEL`, `GOOGLE_ADS_CALL_LABEL` | One conversion action each: "Form lead" and "Call click" |
| `META_PIXEL_ID` | Meta Events Manager › the client's dataset |

After it is live, use the Cloud Run URL as the ads' final URL. That leaves the website exactly as it is. Later, if you want a branded address, add a subdomain such as `oferta.southwestrs.com` that points only to this app. A new DNS record doesn't change the existing site. Send one test lead and check that it reaches the CRM and shows in both ad platforms before turning the ads on.
