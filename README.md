# Elixir School

> Lessons about the Elixir programming language, inspired by Twitter's [Scala School](http://twitter.github.io/scala_school/).

Lessons can now be viewed on [ElixirSchool.com](https://elixirschool.com).

_Feedback and participation is welcome. Please see [Contributing](CONTRIBUTING.md) for more details on how to get involved._

### Running

[ElixirSchool.com](https://elixirschool.com) is generated using [Jekyll](https://github.com/jekyll/jekyll).  To run locally you need both Ruby and Bundler installed.

1. Install dependencies:

	```shell
	$ bundle install
	```

1. Update `url` in `_config.yml` to match your machine:

  ```yaml
  title: Elixir School
  description: Lessons about the Elixir programming language
  baseurl: /
  url: http://localhost:4000
  ```

1. Run Jekyll:

	```shell
	$ bundle exec jekyll s
	```

1. Read it at [http://localhost:4000](http://localhost:4000)

### Translating

In addition to the steps above there are a few addition steps required for translation.

#### New Language

1. Create a folder using the 2 character code (e.g. ja, en, es, etc) with lesson subfolders:

  ```shell
  $ cd elixirschool
  $ mkdir -p ja/lessons/{basics,advanced,specifics,libraries}
  $ touch ja/lessons/{basics,advanced,specifics,libraries}/.gitignore
  ```

1. Update `_config.yml` by including the 2 character code in `languages` and adding translations to `sections`, `description` and `toc`:

  ```yaml
  languages: ['en', 'ja']
  default_lang: en
  exclude_from_localization: []
  sections:
    - tag: basics
      label:
        en: Basics
        ja: 基本

  description:
    en: Lessons about the Elixir programming language
    ja: プログラミング言語Elixirのレッスン

  toc:
    en: Table of Contents
    ja: 目次
  ```

1. If the new language is RTL (right-to-left) it should also be added to the `rtl_languages` list:

  ```yaml
  rtl_languages: ['ar']
  ```

1. Add it to list in `index.md`:

  ```markdown
  Available in [Việt ngữ][vi], [汉语][cn], [Español][es], [Slovenčina][sk], [日本語][ja], [Polski][pl] [Português][pt], [Русском][ru] and [Bahasa Melayu][ms] and other.
  ```

#### Translated Lesson

1. Translated lessons must include the page metadata.
   * `title` should be a translation of the original lesson's `title`.
   * `version` should consist of three digits: `major.minor.patch`, so:
     * if this is a initial lesson translation, the version should be set to `1.0.0`;
     * if you apply the original lesson updates to the translation, the version should be copied from the corresponding state of the original lesson;
     * else bump one of the version numbers depending on how important is your change.
   * `redirect_from` should be removed since it is used only for English lessons that earlier were hosted in a separate folder without a language prefix.

   For example `/ja/lessons/basics/basics.md`:

  ```yaml
  ---
  title: 基本
  version: 1.0.0
  ---
  ```
