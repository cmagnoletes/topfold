---
name: topfold
description: Design and ship high-impact LinkedIn profile banners from scratch. Use when user wants to create, redesign, critique, or iterate on their LinkedIn cover image / banner. Handles proof mining, copy architecture, layout design, HTML/CSS generation, Playwright rendering, asset sourcing, and upload troubleshooting. Standalone — does not depend on other skills.
---

# topfold — LinkedIn Banner Skill

Create high-impact LinkedIn profile banners through a structured 5-phase process. The skill guides the user from raw career material (proof mining) all the way through to a rendered, uploaded, display-tested banner — with 3 layout templates, a universal CSS design system, and known-issue troubleshooting baked in.

This skill was extracted from a 3-hour real-world iteration (6+ rounds of refinement) that produced a polished banner for a high-credibility operator. Every rule, template, and fix in this skill corresponds to something that was actually tried and validated, not theory.

---

## 1. When to Use This Skill

**Activate when the user says any of these:**
- "Build me a LinkedIn banner" / "Design my LinkedIn cover"
- "Help me redesign my profile banner"
- "My LinkedIn banner looks bad, fix it"
- "I want a banner like [shares reference]"
- Shares a screenshot of their current banner asking for critique
- Asks to iterate on an existing banner in progress

**Do NOT activate for:**
- LinkedIn feed posts (→ `linkedin-post` skill)
- Multi-slide carousels (→ `linkedin-carousel` skill)
- Single-image infographics for posts (→ `linkedin-infographic` skill — those are 1080×1350 portrait, not 1584×396 landscape)
- Company page banners (different aspect ratio, different constraints)
- LinkedIn ad creatives (different specs)
- Animated/video banners (LinkedIn banners are static only)

---

## 2. LinkedIn Banner Non-Negotiables

These constraints are fixed by LinkedIn. Design within them or the banner will fail.

### Dimensions and format
- **Native resolution:** exactly **1584 × 396 pixels** (4:1 aspect ratio)
- **Display scale:** LinkedIn renders at roughly **50% of native** on both web and mobile. **This means everything must be designed ~2x larger than it would be for a normal web page.** Font sizes, logo heights, stroke widths — all roughly double what feels "right" on your design canvas.
- **Color space:** sRGB. Always embed the sRGB profile on export.
- **File format priority:** JPG with embedded sRGB (most reliable), PNG as fallback
- **File size:** under 2 MB (LinkedIn allows 8 MB, but smaller = faster upload and fewer upload failures)

### Profile photo overlay zones

LinkedIn overlays a circular profile photo on the **bottom-left of the banner**. The photo extends below the banner into the profile content area, but the upper half of the circle covers the banner itself. **The photo is narrower at the top of the banner and wider at the bottom**, because the circle's widest point is near the bottom edge.

This means your "photo-safe" horizontal indent must be **vertically-position-aware**, not a single constant.

Approximate safe zones (measured from the banner's left edge):

| Vertical zone | y range | Photo covers | Safe content starts at |
|---|---|---|---|
| Hero top | 0 – 40% of banner height | x=0 to ~200px | **240–280px** |
| Middle | 40 – 70% | x=0 to ~350px | **300–340px** |
| Bottom strip | 70 – 100% | x=0 to ~450px | **380–440px** |

**Recommended default indents:**
- Headline / eyebrow (top of banner): `padding-left: 240–280px`
- Middle row (if any): `padding-left: 300–340px`
- Brand strip / footer (bottom of banner): `padding-left: 380–400px`

**If you use a single constant indent:** use 400px to be safe everywhere. You'll waste horizontal space in the top rows, but nothing will be covered.

### Upload protocol

In order of reliability:

1. **LinkedIn mobile app** (highest success rate). Open app → tap profile → tap banner → upload. This path works when web uploads fail.
2. **Web in an incognito window** (rules out browser extensions, ad blockers, stale session state)
3. **Different browser** (Safari if using Chrome, or vice versa — Chrome sometimes fights with LinkedIn's uploader)
4. **Wait 5–10 minutes** and retry (if it's a rate limit or temporary server issue, it'll clear)

If all of the above fail, run the **diagnostic test** from Phase 5 to isolate file vs session issues.

---

## 3. The Content Chain

Every element on a successful LinkedIn banner has exactly **one job**. Don't mix jobs between elements. This is the single most important content principle in the skill.

| Element | Single job | Example |
|---|---|---|
| **Headline** | Positioning (who you help + how) | "Installing audience-led growth engines for founder-led brands" |
| **Stats / Pills** | Proof (scale/outcome numbers) | `$80M+ Generated` · `1M+ YouTube Subs` · `4M+ Users Reached` |
| **Brand strip label** | Ownership / authorship claim | "I built products used by teams at" |
| **Brand logo strip** | Social proof (who trusts or uses your work) | Amazon · Netflix · Dell · Adobe · Microsoft |
| **Monogram / Logo** | Identity anchor | CM initials in brand color, top-left |
| **Eyebrow text** | Credential short form | "Ex-CEO Ubersuggest & ATP" |
| **Tagline** (optional) | Single-sentence elaboration | — |
| **CTA** (optional, discouraged) | Next-step prompt | "Currently taking 3 clients" |

### Content chain rules

1. **One element = one job.** If the headline already establishes positioning, the tagline shouldn't repeat positioning — it should amplify or specify.

2. **Don't put a job twice.** If the headline says "ex-CEO of X", the brand strip label shouldn't also say "I ran X". Each element claims different territory.

3. **Customer brand strip > product logo strip.** "I built products used by teams at Amazon, Netflix..." is 10x stronger than listing your own product logos. It shifts proof from "these are my products" to "these household names trusted what I built." If the user has customer brands to reference, always prefer them.

4. **Each element must earn its space.** If an element doesn't clearly do one of the jobs above, delete it. Whitespace beats weak content.

5. **CTA is usually a trap.** LinkedIn banners aren't clickable. A "DM me X" or "Book a call" CTA wastes real estate and cheapens premium positioning. The headline + proof stack should make the next step obvious. Put the CTA in the About section, not the banner.

---

## 4. Phase Workflow

The skill runs in 5 sequential phases. Each phase has a clear goal, a deliverable, and a transition criterion.

### Phase 1 — Proof Mining (~30 min)

**Goal:** Extract raw career material from the user and organize it into a proof stack.

**Process:**

Ask the user these 6 questions, one or two at a time:

1. **Biggest financial number.** Revenue generated, ARR built, profit contributed, funding raised, budget managed, cost savings delivered. What's the biggest dollar figure you can credibly claim?

2. **Biggest audience number.** Subscribers, followers, MAU, monthly users, views, reach. What's your largest audience metric?

3. **Most recognizable brands or people you've worked with.** Organize into tiers:
   - Tier 1: household names (Neil Patel, Tim Ferriss, Fortune 500, etc.)
   - Tier 2: industry-known (influential in your niche)
   - Tier 3: warm credibility (companies or people your ICP would recognize)

4. **Customer brands that USED what you built.** This is different from question 3. It's the brands whose teams used your product, your service, your tool, your content. Tier 1 (household names) is gold here — "Amazon, Netflix, Microsoft used my product" is dramatically stronger than "I worked at X."

5. **Unique angle.** What can you claim that almost nobody else in your field can? (E.g., "self-taught operator who ran a $16M ARR SaaS"; "8 years inside one famous operator's empire"; "built and shipped AI products before AI was mainstream")

6. **The positioning sentence.** Fill in the blank: "I help [who] with [what] by [how]." Example: "I help founder-led SaaS brands at $5–10M ARR scale audience and revenue by installing the same playbook I ran at [product]."

**Deliverable: a proof stack document**

Organize the raw material into 4 tiers and save as `{user-workspace}/topfold/proof-stack.md`:

```markdown
# Proof Stack — {User Name}

## Heavy Hitters (crushing numbers)
- {biggest financial number + context}
- {biggest audience number + context}
- {biggest operational win + context}

## Credibility Anchors (brands, people)
### Tier 1 — household names
- {brand/person + relationship}

### Tier 2 — industry-known
- ...

### Tier 3 — warm credibility
- ...

## Unique Angles (differentiation)
- {thing only I can claim}
- ...

## Range (breadth)
- {scale of team led / product built / audience reached}
- ...

## Customer Brands (used what I built)
- {brand 1}
- {brand 2}
- ...
```

**Transition criterion:** User confirms the proof stack captures the strongest material. Move to Phase 2.

### Phase 2 — Copy Architecture (~15 min)

**Goal:** Assign one job to each element, draft copy for each, lock in the copy brief.

**Process:**

1. **Review the Content Chain table** (Section 3) with the user. Confirm which elements they want on the banner. Minimum: headline, stats, brand strip. Recommended: all of the above plus a monogram.

2. **For each element, propose 2–3 copy options** — and explicitly call out the job each option serves. Example:

   > **Headline — job: positioning (who you help + how)**
   >
   > Option A: "Installing audience-led growth engines for founder-led brands"
   > Option B: "From 0 to $16M ARR for Neil Patel. Now for founder-led brands."
   > Option C: "I help founder-led SaaS brands scale audience and revenue"
   >
   > My pick: Option A. Reasoning: "Installing" is an operator verb, not consultant verb. "Engines" implies systems. "Founder-led brands" is precise ICP.

3. **Apply the don't-put-a-job-twice rule.** If the headline says "ex-CEO of X", don't repeat that credential in the brand strip label. Each element claims different territory.

4. **Apply the customer-brand-strip rule.** If the user has customer brands to reference, build the brand strip around those, not around the user's own product logos.

5. **Lock in copy for each element.** Iterate if the user wants — expect 2–3 rounds.

**Deliverable: a copy brief**

Save as `{user-workspace}/topfold/copy-brief.md`:

```markdown
# Copy Brief — {User Name} LinkedIn Banner

## Headline
**{Final headline text}**
- Job: positioning
- Voice notes: {e.g., italic emphasis on key phrase}

## Stats / Pills
1. **{NUMBER 1}** — {label 1}
2. **{NUMBER 2}** — {label 2}
3. **{NUMBER 3}** — {label 3}
- Job: proof

## Brand strip label
**{Final label text}**
- Job: ownership claim

## Brand logos (customer brands)
{Brand 1} · {Brand 2} · {Brand 3} · ...
- Job: social proof

## Monogram / Logo
{Description or file path}
- Job: identity anchor

## Eyebrow (if used)
**{Eyebrow text}**
- Job: credential short form
```

**Transition criterion:** Copy brief locked. Move to Phase 3.

### Phase 3 — Visual Design (~30 min)

**Goal:** Pick layout, collect brand assets, confirm palette and typography.

**Sub-steps:**

#### 3a. Reference gathering

Ask the user for **5–8 reference banners they like**. Have them share screenshots.

For each reference, extract patterns:
- Logo placement (top-left, top-right, inline with headline, absent)
- Headline style (big serif, sans-serif, all-caps, title case)
- Proof stat style (pills, cards, inline text, large hero numbers)
- Brand strip position (bottom, top, beside content, absent)
- Color mood (dark, light, brand-color-dominant)
- Photo vs no photo (personal headshot, product shot, no photo)
- Whitespace density (tight or airy)

Summarize the patterns in 2–3 sentences and identify the "directions" the user gravitates toward. Example:

> "Your references lean dark-background, serif headlines, and pill-shaped stats. 5 of 8 use a 'Trusted by' or 'Featured in' logo strip at the bottom. None use a personal photo in the banner itself. You gravitate toward proof-dense designs with clean typography, not minimalist or text-heavy narrative styles."

#### 3b. Brand mining — DO NOT SKIP THIS

**This is the single most important step for a brand-matched banner.** The skill's default palette is a warm dark with terracotta — it will NOT match most users' brands. You must either (a) ask the user directly for their tokens, or (b) mine them from an existing website/asset.

**Never guess brand colors from qualitative descriptions** ("looks dark navy with red accents"). Qualitative descriptions produce hex codes that are close but wrong, and "close but wrong" reads as a fake brand match. Always extract **actual pixel values** from real assets.

There are THREE mining modes. Pick based on what the user provides:

##### Mode A — User provides tokens directly

The easiest case. Ask:

1. **Hex codes:** background (primary), accent (brand color), text, muted
2. **Logo or monogram:** PNG or SVG with transparent background
3. **Headshot:** only if they pick the Photo+Proof variant
4. **Typography:** font family names (Google Fonts preferred)
5. **Customer brand logos:** do they have them locally or should the skill source them

If the user has all of these, you're done — skip to 3c.

##### Mode B — User has a website (mine from the live site)

**Use this when:** the user has a personal website, portfolio, blog, or landing page. Their brand is already defined there — extract it, don't invent it.

Perform these 4 steps in order:

**Step 1 — Extract CSS tokens via WebFetch.**

```
WebFetch URL: {user's website URL}
Prompt: "Extract raw brand identity tokens. Return:
 1. All CSS custom properties (--variables) in :root or other selectors, especially color and font
 2. All hex color codes in HTML/CSS (#xxxxxx patterns, with context where each appears)
 3. All Google Fonts imports (exact URL and family names)
 4. Body and heading font-family values (h1/h2/h3/body)
 5. Meta theme-color tag if present (often the brand primary)
 6. Background colors used in hero section, buttons, CTAs
 7. Image references to logo/monogram files (URL + dimensions)
 8. Hero banner image URL if the user already has one
 Return raw data — no summaries, no interpretation."
```

This gives you CSS-declared values. It does NOT give you the actual rendered colors — which is often different because of gradients, overlays, or image-based backgrounds.

**Step 2 — Download brand image assets.**

If the WebFetch found logo/banner image URLs, download them:

```bash
curl -sL -A "Mozilla/5.0" "{logo-url}" -o "logos/{user}-logo.{ext}"
curl -sL -A "Mozilla/5.0" "{banner-url}" -o "brand-assets/existing-banner.{ext}"
```

If the files are WebP, convert to PNG for easier inspection:

```python
from PIL import Image
img = Image.open('brand-assets/existing-banner.webp')
img.save('brand-assets/existing-banner.png', 'PNG')
```

**Step 3 — Sample actual pixel colors from brand imagery.**

This is the critical step most agents skip. CSS hex values only cover solid colors — brand identity often lives in gradients, photo-based backgrounds, or image overlays. **Sample the pixels directly.**

```python
from PIL import Image
from collections import Counter

img = Image.open('brand-assets/existing-banner.png').convert('RGBA')
w, h = img.size
print(f"Banner: {w}x{h}")

# Sample at 9 key points: corners, midpoints, center
samples = {
    'top-left':     (10, 10),
    'top-right':    (w-10, 10),
    'bottom-left':  (10, h-10),
    'bottom-right': (w-10, h-10),
    'mid-left':     (10, h//2),
    'mid-right':    (w-10, h//2),
    'center':       (w//2, h//2),
    'top-mid':      (w//2, 10),
    'bot-mid':      (w//2, h-10),
}
for name, (x, y) in samples.items():
    r, g, b, a = img.getpixel((x, y))
    print(f"  {name:14}: #{r:02x}{g:02x}{b:02x}")

# Plus top 5 dominant colors
pixels = [img.getpixel((x, y))[:3] for x in range(0, w, 20) for y in range(0, h, 20)]
for color, count in Counter(pixels).most_common(5):
    r, g, b = color
    print(f"  dominant: #{r:02x}{g:02x}{b:02x}  ({count} pixels)")
```

Sampled pixels are **ground truth** — they're literally what the user's brand looks like in context. If the CSS says `#020934` but the banner's bottom-left sample returns `#2cb0da`, that's a gradient and BOTH colors are part of the brand. Use the CSS value as the **anchor** and the sampled dominant colors as **highlights**.

**Step 4 — Identify typography.**

- If the WebFetch returned Google Fonts imports, use the same families
- If no Google Fonts (system font stack), look at the LOGO. Is it condensed sans? serif? geometric? Pick the closest Google Font match:
  - **Condensed geometric sans (like Ken Yarmosh's logo):** Archivo Narrow, Oswald, Barlow Condensed
  - **Geometric sans (like Circular, Proxima Nova):** Inter, Space Grotesk, Archivo
  - **Humanist sans:** Inter, Work Sans
  - **Serif:** DM Serif Display, Playfair Display, Fraunces
  - **Slab serif:** Archivo Slab, Roboto Slab
- Default if you can't tell: **Inter** for body, **Archivo** or **DM Serif Display** for headlines

**Output of Mode B:** a documented brand token map like:

```css
/* Mined from {user's website URL} on {date} */
:root {
  --bg:        #020934;   /* CSS --ghost-accent-color */
  --bg-mid:    #1d7aa6;   /* sampled mid-point of existing banner gradient */
  --bg-light:  #2cb0da;   /* sampled bottom-left of existing banner (brand highlight) */
  --accent:    #2cb0da;   /* brightest signature color */
  --accent-hi: #4dc4e8;   /* 10% lighter accent for emphasis */
  --accent-red: #c94040;  /* CSS announcement bar color (reserve for alert use only) */
  --text:      #ffffff;
  --surface:   #0a1642;   /* slightly lighter navy for pill backgrounds */
  --muted:     #8a9bb8;
  --border:    #1e2e50;
}

/* Typography */
/* Ken's logo is condensed geometric sans → closest Google Fonts match: Archivo Narrow */
@import url('https://fonts.googleapis.com/css2?family=Archivo+Narrow:wght@500;600;700&family=Inter:wght@400;500;600;700&display=swap');
```

This is what the brand token section should look like BEFORE you start writing HTML. Show it to the user for confirmation: "Here's what I mined from your site — does this feel right?" Let them override if they want.

##### Mode C — User has no website, no tokens, no logo

**Use this when:** the user is early-stage and hasn't built brand identity yet. Don't fake it — offer the default warm dark palette honestly:

```css
:root {
  --bg:        #0f0d0b;  /* near-black warm dark */
  --text:      #fdf7f2;  /* warm cream */
  --accent:    #b94c4c;  /* terracotta — or user's brand color */
  --accent-hi: #d05c5c;  /* 10% lighter accent for emphasis */
  --muted:     #a89a92;  /* warm gray */
  --border:    #2a2320;
  --surface:   #161210;
  --stone:     #d5d0c8;
  --cream:     #eae6df;
}
```

Tell the user: "I'll use a warm dark default palette since you don't have brand tokens yet. If you want a different vibe, pick a primary accent color and I'll rebuild around it."

If the user picks just ONE color (say `#0066ff`), compute the rest:
- `--accent-hi`: lighten the accent by ~10% (HSL lightness +8–12)
- `--bg`: pick a very dark near-black that harmonizes (neutral `#0b0d10` or tinted toward the accent hue)
- `--surface`: slightly lighter than bg (~5% lightness boost)
- `--text`: near-white (`#fafafa` or `#ffffff`)
- Keep `--muted`, `--border`, `--stone`, `--cream` as neutrals unless the user wants a specific mood

##### Critical rule: show your work

After mining (Mode A, B, or C), **always show the user the final token map BEFORE building the HTML**. Format it as a code block. Ask: "Does this feel right? Want me to tweak anything?"

This catches wrong colors, bad font picks, and missing tokens before they get baked into a rendered PNG that takes another 20 minutes to redo.

**Default palette if user has nothing:**

```css
:root {
  --bg:        #0f0d0b;  /* near-black warm dark */
  --text:      #fdf7f2;  /* warm cream */
  --accent:    #b94c4c;  /* terracotta — or user's brand color */
  --accent-hi: #d05c5c;  /* 10% lighter accent for emphasis */
  --muted:     #a89a92;  /* warm gray */
  --border:    #2a2320;
  --surface:   #161210;
  --stone:     #d5d0c8;
  --cream:     #eae6df;
}
```

If the user provides their own primary color, compute `--accent-hi` as ~10% lighter using HSL lightening. Keep the rest of the palette as-is (background, cream, muted) — those are universally usable and don't fight with user brand colors.

#### 3c. Layout selection

Offer the 3 starter layouts described in Section 6. Let the user pick one or more.

| Layout | Best for | Visual summary |
|---|---|---|
| **Numbers Grid** | High-credibility operators with 4 crushing stats; "media kit" aesthetic | 4 big stats horizontally + tagline + customer brand strip |
| **Story Arc** | Narrative-forward positioning, trajectory stories, founders with a "journey" | 3 lines of text stacked (past → present → future) + customer brand strip |
| **Stacked Pills** ⭐ | Balanced proof + clean hierarchy; works for most people | Headline left + 3 stat pills in a vertical column on the right + customer brand strip at the bottom |

**Default recommendation:** Stacked Pills. It's the most versatile, handles most proof stacks, and was the final pick in the reference iteration this skill was extracted from.

**If the user can't decide:** build all 3 variants and let them compare. Each takes ~20 minutes to produce.

**Transition criterion:** Layout chosen, brand tokens locked, reference patterns captured. Move to Phase 4.

### Phase 4 — Build & Render (~30 min per variant)

**Goal:** Produce a PNG and JPG of the banner from HTML/CSS via Playwright.

#### 4a. Asset sourcing

The biggest unseen blocker is sourcing customer brand logos. Paid services (Brandfetch) require API keys. Free sources require knowing where to look.

**Preferred sources, in priority order:**

1. **Simple Icons CDN** — `https://cdn.simpleicons.org/{brand-slug}/{color-hex-no-hash}`
   - Works for: Netflix, Dell, eBay, Nike, X, Meta, Spotify, Slack, GitHub, Airbnb, Uber, Shopify, most tech brands
   - Returns SVG with specified fill color
   - Example: `https://cdn.simpleicons.org/netflix/fdf7f2` returns Netflix logo in cream color
   - Does NOT work for: trademark-protected brands (Amazon, Adobe, Microsoft, Google, Apple, Coca-Cola, Tesla, Samsung)

2. **Wikimedia Commons SVGs** — for trademark-protected brands Simple Icons refuses to serve
   - Find via Wikipedia search for the brand → look at infobox logo → right-click the SVG source
   - Common paths documented in the cheat sheet (Section 8)
   - These are the original brand-colored logos — you'll need to recolor or filter them to cream/white

3. **Brand's own website press kit** — last resort, usually requires manual download
   - Many brands have `{brand}.com/press` or `{brand}.com/brand-assets` pages
   - Download the official press kit SVG or PNG

4. **Text wordmarks as fallback** — if no logo is available or accessible, render the brand name as uppercase Inter text in HTML
   - Less recognizable than a real logo, but consistent styling across all brands

**Stripping backgrounds** — if you get a logo with a white background (common with `encrypted-tbn0.gstatic.com` thumbnails), strip it with Python + Pillow:

```python
from PIL import Image
img = Image.open('logo-raw.png').convert('RGBA')
data = img.getdata()
new_data = []
for item in data:
    # Make near-white pixels transparent
    if item[0] > 240 and item[1] > 240 and item[2] > 240:
        new_data.append((255, 255, 255, 0))
    else:
        new_data.append(item)
img.putdata(new_data)
img.save('logo-transparent.png', 'PNG')
```

**Cropping bounding boxes** — for logos with lots of empty space around the wordmark:

```python
from PIL import Image
img = Image.open('logo.png').convert('RGBA')
bbox = img.getbbox()
img.crop(bbox).save('logo-cropped.png', 'PNG')
```

**File layout:** save all sourced assets to `{user-workspace}/topfold/logos/`:

```
logos/
├── amazon.svg
├── netflix.svg
├── dell.svg
├── ebay.svg
├── adobe.svg
├── microsoft.svg
├── nike.svg
├── x.svg
└── cm-monogram.png      (user's personal logo)
```

#### 4b. HTML + CSS generation

Use one of the 3 inline templates in Section 6 as the starting point. Customize:

1. **Replace the CSS variables** in `:root` with the user's brand colors
2. **Replace copy** with the Phase 2 copy brief content
3. **Update image paths** in the logo strip to the Phase 4a sourced assets
4. **Adjust photo-safe indents** if needed (use the defaults from Section 2)

Save as `{user-workspace}/topfold/YYYY-MM-DD_v1-{layout}.html`.

**Critical CSS rules:**

- Always use `box-sizing: border-box` globally
- Always use `grid-template-columns: minmax(0, 1fr) auto` when using grid with flexible + fixed columns — this prevents content overflow
- Always set `width: 1584px; height: 396px` on the `.banner` div — don't use viewport units
- Always include a fallback body background so the standalone HTML file looks OK (the screenshot will strip body styles)

#### 4c. Rendering pipeline

Playwright element screenshots are the reliable method. Headless Chrome `--screenshot` silently clips content in some edge cases.

**Step-by-step:**

1. **Start a local Python HTTP server** in the banners directory:
   ```bash
   cd "{user-workspace}/topfold/" && python3 -m http.server 8879 &
   ```

2. **Navigate Playwright** to the file:
   ```javascript
   await page.goto('http://localhost:8879/YYYY-MM-DD_v1-layout.html');
   ```

3. **Wait for fonts and reset body styles**:
   ```javascript
   async () => {
     await document.fonts.ready;
     await new Promise(r => setTimeout(r, 800));
     document.body.style.padding = '0';
     document.body.style.margin = '0';
     document.body.style.minHeight = 'auto';
     document.body.style.display = 'block';
     return 'ready';
   }
   ```

4. **Resize viewport to exact banner dimensions**:
   ```javascript
   await page.setViewportSize({ width: 1584, height: 396 });
   ```

5. **Take a viewport screenshot** (not element screenshot — element screenshots need accessibility refs):
   ```javascript
   await page.screenshot({
     path: '{absolute-path}/YYYY-MM-DD_v1-layout.png',
     scale: 'css',
     type: 'png'
   });
   ```

6. **Export JPG with embedded sRGB profile**:
   ```bash
   python3 -c "
   from PIL import Image, ImageCms
   img = Image.open('banner.png').convert('RGB')
   srgb = ImageCms.createProfile('sRGB')
   srgb_bytes = ImageCms.ImageCmsProfile(srgb).tobytes()
   img.save('banner.jpg', 'JPEG', quality=95, optimize=True, icc_profile=srgb_bytes)
   "
   ```

7. **Stop the server**:
   ```bash
   pkill -f "python3 -m http.server 8879"
   ```

#### 4d. Technical self-check

Run these technical validations:

- **Dimensions are exactly 1584×396** — check with `sips -g pixelWidth -g pixelHeight file.png`
- **All images loaded** — check with Playwright `img.complete && img.naturalHeight > 0` before taking the screenshot
- **File size under 2 MB** — `ls -lh file.jpg`
- **JPG has sRGB profile embedded** — `file banner.jpg` should mention JFIF
- **Aspect ratio preservation of all child images** — for each `<img>` in the banner, verify rendered width/height ratio matches natural width/height ratio (Playwright: `img.getBoundingClientRect()` vs `img.naturalWidth/naturalHeight`). This catches flex-stretch bugs on logos/monograms.

If any check fails, fix before moving to 4e.

#### 4e. Design critique — NEVER SKIP THIS

**Mandatory pre-delivery step.** After the technical self-check passes, the agent MUST visually inspect the rendered PNG and run a structured design critique before showing anything to the user. Technical validation catches dimensions and file integrity — it does NOT catch visual bugs like stretched logos, cramped layouts, color clashes, or hierarchy problems.

**The critique is non-optional.** If you're tempted to skip this because "it looks fine from the code", that's exactly when the visual bug sneaks through. Code-level inspection cannot replace visual inspection.

**How to run the critique:**

**Step 1: View the rendered PNG.**

Use the `Read` tool with the absolute path to the rendered PNG. This loads the image into the agent's visual context so the agent can actually SEE what it produced, not just imagine it from the HTML.

**Step 2: Run the 10-point checklist.**

Score each point as PASS, FAIL, or WARN. Write down the score explicitly in your reasoning — don't skim.

**VISUAL INTEGRITY**
1. **Aspect ratios preserved.** All `<img>` elements (logos, monogram, headshot, brand strip) display at their natural width/height ratio. NO stretching or squashing. This is the #1 most common bug — flex containers with default `align-items: stretch` will horizontally distort child images.
2. **No content clipped at edges.** Nothing runs off the top, bottom, left, or right of the banner. Text doesn't truncate mid-word.
3. **No element overflow.** Nothing spills outside its container. Grid/flex items stay within their assigned cells.

**PROPORTION & HIERARCHY**
4. **Headline is the most prominent text.** Larger than stats, labels, eyebrows, and any other text element. The eye should land on the headline first.
5. **Stat pills are consistently sized.** All pills in the column have the same width and height. No pill is visually dominating or shrinking relative to its siblings.
6. **Brand strip logos have balanced visual weights.** No single logo dominates while others disappear. Small-but-important logos (Nike swoosh, X, Microsoft wordmark) are bumped to compensate for their intrinsic tiny size.
7. **Monogram/logo doesn't compete with the headline.** If present, the monogram is a supporting identity anchor, NOT a competing visual element. The headline should be 2-3x larger than the monogram.

**SPACING & ALIGNMENT**
8. **Consistent alignment.** Left edges of stacked elements line up. Vertical centers match where appropriate. No drifting, no accidental staircase.
9. **Adequate whitespace.** Nothing feels cramped. There's breathing room between elements and around the banner edges.
10. **Photo-safe zones respected.** Bottom-left content is pushed right past the 400px photo-safe indent. Middle content past 340px. Top content past 240px.

**Step 3: For each FAIL or WARN, identify the specific cause and fix.**

Common visual bugs and their fixes:

| Visual bug | Cause | Fix |
|---|---|---|
| Wordmark/monogram horizontally stretched | Flex column default `align-items: stretch` distorts child `<img>` | Add `align-items: flex-start` to the flex container, OR add `align-self: flex-start` on the image |
| Headshot distorted in the photo variant | Missing aspect-ratio preservation on cover image | Use `object-fit: cover` with a fixed container aspect-ratio |
| Stat pills have inconsistent widths | Container width not pinned; each pill sizes to its own content | Add `width: 300px` (or specific px) to `.stats-column`, not `min-width` |
| Brand logos inconsistent sizes | Different SVG viewBoxes; uniform height CSS produces non-uniform visual weight | Tune per-brand heights via `data-brand` attribute selectors; compensate for viewBox padding (Nike needs +50%) |
| Headline wrapping awkwardly mid-phrase | Grid `1fr` allowing content to push column wider than intended | Use `grid-template-columns: minmax(0, 1fr) auto` |
| Text barely visible at display scale | Font sizes designed for native (1584px) but LinkedIn renders at 50% | Bump all fonts ~50% larger than normal web design |
| Colors clash with user's brand | Used default palette instead of mining user's actual brand tokens | Re-do Phase 3b Mode B (WebFetch + pixel sampling) |
| Logo competing with headline | Logo height too tall (> 1/3 of headline height) | Reduce logo height; increase gap between logo and headline |
| Content drifts toward one side of the banner | Hero column grew beyond its grid cell because of nowrap text | Add `min-width: 0` to the hero cell + remove `white-space: nowrap` if present |
| Cramped layout, everything touching | Too many elements competing | Remove the weakest element (usually eyebrow, tagline, or monogram) |

**Step 4: Apply the fix.**

Edit the HTML/CSS, then re-render via Playwright. Run the critique AGAIN on the new render. Iterate until all 10 points PASS.

**Step 5: Present with the critique attached.**

When showing the user the final banner, include the critique score as a summary:

> Here's the banner. Design critique: ✅ 10/10 passed.
> - Visual integrity: ✅ ✅ ✅
> - Proportion & hierarchy: ✅ ✅ ✅ ✅
> - Spacing & alignment: ✅ ✅ ✅

This tells the user you actually looked at the image, not just wrote HTML and hoped.

**If something can't be fixed** (e.g. the user's logo has a weird aspect ratio that doesn't fit any layout gracefully), call it out explicitly: "Critique: 9/10 passed. The {X} logo's aspect ratio is unusually wide — I'd suggest replacing it with a square monogram or skipping it. Flagged for your decision." Don't silently accept a fail.

**Transition criterion:** Design critique scores 10/10 (or 9/10 with an acknowledged exception). Only then move to Phase 5.

### Phase 5 — Test & Ship (~15 min + iteration)

**Goal:** Upload to LinkedIn, diagnose any failures, critique the live render, iterate if needed.

#### 5a. Upload protocol

Have the user try uploading the JPG first, in this order:

1. **LinkedIn mobile app** — open app → tap your profile → tap the banner area → upload photo. **Highest success rate.** If this works, skip to 5c.

2. **Web in incognito/private window** — rules out browser extensions (ad blockers especially)

3. **Different browser** — Safari if Chrome, Firefox if Safari, etc.

4. **Hard-reload LinkedIn and retry** — `Cmd+Shift+R` on desktop, force-close and reopen app on mobile

5. **Wait 5–10 minutes** — if LinkedIn is having a server issue, it clears

If all 5 fail, run the diagnostic test (5b).

#### 5b. Diagnostic test (when upload fails repeatedly)

Generate a minimal test image at the same dimensions to isolate file vs session issues:

```python
from PIL import Image, ImageDraw, ImageCms
img = Image.new('RGB', (1584, 396), color=(15, 13, 11))
draw = ImageDraw.Draw(img)
draw.rectangle([50, 50, 1534, 346], outline=(185, 76, 76), width=4)
draw.text((700, 180), "TEST", fill=(253, 247, 242))
srgb = ImageCms.createProfile('sRGB')
img.save('test-diagnostic.jpg', 'JPEG', quality=90,
         icc_profile=ImageCms.ImageCmsProfile(srgb).tobytes())
```

Have the user try to upload `test-diagnostic.jpg`:

- **Test image uploads fine** → the banner file has some quirk LinkedIn rejects. Iterate on export parameters: strip metadata, reduce JPG quality to 85, try PNG instead
- **Test image also fails** → it's a LinkedIn session/browser issue, not your file. Advise the user to try from a completely different device or come back in an hour

#### 5c. Live render critique

Once the banner is live on the user's profile, ask them to share a screenshot of their actual profile page showing the banner at LinkedIn's display scale. Then run a critique against these 6 checkpoints:

1. **Legibility at display scale** — can you read the smallest text (brand strip label, stat pill labels)?
2. **Profile photo overlap** — is any content being covered by the profile photo circle?
3. **Content chain integrity** — is every element doing its one job?
4. **Brand logo readability** — are the smallest logos recognizable? (Dell, eBay, Nike, Microsoft, X are the usual culprits at small scale)
5. **Color contrast** — do the stats and labels have enough contrast against the background?
6. **Vertical balance** — does the banner feel top-heavy, bottom-heavy, or centered?

Score each checkpoint and offer specific fixes for any that fail.

#### 5d. Iteration triggers — common fixes from real iterations

| Problem | Likely cause | Fix |
|---|---|---|
| "Stats are hard to read" | Accent-on-tinted-accent background (low contrast) | Change pill background to `var(--surface)` (pure dark), bump number color to `var(--accent-hi)`, bump font-size to 32px |
| "Label is barely visible" | 16px muted-color text at display scale is too small/dim | Bump to 20px, change color from muted to stone, tighten letter-spacing from 2.8 → 2.2 |
| "Nike/icon-style logos look tiny" | SVG viewBox has padding; logo fills only ~33% of height | Bump to 50px to compensate — the visible swoosh ends up ~17px tall |
| "eBay / Dell / Microsoft small" | Wordmark logos fill their box but are visually narrow | Bump to 44–48px height |
| "Content covered by profile photo" | Photo-safe zone too small at the bottom | Use vertically-aware indents: 240px top, 340px middle, 400px bottom |
| "Headline wraps awkwardly" | Grid `1fr` is letting content push columns wider than intended | Use `grid-template-columns: minmax(0, 1fr) auto` instead of `1fr auto` |
| "Pills aren't vertically centered" | `justify-content: space-between` stretches them to edges | Use `justify-content: center` with gap for a grouped, centered look |
| "Banner feels cramped" | Too much content competing for space | Remove an element that doesn't clearly earn its space (usually eyebrow or tagline) |
| "Content drifts right with unbalanced negative space" | Hero padding-left set too aggressive | Reduce headline indent; use vertically-aware photo-safe zones |
| "Customer logos look inconsistent" | Mixing real logos with text wordmarks | Commit to one approach: all real logos (use Wikimedia fallbacks for trademarked), or all text wordmarks |

**Transition criterion:** User is satisfied with the live render, or has chosen to ship at "good enough" and revisit later.

---

## 5. CSS Design System

Universal design tokens. These work for any brand — either as-is (warm dark + terracotta default) or with the user's brand colors substituted in.

### Color tokens

```css
:root {
  /* Backgrounds */
  --bg:        #0f0d0b;   /* near-black warm dark — primary background */
  --surface:   #161210;   /* slightly lighter surface for cards/pills */
  --border:    #2a2320;   /* subtle divider */

  /* Text */
  --text:      #fdf7f2;   /* primary text — cream */
  --cream:     #eae6df;   /* secondary cream for headlines */
  --stone:     #d5d0c8;   /* tertiary for labels */
  --muted:     #a89a92;   /* muted text for eyebrows, captions */

  /* Accent (brand color) */
  --accent:    #b94c4c;   /* primary accent — terracotta default */
  --accent-hi: #d05c5c;   /* ~10% lighter for emphasis */

  /* Photo-safe zones (vertically-aware) */
  --photo-safe-top:    240px;  /* hero row left indent */
  --photo-safe-middle: 340px;  /* middle row left indent */
  --photo-safe-bottom: 400px;  /* brand strip left indent */
}
```

### Typography

```css
/* Google Fonts — no local install */
@import url('https://fonts.googleapis.com/css2?family=DM+Serif+Display:ital@0;1&family=Inter:wght@400;500;600;700&display=swap');

/* Headline — DM Serif Display, 52px, line-height 1.1 */
.headline {
  font-family: 'DM Serif Display', serif;
  font-size: 52px;
  line-height: 1.1;
  color: var(--cream);
}
.headline .accent {
  color: var(--accent);
  font-style: italic;
}

/* Eyebrow — Inter, 17px, wide letter-spacing, uppercase */
.eyebrow {
  font-family: 'Inter', sans-serif;
  font-size: 17px;
  font-weight: 700;
  letter-spacing: 2.8px;
  text-transform: uppercase;
  color: var(--accent);
}

/* Stat pill text — Inter, 16px, wide letter-spacing, uppercase */
.stat-pill {
  font-family: 'Inter', sans-serif;
  font-size: 16px;
  font-weight: 700;
  letter-spacing: 2.2px;
  text-transform: uppercase;
  color: var(--cream);
}

/* Stat pill number — DM Serif Display, 32px, line-height 1 */
.stat-pill-num {
  font-family: 'DM Serif Display', serif;
  font-size: 32px;
  line-height: 1;
  color: var(--accent-hi);
}

/* Brand strip label — Inter, 20px, wide letter-spacing, uppercase */
.brand-strip-label {
  font-family: 'Inter', sans-serif;
  font-size: 20px;
  font-weight: 700;
  letter-spacing: 2.2px;
  text-transform: uppercase;
  color: var(--stone);
}
```

### Stat pill component

```css
.stat-pill {
  display: inline-flex;
  align-items: center;
  gap: 14px;
  padding: 18px 22px;
  border: 1.5px solid var(--accent);
  border-radius: 6px;
  background: var(--surface);
  justify-content: flex-start;
  white-space: nowrap;
}
```

**Critical:** use `background: var(--surface)` (pure dark), NOT `background: rgba(accent, 0.1)`. Tinted-accent backgrounds destroy contrast for the accent-colored number inside.

### Background glows (optional)

```css
.banner::before {
  content: '';
  position: absolute;
  top: -200px;
  right: -150px;
  width: 750px;
  height: 750px;
  background: radial-gradient(circle, var(--accent) 0%, transparent 65%);
  opacity: 0.18;
  pointer-events: none;
  z-index: 0;
}
.banner::after {
  content: '';
  position: absolute;
  bottom: -220px;
  right: -120px;
  width: 600px;
  height: 600px;
  background: radial-gradient(circle, var(--accent) 0%, transparent 65%);
  opacity: 0.10;
  pointer-events: none;
  z-index: 0;
}
```

### Brand logo strip

```css
.brand-strip {
  display: flex;
  align-items: center;
  gap: 44px;
  width: 100%;
  border-top: 1px solid var(--border);
  padding-top: 14px;
}

.brand-logo {
  width: auto;
  filter: brightness(0) invert(1);  /* force to white silhouette */
  opacity: 0.78;
}

/* Logo-specific heights — these are the validated sizes for LinkedIn display scale */
.brand-logo[data-brand="amazon"]    { height: 42px; }
.brand-logo[data-brand="netflix"]   { height: 40px; }
.brand-logo[data-brand="dell"]      { height: 48px; }
.brand-logo[data-brand="ebay"]      { height: 44px; }  /* bumped — wordmarks need more */
.brand-logo[data-brand="adobe"]     { height: 44px; }
.brand-logo[data-brand="microsoft"] { height: 32px; }
.brand-logo[data-brand="nike"]      { height: 50px; }  /* bumped — swoosh has padding */
.brand-logo[data-brand="x"]         { height: 38px; }
```

---

## 6. Starter Templates (3 variants)

Three complete HTML+CSS templates, each a self-contained file that can be copied and customized. All three use the same design system, different layouts.

### Template 1: Numbers Grid

**Best for:** high-credibility operators with 4+ strong proof stats, media-kit aesthetic.

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>LinkedIn Banner — Numbers Grid</title>
<style>
@import url('https://fonts.googleapis.com/css2?family=DM+Serif+Display:ital@0;1&family=Inter:wght@400;500;600;700&display=swap');

:root {
  --bg: #0f0d0b;
  --text: #fdf7f2;
  --accent: #b94c4c;
  --accent-hi: #d05c5c;
  --muted: #a89a92;
  --border: #2a2320;
  --surface: #161210;
  --stone: #d5d0c8;
  --cream: #eae6df;
}

*, *::before, *::after { margin: 0; padding: 0; box-sizing: border-box; }

body {
  background: #2a2320;
  font-family: 'Inter', sans-serif;
  -webkit-font-smoothing: antialiased;
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  padding: 40px;
}

.banner {
  width: 1584px;
  height: 396px;
  background: var(--bg);
  position: relative;
  overflow: hidden;
  display: grid;
  grid-template-rows: auto 1fr auto auto;
  padding: 28px 80px 26px 80px;
}

.banner::before {
  content: '';
  position: absolute;
  top: -180px;
  right: -120px;
  width: 700px;
  height: 700px;
  background: radial-gradient(circle, var(--accent) 0%, transparent 65%);
  opacity: 0.16;
  pointer-events: none;
  z-index: 0;
}

.banner > * { position: relative; z-index: 1; }

/* Header row: eyebrow + optional monogram */
.banner-header {
  display: flex;
  align-items: center;
  gap: 16px;
  margin-bottom: 6px;
  padding-left: 280px;  /* photo-safe top */
}

.banner-eyebrow {
  font-family: 'Inter', sans-serif;
  font-size: 16px;
  font-weight: 700;
  letter-spacing: 2.8px;
  text-transform: uppercase;
  color: var(--accent);
}

/* 4 stats in a row */
.banner-stats {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  align-items: center;
  padding: 0 80px;
  flex: 1;
}

.stat-block {
  text-align: center;
  padding: 0 24px;
  border-right: 1px solid var(--border);
}
.stat-block:last-child { border-right: none; }

.stat-number {
  font-family: 'DM Serif Display', serif;
  font-size: 68px;
  line-height: 1;
  color: var(--accent-hi);
  text-shadow: 0 0 80px rgba(185, 76, 76, 0.45);
  margin-bottom: 10px;
}

.stat-label {
  font-family: 'Inter', sans-serif;
  font-size: 13px;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 1.8px;
  color: var(--stone);
  line-height: 1.4;
}

/* Tagline below stats */
.banner-tagline {
  font-family: 'DM Serif Display', serif;
  font-size: 28px;
  text-align: center;
  color: var(--cream);
  margin: 14px 0;
  padding-left: 240px;  /* still photo-safe */
}

.banner-tagline .accent {
  color: var(--accent);
  font-style: italic;
}

/* Brand strip (two lines: label + logos) */
.bottom-block {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  gap: 10px;
  padding-left: 400px;  /* photo-safe bottom */
}

.brand-strip-label {
  font-family: 'Inter', sans-serif;
  font-size: 18px;
  font-weight: 700;
  letter-spacing: 2.2px;
  text-transform: uppercase;
  color: var(--stone);
}

.brand-strip {
  display: flex;
  align-items: center;
  gap: 44px;
  width: 100%;
  border-top: 1px solid var(--border);
  padding-top: 12px;
}

.brand-logo {
  width: auto;
  filter: brightness(0) invert(1);
  opacity: 0.78;
}
.brand-logo[data-brand="amazon"] { height: 42px; }
.brand-logo[data-brand="netflix"] { height: 40px; }
.brand-logo[data-brand="dell"] { height: 48px; }
.brand-logo[data-brand="ebay"] { height: 44px; }
.brand-logo[data-brand="adobe"] { height: 44px; }
.brand-logo[data-brand="microsoft"] { height: 32px; }
.brand-logo[data-brand="nike"] { height: 50px; }
.brand-logo[data-brand="x"] { height: 38px; }
</style>
</head>
<body>
<div class="banner">
  <div class="banner-header">
    <span class="banner-eyebrow">{USER NAME · CREDENTIAL}</span>
  </div>

  <div class="banner-stats">
    <div class="stat-block">
      <div class="stat-number">{STAT 1}</div>
      <div class="stat-label">{LABEL<br>LINE 1}</div>
    </div>
    <div class="stat-block">
      <div class="stat-number">{STAT 2}</div>
      <div class="stat-label">{LABEL<br>LINE 2}</div>
    </div>
    <div class="stat-block">
      <div class="stat-number">{STAT 3}</div>
      <div class="stat-label">{LABEL<br>LINE 3}</div>
    </div>
    <div class="stat-block">
      <div class="stat-number">{STAT 4}</div>
      <div class="stat-label">{LABEL<br>LINE 4}</div>
    </div>
  </div>

  <div class="banner-tagline">
    {HEADLINE PART 1} <span class="accent">{accent phrase}</span> {HEADLINE PART 2}
  </div>

  <div class="bottom-block">
    <span class="brand-strip-label">{OWNERSHIP CLAIM LABEL}</span>
    <div class="brand-strip">
      <img src="logos/amazon.svg" class="brand-logo" data-brand="amazon" alt="Amazon">
      <img src="logos/netflix.svg" class="brand-logo" data-brand="netflix" alt="Netflix">
      <img src="logos/dell.svg" class="brand-logo" data-brand="dell" alt="Dell">
      <img src="logos/adobe.svg" class="brand-logo" data-brand="adobe" alt="Adobe">
      <img src="logos/microsoft.svg" class="brand-logo" data-brand="microsoft" alt="Microsoft">
      <img src="logos/nike.svg" class="brand-logo" data-brand="nike" alt="Nike">
      <img src="logos/x.svg" class="brand-logo" data-brand="x" alt="X">
    </div>
  </div>
</div>
</body>
</html>
```

### Template 2: Story Arc

**Best for:** narrative-forward positioning, journey/trajectory stories, founders with a "from-to" story.

Same CSS as Template 1 — just swap the middle section:

```html
<!-- Replace .banner-stats and .banner-tagline with: -->

<div class="story-arc" style="
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: flex-start;
  gap: 8px;
  padding-left: 360px;  /* photo-safe, slightly more conservative */
">
  <div style="
    font-family: 'DM Serif Display', serif;
    font-size: 40px;
    line-height: 1.15;
    color: var(--cream);
  ">{PAST — e.g., "From 4 engineers to a $16M ARR SaaS portfolio."}</div>

  <div style="
    font-family: 'DM Serif Display', serif;
    font-size: 28px;
    font-style: italic;
    line-height: 1.15;
    color: var(--accent);
  ">{TRANSITION — e.g., "From 200K to 1M+ YouTube subs for Neil Patel."}</div>

  <div style="
    font-family: 'Inter', sans-serif;
    font-size: 18px;
    font-weight: 500;
    color: var(--stone);
    margin-top: 6px;
  ">{PRESENT — e.g., "Now installing <strong>audience-led growth engines</strong> for founder-led brands."}</div>
</div>
```

### Template 3: Stacked Pills (recommended default)

**Best for:** most users. Balances headline + proof without either dominating. This was the final chosen layout in the reference iteration this skill was extracted from.

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>LinkedIn Banner — Stacked Pills</title>
<style>
@import url('https://fonts.googleapis.com/css2?family=DM+Serif+Display:ital@0;1&family=Inter:wght@400;500;600;700&display=swap');

:root {
  --bg: #0f0d0b;
  --text: #fdf7f2;
  --accent: #b94c4c;
  --accent-hi: #d05c5c;
  --muted: #a89a92;
  --border: #2a2320;
  --surface: #161210;
  --stone: #d5d0c8;
  --cream: #eae6df;
}

*, *::before, *::after { margin: 0; padding: 0; box-sizing: border-box; }

body {
  background: #2a2320;
  font-family: 'Inter', sans-serif;
  -webkit-font-smoothing: antialiased;
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  padding: 40px;
}

.banner {
  width: 1584px;
  height: 396px;
  background: var(--bg);
  position: relative;
  overflow: hidden;
  display: grid;
  grid-template-columns: minmax(0, 1fr) auto;
  grid-template-rows: minmax(0, 1fr) auto;
  grid-template-areas:
    "hero  stats"
    "brand brand";
  padding: 36px 60px 26px 60px;
  column-gap: 32px;
  row-gap: 18px;
}

.banner::before {
  content: '';
  position: absolute;
  top: -200px;
  right: -150px;
  width: 750px;
  height: 750px;
  background: radial-gradient(circle, var(--accent) 0%, transparent 65%);
  opacity: 0.18;
  pointer-events: none;
  z-index: 0;
}

.banner::after {
  content: '';
  position: absolute;
  bottom: -220px;
  right: -120px;
  width: 600px;
  height: 600px;
  background: radial-gradient(circle, var(--accent) 0%, transparent 65%);
  opacity: 0.12;
  pointer-events: none;
  z-index: 0;
}

.banner > * { position: relative; z-index: 1; }

/* Hero — headline on the left with photo-safe indent */
.hero {
  grid-area: hero;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding-left: 240px;
  min-width: 0;
}

.hero-headline {
  font-family: 'DM Serif Display', serif;
  font-size: 52px;
  line-height: 1.1;
  color: var(--cream);
}

.hero-headline .accent {
  color: var(--accent);
  font-style: italic;
}

/* Stats column — 3 pills stacked, centered vertically */
.stats-column {
  grid-area: stats;
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 14px;
  align-items: stretch;
  width: 300px;
}

.stat-pill {
  display: inline-flex;
  align-items: center;
  gap: 14px;
  padding: 18px 22px;
  border: 1.5px solid var(--accent);
  border-radius: 6px;
  font-family: 'Inter', sans-serif;
  font-size: 16px;
  font-weight: 700;
  letter-spacing: 2.2px;
  text-transform: uppercase;
  color: var(--cream);
  background: var(--surface);
  justify-content: flex-start;
  white-space: nowrap;
}

.stat-pill .stat-pill-num {
  color: var(--accent-hi);
  font-family: 'DM Serif Display', serif;
  font-size: 32px;
  letter-spacing: 0;
  text-transform: none;
  min-width: 86px;
  line-height: 1;
}

/* Bottom block: label OUTSIDE strip + strip with border-top */
.bottom-block {
  grid-area: brand;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  gap: 12px;
  padding-left: 400px;  /* photo-safe bottom (widest profile photo zone) */
}

.brand-strip-label {
  font-family: 'Inter', sans-serif;
  font-size: 20px;
  font-weight: 700;
  letter-spacing: 2.2px;
  text-transform: uppercase;
  color: var(--stone);
  white-space: nowrap;
}

.brand-strip {
  display: flex;
  align-items: center;
  gap: 44px;
  width: 100%;
  border-top: 1px solid var(--border);
  padding-top: 14px;
}

.brand-logo {
  width: auto;
  filter: brightness(0) invert(1);
  opacity: 0.82;
}
.brand-logo[data-brand="amazon"]    { height: 42px; }
.brand-logo[data-brand="netflix"]   { height: 40px; }
.brand-logo[data-brand="dell"]      { height: 48px; }
.brand-logo[data-brand="ebay"]      { height: 44px; }
.brand-logo[data-brand="adobe"]     { height: 44px; }
.brand-logo[data-brand="microsoft"] { height: 32px; }
.brand-logo[data-brand="nike"]      { height: 50px; }
.brand-logo[data-brand="x"]         { height: 38px; }
</style>
</head>
<body>
<div class="banner">
  <div class="hero">
    <div class="hero-headline">{HEADLINE PART 1} <span class="accent">{accent phrase}</span><br>{HEADLINE PART 2}</div>
  </div>

  <div class="stats-column">
    <span class="stat-pill"><span class="stat-pill-num">{NUM1}</span> {LABEL1}</span>
    <span class="stat-pill"><span class="stat-pill-num">{NUM2}</span> {LABEL2}</span>
    <span class="stat-pill"><span class="stat-pill-num">{NUM3}</span> {LABEL3}</span>
  </div>

  <div class="bottom-block">
    <span class="brand-strip-label">{OWNERSHIP CLAIM LABEL}</span>
    <div class="brand-strip">
      <img src="logos/amazon.svg" class="brand-logo" data-brand="amazon" alt="Amazon">
      <img src="logos/netflix.svg" class="brand-logo" data-brand="netflix" alt="Netflix">
      <img src="logos/dell.svg" class="brand-logo" data-brand="dell" alt="Dell">
      <img src="logos/ebay.svg" class="brand-logo" data-brand="ebay" alt="eBay">
      <img src="logos/adobe.svg" class="brand-logo" data-brand="adobe" alt="Adobe">
      <img src="logos/microsoft.svg" class="brand-logo" data-brand="microsoft" alt="Microsoft">
      <img src="logos/nike.svg" class="brand-logo" data-brand="nike" alt="Nike">
      <img src="logos/x.svg" class="brand-logo" data-brand="x" alt="X">
    </div>
  </div>
</div>
</body>
</html>
```

**Placeholder substitution — replace these when customizing:**

- `{HEADLINE PART 1}` — beginning of positioning sentence
- `{accent phrase}` — key phrase to emphasize in italic terracotta
- `{HEADLINE PART 2}` — rest of positioning sentence (optional, can be on same line or after `<br>`)
- `{NUM1}`, `{NUM2}`, `{NUM3}` — three hero stat numbers (e.g., `$80M+`, `1M+`, `4M+`)
- `{LABEL1}`, `{LABEL2}`, `{LABEL3}` — corresponding stat labels (e.g., `Generated`, `YouTube Subs`, `Users Reached`)
- `{OWNERSHIP CLAIM LABEL}` — the brand strip label (e.g., "I built products used by teams at")
- Brand logo image tags — remove or add to match the user's customer brands

---

## 7. Rendering Pipeline (exact commands)

Reusable code blocks for the full render cycle. Substitute `{workspace}` with the user's actual workspace path.

### Start local server (required before Playwright)

```bash
cd "{workspace}/topfold/" && python3 -m http.server 8879 > /tmp/banner-server.log 2>&1 &
sleep 1
curl -s -o /dev/null -w "test: %{http_code}\n" "http://localhost:8879/"
```

### Navigate and wait for ready state (Playwright MCP)

After calling `mcp__plugin_playwright_playwright__browser_navigate` to the banner URL, run:

```javascript
async () => {
  await document.fonts.ready;
  await new Promise(r => setTimeout(r, 800));
  document.body.style.padding = '0';
  document.body.style.margin = '0';
  document.body.style.minHeight = 'auto';
  document.body.style.display = 'block';
  return 'ready';
}
```

### Resize viewport to exact banner dimensions

Use `mcp__plugin_playwright_playwright__browser_resize`:
- `width: 1584`
- `height: 396`

### Take viewport screenshot

Use `mcp__plugin_playwright_playwright__browser_take_screenshot`:
- `filename: "{workspace}/topfold/YYYY-MM-DD_v1-layout.png"`
- `type: "png"`

### Export JPG with sRGB profile

```bash
cd "{workspace}/topfold/" && python3 -c "
from PIL import Image, ImageCms
img = Image.open('YYYY-MM-DD_v1-layout.png').convert('RGB')
srgb = ImageCms.createProfile('sRGB')
srgb_bytes = ImageCms.ImageCmsProfile(srgb).tobytes()
img.save('YYYY-MM-DD_v1-layout.jpg', 'JPEG', quality=95, optimize=True, icc_profile=srgb_bytes)
print('exported jpg')
"
```

### Stop server (always, to avoid port conflicts)

```bash
pkill -f "python3 -m http.server 8879" 2>/dev/null
```

### Validation

```bash
sips -g pixelWidth -g pixelHeight "YYYY-MM-DD_v1-layout.png" | tail -2
ls -lh "YYYY-MM-DD_v1-layout.jpg"
file "YYYY-MM-DD_v1-layout.jpg"
```

Should print:
- Width 1584, height 396
- File size under 2 MB
- `JPEG image data, JFIF standard 1.01, ..., 1584x396`

---

## 8. Asset Sourcing Cheat Sheet

Pre-validated URLs for the most common customer brand logos users will want. First column uses Simple Icons CDN; second column uses Wikimedia Commons for trademark-protected brands Simple Icons refuses.

### Simple Icons CDN (primary)

Format: `https://cdn.simpleicons.org/{slug}/{hex-no-hash}`

| Brand | Slug | Example URL (cream color) |
|---|---|---|
| Netflix | `netflix` | `https://cdn.simpleicons.org/netflix/fdf7f2` |
| Dell | `dell` | `https://cdn.simpleicons.org/dell/fdf7f2` |
| eBay | `ebay` | `https://cdn.simpleicons.org/ebay/fdf7f2` |
| Nike | `nike` | `https://cdn.simpleicons.org/nike/fdf7f2` |
| X (Twitter) | `x` | `https://cdn.simpleicons.org/x/fdf7f2` |
| Meta | `meta` | `https://cdn.simpleicons.org/meta/fdf7f2` |
| Spotify | `spotify` | `https://cdn.simpleicons.org/spotify/fdf7f2` |
| Slack | `slack` | `https://cdn.simpleicons.org/slack/fdf7f2` |
| GitHub | `github` | `https://cdn.simpleicons.org/github/fdf7f2` |
| Airbnb | `airbnb` | `https://cdn.simpleicons.org/airbnb/fdf7f2` |
| Uber | `uber` | `https://cdn.simpleicons.org/uber/fdf7f2` |
| Shopify | `shopify` | `https://cdn.simpleicons.org/shopify/fdf7f2` |
| HubSpot | `hubspot` | `https://cdn.simpleicons.org/hubspot/fdf7f2` |
| Stripe | `stripe` | `https://cdn.simpleicons.org/stripe/fdf7f2` |
| Notion | `notion` | `https://cdn.simpleicons.org/notion/fdf7f2` |
| Figma | `figma` | `https://cdn.simpleicons.org/figma/fdf7f2` |
| LinkedIn | `linkedin` | `https://cdn.simpleicons.org/linkedin/fdf7f2` |
| Zoom | `zoom` | `https://cdn.simpleicons.org/zoom/fdf7f2` |

Substitute the hex color in the URL to match your brand. The `fdf7f2` in these examples is a cream color that looks good on dark backgrounds.

### Wikimedia Commons fallbacks (for trademark-protected brands)

These brands are NOT served by Simple Icons. Use Wikimedia Commons instead.

| Brand | Wikimedia URL |
|---|---|
| Amazon | `https://upload.wikimedia.org/wikipedia/commons/a/a9/Amazon_logo.svg` |
| Adobe | `https://upload.wikimedia.org/wikipedia/commons/7/7b/Adobe_Systems_logo_and_wordmark.svg` |
| Microsoft | `https://upload.wikimedia.org/wikipedia/commons/9/96/Microsoft_logo_%282012%29.svg` |
| Google | `https://upload.wikimedia.org/wikipedia/commons/2/2f/Google_2015_logo.svg` |
| Apple | `https://upload.wikimedia.org/wikipedia/commons/f/fa/Apple_logo_black.svg` |
| Coca-Cola | `https://upload.wikimedia.org/wikipedia/commons/c/ce/Coca-Cola_logo.svg` |
| Tesla | `https://upload.wikimedia.org/wikipedia/commons/b/bd/Tesla_Motors.svg` |
| Samsung | `https://upload.wikimedia.org/wikipedia/commons/2/24/Samsung_Logo.svg` |
| IBM | `https://upload.wikimedia.org/wikipedia/commons/5/51/IBM_logo.svg` |
| Oracle | `https://upload.wikimedia.org/wikipedia/commons/5/50/Oracle_logo.svg` |

Wikimedia SVGs come in their original brand colors. Apply `filter: brightness(0) invert(1)` in CSS to convert to white silhouette, or use Python + Pillow to recolor.

### Download helper

```bash
# Batch download Simple Icons logos
cd "{workspace}/topfold/logos" && for brand in netflix dell ebay nike x; do
  curl -sL "https://cdn.simpleicons.org/${brand}/fdf7f2" -o "${brand}.svg"
done

# Download Wikimedia fallbacks (use Mozilla user-agent to avoid blocks)
curl -sL -A "Mozilla/5.0" "https://upload.wikimedia.org/wikipedia/commons/a/a9/Amazon_logo.svg" -o amazon.svg
curl -sL -A "Mozilla/5.0" "https://upload.wikimedia.org/wikipedia/commons/9/96/Microsoft_logo_%282012%29.svg" -o microsoft.svg
curl -sL -A "Mozilla/5.0" "https://upload.wikimedia.org/wikipedia/commons/7/7b/Adobe_Systems_logo_and_wordmark.svg" -o adobe.svg
```

### Background transparency / cropping

If a downloaded logo has a white background or lots of empty space, clean it up:

```bash
python3 << 'EOF'
from PIL import Image
# Strip white background
img = Image.open('logo-raw.png').convert('RGBA')
data = img.getdata()
new_data = [(255, 255, 255, 0) if all(c > 240 for c in item[:3]) else item for item in data]
img.putdata(new_data)
# Crop to content bounding box
img = img.crop(img.getbbox())
img.save('logo-clean.png', 'PNG')
EOF
```

---

## 9. Output Convention

All skill outputs go to `{user-workspace}/topfold/`:

```
topfold/
├── proof-stack.md                         ← Phase 1 deliverable
├── copy-brief.md                          ← Phase 2 deliverable
├── logos/                                 ← Phase 4a sourced assets
│   ├── amazon.svg
│   ├── netflix.svg
│   ├── dell.svg
│   ├── ebay.svg
│   ├── adobe.svg
│   ├── microsoft.svg
│   ├── nike.svg
│   ├── x.svg
│   └── {user}-monogram.png
├── YYYY-MM-DD_v1-numbers-grid.html        ← Phase 4b source
├── YYYY-MM-DD_v1-numbers-grid.png         ← Phase 4c PNG render
├── YYYY-MM-DD_v1-numbers-grid.jpg         ← Phase 4c JPG export (sRGB)
├── YYYY-MM-DD_v2-story-arc.html           (optional variants)
├── YYYY-MM-DD_v3-stacked-pills.html
└── ...
```

**Naming convention for iterations:** `YYYY-MM-DD_v{N}{letter}-{layout}.{ext}`. Use `v1`, `v2`, etc. for major versions and `v1a`, `v1b`, `v1c` for minor refinements within the same version. Example:

- `2026-04-08_v1-numbers-grid.html` — initial v1
- `2026-04-08_v1b-numbers-grid.html` — legibility pass on v1
- `2026-04-08_v2-stacked-pills.html` — switched to different layout
- `2026-04-08_v2c-stacked-pills.html` — final polish

Keep ALL iterations in the folder. Don't delete old versions — they're the record of the iteration process and occasionally contain ideas worth reviving.

---

## 10. Integration Notes

### Standalone — does not depend on

This skill is intentionally self-contained. It does NOT require:

- `brand-deck` — but WILL use user's brand palette if they provide one
- `linkedin-infographic` — different output format (1080×1350 portrait), different dimensions, different use case
- `linkedin-carousel` — different output format (multi-slide PDF)
- Any specific brand system — the default palette works for anyone

### Can coordinate with (if available in the user's setup)

- **`identity-loader`** — if the user has identity files (biography, voice-profile, achievements), Phase 1 proof mining can load these automatically instead of asking the 6 questions. Check for `memory/identity/biography.md` or equivalent.
- **`brand-voice`** — if the user has a brand voice guide, consult it when proposing copy options in Phase 2.

### Does NOT coordinate with

- `linkedin-post`, `linkedin-carousel`, `linkedin-infographic` — different content types, different pipelines.

---

## 11. Known Issues + Troubleshooting

Every item in this table corresponds to a real problem encountered during the reference iteration this skill was built from. When a user reports something similar, refer to the matching fix.

| Symptom | Likely cause | Fix |
|---|---|---|
| **LinkedIn "Save failed" on upload** | Usually a session/browser issue, not the file | Try mobile app first; then incognito window; then different browser; then wait 5–10 min; finally run diagnostic test |
| **PNG fails but JPG works** | LinkedIn's uploader has inconsistent PNG handling | Always export JPG as primary, PNG as backup |
| **Content covered by profile photo** | Photo-safe zone too small for the vertical position | Use vertically-aware indents: 240 top, 340 middle, 400 bottom |
| **Text illegible at display scale** | Designed for native, not 50% display | Bump all fonts ~50% larger than felt right originally |
| **Small icon logos (Nike swoosh, X)** | SVG viewBox has padding; logo fills only 30-40% of box | Bump height to 50px to compensate — visible content ends up ~17px tall |
| **Wordmark logos (eBay, Dell, Microsoft) look small** | Wider than tall, height-based sizing makes them narrow visually | Bump to 44–48px height |
| **Multi-color logos lose detail after filter** | `filter: brightness(0) invert(1)` turns everything into white silhouette | Accept the silhouette — colored brand logos usually look cluttered at small scale anyway. Or source single-color variants. |
| **Pill numbers low contrast** | Accent color on tinted-accent background | Change pill background to `var(--surface)` (pure dark), change number color to `var(--accent-hi)` (lighter), bump to 32px |
| **Content stretches past banner edges** | Grid `1fr` allows content to push columns wider than intended | Use `grid-template-columns: minmax(0, 1fr) auto` (same for rows) |
| **Pills not vertically centered** | `justify-content: space-between` stretches them to edges instead of grouping | Use `justify-content: center` with `gap: 14px` |
| **Headline wraps awkwardly** | Long headline at large font hits column boundary | Reduce font size OR reduce hero padding-left OR increase column-gap trade-off |
| **Banner feels cramped** | Too many elements competing | Remove whichever element least clearly earns its space (usually eyebrow or CTA) |
| **Customer brand logos look inconsistent** | Mixing real logos with text wordmarks | Commit to one approach consistently — all real logos (Wikimedia fallback for trademarked) or all text wordmarks |
| **First render has too many bugs** | Rushing to full content without a base template check | Render a simplified test version first (just the banner box + placeholder text), verify dimensions + fonts load, then add content |
| **Upload takes forever** | File size too large (over 2 MB) | Reduce JPG quality from 95 to 85 or 90, re-export |
| **Banner aesthetic doesn't match user's brand** | Skipped brand mining or used qualitative description instead of pixel sampling | Re-do Phase 3b in Mode B: fetch the user's website via WebFetch, extract CSS custom properties + hex codes, download brand imagery, sample actual pixels via Python + Pillow, identify typography from logo, show final token map BEFORE rebuilding HTML |
| **Colors are "close but wrong"** | Guessed colors from screenshots or verbal descriptions | Never guess. Sample pixels from real brand assets. Use CSS custom properties as anchors and pixel samples as highlights. |
| **Typography fights the user's existing brand** | Used DM Serif Display default when user's logo is condensed geometric sans | Match typography to logo style, not skill defaults. Archivo Narrow for condensed, Space Grotesk for modern geometric, Playfair for elegant serif, etc. |

---

## 12. Critical Lessons (do not lose these)

These are the 10 most important principles from the reference iteration. If the skill is ever trimmed, these stay.

1. **LinkedIn displays banners at ~50% scale.** Design 2x bigger than feels right. Single biggest cause of "looks too small" failures.

2. **Profile photo circle is WIDER at the bottom of the banner than at the top.** Photo-safe indent is vertically-position-aware: 240px top, 340px middle, 400px bottom. Not a single constant.

3. **Customer brand strip > product logo strip.** "I built products used by teams at [Amazon/Netflix/etc.]" is 10x stronger than listing your own product logos. Shifts proof from "these are my things" to "household names trusted what I built."

4. **Content Chain rule: one element = one job.** Don't mix positioning, proof, ownership, and social proof in the same element. Each element claims different territory.

5. **Upload protocol: mobile first, JPG first, incognito second, different browser third.** This sequence fails less than anything else.

6. **sRGB profile embedding is trivial (PIL + ImageCms) and prevents color shifts.** Always do it on JPG export.

7. **Iteration is the norm — expect 4–6 rounds.** First render is never the final. Set user expectations up front. The reference iteration went v1 → v4-final across 6 rounds.

8. **Dell, eBay, Nike, Microsoft logos always look small.** Bump them 30–50% larger than other logos to balance visual weight.

9. **Pills on dark surface background with brighter accent number > pills on tinted-accent background with muted accent number.** This is a contrast issue, not a color preference — the tinted background kills the accent's readability.

10. **The diagnostic test (solid-color PNG) isolates file vs session issues on upload failures.** When "Save failed" appears twice, generate the test image and try to upload that. If it fails, it's LinkedIn. If it succeeds, it's your banner file.

11. **Never guess brand colors from qualitative descriptions.** "Dark navy with red accents" will produce hex codes that are close but wrong, and wrong-hex-codes read as fake. Always extract actual pixel values — either from the user directly (Mode A), from their live website via WebFetch + Python pixel sampling (Mode B), or use the honest default palette (Mode C). See Section 4 Phase 3b — Brand mining for the full protocol. This applies to TYPOGRAPHY too: if a user's logo uses condensed geometric sans, don't default to DM Serif Display just because it's the skill's default.

12. **Every render gets a design critique before delivery. No exceptions.** After rendering a PNG, load it via the `Read` tool and run the 10-point visual critique from Phase 4e. Technical validation (dimensions, file size, sRGB profile) does NOT catch visual bugs like stretched logos, cramped layouts, or color clashes — only visual inspection does. The single biggest cause of low-quality deliverables is skipping this step because "it probably looks fine from the code." It doesn't. View the image. Run the checklist. Then show the user.

---

## 13. Quick Reference — Full Session Flow

A condensed version of the entire workflow for a user who's already familiar with the skill and just wants the happy path:

```
Phase 1 — Proof Mining (30 min)
  Ask 6 questions → organize into proof stack → save proof-stack.md

Phase 2 — Copy Architecture (15 min)
  Map elements to jobs → propose copy options → lock copy brief → save copy-brief.md

Phase 3 — Visual Design (30 min)
  3a. Reference gathering (5–8 banners)
  3b. Brand MINING (Mode A: user provides / Mode B: WebFetch + pixel sample / Mode C: defaults)
      → produce token map, SHOW USER before moving on
  3c. Layout pick (Numbers Grid / Story Arc / Stacked Pills)

Phase 4 — Build & Render (30 min per variant)
  4a. Source logos (Simple Icons + Wikimedia)
  4b. Generate HTML from template + copy brief + brand tokens
  4c. Playwright render:
      - Start server (python3 -m http.server 8879)
      - Navigate + wait for fonts
      - Resize viewport 1584×396
      - Screenshot viewport → PNG
      - PIL export JPG with sRGB
      - Stop server
  4d. Technical self-check (dimensions, file size, aspect ratios, sRGB)
  4e. DESIGN CRITIQUE — view PNG via Read tool, run 10-point checklist,
      fix any FAIL/WARN, repeat until 10/10. NEVER SKIP THIS STEP.

Phase 5 — Test & Ship (15 min + iteration)
  5a. Upload via mobile app first
  5b. If fails, diagnostic test (solid-color PNG)
  5c. Once live, critique 6 checkpoints (legibility / photo overlap / content chain / logo readability / contrast / balance)
  5d. Iterate on trigger fixes from the known-issues table
```

Total time from zero to shipped: ~2 hours for a single variant, 3-4 hours across multiple iterations if the user wants to refine hard.
