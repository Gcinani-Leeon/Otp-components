# Interactive OTP Component — Orbit (V3)

## What is this?

This is a visual **OTP code verification** component (the typical 4-digit box that asks for a code sent via SMS), ready to drop into a landing page. It simulates the "verify your number" step used in signup flows, apps, checkouts, etc.

It's **100% interactive**: it's not a video or a recorded animation. The user taps the card, their phone's numeric keyboard opens, and they type a real code. The component responds based on what was typed:

- Typing the correct code → triggers the "Verified" sequence: the four boxes arc inward with a slight rotation, converge into a glowing ring, and draw a checkmark with a directional spark burst.
- Typing any other 4-digit code → triggers a red shake + flicker, then clears itself automatically so the person can retype without touching anything else.

## What's new in V3 (Orbit)

- **New visual identity**: dark navy background with a diagonal light streak and a periwinkle-blue accent (palette based on the Aura Studio reference), replacing V2's light fintech-green look.
- **New confirmation animation**: the digits orbit into a single ring instead of sliding straight into a grid — a more cinematic, less templated sequence.
- **New typing micro-interaction**: each digit "focus-pulls" into place (blur + scale settling into focus) instead of a flat pop, and doesn't re-flash when adjacent digits are typed.
- **API hook built in**: a small config block lets you point the component at a real verification endpoint, with a working local fallback if you don't have one yet (see below).
- **No play/demo button**: V2 was built to be recorded as content (autoplay loop). V3 is meant to be handed to a client as a working component — it's live and interactive from the moment it loads.

## How is it built?

- **A single `.html` file**, with no external dependencies besides Google Fonts (Space Grotesk, Inter, JetBrains Mono) loaded via `@import`.
- All the code — HTML, CSS, and JavaScript — lives inside that single file. It opens in any browser with a double-click, can be uploaded to any hosting, and can be pasted directly inside an `<iframe>` on another page without breaking anything (no shared IDs/classes with the rest of a site).
- Uses **pure CSS** for the animations (transitions, keyframes) and a bit of **Canvas** for the spark burst on successful verification.

## Keyboard behavior (UX)

- Tapping anywhere on the code field focuses an invisible numeric input, so on mobile the native keyboard opens. It also autofocuses on load for desktop use.
- Correct code → the keyboard closes itself (`blur()`) right as the success animation starts.
- Incorrect code → the keyboard stays open, the boxes clear themselves after the shake, and the person can type again immediately.
- The **"Resend"** link manually resets the component at any time.

## Verifying codes: local demo mode vs. a real API

At the top of the `<script>` there's a config block:

```js
const OTP_CONFIG = {
  apiEndpoint: '',              // e.g. 'https://your-api.com/verify-otp'
  apiMethod: 'POST',
  extraPayload: {},             // e.g. { phone: '+54911...', sessionId: '...' }
  localDemoCode: '1234'         // used only while apiEndpoint is empty
};
```

- **`apiEndpoint` empty (default)** → the component works standalone, no backend needed. It just compares the typed code against `localDemoCode`.
- **`apiEndpoint` set** → every submitted code is sent as a `POST` (or whatever `apiMethod` you set) with `{ code, ...extraPayload }` in the body, to whatever endpoint the client's backend exposes. The response is expected as `{ success: true|false }` — if their API returns something different, there's a single line marked in `verifyWithApi()` where the response parsing should be adjusted.

This means the file works immediately out of the box for a demo, and only needs one field filled in to go live with real SMS/WhatsApp verification once the client has a provider (Twilio, MercadoPago, etc.) wired up on their end.

## How to use it

**Option 1 — Standalone:** open the `.html` file directly in the browser, or upload it to your hosting as just another page.

**Option 2 — Embedded in another page (recommended for funnels):** paste it inside an `<iframe>` with `sandbox="allow-scripts"`, setting its content via `srcdoc`. Since it's fully isolated, it won't clash with the rest of your landing page's CSS/JS.

## How to customize it

Everything is editable directly in the file, no special tools needed:

| What you want to change | Where it is |
|---|---|
| The verification endpoint / demo code | `OTP_CONFIG` at the top of the `<script>` |
| Text (title, subtitle, error message) | `id="title"` / `id="subtitle"` in the HTML, and their equivalents in the JS (`title.textContent`, `subtitle.innerHTML`) inside `verify()`, `triggerError()`, `playSuccessSequence()`, and `resetComponent()` |
| Colors | CSS variables at the top of the `<style>` (`--bg-a`, `--bg-b`, `--accent`, `--success`, `--error`, etc.) |
| Fonts | The `@import` at the top of `<style>`, plus `font-family` on `h1`, `.subtitle`/body, and `.slot` |
| Animation timing | The `delay(...)` calls inside `playSuccessSequence()`, and the `setTimeout(...)` inside `triggerError()` |
| Number of digits | Currently fixed at 4 slots (`s0`–`s3`) — adding a slot means adding a matching entry in `ROW_POS` and `ORBIT_WAYPOINT`, plus a new `.slot` element in the HTML |

## Important note

Verification only becomes real once `apiEndpoint` is pointed at an actual backend. Out of the box, with `apiEndpoint` left empty, this is still a **visual simulation**: no SMS is sent, and the "correct code" is just a local string compared in the browser. This file always handles the visual/UX layer — the actual code delivery and server-side validation depend on whatever provider is wired up behind `apiEndpoint`.
