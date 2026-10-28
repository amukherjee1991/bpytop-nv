# Releasing bpytop-nv

Releases go to PyPI from GitHub Actions ([`.github/workflows/publish.yml`](../.github/workflows/publish.yml)) using [Trusted Publishing](https://docs.pypi.org/trusted-publishers/). No API token is stored anywhere.

## One-time setup

1. On PyPI, open [Publishing](https://pypi.org/manage/account/publishing/) and add a **pending publisher** with these values:
   - PyPI project name: `bpytop-nv`
   - Owner: `amukherjee1991`
   - Repository name: `bpytop-nv`
   - Workflow name: `publish.yml`
   - Environment name: `pypi`
2. On GitHub, create an environment named `pypi` (Settings → Environments). To have each upload wait for a manual approval, add yourself as a required reviewer.

## Each release

1. Set the same new version in both places:
   - `version` in `pyproject.toml`
   - `VERSION` in `bpytop_nv/__init__.py`
2. Commit and push to `main`. The workflow builds the package and checks it.
3. Publish a GitHub release with the tag `v<version>`, for example `v0.1.1`:

   ```sh
   gh release create v0.1.1 --generate-notes
   ```

   The workflow checks that the tag matches the version, builds the package, smoke-tests it and uploads it to PyPI.

PyPI never accepts the same version twice, so a failed release needs a new version number.
