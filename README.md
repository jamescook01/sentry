# Sentry Documentation

The Sentry documentation is a static site, generated by [Jekyll][jekyll] and [Gatsby][gatsby].

## Getting started

You will need [Ruby][ruby], [Bundler][bundler], and [Volta][volta] installed. If you don't have opinions about the process, this will get you going:

```bash
# Install Homebrew and everything mentioned above
$ bin/bootstrap
```

Once you have the required system dependencies:

```bash
# Install or update application dependencies
$ make
```

Now run the development webserver:

```bash
$ yarn start
```

You will now be able to access docs via http://localhost:3000.

Note: This is running both the Jekyll (port 9001) and Gatsby (port 9002) servers, with a proxy routing requests between the two.

[jekyll]: https://jekyllrb.com/
[gatsby]: https://gatsbyjs.org
[ruby]: https://www.ruby-lang.org/
[bundler]: http://bundler.io/
[volta]: https://volta.sh/

## The Great Gatsby Migration

The repository currently contains a Jekyll site (`./`) as well as a Gatsby site (`./gatsby`). This is to aid with a progressive migratiopn over to Gatsby. There's a bit of magic that you need to understand in how this is deployed:

- **Sidebar:** Jekyll and Gatsby _both_ control the sidebar. If you add a new page in Gatsby at the top level (aka `/docs/parent/THISLEVEL.md`) you will also need to add an empty page in Jekyll at the same location with the `title`, `sidebar_order`, and `gatsby` frontmatter elements:

  ```yaml
  ---
  title: Page Name
  sidebar_order: 0
  gatsby: true
  ---
  ```

- When you convert a page from Jekyll to Gatsby you should remove all text contents of the document in Jekyll (this makes it clear its not used), and add `gatsby: true` to the frontmatter. This ensures it still renders in the sidebar, but both systems recognize the content is in Gatsby.

- In production, the `nginx.conf` file determines which routes are served by Jekyll vs Gatsby. Gatsby is the default behavior, whereas Jekyll routes are listed explicitly.

- In development, the proxy server (`server.js`) manages routing between Jekyll (http://localhost:9001) and Gatsby (http://localhost:9002). Route configuration is the same as whats in nginx.

- We've disabled the default optimized static links within Gatsby due to the complexity of navigating between two codebases. Once the Jekyll conversion is done, we'll want to make the change in `<SmartLink>`.

- We're automatically importing _all_ Jekyll pages inside of Gatsby. They won't all work. When formally converting a page over to Gatsby you should move it into a parallel location inside of the Gatsby file tree (`./src/docs/...`). You also like want to convert it to `.mdx` instead of `.md`. (DC: you will need to update `gatsby-node.js` to handle indexing `.mdx` once the first is added!)

- This is likely brittle. We want to move away from Jekyll as quickly as possible, so ideally all new content is done in Gatsby.

- You can determine which engine is used by viewing source and looking for `"Rendered with"` in the HTML.

## MDX Components + Markdown

:pray: that MDX v2 fixes this.

MDX has its flaws. When rendering components, any text inside of them is treated as raw text (_not_ markdown). To work around this you can use the `<markdown>` tag, but it also has its issues. Generally speaking, put an empty line after the opening tag, and before the closing tag.

```jsx
// dont do this as parsing will hit weird breakages
<markdown>
foo bar
</markdown>
```

```jsx
// do this
<markdown>

foo bar

</markdown>
```

## Development mode (Gatsby-specific)

The following instructions assume you're in `./gatsby`.

Nothing to really see here yet. We'll update this when the Jekyll wrappers are gone.

```bash
$ yarn start
```

## Development mode (Jekyll-specific)

There are a number of enhancements in place that are only available when developing locally.

- Numerous optimizations have been made to speed up builds. See [environment-variables] for more info.
- Pressing the `` ` `` (backtick) key reveals a drawer with meta info for each page.
- The sidebar includes links to documentation of special components available to the docs.

### Environment Variables

The following variables can be set when using `bin/server` (aka `yarn start-jekyll`). Defaults prefer performance.

- `FAST_BUILD=true` The master switch. When true, all performance adjustments are enabled. Disable to debug strange behavior.
- `JEKYLL_INCREMENTAL=true` When true, Jekyll only rebuilds pages that have changed. In some cases this can create [unpredictable behavior](https://jekyllrb.com/docs/configuration/#incremental-regeneration).
- `JEKYLL_ENABLE_PLATFORM_API=false` When false, `_platforms/*` will not be generated.

## Markdown documentation

Documentation is written in Markdown, which is parsed by [kramdown](https://kramdown.gettalong.org/).

[<kbd>Read the quick reference</kbd>](https://kramdown.gettalong.org/quickref.html)

While the server is running, you can also view the [Markdown styleguide](http://0.0.0.0:9000/markdown-styleguide/links/)

## Custom components

While in dev mode, the sidebar includes documentation for the special tags and includes available for formatting content.

## Adding content

The Sentry documentation contains two document types: categories and documents. Documents may have child documents.

### Add a category

Categories are manually defined.

1. Add a new entry to _src/data/documentation_categories.yml_. Order is defined by the order in this document.

```
    # Display name for the category
  - title: Category Name

    # slug matching a filename in _includes/svg/
    icon: category-icon

    # A slug for the category. Must match a file or folder in collections/_documentation/
    slug: category-slug
```

1. Create a markdown file or a folder of markdown files in collections/_documentation/

### Add a document

Category hierarchy is automatically defined by folder structure. Add a markdown file to a folder within _src/collections/_documentation_. Order is alphabetical by default, or you may force sorting order by adding an `sidebar_order` integer value in the frontmatter of the document.

## Updating Test Snapshots

When adding new pages, use `bin/test -u` to update test snapshots.
