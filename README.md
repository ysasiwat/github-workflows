<a id="readme-top"></a>



<!-- PROJECT LOGO -->
<br />
<div align="center">
  <h3 align="center">GitHub Workflows</h3>

<!-- PROJECT SHIELDS — keep the blank lines around this block and no indent -->
[![Contributors][contributors-shield]][contributors-url]
[![Forks][forks-shield]][forks-url]
[![Stargazers][stars-shield]][stars-url]
[![Issues][issues-shield]][issues-url]
[![License][license-shield]][license-url]

  <p align="center">
    Reusable GitHub Actions workflows that check a repo's version and release it from its changelog.
    <br />
    <a href="https://github.com/ysasiwat/github-workflows"><strong>Explore the docs »</strong></a>
  </p>
</div>



<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#about-the-project">About The Project</a>
      <ul>
        <li><a href="#features">Features</a></li>
        <li><a href="#built-with">Built With</a></li>
      </ul>
    </li>
    <li><a href="#quick-start">Quick Start</a></li>
    <li><a href="#usage">Usage</a></li>
    <li><a href="#configuration">Configuration</a></li>
    <li><a href="#troubleshooting">Troubleshooting</a></li>
    <li><a href="#roadmap">Roadmap</a></li>
    <li><a href="#contributing">Contributing</a></li>
    <li><a href="#license">License</a></li>
  </ol>
</details>



<!-- ABOUT THE PROJECT -->
## About The Project

Every repo of mine keeps its version in `VERSION` and its history in `CHANGELOG.md`, and
each used to carry its own copy of the same CI. This repo holds that CI once; other repos
call it with a short caller workflow pinned to `@v1`.

Here's why:

* One place to fix the version and release workflows
* Each repo keeps only `scripts/version.sh` and a caller file of about 20 lines
* A release is a pushed tag; its notes come from the changelog, not by hand

<p align="right">(<a href="#readme-top">back to top</a>)</p>



### Features

- **✅ Version check:** fails a push or pull request when `VERSION`, its `CHANGELOG.md`
  section and the compare links disagree
- **🏷️ Release from a tag:** on a `vX.Y.Z` tag, checks the tag matches `VERSION` and makes
  the GitHub release from that version's changelog section
- **🔖 Version markers:** optionally keeps a `name vX.Y.Z` marker in other files in step
- **↪️ Major tag:** can move `v1` to each new `v1.x.y` release

<p align="right">(<a href="#readme-top">back to top</a>)</p>



### Built With

* [![GitHub Actions][Actions-shield]][Actions-url]
* [![Bash][Bash-shield]][Bash-url]

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- QUICK START -->
## Quick Start

In the root of a repo that has a `VERSION` file and a Keep a Changelog `CHANGELOG.md`:

```sh
mkdir -p scripts .github/workflows
curl -fsSLo scripts/version.sh https://raw.githubusercontent.com/ysasiwat/github-workflows/v1/scripts/version.sh
curl -fsSLo .github/workflows/version.yml https://raw.githubusercontent.com/ysasiwat/github-workflows/v1/examples/version.yml
bash scripts/version.sh check
```

Then set `branches:` in `.github/workflows/version.yml` to the repo's default branch.
The `setup-github-workflows` skill in my dotfiles does these steps and converts an older
changelog.

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- USAGE EXAMPLES -->
## Usage

Write changes under `## [Unreleased]` in `CHANGELOG.md` as you go. To release:

```sh
scripts/version.sh bump 1.2.0   # moves [Unreleased] under 1.2.0, sets VERSION and the compare links
scripts/version.sh check        # the same check CI runs on every push and pull request
git commit -am "chore(release): 1.2.0"
git tag -a v1.2.0 -m v1.2.0
git push origin main v1.2.0
```

Pushing the tag runs the release job. `scripts/version.sh notes [X.Y.Z]` prints the notes it
will use.

### Releasing this repo

The same steps. This repo calls its own workflows by local path, so a change is tested in
the push that makes it, and its release job moves the `v1` tag. A breaking change to a
workflow's inputs or behaviour gets `v2.0.0`, and callers move to `@v2` when they are ready.

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- CONFIGURATION -->
## Configuration

Workflow inputs:

| Workflow | Input | Type | Default | Description |
|----------|-------|------|---------|-------------|
| `version-check.yml`, `release.yml` | `script` | string | `scripts/version.sh` | Path of the version script in the calling repo |
| `release.yml` | `move-major-tag` | boolean | `false` | After the release, point the major tag at the released commit |

The release job needs `permissions: contents: write` in the caller, as in
[examples/version.yml](examples/version.yml): a called workflow cannot raise the token's
permissions.

### Version markers

To keep a version marker in other files, add `scripts/version.conf` next to the script:

```sh
MARKER_FILES=(README.md docs/index.md)
MARKER_PREFIX='my-tool v'
```

`check` then fails when a file lacks `my-tool v<VERSION>`, and `bump` rewrites the marker.
Keep `version.sh` itself identical to this repo's copy.

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- TROUBLESHOOTING -->
## Troubleshooting

**`tag v1.2.0 does not match VERSION 1.1.0`**

The tag was pushed before the bump commit. Delete the tag, run `bump`, commit, and tag again.

**`CHANGELOG.md: [Unreleased] is empty, write the changes there first`**

`bump` moves the entries under `[Unreleased]`, so write them before bumping.

**The release job fails before it starts with a permissions error**

The caller's `release` job lacks `permissions: contents: write`.

**Still stuck?** Search the [open issues](https://github.com/ysasiwat/github-workflows/issues).

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- ROADMAP -->
## Roadmap

- [ ] Offline link check (lychee) as a reusable workflow

See the [open issues](https://github.com/ysasiwat/github-workflows/issues) for a full
list of proposed features (and known issues).

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- CONTRIBUTING -->
## Contributing

Contributions are what make the open source community an amazing place to learn,
inspire, and create. Any contributions you make are **greatly appreciated**.

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- LICENSE -->
## License

Distributed under the MIT License. See `LICENSE` for more information.

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- MARKDOWN LINKS & IMAGES -->
[contributors-shield]: https://img.shields.io/github/contributors/ysasiwat/github-workflows.svg?style=for-the-badge
[contributors-url]: https://github.com/ysasiwat/github-workflows/graphs/contributors
[forks-shield]: https://img.shields.io/github/forks/ysasiwat/github-workflows.svg?style=for-the-badge
[forks-url]: https://github.com/ysasiwat/github-workflows/network/members
[stars-shield]: https://img.shields.io/github/stars/ysasiwat/github-workflows.svg?style=for-the-badge
[stars-url]: https://github.com/ysasiwat/github-workflows/stargazers
[issues-shield]: https://img.shields.io/github/issues/ysasiwat/github-workflows.svg?style=for-the-badge
[issues-url]: https://github.com/ysasiwat/github-workflows/issues
[license-shield]: https://img.shields.io/github/license/ysasiwat/github-workflows.svg?style=for-the-badge
[license-url]: https://github.com/ysasiwat/github-workflows/blob/main/LICENSE
[Actions-shield]: https://img.shields.io/badge/GitHub_Actions-2088FF?style=for-the-badge&logo=githubactions&logoColor=white
[Actions-url]: https://docs.github.com/actions
[Bash-shield]: https://img.shields.io/badge/Bash-4EAA25?style=for-the-badge&logo=gnubash&logoColor=white
[Bash-url]: https://www.gnu.org/software/bash/
