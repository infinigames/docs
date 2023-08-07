# Docs creation and configurations

We(I) created this site on my Windows computer using powershell, but commands are similar and usable on any operating system.

```powershell
mkdocs new docs
Set-Location docs/ # (1)!
git init
git status
```

1. In bash, it's `cd`.

Output should look like this:

```
INFO    -  Writing config file: docs\mkdocs.yml
INFO    -  Writing initial docs: docs\docs\index.md
Initialized empty Git repository in YourDrive:/path/to/docs/.git/
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        docs/
        mkdocs.yml

nothing added to commit but untracked files present (use "git add" to track)
```

After created empty repository on Github, type:

```powershell
git add .
git commit -m "Initialized docs"
Get-Content mkdocs.yml # (1)!
```

1. In bash, it's `cat`.

Output:
```yaml
site_name: My Docs
```

Configure `mkdocs.yml` like this:

```yaml

# site fundamental configurations.

site_name: "Dimensium Docs"
site_description: "The official documentation repository of Dimensium, using mkdocs with material theme for mkdocs."
site_author: "The Dimensium Software Organization"
site_url: "https://dimensium.github.io"
strict: false

# repository configurations.
repo_name: "dimensium/docs"
repo_url: "https://github.com/dimensium/docs"
edit_uri: "https://github.com/dimensium/docs/edit/master/docs"

# copyright.
copyright: "Copyright &copy; 2023 Dimensium Software Organization"

# favicon: assets/images/dimensium-logo.ico # enable it till we have the icon in .ico format.
```

These are fundamental configurations. Use these extensions to enable full power of markdown:

```yaml

markdown_extensions:
  - attr_list
  - pymdownx.emoji:
      emoji_index: !!python/name:materialx.emoji.twemoji
      emoji_generator: !!python/name:materialx.emoji.to_svg
  - abbr
  - admonition
  - attr_list
  - def_list
  - footnotes
  - md_in_html
  - toc:
      permalink: true
  - tables
  - pymdownx.arithmatex:
      generic: true
  - pymdownx.betterem
  - pymdownx.caret
  - pymdownx.mark
  - pymdownx.tilde
  - pymdownx.critic
  - pymdownx.details
  - pymdownx.highlight:
      anchor_linenums: true
  - pymdownx.superfences 
```

These features:

```yaml
theme:
  features:
    - navigation.instant
    - navigation.tabs
    - navigation.sections
    - navigation.indexes
    - navigation.footer

    - toc.integrate

    - content.action.edit
    - content.action.view

    - search.suggest
    - search.highlight
    - search.share
```

These plugins:

```yaml
# mkdocs material plugins.
plugins:
  
  - search

  - tags:
      tags_file: tags.md
    

  # revision date plugin, show the date when they edited and created on each page.
  - git-revision-date-localized:
      enable_creation_date: true
```


We use these theme settings:

```yaml
# theme settings.
theme:
  name: material
  lang: en
  palette:

    # Palette toggle for light mode
    - scheme: default
      primary: white
      accent: light blue
      toggle:
        icon: material/toggle-switch
        name: Switch to dark mode

    # Palette toggle for dark mode
    - scheme: slate
      primary: black
      accent: light blue
      toggle:
        icon: material/toggle-switch-off-outline
        name: Switch to light mode
  
  logo: assets/images/dimensium-logo.svg

  icon:
    repo: fontawesome/brands/github
    edit: material/pencil-ruler
    view: material/eye

```

Used `material/toggle-switch-*` as the scheme-changing button, used `material/pencil-ruler` and `material/eye` as the button icon for editing and viewing on GitHub. Used `fontawesome/brands/github` as the git repository icon on the top right.

