# Site automation using GitHub Actions

We use [GitHub Actions] to automate the site building and deploying. GitHub Actions uses YAML to configure the automation steps and running ways, and the whole deployment process seemed like this:

1. You pushed (a) new commit(s) to this repository, using `git push -u origin master` to your fork and creating a pull request;
2. The [action file](#action-file) makes the action started to build the whole site with your new/modified file, and created a push to another branch called `gh-pages`;
3. The repository settings makes this site deployed from the `gh-pages` branch, when that [action file](#action-file) completed building the site, another action called `pages build and deployment` automatically runs and deploys the generated site to the target, i.e. `https://dimensium.github.io/docs`.


## Site building automation

A workflow written in YAML named `page-building.yml` is in the `.github/workflows/` directory.

This is the workflow:

```yaml 
name: Page buildings
on:
  push:
    branches:
      - master
      - main
permissions:
  contents: write
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-python@v4
        with:
          python-version: 3.x
      - run: echo "cache_id=$(date --utc '+%V')" >> $GITHUB_ENV 


      - uses: actions/cache@v3
        with:
          key: mkdocs-material-${{ env.cache_id }}
          path: .cache
          restore-keys: |
            mkdocs-material-
      - run: pip install mkdocs-material mkdocs-git-revision-date-localized-plugin jieba pillow cairosvg



      - run: mkdocs gh-deploy --force
```
{: #action-file}
<!-- please notice, the attribute list must put after the code block. previously this attribute list was incorrectly put before this code block, which causes to wrong outputs. -->

It defines a workflow called `Page buildings`, when there are fresh pushes on main/master repository (the master branch will shortly renamed to "main"), this action starts on the latest ubuntu linux, changes the environment variable, install packages we needed, and `mkdocs gh-deploy --force` deploys the docs to the `gh-pages` branch.

## Problems with multi-language selector and it's solution

Although the discussion [#2346](https://github.com/squidfunk/mkdocs-material/discussions/2346) provided a solution for multi-language support in one git repository, we choosed to keep it single-language but provided a language switching button which is configurable in `mkdocs.yml`, to let the multi-language support elegant and flexible.

To add your translations, just:

1. Simply fork this repository, changes the `en` in setting to your language code [^1], translate the document while lefting un-translated pages in English.

2. Add your link [^2] in the config file following the comment provided in config file. 

[^1]:  The language code reference is available in <https://squidfunk.github.io/mkdocs-material/setup/changing-the-language/#site-language>.

[^2]:  Best deployed in [GitHub Pages]. It's documentation is available in <https://docs.github.com/en/pages>.

[GitHub Actions]: https://github.com/features/actions
[GitHub Pages]: https://pages.github.cpm


