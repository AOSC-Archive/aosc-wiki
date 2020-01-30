<!-- TITLE: Community Portal -->
<!-- SUBTITLE: Maintenance Notes for the Community Portal -->

# Basic Directory Structures

- Basic files
    - `Gemfile` Ruby dependencies
    - `index.html` Landing page
    - `build.sh` Build script
    - `404.html` Not found placeholder
    - `_config.yml` Configuration for Jekyll

- Directories containing HTML pages
    - `about`
    - `downloads`
    - `mail`
    - `news`
    - `people`
    - `repo`

- Utility directories for Jekyll
    - `_data` Datasets for different pages
    - `_sass` SCSS stylesheets
    - `_layouts` Page templates
    - `_post` News posts

- Utility directory

    - `tools` Contains tools for converting or migrating from old website
    - `daemon` Mirror information aggregator, provides API for querying mirror status(-es)

- Automatically generated directories
    - `_site` The final result
    - `assets/img/de-preview` Generated (downscaled) thumbnails
    - `vendor` Ruby dependencies and binaries

# Build the Pages Locally

- Required software packages
    1. `ruby`
    1. `ruby-bundler`
    1. `devel-base` (This is required for some Ruby Gems required by Jekyll)

- Run the following commands to install Jekyll (You can skip this step if you already got Jekyll installed)

```
bundle config set path vendor/bundle
bundle install
```

- Generate/Live Preview

To generate pages, run `build.sh`. If you want to design or modify the pages and see the modifications more quickly, you can run `bundle exec jekyll s` and follow the on-screen instructions.

- Mirror Information Aggregator

To use this, you need `Python 3` and after switching to the directory `daemon/` run `pip install -r requirements.txt` in a `venv`. Then execute it: `python3 watcher.py`.

# Adding New Posts

## Manually add new posts

Simply add a new file with the file name `YYYY-mm-dd-title.md` in the `_posts` directory.

For "front-matter" (the metadata at the top), here is an example which you may want to copy:

```
---
layout: post
title: 'Title'
type: news
important: false
---
```

Note that the `type` could be one of `news` or `community`.

## Using `jekyll-compose` plugin

TODO


# Publishing Your Changes

Simply push your commits to `master` branch. The deployment of the website is automated, you can see [the process here](https://dev.azure.com/AOSC-Dev/aosc-portal-kiss.github.io/_build?definitionId=1&_a=summary). If you don't have the permission to do so, you may open a new PR instead.