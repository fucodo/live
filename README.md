# fucodo/live

A small Neos/Flow development helper package that ships the Live.js script to auto‑reload changed assets while developing. In the Development context, this package registers `livejs.js` so that CSS/JS/HTML changes in the browser are detected and applied without manual refresh.

## Overview

- Stack: PHP with Neos Flow/Neos CMS package (`type: neos-package`).
- Purpose: Provide Live.js during development to speed up frontend iteration.
- Main asset: `Resources/Public/JavaScript/live.js/livejs.js` (MIT-licensed from https://livejs.com).
- Wiring: Enabled via `Configuration/Development/Settings.KayStrobach.Backend.PageRenderer.yaml`, which injects the JS file into the backend page renderer in Development context.

## Requirements

- Neos/Flow application (this repository is a Neos distribution).
- PHP and Composer compatible with the parent project.
  - TODO: State the exact supported PHP and Neos/Flow versions (check root `composer.json`).

## Installation

This package is already part of the distribution under `DistributionPackages/fucodo.live`.

If you want to install it in another Neos/Flow project:

1. Add the package (as a path repo or VCS) and require it via Composer so that it appears under `DistributionPackages/` and is discovered by Flow.
   - TODO: Provide the Composer package name and repository URL if published (current `name` is `fucodo/live`).
2. Ensure your application runs in `Development` context to activate the provided settings.

## Usage

- Start your Neos/Flow application in Development context (e.g. `FLOW_CONTEXT=Development`).
- Visit the application; `livejs.js` will be included automatically via the PageRenderer as configured here:

  ```yaml
  # Configuration/Development/Settings.KayStrobach.Backend.PageRenderer.yaml
  KayStrobach:
    Backend:
      PageRenderer:
        Defaults:
          jsFiles:
            liveJs:
              file: 'resource://fucodo.live/Public/JavaScript/live.js/livejs.js'
              type: text/javascript
              section: footer
              forceOnTop: false
              async: false
              integrity: null
              defer: null
              crossorigin: null
  ```

- Optional: Control what Live.js watches by adding a hash to the script URL (css/js/html). The stock `livejs.js` supports fragments like `#css`, `#js`, `#html`, and `#css,notify`. To change behavior, adapt the configuration above and append `#css` etc. to the `file` value.
  - Example: `resource://fucodo.live/Public/JavaScript/live.js/livejs.js#css`

## Scripts and entry points

- Composer scripts: none defined in this package (`composer.json`).
- NPM scripts: none in this package.
- Runtime entry point: the browser loads `Resources/Public/JavaScript/live.js/livejs.js` as configured.

## Configuration and environment variables

- Context-dependent: The inclusion is only configured for the `Development` context via the file under `Configuration/Development/...`.
- Environment variables: None specific to this package.
- Flow/Neos context: set `FLOW_CONTEXT=Development` (or use the equivalent per your setup) to enable.

## Testing

- This package does not contain automated tests.
- Manual verification:
  1. Run the app in Development.
  2. Open a page that includes the backend PageRenderer assets.
  3. Modify a local CSS or JS file; observe Live.js applying changes/reloading.

## Project structure

```
DistributionPackages/fucodo.live/
├─ Configuration/
│  └─ Development/
│     └─ Settings.KayStrobach.Backend.PageRenderer.yaml
├─ Resources/
│  └─ Public/
│     └─ JavaScript/
│        └─ live.js/
│           ├─ livejs.js
│           └─ license.txt
├─ composer.json
├─ license.txt
└─ README.md (this file)
```

## Development notes

- Namespace is configured for a `Classes/` directory via Composer autoload PSR‑4 (`fucodo\\live\\` → `Classes/`), but no PHP classes are currently present. You can add PHP code under `Classes/` if needed.
- The `Settings.KayStrobach.Backend.PageRenderer.yaml` file uses the `resource://` scheme to reference the public asset.

## License

- The package is MIT-licensed (`composer.json` specifies `MIT`).
- `livejs.js` is MIT-licensed by Martin Kool and Q42; see `Resources/Public/JavaScript/live.js/license.txt`.

## Troubleshooting

- Live.js does not load: Confirm you are in `Development` context and that the PageRenderer configuration is active in your setup.
- Conflicts with CSP or SRI: The configuration sets `integrity: null` and `crossorigin: null`. If you use CSP/SRI, adjust accordingly.

## TODO

- Document exact supported PHP and Neos/Flow versions for this distribution.
- Publish Composer installation instructions if this package is distributed outside this repository.
- Add tests or a simple health check if future functionality is added beyond asset delivery.
