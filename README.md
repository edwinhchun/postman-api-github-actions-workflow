# PokéAPI Pagination Testing — Postman + Newman + GitHub Actions

This repo automates a Postman collection that tests pagination against the [PokéAPI](https://pokeapi.co/). It started as a Postman exercise and turned into a full CI pipeline — the same tests I run locally also run automatically on every push via GitHub Actions.

## What it does

The collection hits PokéAPI's `/pokemon` endpoint and walks through every page of results using a self-looping request:

1. **Start Request** — kicks off the flow, grabs the first `next` URL from the response and saves it to a runtime variable.
2. **Fetch Next URL Request** — uses that saved URL to fetch the next page, saves the new `next` URL, and loops back to itself. It keeps looping until PokéAPI returns no more pages, at which point it stops.

No hardcoded page URLs anywhere — the collection figures out where to go next based on the API's own response, request after request.

## Running it locally

You'll need [Node.js](https://nodejs.org/) installed, then:

```bash
npm install -g newman
newman run Pagination.postman_collection.json
```

That's it — no environment file needed. The pagination URL (`Next_url`) is created and updated live during the run itself via `pm.environment.set()`, so there's nothing to pre-configure.

## Running it automatically (GitHub Actions)

Every push to `main` triggers `.github/workflows/newman-tests.yml`, which:

1. Spins up a clean Ubuntu machine
2. Installs Node.js
3. Installs Newman
4. Runs the collection and prints the results

You can also trigger a run manually from the **Actions** tab without pushing any code — the workflow includes `workflow_dispatch`, which adds a "Run workflow" button for exactly that.

## Why this exists

This project came out of learning API testing fundamentals in Postman — auth flows, assertions, pagination patterns — and pushing further into the tooling that surrounds it: running collections from the CLI instead of just the GUI, and wiring that into CI so tests run without anyone needing to remember to click anything.

## Next steps

Planning to point this same test pattern at a small API I build myself on Cloudflare Workers, so the pipeline validates infrastructure I've written instead of just a public API.
