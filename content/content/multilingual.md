---
date: 2016-01-02T21:21:00Z
menu:
  main:
    parent: content
prev: /content/markdown-extras
next: /content/example
title: 多國語言 Multilingual Mode
weight: 68
toc: true
---
雨果支持多語言並行（雨果 v0.17 新增）。
在最上層的 `config.toml`（或等量值）中定義 `Languages` 可用的語言部分。

Hugo supports multiple languages side-by-side (added in `Hugo 0.17`). Define the available languages in a `Languages` section in your top-level `config.toml` (or equivalent).

範例：

Example:

```
DefaultContentLanguage = "en"
copyright = "Everything is mine"

[params.navigation]
help  = "Help"

[Languages]
[Languages.en]
title = "My blog"
weight = 1
[Languages.en.params]
linkedin = "english-link"

[Languages.fr]
copyright = "Tout est à moi"
title = "Mon blog"
weight = 2
[Languages.fr.params]
linkedin = "lien-francais"
[Languages.fr.navigation]
help  = "Aide"

```

任何在 `[Languages]` 中未定義的值將回追至全域中找尋（像是此範例的 `copyright` 在英語（`en`）的情況）。


Anything not defined in a `[Languages]` block will fall back to the global
value for that key (like `copyright` for the English (`en`) language in this example).

在上面的配置，所有的內容、sitemap、RSS feeds、paginations
和 taxonomy 頁面都將被渲染成 `/` 下使用英文（你的預設語言），
而 `/fr` 下使用法語。

With the config above, all content, sitemap, RSS feeds, paginations
and taxonomy pages will be rendered below `/` in English (your default content language), and below `/fr` in French.

When working with params in frontmatter pages, omit the `params` in the key for the translation.

If you want all of the languages to be put below their respective language code, enable `defaultContentLanguageInSubdir: true` in your configuration.

只有明顯不是全域的選項能被語言選項覆蓋。
全域選項例如有 `BaseURL`、`BuildDrafts`、... 等等。

Only the obvious non-global options can be overridden per language. Examples of global options are `BaseURL`, `BuildDrafts`, etc.

Taxonomies 和 Blackfriday 也能被依語言而有所不同，例如：

Taxonomies and Blackfriday configuration can also be set per language, example:

```
[Taxonomies]
tag = "tags"

[blackfriday]
angledQuotes = true
hrefTargetBlank = true

[Languages]
[Languages.en]
weight = 1
title = "English"
[Languages.en.blackfriday]
angledQuotes = false

[Languages.fr]
weight = 2
title = "Français"
[Languages.fr.Taxonomies]
plaque = "plaques"
```


### 翻譯你的內容 Translating your content

翻譯的文章是藉由檔名來辨別。

Translated articles are identified by the name of the content file.

翻譯文章的範例：

Example of translated articles:

1. `/content/about.en.md`
2. `/content/about.fr.md`

你也能使用：

You can also have:

1. `/content/about.md`
2. `/content/about.fr.md`

In which case the config variable `defaultContentLanguage` will be used to affect the default language `about.md`.  This way, you can
slowly start to translate your current content without having to rename everything.

如果保留不修改，`defaultContentLanguage` 預設值為 `en`。

If left unspecified, the value for `defaultContentLanguage` defaults to `en`.

藉由相同的 _基本檔名_ ，內容片段和翻譯片段能因此被結合。

By having the same _base file name_, the content pieces are linked together as translated pieces.

### 連結翻譯內容 Link to translated content

To create a list of links to translated content, use a template similar to this:

```
{{ if .IsTranslated }}
<h4>{{ i18n "translations" }}</h4>
<ul>
    {{ range .Translations }}
    <li>
        <a href="{{ .Permalink }}">{{ .Lang }}: {{ .Title }}{{ if .IsPage }} ({{ i18n "wordCount" . }}){{ end }}</a>
    </li>
    {{ end}}
</ul>
{{ end }}
```
The above can be put in a `partial` and included in any template, be it for a content page or the home page.  It will not print anything if there are no translations for a given page, or if it is -- in the case of the home page, section listing etc. -- a site with only one language.

The above also uses the `i18n` func, see [Translation of strings](#translation-of-strings).

### Translation of strings

Hugo uses [go-i18n](https://github.com/nicksnyder/go-i18n) to support string translations.  Follow the link to find tools to manage your translation workflows.

Translations are collected from the `themes/[name]/i18n/` folder (built into the theme), as well as translations present in `i18n/` at the root of your project.  In the `i18n`, the translations will be merged and take precedence over what is in the theme folder.  Language files should be named according to RFC 5646  with names such as `en-US.yaml`, `fr.yaml`, etc.

From within your templates, use the `i18n` function like this:

```
{{ i18n "home" }}
```

This uses a definition like this one in `i18n/en-US.yaml`:

```
- id: home
  translation: "Home"
```

Often you will want to use to the page variables in the translations strings. To do that, pass on the "." context when calling `i18n`:

```
{{ i18n "wordCount" . }}
```

This uses a definition like this one in `i18n/en-US.yaml`:

```
- id: wordCount
  translation: "This article has {{ .WordCount }} words."
```
An example of singular and plural form:

```
- id: readingTime
  translation:
    one: "One minute read"
    other: "{{.Count}} minutes read"
```
And then in the template:

```
{{ i18n "readingTime" .ReadingTime }}
```
To track down missing translation strings, run Hugo with the `--i18n-warnings` flag:

```bash
 hugo --i18n-warnings | grep i18n
i18n|MISSING_TRANSLATION|en|wordCount
```

### Menus

You can define your menus for each language independently. The [creation of a menu]({{< relref "extras/menus.md" >}}) works analogous to earlier versions of Hugo, except that they have to be defined in their language-specific block in the configuration file:

```toml
defaultContentLanguage = "en"

[languages.en]
weight = 0
languageName = "English"

[[languages.en.menu.main]]
url    = "/"
name   = "Home"
weight = 0


[languages.de]
weight = 10
languageName = "Deutsch"

[[languages.de.menu.main]]
url    = "/"
name   = "Startseite"
weight = 0
```

The rendering of the main navigation works as usual. `.Site.Menus` will just contain the menu of the current language. Pay attention to the generation of the menu links. `absLangURL` takes care that you link to the correct locale of your website. Otherwise, both menu entries would link to the English version because it's the default content language that resides in the root directory.

```html
<ul>
    {{- $currentPage := . -}}
    {{ range .Site.Menus.main -}}
    <li class="{{ if $currentPage.IsMenuCurrent "main" . }}active{{ end }}">
        <a href="{{ .URL | absLangURL }}">{{ .Name }}</a>
    </li>
    {{- end }}
</ul>

```

### Missing translations

If a string does not have a translation for the current language, Hugo will use the value from the default language. If no default value is set, an empty string will be shown.

While translating a Hugo site, it can be handy to have a visual indicator of missing translations. The `EnableMissingTranslationPlaceholders` config option will flag all untranslated strings with the placeholder `[i18n] identifier`, where `identifier` is the id of the missing translation.

**Remember: Hugo will generate your website with these placeholders. It might not be suited for production environments.**

### Multilingual Themes support

To support Multilingual mode in your themes, some considerations must be taken for the URLs in the templates. If there are more than one language, URLs  must either  come from the built-in `.Permalink` or `.URL`, be constructed with `relLangURL` or `absLangURL` template funcs -- or prefixed with `{{.LanguagePrefix }}`.

If there are more than one language defined, the`LanguagePrefix` variable will equal `"/en"` (or whatever your `CurrentLanguage` is). If not enabled, it will be an empty string, so it is harmless for single-language sites.
