# Interactive OTP Component (Verification Demo) — V1

## What is this?

This is a visual **OTP code verification** component (the typical 4-digit box that asks for a code sent via SMS), ready to drop into a landing page. It simulates the "verify your number" step used in signup flows, apps, checkouts, etc.

It's **100% interactive**: it's not a video or a recorded animation. The user taps the card, their phone's numeric keyboard opens, and they type a real code. The component responds based on what was typed:

- Typing **any 4-digit code that is NOT `0000`** → triggers the "successfully verified" animation (the digits travel into a grid, form a green checkmark with a particle effect, and the "Verified and Secure" stamp appears).
- Typing **`0000`** → triggers a red error shake and lets you retry without reloading anything.

> Note: in this version (V1) the logic is "everything works except the code 0000," the opposite of V2 where only `1234` is valid. This was kept intentionally, exactly as it was in the original file.

## What is it for?

Built for anyone who sells (or uses) **landing pages / conversion funnels** that need to show a believable "verification" step before a download or access — typical in app pages, promotions, digital licenses, etc. It gives the user the feeling of going through a real security process, increasing the perceived legitimacy of the page.

## How is it built?

- **A single `.html` file**, with no external dependencies (no libraries, no backend needed, no calls to any real server).
- All the code —HTML, CSS, and JavaScript— lives inside that single file. It opens in any browser with a double-click, can be uploaded to any hosting, and can be pasted directly inside an `<iframe>` on another page without breaking anything.
- Uses **pure CSS** for the animations (transitions, keyframes) and **Canvas** for the particle effect and the background dot texture on successful verification.
- Includes **haptic vibration** (`navigator.vibrate`) while typing, on error, and on success — this only has an effect on phones that support it, it does nothing on desktop.
- The code that triggers the error (`0000`) is *hardcoded* in the JavaScript, as a demo — it doesn't validate anything against a real server.

## Keyboard behavior (UX)

- Tapping the card focuses an invisible numeric input, so on mobile the native keyboard opens.
- **Correct** code (anything other than `0000`) → the keyboard closes itself (`blur`) right as the verification animation starts.
- **Incorrect** code (`0000`) → the keyboard stays open during the shake, the box resets automatically ~600ms later, and the input is refocused to retry without touching anything else.
- The **"Resend"** link manually resets the component at any time and refocuses the input.

## How to use it

**Option 1 — Standalone:** open the `.html` file directly in the browser, or upload it to your hosting as just another page.

**Option 2 — Embedded in another page (recommended for funnels):** paste it inside an `<iframe>` with `sandbox="allow-scripts"`, setting its content via `srcdoc`. Since it's fully isolated (no IDs or classes shared with the rest of the site), it won't clash with your landing page's CSS/JS.

## How to customize it

Everything is editable directly in the file, no special tools needed:

| What you want to change | Where it is |
|---|---|
| The code that triggers the error (`0000`) | Look for `val === '0000'` inside the `<script>` |
| Text (title, subtitle, messages) | Look for `id="title"` / `id="subtitle"` in the HTML, and their equivalents in the JS (`title.textContent`, `subtitle.innerHTML`) |
| Colors | CSS variables at the top of the `<style>` (`--accent-orange`, `--accent-green`, etc.) |
| Animation timing | The `sleep(...)` calls inside `playAnimation()` and the `setTimeout` in `triggerError()` |
| Haptic vibration | Calls to `vibrate([...])` — can be removed or the pattern adjusted |

## Important note

This component is a **visual simulation**, not a real verification system. It doesn't send SMS, doesn't validate against any backend, and the code that triggers the error is fixed in the source code. If real OTP verification is needed (with SMS/WhatsApp sending and server-side validation), it needs to be integrated with an external provider (Twilio Verify, MercadoPago, etc.) — this file only handles the visual/UX part.
