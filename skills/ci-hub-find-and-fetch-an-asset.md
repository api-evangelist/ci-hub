---
name: ci-hub-find-and-fetch-an-asset
description: Find an asset in a connected DAM by keyword, by visual similarity, or by walking the folder tree, then resolve its download URL and fetch the bytes. Assumes a live CI HUB session with a DAM connected.
api: ci-hub:access-sdk
base_url: https://live.ci-hub.com/api/v1
operations:
  - searchAssetsSdk
  - searchSimilarAssetsSdk
  - getFolderSdk
  - getAssetSdk
  - getAssetVersionsSdk
requires_skill: ci-hub-connect-a-dam
generated: '2026-08-12'
method: generated
source: openapi/ci-hub-access-openapi.yml + https://developer.ci-hub.com/access/content
---

# Find an asset and fetch its bytes

Every call here needs **both** tokens: `Authorization: Bearer <CI HUB access token>` and
`provider-authorization: Bearer <DAM connection token>`. Run `ci-hub-connect-a-dam` first.

The Access SDK is **read-only** in this release. There is no upload, update, delete, rename or
move operation — the client library declares `upload()` and `update()` but throws
`cihub-not-implemented`. If you need to write, that surface exists only on the MCP server
(`mcp/ci-hub-mcp.yml`).

## Three ways in

### Keyword search

`searchAssetsSdk` (`GET /assets/search`) with `query`. Optionally `parentId` to scope the search
to a folder — but only where the provider declares `capabilities.assetSearch.supportsParentId`,
and note some providers set `alwaysSearchInFolder` and will scope whether you ask or not.

Narrow with `filters`: an array of option ids. You do not know these in advance. They come back in
the `filters` block of a **previous** response for this provider, so the flow is search, read the
facets offered, then search again with the ones you want. Discovery-driven, not declared.

### Similarity search

`searchSimilarAssetsSdk` (`POST /assets/search`) with a JSON body carrying `dataBase64` — either
base64 image data or an HTTP/HTTPS URL to the image.

Check `capabilities.assetSearch.similarSearch` on the connected provider first. Where it is false
you get `501 integration-not-supported`, which is permanent, not transient.

### Walk the folder tree

`getFolderSdk` (`GET /assets/folder/{folderId}`). Start at the literal id `root`. The response
carries `folders` and `assets`.

## Paging

Both search and folder browse use an opaque forward cursor.

- Response carries `more` when another page exists. Send it back as the `more` query parameter.
- **No `more` means you are done.** Do not infer the end from page size — a short page can be
  followed by another, and a full page can be the last.
- Treat `more` as opaque. Its form differs per provider; pass it back unchanged rather than
  computing offsets.
- `size` sets page size. It is **clamped** to the provider's maximum rather than rejected, so a
  large value fails silently-but-safely. Keep it stable across one traversal.
- **`more` pages `assets` only.** The `folders` array is not paged by it: depending on the
  provider you get subfolders on the first page alone, or repeated on every page. Read folders
  from the first page, or dedupe by `id` as you accumulate.

## Read one asset

`getAssetSdk` (`GET /assets/asset/{assetId}`) for full detail.
`getAssetVersionsSdk` (`GET /assets/assetversions/{assetId}`) for version history.

Reading an `Asset`, two things will bite you:

- **`created` and `modified` are Unix epoch milliseconds**, not ISO 8601 strings.
- **Exactly one hash field is populated**, and which one depends on the provider's
  `capabilities.assetHashAlgorithm` (`Md5`, `Sha1`, `Sha256`, `Sha256Split4MB`,
  `Sha256First16MB`, `Sha512`, `FileAttributes`, `Crc32`). Read the capability, then read the
  matching `downloadHash*` field. Do not assume `downloadHashMd5`.

If an asset carries `masterId`, it *is* a version and `masterId` names the asset it belongs to. If
it carries `relatedFolderId`, the same object is also addressable as a folder — some source
systems model containers that are simultaneously assets.

## Fetch the bytes

You do not request a download by asset id. Every asset already carries `thumbnailUrl` and
`downloadUrl`. Resolve the URL before fetching, in this order:

1. **Strip the CI HUB signature** — keep `cihubSig` only on `/api/v1/assets/download` URLs; remove
   it from every other URL.
2. **Resolve `$...$` placeholders.** At most one authentication placeholder appears per URL.
3. **Add CI HUB auth for CI HUB endpoints.** If the URL points at `/api/v1/assets/download` or
   `/api/v1/assets/thumbnail`, send both CI HUB headers and keep the URL's own signature parameter
   (`cihubSig` on a download URL, `sig` on a thumbnail URL). A modified CI HUB URL fails its
   signature check with `400`.

Provider-direct URLs go straight to the DAM — do **not** attach CI HUB credentials to those.

Append `noRedirect=true` to a download URL to get `{"downloadUrl": "..."}` as JSON instead of the
bytes or a `302`, which is what you want when handing a URL to an `<img>` tag or a browser
download.

**Media URLs do not return the structured error envelope.** They reply with a plain HTTP status
and at most a short text body: `400` when the signature is missing or modified, `404` when the
proxy cannot resolve the asset. Branch on the status code here; do not try to parse an `error`
object.

## Errors

The envelope is CI HUB's own, not RFC 9457. Switch on `error.code`, show `error.message`, and
route the ticket by `error.source`:

- `source: cihub` — CI HUB's fault. Your ticket goes to CI HUB.
- `source: integration` — the DAM's fault, and `provider` names which one. Your ticket goes to the
  DAM owner.

Check that `error` exists before reading `error.code`: DAM errors are mid-migration and some still
arrive as `{"message":"Error","details":"..."}` with no `error` object.

Content-path codes you should handle: `integration-not-found` (404, asset moved or deleted, or the
user cannot see it — re-query the parent folder), `integration-not-available` (404, the asset
exists but that rendition does not — fall back to another, check `providerInfo`),
`integration-forbidden` (403, the DAM denied permission — surface it, the DAM governs this), and
`integration-not-supported` / `integration-not-implemented` (501, gate the affordance on capability
flags instead).

Full catalogue: `errors/ci-hub-problem-types.yml`.

## Rate limits

Read the headers, do not hard-code a number. Responses carry `RateLimit-Limit`,
`RateLimit-Remaining` and `RateLimit-Reset` (plus legacy `X-RateLimit-*`, where the reset is an
absolute epoch rather than a delta — read one family, not a mix). `Retry-After` is added on `429`.
Only token exchange has a documented limit; the platform-wide budget observed on live is
undocumented and may change. See `rate-limits/ci-hub-rate-limits.yml`.
