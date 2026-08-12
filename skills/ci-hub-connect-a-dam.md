---
name: ci-hub-connect-a-dam
description: Take a user from nothing to a live, authenticated CI HUB session with a DAM provider connected, ready to make content calls. Use this before any CI HUB asset skill — every content operation depends on the two tokens this flow produces.
api: ci-hub:access-sdk
base_url: https://live.ci-hub.com/api/v1
operations:
  - exchangeToken
  - getProvidersSdk
  - damLoginInitiate
  - damLoginPoll
  - getProviderInfoSdk
generated: '2026-08-12'
method: generated
source: openapi/ci-hub-access-openapi.yml + https://developer.ci-hub.com/access/getting-started
---

# Connect a user to a DAM through CI HUB

CI HUB brokers to 60+ DAM, PIM, CMS and storage systems behind one contract. Nothing works until
two separate authentications have happened: CI HUB must know who the user is, and the DAM must
separately authorize them. This skill produces both tokens.

## Before you start

Two things are agreed with CI HUB during onboarding and cannot be self-served:

- **Partner registration** — CI HUB holds your issuer string and your JWKS URL.
- **An SDK subscription** — without one every call returns `402 cihub-sdk-no-subscription`.

## 1. Exchange a partner JWT for a CI HUB session

Sign an **RS256** JWT on your backend with the private key whose public half is in your published
JWKS, then call `exchangeToken` (`POST /auth/exchangeToken`) with it in `Authorization: Bearer`.

Claims: `iss` (must match the registered value **exactly, including any trailing slash**), `aud`
(default `https://api.ci-hub.com`), `sub`, `iat`, `exp`, and `email`. The header needs `alg: RS256`
and a `kid` present in your JWKS. `HS256` is rejected.

Two traps worth stating plainly:

- **`aud` does not follow the host.** It is `https://api.ci-hub.com` on staging and on production
  alike. Pointing it at your `baseUrl` returns `403 cihub-sdk-audience-invalid`.
- **Sign a fresh JWT per exchange.** A token older than `maxTokenAge` (default 3600s, measured
  from `iat`) is rejected with `cihub-sdk-token-invalid`, and an `iat` more than 30s in the future
  is rejected too.

If your JWT carries no `email` claim, send it in the JSON body instead — with
`Content-Type: application/json`, because only a JSON body is parsed for it. The email drives
just-in-time user resolution: CI HUB finds the matching user or creates one on first exchange.

You get back `access_token` (1 hour), `refresh_token` (30 days), `expires_in` and
`token_type: Bearer`. **Cache them.** The exchange endpoint is the one rate-limited operation —
60/minute per partner — so exchanging per request will get you `429 cihub-rate-limited`.

## 2. List the providers the user can connect

Call `getProvidersSdk` (`GET /auth/providers`) with the access token.

Send the token. Without it the endpoint still answers `200`, but returns a single degraded entry
rather than the real list — a silent wrong answer rather than an error.

## 3. Start the DAM login

Call `damLoginInitiate` (`POST /auth/login`) with `provider` (an id from step 2) and
`polling=true`. You get a one-time redirect URI and a `state`.

If the provider is one of `bynder`, `dash`, `fotoware`, `frontify`, `picturepark` or `purered`,
pass `serverUrl` as well — the full origin of the customer's DAM instance. It answers up front the
"which instance?" question CI HUB would otherwise put in front of the user, removing a page from
the flow.

Open the redirect URI in the user's browser.

## 4. Poll until the login completes

Call `damLoginPoll` (`GET /auth/login`) with the `state` and `polling=true`. Three outcomes, all
on `200`:

- **empty object** — still in progress; keep polling.
- **tokens** — done. Keep `access_token` (this is the *DAM connection* token) and `refresh_token`.
- **an `error` property** — the state is dead: unknown, expired, consumed, or the login failed.
  Do not retry the same state; start a new login.

## 5. Read what this DAM can actually do

Call `getProviderInfoSdk` (`GET /system/providerInfo`) with **both** tokens.

Do not skip this. Because one contract fronts many systems, capability varies per connection:
whether similarity search exists, whether search can be scoped to a folder, whether the DAM
supports tasking or brand hub, which hash algorithm its assets report, its upload size ceiling.
Gate your UI on these flags. Skipping this step is what produces `501
integration-not-implemented` and `501 integration-not-supported` at runtime.

## The two tokens, from here on

```
Authorization:           Bearer <CI HUB access_token>     (always)
provider-authorization:  Bearer <DAM connection token>    (on content calls)
```

**One exception:** on `refreshTokenSdk` (`GET /auth/refreshToken`), `provider-authorization`
carries the CI HUB *refresh* token, not a DAM token. It is the same header doing a different job.

## Keeping the session alive

- **CI HUB token** — refresh proactively, a few minutes before `expires_in`. Both tokens are
  replaced on each refresh; the old ones stay valid until their own expiry, so a slow switch-over
  is safe. The refresh token's `sub` must match the access token's `sub`.
- **DAM token** — reports no lifetime at login, so refresh *reactively* on the first `401`. Some
  providers have no refresh path at all; when that surfaces, run a fresh DAM login.

## Failure modes

| Code | Status | What to do |
|---|---|---|
| `cihub-sdk-partner-unknown` | 403 | `iss` is not registered. Check it character for character. |
| `cihub-sdk-audience-invalid` | 403 | `aud` must be the registered value, not your base URL. |
| `cihub-sdk-token-invalid` | 401 | Malformed/wrong-alg/stale JWT. Mint a fresh one. Rarely a transient backend fault — retry once before blaming the token. |
| `cihub-sdk-no-subscription` | 402 | Commercial, not technical. Contact the partner manager. |
| `cihub-license-required` | 402 | The user has no seat on the subscription. |
| `provider-access-token-missing` | 403 | You made a content call without `provider-authorization`. |
| `cihub-rate-limited` | 429 | You are exchanging too often. Cache the session; honor `Retry-After`. |

Full catalogue: `errors/ci-hub-problem-types.yml`.
