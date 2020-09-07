![npm](https://img.shields.io/npm/v/vuepress-markdown-it-wikilink?color=cb3837&logo=npm)
![npm collaborators](https://img.shields.io/npm/collaborators/vuepress-markdown-it-wikilink?color=cb3837&logo=npm)

# vuepress-markdown-it-wikilink

> [Wikimedia-style links](https://www.mediawiki.org/wiki/Help:Links#Internal_links) for [VuePress](https://vuepress.vuejs.org/) using the [markdown-it](https://github.com/markdown-it/markdown-it) parser.

_This plugin is intended to be used together with VuePress. If you are looking for a plugin to use for `markdown-it` only, please resolve to: [kwvanderlinde/markdown-it-wikilinks](https://github.com/kwvanderlinde/markdown-it-wikilinks)._

## Installation

Install this inside your VuePress project folder:

```bash
# with yarn
yarn add vuepress-markdown-it-wikilink

# with npm
npm i vuepress-markdown-it-wikilink
```

And in your VuePress configuration file (most often in `docs/.vuepress/config.js`):

```js
const wikilinks = require('vuepress-markdown-it-wikilink')({
  // ... options here ...
})

module.exports = {
  // ...
  markdown: {
    extendMarkdown: (md) => {
      md.use(wikilinks)
    },
  },
  // ...
}
```

## Options

> Options here defined will render as expected inside VuePress. Only the `<a></a>` tags are converted to `<router-link></router-link>` and `hrefs` are converted in format: `to="href"`.

| Option                      | Default value            | Note                                                              | Example                                                  |
| :-------------------------- | :----------------------- | :---------------------------------------------------------------- | :------------------------------------------------------- |
| `baseURL`                   | `/`                      | The base URL for absolute wiki links.                             | [#baseurl](#baseurl)                                     |
| `relativeBaseURL`           | `/`                      | The base URL for relative wiki links.                             | [#relativeBaseURL](#relativebaseurl)                     |
| `makeAllLinksAbsolute`      | false                    | Render all wiki links as absolute links.                          |                                                          |
| `uriSuffix`                 | `.html`                  | Append this suffix to every URL.                                  | [#uriSuffix](#urisuffix)                                 |
| `htmlAttributes`            | `{ class: 'wikilinks' }` | An object containing HTML attributes to be applied to every link. | [#htmlAttributes](#htmlattributes)                       |
| `generatePageNameFromLabel` |                          | Provide a custom page name generator.                             | [#generatePageNameFromLabel](#generatepagenamefromlabel) |
| `postProcessPageName`       |                          | A transform applied to every page name.                           | [#postProcessPageName](#postprocesspagename)             |
| `postProcessLabel`          |                          | A transform applied to every link label.                          | [#postProcessLabel](#postprocesslabel)                   |


### `baseURL`

```js
const html = require('markdown-it')()
  .use(require('vuepress-markdown-it-wikilink')({ baseURL: '/wiki/' }))
  .render('[[Main Page]]')
// <p><router-link to="./wiki/Main_Page.html">Main Page</router-link></p>
```

### `relativeBaseURL`

```js
const html = require('markdown-it')()
  .use(require('vuepress-markdown-it-wikilink')({ relativeBaseURL: '#', suffix: '' }))
  .render('[[Main Page]]')
// <p><router-link to="#Main_Page">Main Page</router-link></p>
```

### `uriSuffix`

```js
const html = require('markdown-it')()
  .use(require('vuepress-markdown-it-wikilink')({ uriSuffix: '.php' }))
  .render('[[Main Page]]')
// <p><router-link to="./Main_Page.php">Main Page</router-link></p>
```

### `htmlAttributes`

```js
const attrs = {
  class: 'wikilink',
  rel: 'nofollow',
}
const html = require('markdown-it')()
  .use(require('vuepress-markdown-it-wikilink')({ htmlAttributes: attrs }))
  .render('[[Main Page]]')
// <p><router-link to="./Main_Page.html" class="wikilink" rel="nofollow">Main Page</router-link></p>
```

### `generatePageNameFromLabel`

Unless otherwise specified, the labels of the links are used as the targets. This means that a non-[piped](https://meta.wikimedia.org/wiki/Help:Piped_link) link such as `[[Slate]]` will point to the `Slate` page on your website. But say you wanted a little more flexibility - like being able to have `[[Slate]]`, `[[slate]]`, `[[SLATE]]` and `[[Slate!]]` to all point to the same page. Well, you can do this by providing your own custom `generatePageNameFromLabel` function.

#### Example

```js
const _ = require('lodash')

function myCustomPageNameGenerator(label) {
  return label.split('/').map(function (pathSegment) {
    // clean up unwanted characters, normalize case and capitalize the first letter
    pathSegment = _.deburr(pathSegment)
    pathSegment = pathSegment.replace(/[^\w\s]/g, '')

    // normalize case
    pathSegment = _.capitalize(pathSegment.toLowerCase())

    return pathSegment
  })
}

const html = require('markdown-it')()
  .use(
    require('vuepress-markdown-it-wikilink')({
      generatePageNameFromLabel: myCustomPageNameGenerator,
    })
  )
  .render('Vive la [[révolution!]] VIVE LA [[RÉVOLUTION!!!]]')
// <p>Vive la <router-link to="./Revolution.html">révolution!</router-link> VIVE LA <router-link to="./Revolution.html">RÉVOLUTION!!!</router-link></p>
```

Please note that the `generatePageNameFromLabel` function does not get applied for [piped links](https://meta.wikimedia.org/wiki/Help:Piped_link) such as `[[/Misc/Cats/Slate|kitty]]` since those already come with a target.

### `postProcessPageName`

A transform applied to every page name. You can override it just like `generatePageNameFromLabel` (see above). The default transform does the following things:

- trim surrounding whitespace
- [sanitize](https://github.com/parshap/node-sanitize-filename) the string
- replace spaces with underscores

### `postProcessLabel`

A transform applied to every link label. You can override it just like `generatePageNameFromLabel` (see above). All the default transform does is trim surrounding whitespace.

## Credits

Based on [markdown-it-ins](https://github.com/markdown-it/markdown-it-ins) by Vitaly Puzrin, Alex Kocharin. Some adjustments are applied according to this fork: [ceilfors/markdown-it-wikilinks](https://github.com/ceilfors/markdown-it-wikilinks).

---

**vuepress-markdown-it-wikilink** ©Spencer Woo. Released under the [MIT License](./LICENSE).

Authored and maintained by Spencer Woo.

[@Portfolio](https://spencerwoo.com/) · [@Blog](https://blog.spencerwoo.com/) · [@GitHub](https://github.com/spencerwooo)
