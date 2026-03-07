# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a single-file, self-contained HTML tool (`TextTransformer.html`) that lets users write and run TypeScript code against arbitrary input text directly in the browser. No build step, no server, no dependencies to install.

## Running the App

Open `TextTransformer.html` directly in a browser. No build or serve step required.

## Architecture

Everything lives in `TextTransformer.html`:

- **Input textarea** — user pastes raw text to be processed
- **Monaco Editor** (`monaco-editor@0.45.0`) — in-browser code editor with TypeScript syntax support
- **Snippet selector** — dropdown populated from a hardcoded `snippets` array in the script; selecting a snippet loads it into the editor
- **Run button** — transpiles the TypeScript via `ts.transpile()` (TypeScript `5.4.5` loaded as a UMD global), then executes it using `new Function(...)`, calling the `process(input: string): string` function the user defines
- **Output textarea** — displays the string result of `process(input)`
- **Error box** — shown on transpile/runtime exceptions

## Conventions

- **TextTransformer**: contract is `process(input: string): string`. Snippets live in the `snippets` array (~line 129).
- **ImageTransformer**: contract is `process(input: HTMLCanvasElement): HTMLCanvasElement | HTMLCanvasElement[]`. Returning an array displays all results in an auto-computed grid (`cols = ceil(sqrt(n))`). Snippets live in the `snippets` array.
- New snippets are added as `{ name: string, snippet: string }` objects in their respective `snippets` array.
- External dependencies (TypeScript compiler, Monaco) are loaded from `unpkg.com` CDN — no local assets.
