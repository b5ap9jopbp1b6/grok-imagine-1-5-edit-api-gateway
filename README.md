# Grok Imagine 1.5 Edit API (`grok-imagine-1.5-edit-apimart`)

<!-- conv-kit:v1 -->

<p align="center">
  <img src="assets/badges/price.svg" alt="observed unit price"> <img src="assets/badges/billing.svg" alt="billing model"> <img src="assets/badges/compat.svg" alt="OpenAI-compatible endpoint">
</p>

<p align="center">
  <img src="assets/01-product-hero-perfume.jpg" width="820" alt="Sample render via the APIMart image API">
</p>

> **from $0.015 per image** — one OpenAI-compatible endpoint at `https://api.apimart.ai/v1`, no monthly plan required. *(observed 2026-10-12)*

**[Get an API key](https://go.apimart.ai/k-e5af31)** · **[Live pricing](https://go.apimart.ai/k-09197d)** · **[Model page](https://go.apimart.ai/k-d0fb57)** · [⚡ 60-second quickstart](#quickstart)

**Why teams call Grok Imagine 1.5 Edit (`grok-imagine-1.5-edit-apimart`) through APIMart**

- **One key, entire catalog.** The same `https://api.apimart.ai/v1` base URL and `Authorization` header reach Grok Imagine 1.5 Edit (`grok-imagine-1.5-edit-apimart`) and 300+ other image, video and language models — switch the `model` field, not your client.
- **$1 minimum, pay as you go.** No subscription and no prepaid plan to size up front: top up from $1 and spend it on calls. There is no free quota to burn through first, so the price in this table is the price you pay.
- **The charge comes back in the response.** Every call reports the amount billed (`cost` / `credits_cost`), so a spend number is read per call instead of guessed at month end.
- **Async by design.** Submit, take the `task_id`, poll `GET /v1/tasks/{id}` — batching and retries are ordinary queue work, not a bespoke integration.

<!-- /conv-kit:v1 -->



Grok Imagine 1.5 Edit handles reference-guided image editing, priced per image.

## Model id and routes

| Route | `model` value | Billing | Notes |
| --- | --- | --- | --- |
| Per-image | `grok-imagine-1.5-edit-apimart` | per delivered image, by resolution | see the [model page](https://go.apimart.ai/k-d0fb57) for the exact tiers |

Endpoint: `POST https://api.apimart.ai/v1/images/generations`, then poll `GET /v1/tasks/{task_id}`.

## Pricing

<!-- pricing:model:start -->
| Output | List price | Effective price |
| --- | --- | --- |
| default | 0.01875 | 0.015 |


<!-- pricing:model:end -->

Prices are a snapshot; the [pricing page](https://go.apimart.ai/k-09197d) and [`data/model.json`](data/model.json) are refreshed by CI, and a completed task reports the exact amount in its `cost` field.

## Request parameters

| Field | Type | Default | Notes |
| --- | --- | --- | --- |
| `model` | string | required | the model id shown in the route table above |
| `prompt` | string | required | what to generate, or what to change when editing |
| `size` | string | `auto` / `1:1` | aspect ratio, or `auto` where the route supports it |
| `resolution` | string | `1K` | output resolution tier — cost scales with it |
| `n` | integer | `1` | number of images per task, where the route allows more than one |
| `image_urls` | string[] | — | reference images as public HTTP(S) URLs or base64 data URLs |
| `nsfw_check` | boolean | `false` | runs `omni-moderation-latest` before submitting |

The table lists the fields accepted by the APIMart `images/generations` endpoint. Check the [model page](https://go.apimart.ai/k-d0fb57) for anything this model adds on top.

## Quickstart

```bash
curl --request POST --url https://api.apimart.ai/v1/images/generations \
  --header "Authorization: Bearer $APIMART_API_KEY" --header 'Content-Type: application/json' \
  -d '{"model":"grok-imagine-1.5-edit-apimart","prompt":"A bamboo forest at sunrise, slow push-in","size":"1:1","resolution":"1K"}'
```

```python
import os, time, requests

BASE = "https://api.apimart.ai/v1"
HEADERS = {"Authorization": f"Bearer {os.environ['APIMART_API_KEY']}", "Content-Type": "application/json"}

created = requests.post(f"{BASE}/images/generations", headers=HEADERS, timeout=60, json={
    "model": "grok-imagine-1.5-edit-apimart", "prompt": "A bamboo forest at sunrise, slow push-in",
}).json()
task_id = created["data"]["id"]

while True:
    task = requests.get(f"{BASE}/tasks/{task_id}", headers=HEADERS, timeout=60).json()["data"]
    if task["status"] in ("completed", "failed"):
        break
    time.sleep(5)
print(task.get("cost"), task["result"]["images"][0]["url"])
```

Runnable versions: [`examples/`](examples). The task lifecycle is `pending → processing → completed | failed`.

## FAQ

**What is the Grok Imagine 1.5 Edit model id?**

`grok-imagine-1.5-edit-apimart`. See the route table above.

**How is it billed?**

**from $0.015 per image**. The completed task reports the exact amount in its `cost` field; a failed task is refunded.

**How do I get a key?**

Use the [Get an API key](https://go.apimart.ai/k-e5af31) link — $1 minimum top-up, no plan required.

<!-- related-repos:auto -->
## Related repositories

- **Same model, other languages** — 9 more language builds: 中文 `grok-imagine-1-5-edit-api-cn` · Deutsch `grok-imagine-1-5-edit-api-de` · Español [grok-imagine-1-5-edit-api-es](https://github.com/c-h93bzyc38vo3/grok-imagine-1-5-edit-api-es) · Français `grok-imagine-1-5-edit-api-fr` · Bahasa `grok-imagine-1-5-edit-api-id` · 日本語 `grok-imagine-1-5-edit-api-ja` · 한국어 [grok-imagine-1-5-edit-api-ko](https://github.com/id5913m6/grok-imagine-1-5-edit-api-ko) · Português [grok-imagine-1-5-edit-api-pt](https://github.com/lvgf2km19cgt5/grok-imagine-1-5-edit-api-pt) · Русский `grok-imagine-1-5-edit-api-ru`
- **More models in this language** — [midjourney-api-gateway](https://github.com/apimartxc3iskw/midjourney-api-gateway) · [gemini-2-5-flash-image-preview-api-gateway](https://github.com/mcqdlebg/gemini-2-5-flash-image-preview-api-gateway) · [nano-banana-api-gateway](https://github.com/gix2kust/nano-banana-api-gateway) · [gemini-3-1-flash-image-preview-api-gateway](https://github.com/nn6jtthao8/gemini-3-1-flash-image-preview-api-gateway) · [nano-banana-2-api-gateway](https://github.com/u9mdw3pt594q2r/nano-banana-2-api-gateway) · [gemini-3-pro-image-preview-api-gateway](https://github.com/uqu9q/gemini-3-pro-image-preview-api-gateway)
- **Topic hubs** — every build of this model, all languages: [grok-imagine-1-5-edit](https://github.com/topics/grok-imagine-1-5-edit) · [grok-imagine-1-5-edit-api](https://github.com/topics/grok-imagine-1-5-edit-api)

This repository is one build in a cross-linked set of API guides for the same model family; each language build links back to the others.
<!-- /related-repos:auto -->

## Attributed links (how this repository is measured)

| Purpose | Attributed link | Target |
| --- | --- | --- |
| Model page | <https://go.apimart.ai/k-d0fb57> | `apimart.ai/model/grok-image` |
| Live pricing | <https://go.apimart.ai/k-09197d> | `apimart.ai/pricing` |
| Get an API key | <https://go.apimart.ai/k-e5af31> | `apimart.ai/keys` |

Outbound APIMart links are minted through the promo link API; hand-made tracking parameters are rejected by
`tools/check_links.py` in CI.

## Disclosure

Grok Imagine 1.5 Edit is a third-party model served through APIMart; this repository documents how to call it and publishes
model ids and prices, and does not claim official status. Model names, prices and documentation belong to their
respective owners.

## License

MIT — see [LICENSE](LICENSE).
