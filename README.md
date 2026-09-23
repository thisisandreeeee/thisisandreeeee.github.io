# thisisandreeeee.github.io

Personal website built with [Hugo](https://gohugo.io/) and the
[Introduction theme](https://github.com/victoriadrake/hugo-theme-introduction).

## Requirements

- Git
- [Hugo Extended](https://gohugo.io/installation/) 0.166 or newer
- [Node.js](https://nodejs.org/en/download) 22.12 or newer; Node.js 24 LTS is recommended

### Install with Homebrew (recommended on macOS)

Homebrew installs the required Hugo Extended edition:

```sh
brew install hugo
```

### Download Hugo Extended manually on macOS

1. Open the [latest Hugo release](https://github.com/gohugoio/hugo/releases/latest).
2. Expand **Assets** and download
   `hugo_extended_<version>_darwin-universal.pkg`. The universal package works
   on both Apple Silicon and Intel Macs.
3. Download `hugo_<version>_checksums.txt` from the same release.
4. In Terminal, calculate the package checksum:

   ```sh
   cd ~/Downloads
   shasum -a 256 hugo_extended_<version>_darwin-universal.pkg
   ```

5. Compare the result with the corresponding line in the downloaded checksum
   file. They must match exactly.
6. Double-click the `.pkg` file and follow the macOS installer.

For Linux or Windows, download the `hugo_extended` archive for your operating
system and architecture from the same release page, extract it, and place the
`hugo` executable somewhere on your `PATH`.

Verify the tools before continuing. The Hugo version must contain `extended`.

```sh
hugo version
node --version
npm --version
```

## First-time setup

If you are cloning the repository from scratch, include the theme submodule:

```sh
git clone --recurse-submodules git@github.com:thisisandreeeee/thisisandreeeee.github.io.git
cd thisisandreeeee.github.io
```

For an existing checkout, initialize or refresh the theme and install the locked
CSS dependencies:

```sh
git submodule update --init --recursive
npm ci
```

Use `npm ci` rather than `npm install` for routine setup. It installs the exact
versions recorded in `package-lock.json`.

## Preview the site locally

Start Hugo's development server:

```sh
npm run dev
```

Open <http://localhost:1313/>. Hugo watches the source files and refreshes the
page after saved changes. Press `Ctrl-C` in the terminal to stop the server.

If port 1313 is already occupied, choose another one:

```sh
npm run dev -- --port 1314
```

## Make changes

Homepage content lives in `content/home/`:

| File              | Purpose                      |
| ----------------- | ---------------------------- |
| `index.md`        | Main greeting and tagline    |
| `about.md`        | Biography and portrait       |
| `work.md`         | Working style and experience |
| `testimonials.md` | Recommendations              |
| `contact.md`      | Closing contact section      |

Each file starts with front matter between `---` lines. The `title` appears as
the section heading, while `weight` controls the section order.

Site-wide settings live in `hugo.toml`. Update that file for:

- Site title and search description
- Light, dark, or automatic theme selection
- Resume and social-profile links
- Footer text

Images used by a homepage section belong beside the Markdown files in
`content/home/`. Files that must retain an exact public URL, such as the Google
verification file, belong in `static/`.

Do not edit files inside `themes/introduction/`. That directory is a pinned Git
submodule; site-specific content and configuration should remain outside it.

## Test before committing

While the development server is running, check both a desktop-sized window and
a narrow mobile-sized window:

- The navigation reaches every section.
- The mobile menu opens and closes.
- The portrait and social icons load.
- The resume, LinkedIn, GitHub, and Medium links are correct.
- Light and dark modes are readable.
- The browser console has no errors.

Then create the same production build GitHub Pages will use:

```sh
npm run build
```

The build must finish without warnings or errors. Generated files are written
to `public/`; this directory is ignored by Git and must not be committed.

Review the changes and commit them normally:

```sh
git status
git diff
git add content hugo.toml static
git commit -m "Update website"
```

Include other intentionally changed files in `git add` when necessary.

## Repository structure

```text
.github/workflows/hugo.yaml  GitHub Pages build and deployment
content/home/                Homepage content and portrait
static/                      Files copied directly to the built site
themes/introduction/         Pinned theme submodule
hugo.toml                    Site and theme configuration
package.json                 Local commands and CSS dependencies
package-lock.json            Locked dependency versions
```

## Publishing checklist

Publishing is intentionally separate from local development. When the new site
has been reviewed and is ready to replace the current site:

1. Push the feature branch and open a pull request into `master`.
2. Confirm the pull-request build succeeds.
3. Review the generated site one final time.
4. Build locally again and merge the pull request.
5. In GitHub, open **Settings → Pages** and set **Source** to **GitHub Actions**.
6. Confirm the `Build and deploy Hugo site` workflow succeeds on `master`.
7. Verify <https://thisisandreeeee.github.io/> in a private browser window.

Until that checklist is performed, the existing GitHub Pages site remains the
live version.
