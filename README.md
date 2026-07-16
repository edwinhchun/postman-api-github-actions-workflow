# PokéAPI Pagination Testing: Postman + Newman + GitHub Actions

This repo automates a Postman collection that tests pagination against the PokéAPI. The same tests I run locally on Postman also run automatically on every push via GitHub Actions.

## What it does

The collection hits PokéAPI's API and walks through every page of results using a self-looping request:

1. **Start Request** — kicks off the flow, grabs the first `next` URL from the response and saves it to a variable.
2. **Fetch Next URL Request** — uses that saved URL to fetch the next page, saves the new `next` URL, and loops back to itself. It keeps looping until PokéAPI returns no more pages.

## Running it automatically (GitHub Actions)

Every push to `main` triggers `.github/workflows/newman-tests.yml`. This:

1. Spins up a clean Linux Ubuntu machine
2. Installs Node.js
3. Installs Newman
4. Runs the collection and prints the results

You can also trigger a run manually from the **Actions** tab without pushing any code. The workflow includes `workflow_dispatch`, which adds a "Run workflow" button for a manual run (what I did to test the workflow).

## Why this exists

This project came out of learning and experimenting with API testing fundamentals using Postman and. I first ran this in the terminal using Newman with the exported collection. Finally, I built on this further by creating a Github Action workflow, leveraging Claude to write me the YAML file. 

---

**Built with:** [Postman](https://www.postman.com/) · [PokéAPI](https://pokeapi.co/) · [Claude](https://claude.ai/)
