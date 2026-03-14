# AGENTS Log

Date: 2026-02-08

Session Summary (detailed)
- Added Node.js and npm to the Docker build stage so the web app (Model Manager UI) can be compiled.
- Forced CMake to build the web app when BUILD_WEB_APP is enabled, ensuring resources/web-app is produced.
- Diagnosed Docker build failure caused by assets symlinking into docs/ (excluded by .dockerignore).
- Added a Dockerfile step to materialize web-app assets and replace favicon symlinks with a placeholder file inside the build context.
- Wrapped the web-app asset normalization and build flag behind BUILD_WEB_APP to allow server-only builds.
- Built the image successfully and verified endpoints: /live returned status ok and / served the web UI.

Artifacts and Checks
- Image built locally: lemonade:model-manager
- Verified HTTP:
  - GET /live -> {"status":"ok"}
  - GET / -> web app HTML served (renderer.bundle.js present)

Notes
- The web-app assets in src/web-app are symlinks to src/app/assets, and favicon.ico further points to docs/assets/favicon.ico. Because docs/ is excluded from the Docker build context, the symlink breaks inside the image unless replaced.
