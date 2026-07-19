# VitruvianOS Wiki

Source for the [VitruvianOS documentation site](https://wiki.v-os.dev/).

This is a [Hugo](https://gohugo.io/) site built with the
[hugo-book](https://github.com/alex-shpak/hugo-book) theme. Content lives
in `content/docs/` as Markdown, organised by section (`getting-started`,
`development`, `reference`).


## Local preview

```bash
git clone https://github.com/VitruvianOS/vos-wiki.github.io.git
cd vos-wiki.github.io
git submodule update --init --recursive   # pulls the theme
hugo server
```

Then open <http://localhost:1313/>. Changes to Markdown files reload
automatically.

You need Hugo (extended is fine, not required). On Debian/Ubuntu:

```bash
sudo apt install hugo
```

macOS:

```bash
brew install hugo
```


## Contributing

Documentation contributions are welcome and generally the fastest way to
help the project. Anything is fair game: fixing a typo, correcting a
factual error, expanding a stub page, translating a section, or writing a
new topic from scratch.

The workflow is standard GitHub:

1. Fork this repository.
2. Create a branch (`docs/some-topic`).
3. Edit or add Markdown files under `content/docs/`.
4. Preview with `hugo server` if you can — it catches broken links and
   layout issues early.
5. Open a pull request against `main`.

You do not have to be a developer to contribute here. If you use
Vitruvian and something in the docs is wrong or missing, please file an
issue or a PR.


## Content guidelines

- **Be accurate**. If you're not sure a feature works the way you're
  describing, mark it as planned or open an issue instead of asserting
  it.
- **Be concise**. Short pages are easier to keep correct.
- **Prefer plain English**. The project is international.
- **Match the section**. Getting Started is for users. Development is
  for people building or hacking on Vitruvian. Reference is for API and
  system-level details.
- **Cross-link with `{{< relref >}}`**. This gets validated at build
  time and won't rot silently.
- **One topic per page**. If a page grows past a couple of screens,
  consider splitting it.

Front matter is minimal — just `title` and `weight` for the sidebar
order:

```markdown
---
title: "My Page"
weight: 5
---
```


## Reporting issues

Documentation issues, requests, and questions:
<https://github.com/VitruvianOS/vos-wiki.github.io/issues>

For issues about VitruvianOS itself (crashes, build errors, kernel
bugs), please use the main repo:
<https://github.com/VitruvianOS/Vitruvian/issues>


## License

Documentation content is released under the MIT License unless a page
specifies otherwise. By opening a pull request you agree to that.
