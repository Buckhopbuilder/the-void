# Voidpass – Operational Notes (v0.1.2-reveal-stable)

This document records **operational truths** about the Voidpass system as of the
frozen baseline release **v0.1.2-reveal-stable**.

Its purpose is to prevent future ambiguity or accidental regressions.

---

## Reveal Sender Invocation

### How `reveal_sender` is triggered

- The `reveal_sender` Edge Function is **invoked internally by Supabase**.
- It is executed by the **Supabase Edge Runtime lifecycle**, not by:
  - External cron services
  - GitHub Actions
  - Browsers
  - Manual HTTP callers

### Evidence

- Edge Function logs show:
  - `served_by: supabase-edge-runtime`
  - No HTTP request metadata (no user-agent, no IP, no headers)
  - Lifecycle events such as `Shutdown` with reason `EarlyDrop`
- Repeated successful (200) invocations are visible in Supabase logs.

### Dashboard Visibility Note

- No entry appears under **Edge Functions → Scheduled Triggers** in the Supabase UI.
- This is expected behavior.
- Supabase supports internal/platform-driven invocations that are **not exposed** in the Scheduled Triggers list.

This is a **Supabase platform behavior**, not a configuration error.

### Implications

- There is **no external cron dependency** to maintain, migrate, or secure.
- There is **no webhook URL** or third-party scheduler involved.
- The reveal process is self-contained within Supabase.

---

## Stability Guarantee

As of **v0.1.2-reveal-stable**:

- Reveal timing works
- Reveal sender is idempotent
- Email delivery has been confirmed end-to-end
- No action is required to keep reveal delivery functioning

Any future changes must preserve this behavior exactly.

---

_Last verified: 2025-12-26_
