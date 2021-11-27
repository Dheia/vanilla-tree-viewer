<div align="center">
  <h1>vanilla-tree-viewer</h1>

  <a href="https://github.com/abhchand/vanilla-tree-viewer">
    <img
      width="100"
      alt="binoculars"
      src="https://raw.githubusercontent.com/abhchand/vanilla-tree-viewer/master/meta/logo.png"
    />
  </a>

  <p>`VanillaTreeViewer` is a minimalist file browser for compactly displaying several files at once</p>
</div>

---

[![Build Status][ci-badge]][ci] [![NPM Version][npm-version-badge]][npm-version] [![MIT License][license-badge]][license] [![PRs Welcome][prs-badge]][prs]

[![Watch on GitHub][github-watch-badge]][github-watch]
[![Star on GitHub][github-star-badge]][github-star]
[![Tweet][twitter-badge]][twitter]

Show off multiple files or code snippets in an elegant and space saving way.

Perfect for blog posts ([like this one](https://abhchand.me/blog/use-react-in-rails-without-the-react-rails-gem)), tutorials, documentation, etc...

* [view a **live demo**](https://abhchand.me/vanilla-tree-viewer)
* [view this project on npm](https://www.npmjs.com/package/vanilla-tree-viewer)

<img src="meta/demo.png" />

# Table of Contents

- [Quick Start](#quick-start)
- [Syntax Highlighting](#syntax-highlighting)
  - [Default Language Support](#default-language-support)
  - [Highlighting Other Languages](#highlighting-other-languages)
- [Options](#options)
- [Customization](#customization)
  - [Configuring Width and Alignment](#configuring-width-and-alignment)
  - [Customizing Styling](#customizing-styling)
- [Development](#development)
- [Building Releases](#building-releases)
- [Issues / Contributing](#issues-contributing)
- [Changelog](#changelog)

# <a name="quick-start"></a>Quick Start

① Import the latest **script** and **styling** from our CDN ([See all available versions](https://cdn.jsdelivr.net/gh/abhchand/vanilla-tree-viewer@master/dist/))

```html
<head>
  <script type="text/javascript" src="https://cdn.jsdelivr.net/gh/abhchand/vanilla-tree-viewer@1.1.1/dist/index.min.js"></script>
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/abhchand/vanilla-tree-viewer@1.1.1/dist/main.min.css" >
</head>
```

② For each `VanillaTreeViewer` instance you'd like to create, define the list of files to be displayed as an HTML `list`. You **must** include the `.vtv` class.

(See [Options](#options) for a full list of `data-*` attribute options.)

```html
<ol class='vtv' data-language="javascript">

  <!-- File 1 -->
  <!-- Display the contents under the path `src/index.js` -->
  <li data-path="src/index.js" >
import Foo from './foo';
export { Foo.bar }
  </li>

  <!-- File 2 -->
  <!-- Fetch file contents from a `url` instead -->
  <!-- Override syntax highlighting (`data-language`) for this file -->
  <li
    data-path="package.json"
    data-url="https://raw.githubusercontent.com/axios/axios/master/package.json"
    data-language="json">
  </li>
</ol>
```

③ Finally, call `VanillaTreeViewer.renderAll()` after page load.

This will find and parse all `.vtv` elements and render a `VanillaTreeViewer` component at that location.

```html
<script>
  document.addEventListener('DOMContentLoaded', function() {
    VanillaTreeViewer.renderAll();
  }, false);
</script>
```

# <a name="syntax-highlighting"></a>Syntax Highlighting

`VanillaTreeViewer` uses the wonderful [highlight.js](https://highlightjs.org/) library for syntax highlighting.

See the [full list of language syntax definitions](https://cdnjs.com/libraries/highlight.js/10.4.1) supported by `highlight.js`.

## <a name="default-language-support"></a>Defalt Language Support

To keep the bundle size small, `VanillaTreeViewer` supports syntax highlighting for only the most common languages by default.

If you're highlighting files in any of these languages, there's no further action required.

> `bash`, `c`, `cpp`, `csharp`, `css`, `diff`, `go`, `java`, `javascript`, `json`, `makefile`, `xml`, `markdown`, `objectivec`, `php`, `php-template`, `plaintext`, `python`, `ruby`, `rust`, `scss`, `shell`, `sql`, `typescript`, `yaml`

## <a name="highlighting-other-languages"></a>Highlighting Other Languages

If you require syntax highlighting for any language not supported by default, you'll have to manually include the syntax definitions from `Highlight.js`. Don't worry, it's easy!

1. Find your language's syntax definitions from the [available list of language syntax definitions supported by `highlight.js`](https://cdnjs.com/libraries/highlight.js/10.4.1).
2. Then, add the `<script>` for your syntax definitions _after_ the `VanillaTreeViewer` `<script>`.

For example, highlighting `ActionScript`:

```html
<script type="text/javascript" src="https://cdn.jsdelivr.net/gh/abhchand/vanilla-tree-viewer@1.1.1/dist/index.min.js"></script>
<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/highlight.js/10.4.1/languages/actionscript.min.js"></script>
```

# <a name="options"></a>Options

`VanillaTreeViewer` uses HTML attributes on the parent and child nodes to configure behavior

```html
<!-- Parent node -->
<ol class="vtv">
  <!-- Child node -->
  <li data-path="src/index.js">
    <!-- File contents -->
  </li>
</ol>
```

The following attribute options are available:

| Attribute  | Type | Applies to | Required? | Default | Description
| ------------- | ------------- | ------------- | ------------- | ------------- | ------------- |
| `id` | `String` | parent node only | No | auto-generated | Each `VanillaTreeViewer` instance is automatically assigned a unique, sequential `id` (`vtv--1`, `vtv--2`, etc...). However, if you explicitly specify an `id` it will be preserved and used instead of the auto-generated value.
| `class` | `String` | parent node only | **Yes** | n/a | The class name `.vtv` **must** exist on each parent node. Optionally, if you specify any other custom classes they will also be preserved.
| `data-path`  | `String` | child node only | **Yes** | n/a | The path under which the file should be displayed in the viewer tree |
| `data-url`  | `String` | child node only | Yes (if no inline file contents specified) | `null` | The URL to fetch the file contents from (e.g. Github Raw URLs). Any inline file contents in the HTML always take precedence over `data-url`.|
| `data-selected` | `Boolean` | child node only | No | `false` | Indicates whether this file should be selected when the viewer loads. If more than one file is marked `data-selected=true`, the first one is chosen. Similarly, if no file is marked `data-selected=true`, the first file in the list will be selected by default.
| `data-language` | `String` | child or parent node | No | `null` | The `highlight.js` language to use for syntax highlighting. [See a full list of supported languages](https://github.com/highlightjs/highlight.js/tree/master/src/languages).
| `data-style` | `String` | child or parent node | No | `'monokai-sublime'` | The `highlight.js` style (color theme) to use for syntax highlighting. [See a full list of supported styles](https://github.com/highlightjs/highlight.js/tree/master/src/styles). (**NOTE**: The [`highlight.js` demo page](https://highlightjs.org/static/demo/) will let you preview various languages and styles.)


# <a name="customization"></a>Customization

### <a name="configuring-width-and-alignment"></a>Configuring Width and Alignment

All `VanillaTreeViewer` instances are wrapped in a containing `<div>` with a `.vtv-wrapper` class. It is recommended that you style this wrapper element accordingly to set the desired width and alignment.

For example:

```html
<style>
  .vtv-wrapper {
    margin: auto;
    max-width: 980px;
  }
</style>
```

### <a name="customizing-styling"></a>Customizing Styling

The default styling for `VanillaTreeViewer` is based off the look and feel of [Sublime Text](https://www.sublimetext.com/).

While you can change the `style`/theme for any specific file(s), `VanillaTreeViewer` does not provide a programmatic way to customize the component itself. However, you are free to customize the look and feel as needed by overriding [the CSS](https://cdn.jsdelivr.net/gh/abhchand/vanilla-tree-viewer@1.1.1/dist/main.min.css) at your discretion.

* All top-level CSS classes begin with `.vtv*`
* Please be aware that the default styling utilizes [media queries](https://www.w3schools.com/css/css_rwd_mediaqueries.asp) to apply styling at different screen widths.

# <a name="development"></a>Development

If you'd like to edit or develop the component locally, you can run:

```bash
git clone https://github.com/abhchand/vanilla-tree-viewer.git

yarn install
yarn run dev
```

This will open `http://localhost:3035` in a browser window. Any changes made to the `src/` or to the `demo/index.jsx` file will be hot reloaded.

# <a name="building-releases"></a>Building Releases

1. Install `np` globally: `yarn global add np`
2. For non-beta releases, manually update README and other version references to the latest (upcoming) version. Do not update `package.json` - that will be updated by `np` automatically.
3. Build: `yarn run build`
4. Commit the above changes and `git push` to `master`
5. Create new version: `np`

# <a name="issues-contributing"></a>Issues / Contributing

- If you have an issue or feature request, please [open an issue here](https://github.com/abhchand/vanilla-tree-viewer/issues/new).

- Contribution is encouraged! But please open an issue first to suggest a new feature and confirm that it will be accepted before creating a pull request.

You can also help support this project. If you've found this or any other of my projects useful, I would greatly appreciate if you could [buy me a coffee](https://www.buymeacoffee.com/abhchand)!

# <a name="changelog"></a>Changelog

See [release notes](https://github.com/abhchand/vanilla-tree-viewer/releases)

[ci-badge]:
  https://circleci.com/gh/abhchand/vanilla-tree-viewer/tree/master.svg?style=svg
[ci]:
  https://circleci.com/gh/abhchand/vanilla-tree-viewer/tree/master
[npm-version-badge]:
  https://img.shields.io/npm/v/vanilla-tree-viewer.svg?style=flat-square
[npm-version]:
  https://www.npmjs.com/package/vanilla-tree-viewer
[license-badge]:
  https://img.shields.io/npm/l/vanilla-tree-viewer.svg?style=flat-square
[license]:
  https://github.com/abhchand/vanilla-tree-viewer/blob/master/LICENSE
[prs-badge]:
  https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square
[prs]: http://makeapullrequest.com
[github-watch-badge]:
  https://img.shields.io/github/watchers/abhchand/vanilla-tree-viewer.svg?style=social
[github-watch]: https://github.com/abhchand/vanilla-tree-viewer/watchers
[github-star-badge]:
  https://img.shields.io/github/stars/abhchand/vanilla-tree-viewer.svg?style=social
[github-star]: https://github.com/abhchand/vanilla-tree-viewer/stargazers
[twitter]:
  https://twitter.com/intent/tweet?text=Check%20out%20vanilla-tree-viewer%20by%20%40YeaaaahBoiiii%20https%3A%2F%2Fgithub.com%2Fabhchand%2Fvanilla-tree-viewer%20%F0%9F%91%8D
[twitter-badge]:
  https://img.shields.io/twitter/url/https/github.com/abhchand/vanilla-tree-viewer.svg?style=social
