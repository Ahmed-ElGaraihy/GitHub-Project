<!-- Copilot instructions for AI coding agents: concise, actionable, repository-specific -->
# Copilot instructions — GitHub-Project

Purpose
- Help AI agents be productive editing this repository: a small static website with GitHub Actions CI/CD.

Big picture
- Single-page static site: primary files are `index.html`, `style.css` and image assets at repository root.
- No backend or package manager present. The `GitHub-Project/` subfolder is currently empty.
- CI pipeline: [.github/workflows/ci-cd.yml](.github/workflows/ci-cd.yml) runs a simple test, prepares a `build/` folder, and deploys to GitHub Pages.

Key patterns & conventions
- Branch and deploy: the workflow is triggered on pushes/PRs to `main` and publishes `./build` to gh-pages via `peaceiris/actions-gh-pages`.
- Minimal CI test: the `test` job looks for a literal `<button` string in `index.html`. Changing or removing that string will fail CI unless the workflow is updated.
- Asset handling: workflow copies `index.html`, `style.css`, and `*.png`/`*.jpg` into `build/`. If you add other asset types (e.g., `.js`, `.svg`), update the `build` job copy command.

Developer workflows (how to run & verify locally)
- Quick preview (Windows / cross-platform):

  python -m http.server 8000

  Then open http://localhost:8000 in a browser with the repo root as the server root.
- Simple CI sanity: run the workflow's test locally by checking the same condition used in CI, for example:

  grep -q "<button" index.html || (echo "Button missing"; exit 1)

- Build for deploy: the workflow uses `mkdir -p build && cp index.html style.css *.png *.jpg build/`. Mirror this locally to produce the same `build/` layout.

Guidance for common edits
- Adding JavaScript: add a `<script src="main.js"></script>` before `</body>` and update the `build` copy step to include `*.js`.
- Changing CI expectations: if you modify the page structure (remove the `<button>`), update [.github/workflows/ci-cd.yml](.github/workflows/ci-cd.yml) so the `test` job checks for the new element or behaviour.
- Adding components or folders: preserve relative paths in `index.html` (assets are expected at repository root unless the workflow copy step is changed).

Examples from this codebase
- CI check (current): [.github/workflows/ci-cd.yml](.github/workflows/ci-cd.yml) -> the job that greps for `"<button"` in `index.html`.
- Build step (current): [.github/workflows/ci-cd.yml](.github/workflows/ci-cd.yml) -> `cp index.html style.css *.png *.jpg build/`.

What the AI should NOT assume
- There is no bundler, transpiler, or test framework configured — do not add assumptions about `npm`, `node`, `webpack`, or `pytest` unless you also add/update project config and CI to match.

If you need clarification
- Ask which files to include in `build/`, whether to broaden CI tests, or whether to introduce a package manager and automated lint/test tooling.

Next step for maintainers
- If you want richer CI/tests or a JS toolchain, indicate preferred tooling and I'll propose concrete workflow and repo changes.
