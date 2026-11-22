# L’Oréal Routine Builder

A product-aware beauty advisor designed by Eden Souare. The provided 35-product catalog powers browsing and selection. Pink (#ff003b) and gold (#e3a535) accents pair with editorial typography, product photography, and accessible controls.

## Run and publish

Open the project in Codespaces and serve it with Live Server, or run:

```bash
python -m http.server 8000
```

Open port 8000 in your browser. The product catalog is fetched from products.json, so use a server instead of opening index.html directly.

The public endpoint in script.js is already set to https://loreal-routine-worker.eden-souare.workers.dev/. The browser sends a messages array and reads choices[0].message.content. API credentials belong in the Worker's secret settings.

Commit and push the project files, then choose Settings → Pages → Deploy from a branch → main → /(root). The submission URL will be https://edensouare.github.io/09-prj-loreal-routine-builder/.

## Product and conversation behavior

- Click a card to select or deselect it. Keyboard users can press Enter or Space on its selection button. A visible selection mark and aria-pressed communicate state.
- Open Product details to read the description without changing selection.
- Search matches name, brand, category and description while respecting the category filter.
- Remove individual selections or clear them all. localStorage saves IDs and reloads current records from the catalog. A storage failure leaves the current selection usable with an explanatory message.
- Generate sends only selected product JSON, including name, brand, category and description. Optional goals add preferences.
- Follow-ups send the full successful conversation plus off-topic redirects. Failed requests leave the prior conversation and draft available.
- A new generated routine begins a new conversation. Changing selection does not silently change an existing routine. New chat clears the conversation and keeps the saved selection.
- RTL layout mirrors the grid, selection panel and chat through logical CSS. The control changes layout direction; interface labels remain in English. Replies can follow the language requested by the shopper.

## Enable the web-search LevelUp

The existing Worker supports core chat. Web search and longer routines require deploying the included RESOURCE_cloudflare-worker.js.

1. Open your existing Worker in the Cloudflare dashboard. Copy its current code somewhere safe before editing.
2. Replace the Worker code with RESOURCE_cloudflare-worker.js and deploy. Keep the existing OPENAI_API_KEY secret.
3. The default allowed browser origin is https://edensouare.github.io, which covers both project 8 and project 9. For Codespaces previews, add an ALLOWED_ORIGINS text variable containing comma-separated exact origins, such as the GitHub Pages origin and your forwarded port origin. Omit paths and trailing slashes.
4. The updated Worker uses GPT-4.1 with the Responses API web_search tool for routine replies. Scope checks use Chat Completions with a short response. Browser calls still use the same messages request and choices response shape.
5. Source annotations become clickable inline links. The app never invents source URLs. When no sources are returned, it shows the answer without source links.

Web search requires model/tool access and API billing on the Worker account. Its use is not verified until this Worker code is deployed. The existing project 8 chat format remains supported.

Official web-search guide: https://developers.openai.com/api/docs/guides/tools-web-search

## Review before submission

Choose two products, expand their descriptions, remove one from the list, and reload to confirm the remaining selection returns. Search within a category and check that the results match both filters. Generate a routine and ask a follow-up, then try an unrelated SQL question. Add another product and check the selection-change message. Switch RTL layout and check both mobile and desktop widths. Once the updated Worker is deployed, ask for current product information and verify that returned source links open.