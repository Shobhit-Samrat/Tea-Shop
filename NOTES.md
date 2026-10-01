# NOTES: Mistvale store rescue

> Items marked **[FILL IN]** are things only you can answer truthfully (your own testing, your time). Complete or delete them before you submit. Do not leave them as they are.

## 1. What I changed

**Bugs and business rules**
- First visit crashed because `localStorage` returned `null`. The cart is now loaded safely and rebuilt from `PRODUCTS`; unknown, sold-out or over-limit lines are dropped.
- Cart stores only `id` and `qty`. Prices always come from `PRODUCTS` (R1). Before, the price was read from page text, so `₹1,299` became `1`.
- Product ids were compared as string vs number, so one tea could create two cart lines. Fixed.
- Max 5 per tea and never above stock (R2). `+` used string concatenation (`"1"+1 = "11"`), `-` could reach zero or negative, and the typed quantity was ignored. All fixed.
- `splice(i)` removed every line after the clicked one. Now removes one line.
- Cart click handlers were added again on every render, so clicks multiplied. Now one delegated listener.
- Coupon WELCOME10 (R3): case-insensitive, 10% of eligible items only (Gifts excluded), capped at ₹150, needs a ₹399 subtotal, and applying it twice changes nothing. The discount is recalculated on every render, not stored.
- Shipping ₹49, free when the amount after discount reaches the threshold (R4). Only the final total is rounded; all amounts use ₹ with Indian grouping (R5).
- Sold-out teas cannot be added and always sort last, in every sort order (R6).
- Search, category and sort share one state (R7). The old category filter did not match `Green`, ignored "All", and ignored search and sort. The old search filter was inverted (`indexOf` result used as a boolean). Sorting now never changes `PRODUCTS`.
- Search race: the API answers short queries slower than long ones, so an old reply could overwrite newer results. Search is debounced and replies from older keystrokes are ignored.
- Pincode check uses `API.checkPincode()`, shows days, "not serviceable" or a clear invalid-pincode message, has an 8-second timeout, and cannot stay on "Checking…" (R8).
- Replaced `innerHTML` with `textContent` where user text is shown (pincode, email), which removes an XSS risk.
- Quick view opened the wrong product (loop variable bug). The wishlist heart toggled every card. The cart badge counted lines, not items. All fixed.
- Checkout fills `items` (`[{"id":101,"qty":2}]`) and `coupon`, then submits the unchanged `#checkout-form`.

**Design (BRAND.md)**
- Seven brand colours as CSS variables only; Fraunces and Inter in one Google Fonts link with `display=swap`; 8px spacing scale; one 8px radius; pills only on chips and badges.
- Buttons have hover, focus-visible, active and disabled states. Transitions are 200ms on named properties only. No marquee, blinking or bouncing. `prefers-reduced-motion` is respected.
- Sections are in the required order: announcement, header, hero, trust strip, shop, delivery check, reviews, FAQ, newsletter, footer.
- FAQ uses the six approved answers word for word, as an accordion built on `<details>`, with matching FAQPage JSON-LD.
- Newsletter is an inline form with a visible label and inline error and success messages. The auto-opening pop-up is gone.

**UX**
- "Add to cart" is always visible (it used to appear only on hover, which is invisible on phones). A toast confirms each add.
- Cart drawer with quantity stepper, remove, free-shipping meter, coupon message and full totals.
- Quick view, saved wishlist (browser only), "Clear search and filters" empty state, header search button, skip link.

**Images:** see section 3. **Technical:** see below.
- Removed jQuery, animate.css and Font Awesome. No external libraries. Icons are inline SVG.
- Viewport meta, `lang`, responsive from 360px, native `<dialog>` for cart and quick view (focus trap and Esc), `aria-live` regions, visible labels, alt text, lazy-loaded images with width and height.
- SEO: title, description, canonical, Open Graph, Twitter tags, JSON-LD for the store, the FAQ, and each product (price in INR, availability from stock, rating only on the two products that have one).

## 2. What the AI got wrong

- **Image script, shadow bug:** the first version used `ImageChops.offset`, which wraps pixels around the edge. It drew a grey bar at the top of the hero. I saw it in the preview and replaced it with a plain offset paste.
- **Image script, clipped leaves in the hero:** the tea-leaf scatter was drawn on a 1600px layer in a 2400px-wide hero, so it was cut off. Caught in the preview; the layer now matches the canvas.
- **Logo font:** the script picked the first serif font it found, which was a bold-italic file. I noticed the path in the output and switched to the upright bold file.
- **Packs too small** in the first render, so I enlarged them.
- **An automated edit broke indentation** and the script crashed with a syntax error. Running it caught that.
- **Lint warning:** the AI used `-webkit-line-clamp` without the standard `line-clamp`. VS Code flagged it and I fixed it.
- **Limit of the image tool:** it cannot run an image generator, so the images are code-drawn illustrations, not the photo-style images the brand guide describes.
- **Code was delivered without being run in a browser.** [FILL IN: bugs you found when you tested the AI's code, with how you caught them. If you found none, say what you tested so the claim is believable.]

## 3. Images

Made by Claude writing Python (Pillow). No image generator was used. All 8 product images are square 800×800, the hero is 1200×900, and all are exported as WebP with quality lowered until under the size limits. Final sizes: products 29 to 36 KB, hero 35 KB. The logo is an SVG with the text converted to outlines. No text appears in any image except the logo.

## 4. How I tested it

[FILL IN with what you really did. Examples to tick only if true:]
- Browsers: [e.g. Chrome version, Firefox, Safari]
- Phone sizes: [e.g. 360px and 390px in DevTools]
- Keyboard-only run through search, add to cart, cart, coupon and checkout: [yes / no]
- Cart cases: [Masala Chai plus `welcome10`; Sampler plus coupon; adding a sixth pack; Hibiscus Rose]
- Lighthouse scores: [numbers]

## 5. Questions for the team

1. **Free-shipping threshold:** the announcement said ₹499 and the code said ₹599. I used ₹599 (one constant, `FREE`). Which is right?
2. **Countdown and "Diwali sale":** the countdown date was already past and the sale is not in the facts. I removed both. Is there a real sale?
3. **Removed claims:** "As seen on Shark Tank India", "Rated 4.9/5 by 10,000+ customers" and "best tea in the world" had no source. Is any of them true?
4. **Masala Chai description says "Our bestseller".** I hide that phrase on screen only. Can it be proven?
5. **Footer copyright line:** I fixed "MistVale" and "All right reserved" but kept the year 2020, while the company was founded in 2019. I did not touch the approved legal paragraph. Is that okay?
6. **Customer quotes:** I used only the three given quotes. Are they real, with permission?
7. **Quick-view size selector (100g/250g)** was removed. Prices are per pack, so sizes would need real prices.
8. **Newsletter has no backend,** so the success message is simulated.
9. **Sampler stock is 9** but the cap is 5 per order, so 5 applies (R2).
10. **Delivery times:** the API returns days and the FAQ says working days. I show "about N days".
11. **If the cart changes after WELCOME10 is applied** and no longer qualifies, the code stays but the discount is 0 and `coupon` is sent empty at checkout.
12. **Teas come from Assam, Nilgiri and Kashmir,** while the brand story mentions the Darjeeling hills. Is that the whole story?

## 6. Time spent

[FILL IN: your honest estimate in hours]

## 7. Extra features I added

Saved wishlist (browser only), quick view, free-shipping progress meter, skip link, "Clear search and filters" state.

## 8. With more time I would

- Replace the illustrations with real photo-style images from an image generator.
- Add size options (once real prices exist), recently viewed, a product detail view, a gift message, and filters kept in the URL.
- Write automated tests for the cart and coupon maths and run Lighthouse and a screen-reader pass.
