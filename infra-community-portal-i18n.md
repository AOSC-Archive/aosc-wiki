---
title: Community Portal Translation Guide
description: 
published: true
date: 2020-05-07T04:01:11.701Z
tags: 
editor: undefined
---

# Welcome

First, thanks for your interests in translating our website. You will find the instructions on how to add a new locale to the website below.

> This wiki article assumes you have read [the maintenance guide](https://wiki.aosc.io/en/infra-community-portal), please read it first if you haven't.
{.is-warning}

# Files to be Translated

There are two kinds of files need to be translated:

1. Template texts: Template translations are stored in `i18n/` folder
2. Website content texts: see below

The content files listed below need to be translated:

```
content/
├── about
│   └── _index.html
├── downloads
│   ├── _index.html
│   └── screenshots
│       └── _index.html
├── _index.html
├── mail
│   └── _index.html
├── news
│   ├── events.html
│   ├── gallery.html
│   ├── _index.html
│   ├── news.html
├── people
│   ├── _index.html
└── repo
    └── _index.html
```

Files **not** present on the list **should not be** translated

# How to Translate

## Enabling the support of your language

First, you need to determine the language code for your language. You can find the ISO 639-1 language code here: https://www.loc.gov/standards/iso639-2/php/code_list.php.

Open the `config.toml` and add the following under `[languages]`:

```toml
[languages.<language code>]
    languageName = "<name of your language in your language>"
    title = "<translation of the sentence 'AOSC Portal'>"
    weight = 3
```

For example, if you are doing the Russian translation, you will write something like this:

```toml
[languages.ru]
    languageName = "Русский"
    title = "AOSC Портал"
    weight = 3
```

## Translating the overall framework

1. Copy `i18n/en.toml` to `i18n/<language code you obtained>.toml`. For example, if your language has the language code `ja` then it will be `i18n/ja.toml`.

1. Now you can open this file in your favorite editor and translate the texts in it. Keep in mind if you encountered texts begin with `{{` and ends with `}}` (for example `{{ $.copyrightYear }}`) then please do not translate them, they have a special effect on the website.

## Translating the navigation bar

1. Copy `data/navigation.en.yml` to `data/navigation.<language code you obtained>.yml`

1. Now you can open this file in your favorite editor and translate the titles in it. Note that you need to change some of the `url` fields to match your language, for example:

Before translation:

```yaml
- title: News
  url: /news

- title: People
  url: /people
```

After translation:

```yaml
- title: Новости
  url: /ru/news

- title:  Участники
  url: /ru/people
```

## Translating the website contents

> This part is kind of difficult to translate. If you are having issues following the instructions below, please ask us in the Telegram channel.
{.is-info}

1. Copy all the files listed in [Files Need to be Translated](#files-need-to-be-translated) section to have the format `<original filename without extension>.<language code>.html`. For example copy `content/about/_index.html` to `content/about/_index.ru.html` if you are working with Russian translations
2. Edit the files you have copied in your favorite editor. Keep in mind if you encountered texts begin with `{{` and ends with `}}` (for example `{{< aosc-os-downloads >}}`) then please do not translate them, they have a special effect on the website.

# Preview Your Changes

Unlike what was mentioned in [the maintenance guide](https://wiki.aosc.io/en/infra-community-portal), you need to run `hugo server --disableFastRender` to preview your changes. This will make Hugo to load your translations properly.

After you are satisfied with your changes, please submit a Pull Request to https://github.com/AOSC-Dev/aosc-portal-kiss.github.io.