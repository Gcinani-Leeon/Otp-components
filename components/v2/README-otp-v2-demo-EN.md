# Interactive OTP Component (Verification Demo)

## What is this?

This is a visual **OTP code verification** component (the typical 4-digit box that asks for a code sent via SMS), ready to drop into a landing page. It simulates the "verify your number" step used in signup flows, apps, checkouts, etc.

It's **100% interactive**: it's not a video or a recorded animation. The user taps the card, their phone's numeric keyboard opens, and they type a real code. The component responds based on what was typed:

- Typing **`1234`** → triggers the "successful verification" animation (the digits travel, form a green checkmark, and the "Verified and Secure" stamp appears).
- Typing any other 4-digit code → triggers a red error shake and lets you retry without reloading anything.

## What is it for?

Built for anyone who sells (or uses) **landing pages / conversion funnels** that need to show a believable "verification" step before a download or access — typical in app pages, promotions, digital licenses, etc. It gives the user the feeling of going through a real security process, increasing the perceived legitimacy of the page.

## How is it built?

- **A single `.html` file**, with no external dependencies (no libraries, no backend needed, no calls to any real server).
- All the code —HTML, CSS, and JavaScript— lives inside that single file. This is intentional: it opens in any browser with a double-click, can be uploaded to any hosting, and can be pasted directly inside an `<iframe>` on another page without breaking anything (it uses `sandbox` and doesn't collide with the rest of the site's styles/IDs).
- Uses **pure CSS** for the animations (transitions, keyframes) and a bit of **Canvas** for the particle effect on successful verification.
- The "correct code" (`1234`) is *hardcoded* in the JavaScript, as a demo — it doesn't validate anything against a real server.

## Keyboard behavior (UX)

- Tapping the card focuses an invisible numeric input, so on mobile the native keyboard opens.
- Correct code → the keyboard closes itself while the success animation starts.
- Incorrect code → the keyboard stays open, the box resets automatically, and you can type again without touching anything else.
- The **"Resend"** link manually resets the component at any time.

## How to use it

**Option 1 — Standalone:** open the `.html` file directly in the browser, or upload it to your hosting as just another page.

**Option 2 — Embedded in another page (recommended for funnels):** paste it inside an `<iframe>` with `sandbox="allow-scripts"`, setting its content via `srcdoc`. Since it's fully isolated (no shared IDs or classes), it won't clash with the rest of your landing page's CSS/JS.

## How to customize it

Everything is editable directly in the file, no special tools needed:

| What you want to change | Where it is |
|---|---|
| The "correct" code (`1234`) | Look for `val === '1234'` inside the `<script>` |
| Text (title, subtitle, error messages) | Look for `id="title"` and `id="subtitle"` in the HTML, and their equivalents in the JS (`title.textContent`, `subtitle.innerHTML`) |
| Colors | CSS variables at the top of the `<style>` (`--accent-orange`, `--accent-green`, `--accent-red`, etc.) |
| Animation timing | The `setTimeout(...)` calls inside `playSuccessAnimation()` and `triggerError()` |

## Important note

This component is a **visual simulation**, not a real verification system. It doesn't send SMS, doesn't validate against any backend, and the "correct code" is fixed in the source code. If real OTP verification is needed (with SMS/WhatsApp sending and server-side validation), it needs to be integrated with an external provider (Twilio, MercadoPago, etc.) — this file only handles the visual/UX part.
