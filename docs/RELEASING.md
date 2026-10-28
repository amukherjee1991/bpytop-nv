# Releasing bpytop-nv

Releases go to PyPI from GitHub Actions ([`.github/workflows/publish.yml`](../.github/workflows/publish.yml)) using [Trusted Publishing](https://docs.pypi.org/trusted-publishers/). No API token is stored anywhere.

## Versioning

bpytop-nv uses Ubuntu-style versions: `YY.MM`, the year and month of the release. A second release in the same month adds a point number.

| Release | Version | Tag |
| --- | --- | --- |
| October 2026 | `26.10` | `v26.10` |
| A fix later in October 2026 | `26.10.1` | `v26.10.1` |
| April 2027 | `27.04` | `v27.04` |

PyPI drops leading zeros, so `27.04` appears there as `27.4`. Both `pip install bpytop-nv==27.04` and `==27.4` work. The workflow rejects any version that isn't `YY.MM` or `YY.MM.N`.

## One-time setup

1. On PyPI, open [Publishing](https://pypi.org/manage/account/publishing/) and add a **pending publisher** with these values:
   - PyPI project name: `bpytop-nv`
   - Owner: `amukherjee1991`
   - Repository name: `bpytop-nv`
   - Workflow name: `publish.yml`
   - Environment name: `pypi`
2. On GitHub, create an environment named `pypi` (Settings → Environments). To have each upload wait for a manual approval, add yourself as a required reviewer.

## Each release

1. Set the same new version (see [Versioning](#versioning)) in both places:
   - `version` in `pyproject.toml`
   - `VERSION` in `bpytop_nv/__init__.py`
2. Commit and push to `main`. The workflow builds the package and checks it.
3. Publish a GitHub release with the tag `v<version>`, for example `v26.10`:

   ```sh
   gh release create v26.10 --generate-notes
   ```

   The workflow checks that the tag matches the version, builds the package, smoke-tests it and uploads it to PyPI.

PyPI never accepts the same version twice, so a failed release needs a new version number.
