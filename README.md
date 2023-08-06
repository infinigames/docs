# Dimensium Docs

**This is the documentation site of dimensium!!! If you want to visit our news/blogs site's repo, please click [this](https://github.com/dimensium/dimensium.github.io/).**

This page uses [mkdocs] with [material theme] to generate pages automatically using a [workflow], which will generate the whole documentation site on any push commits and pull requests, and then, we set the page will deploy from a branch called [gh-pages], the root directory of that branch is our doc site's root directory.

The site previously used a jekyll theme called "[just the docs]" to provide similar documentation page automation deployments. But because [mkdocs] is cleaner to config, and the prettiness the [material theme] provided, we choosed to use mkdocs to build the site.

It's simple to edit locally and let actions to run the generation job. If you want to build and preview it locally. Clone it, and run following command:

```
mkdocs build
```

This will build the docs. To preview it locally:

```
mkdocs serve
```

And open the link `https://localhost:your_port_to_previewing` to view it in your browser.


[mkdocs]: https://github.com/mkdocs/mkdocs
[material theme]: https://github.com/squidfunk/mkdocs-material
[workflow]: https://github.com/dimensium/docs/actions
[gh-pages]: https://github.com/dimensium/docs/tree/gh-pages
