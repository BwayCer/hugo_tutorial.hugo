---
aliases:
- /doc/configuration/
lastmod: 2016-09-17
date: 2013-07-01
linktitle: 配置 Configuration
menu:
  main:
    parent: getting started
next: /overview/source-directory
toc: true
prev: /overview/usage
title: Configuring Hugo
weight: 40
---
The directory structure of a Hugo web site&mdash;or more precisely,
of the source files containing its content and templates&mdash;provide
most of the configuration information that Hugo needs.
Therefore, in essence,
many web sites wouldn't actually need a configuration file.
This is because Hugo is designed to recognize certain typical usage patterns
(and it expects them, by default).

Nevertheless, Hugo does search for a configuration file bearing
a particular name in the root of your web site's source directory.
First, it looks for a `./config.toml` file.
If that's not present, it will seek a `./config.yaml` file,
followed by a `./config.json` file.

In this `config` file for your web site,
you can include precise directions to Hugo regarding
how it should render your site, as well as define its menus,
and set various other site-wide parameters.

Another way that web site configuration can be accomplished is through
operating system environment variables.
For instance, the following command will work on Unix-like systems&mdash;it
sets a web site's title:
```bash
$ env HUGO_TITLE="Some Title" hugo
```
(**Note:** all such environment variable names must be prefixed with
<code>HUGO\_</code>.)

## 範例 Examples

Following is a typical example of a YAML configuration file.
Three periods end the document:

```yaml
---
baseURL: "http://yoursite.example.com/"
...
```
Following is an example TOML configuration file with some default values.
The values under `[params]` will populate the `.Site.Params` variable
for use in templates:

```toml
contentDir = "content"
layoutDir = "layouts"
publishDir = "public"
buildDrafts = false
baseURL = "http://yoursite.example.com/"
canonifyURLs = true

[taxonomies]
  category = "categories"
  tag = "tags"

[params]
  description = "Tesla's Awesome Hugo Site"
  author = "Nikola Tesla"
```
Here is a YAML configuration file which sets a few more options:

```yaml
---
baseURL: "http://yoursite.example.com/"
title: "Yoyodyne Widget Blogging"
footnoteReturnLinkContents: "↩"
permalinks:
  post: /:year/:month/:title/
params:
  Subtitle: "Spinning the cogs in the widgets"
  AuthorName: "John Doe"
  GitHubUser: "spf13"
  ListOfFoo:
    - "foo1"
    - "foo2"
  SidebarRecentLimit: 5
...
```
## Configuration variables

Following is a list of Hugo-defined variables you can configure,
along with their current, default values:

    ---
    archetypeDir:               "archetypes"
    # hostname (and path) to the root, e.g. http://spf13.com/
    baseURL:                    ""
    # include content marked as draft
    buildDrafts:                false
    # include content with publishdate in the future
    buildFuture:                false
    # include content already expired
    buildExpired:               false
    # enable this to make all relative URLs relative to content root. Note that this does not affect absolute URLs.
    relativeURLs:               false
    canonifyURLs:               false
    # config file (default is path/config.yaml|json|toml)
    config:                     "config.toml"
    contentDir:                 "content"
    dataDir:                    "data"
    defaultExtension:           "html"
    defaultLayout:              "post"
    # Missing translations will default to this content language
    defaultContentLanguage:     "en"
    # Renders the default content language in subdir, e.g. /en/. The root directory / will redirect to /en/
    defaultContentLanguageInSubdir: false
    # The below example will disable all page types and will render nothing.
    disableKinds = ["page", "home", "section", "taxonomy", "taxonomyTerm", "RSS", "sitemap", "robotsTXT", "404"]
    disableLiveReload:          false
    # Do not build RSS files
    disableRSS:                 false
    # Do not build Sitemap file
    disableSitemap:             false
    # Enable GitInfo feature
    enableGitInfo:              false
    # Build robots.txt file
    enableRobotsTXT:            false
    # Do not render 404 page
    disable404:                 false
    # Do not inject generator meta tag on homepage
    disableHugoGeneratorInject: false
    # edit new content with this editor, if provided
    editor:                     ""
    # Enable Emoji emoticons support for page content.
    # See www.emoji-cheat-sheet.com
    enableEmoji:				false
    # Show a placeholder instead of the default value or an empty string if a translation is missing
    enableMissingTranslationPlaceholders: false
    footnoteAnchorPrefix:       ""
    footnoteReturnLinkContents: ""
    # google analytics tracking id
    googleAnalytics:            ""
    languageCode:               ""
    layoutDir:                  "layouts"
    # Enable Logging
    log:                        false
    # Log File path (if set, logging enabled automatically)
    logFile:                    ""
    # "yaml", "toml", "json"
    metaDataFormat:             "toml"
    newContentEditor:           ""
    # Don't sync permission mode of files
    noChmod:                    false
    # Don't sync modification time of files
    noTimes:                    false
    paginate:                   10
    paginatePath:               "page"
    permalinks:
    # Pluralize titles in lists using inflect
    pluralizeListTitles:        true
    # Preserve special characters in taxonomy names ("Gérard Depardieu" vs "Gerard Depardieu")
    preserveTaxonomyNames:      false
    # filesystem path to write files to
    publishDir:                 "public"
    # enables syntax guessing for code fences without specified language
    pygmentsCodeFencesGuessSyntax: false
    # color-codes for highlighting derived from this style
    pygmentsStyle:              "monokai"
    # true: use pygments-css or false: color-codes directly
    pygmentsUseClasses:         false
    # maximum number of items in the RSS feed
    rssLimit:                   15
    # default sitemap configuration map
    sitemap:
    # filesystem path to read files relative from
    source:                     ""
    staticDir:                  "static"
    # display memory and timing of different steps of the program
    stepAnalysis:               false
    # theme to use (located by default in /themes/THEMENAME/)
    themesDir:                  "themes"
    theme:                      ""
    title:                      ""
    # if true, use /filename.html instead of /filename/
    uglyURLs:                   false
    # Do not make the url/path to lowercase
    disablePathToLower:         false
    # if true, auto-detect Chinese/Japanese/Korean Languages in the content. (.Summary and .WordCount can work properly in CJKLanguage)
    hasCJKLanguage:             false
    # verbose output
    verbose:                    false
    # verbose logging
    verboseLog:                 false
    # watch filesystem for changes and recreate as needed
    watch:                      true
    ---

### 整理中（譯者加）

```yaml
## 開發設定值

# newContentEditor： （預設值： ""） 使用此編輯器開啟新內容
newContentEditor: vim
# metaDataFormat： （預設值： "toml"） 文本頭部的中介資料格式，有效值： `yaml`, `toml`, `json`
#     此值功能是在 `hugo new file.md` 時設定前提內容格式，而非決定有效的格式
#     前提內容的有效格式是以開頭及結尾符做區別，`---` 為 `yaml`；`+++` 為 `toml`。
metaDataFormat: yaml
# verbose： （預設值： false） verbose output
verbose: true
# logFile： （預設值： ""） 日誌檔案寫入路徑，並啟用日誌
# verboseLog： （預設值： false） verbose logging
# disableLiveReload： （預設值： false） 是否禁用及時更新
#     https://gohugo.io/extras/livereload/
# noTimes： （預設值： false） 是否不同步文件修改（modification）時間


### 開發設定值 - 模板配置

# archetypeDir： （預設值： "archetypes"） 文本模型資料夾，見 https://gohugo.io/content/archetypes/
# contentDir： （預設值： "content"）
# dataDir： （預設值： "data"） 變量參數資料夾，見 https://gohugo.io/extras/datafiles/
# layoutDir： （預設值： "layouts"）
# staticDir： （預設值： "static"）
# themesDir： （預設值： "themes"） 主題風格資料夾路徑
# publishDir： （預設值： "public"） filesystem path to write files to
# paginate： （預設值： 10）
#     http://gohugo.io/extras/pagination/
# paginatePath： （預設值： "page"）
# permalinks： 固定連接，自定義路由
#     見 https://gohugo.io/extras/permalinks/
# defaultLayout： （預設值： "post"）


### 路由

# source： （預設值： ""） filesystem path to read files relative from
source: http://bwaycer.github/
# watch： （預設值： true） watch filesystem for changes and recreate as needed
# uglyURLs： （預設值： false） 是否顯示副檔名
# disablePathToLower： （預設值： false） 是否網址維持原始名稱而非統一小寫


### 網站資訊

# baseURL： （預設值： ""） 根的主機名或路徑
baseURL: https://bwaycer.github.io/about/
# languageCode： （預設值： ""）
languageCode: zh
# title： （預設值： ""） 網站名稱
title: 關於我
# theme： （預設值： ""） 採用的樣式名稱
theme: hugo-identity-theme # https://github.com/aerohub/hugo-identity-theme.git


### 多國語系

# defaultContentLanguage： （預設值： "en"） 缺少翻譯時默認使用此語言的內容
# defaultContentLanguageInSubdir： （預設值： false）
#     是否默認語言的內容被放置於相對的子路徑下（如：`/en/`），此路徑將自動重定向
# enableMissingTranslationPlaceholders： （預設值： false） 缺少翻譯時使用佔位符（placeholder）替代默認值或者空值
Languages:
    zh:
        - title: 關於我
          weight: 1
          params:
              - linkedin: english-link
    en:
        - title: My blog
          weight: 1
          params:
              - linkedin: english-link
# pygmentsCodeFencesGuessSyntax： （預設值： false） enables syntax guessing for code fences without specified language
# hasCJKLanguage： （預設值： false）
#     if true, auto-detect Chinese/Japanese/Korean Languages in the content.
#     (.Summary and .WordCount can work properly in CJKLanguage)


### 生成文檔

# buildDrafts： （預設值： false） 是否生成頁面時忽略草稿標記
# buildFuture： （預設值： false） include content with publishdate in the future
# buildExpired： （預設值： false） include content already expired
# relativeURLs： （預設值： false） 是否啟用相對路徑
#     enable this to make all relative URLs relative to content root.
#     Note that this does not affect absolute URLs.
# canonifyURLs： （預設值： false） if true, all relative URLs will be canonicalized using baseURL
#     ？ 問號相關 https://seo.dns.com.tw/archives/1184
# enableEmoji： （預設值： false） 是否啟用表情符號
#     關於可用表情符號見： www.emoji-cheat-sheet.com
# pygmentsStyle： （預設值： "monokai"） 著色筆風格
#     見： https://gohugo.io/extras/highlighting/
#     關於高亮語法風格見： http://pygments.org/
# pygmentsUseClasses： （預設值： false） 是否使用類組名而非個別標籤的風格屬性來著色
# disableRSS： （預設值： false） 是否不生成 RSS 文件
# rssLimit： （預設值： 15） RSS 的最大顯示數量
# disableSitemap： （預設值： false） 是否不生成 Sitemap 文件
# sitemap： default sitemap configuration map
#    https://gohugo.io/templates/sitemap/
# enableGitInfo： （預設值： false） Enable GitInfo feature
# enableRobotsTXT： （預設值： false） 是否生成 robots.txt 文件
# disable404： （預設值： false） 是否不顯示 404 頁面


### 額外功能

# googleAnalytics： （預設值： ""） 谷歌分析的追蹤識別碼


### 未知

# defaultExtension： （預設值： "html"）
# disableKinds： （預設值： [ "page", "home", "section", "taxonomy", "taxonomyTerm", "RSS", "sitemap", "robotsTXT", "404" ]）
#     The below example will disable all page types and will render nothing.
#     ? 看不懂 http://www.ctolib.com/topics-111201.html
# disableHugoGeneratorInject： （預設值： false） Do not inject generator meta tag on homepage
# footnoteAnchorPrefix： （預設值： ""）
# footnoteReturnLinkContents： （預設值： ""）
# noChmod： （預設值： false） Don't sync permission mode of files
# pluralizeListTitles： （預設值： true） Pluralize titles in lists using inflect
# preserveTaxonomyNames： （預設值： true） 是否在分類名稱（taxonomy names）中保留特殊字符
#     （如：`Gérard Depardieu` vs `Gerard Depardieu`）
# stepAnalysis： （預設值： false） 是否顯示記憶體和程序的不同步驟的時序（timing of different steps of the program）
stepAnalysis: true
```

關於 `googleAnalytics` 谷歌分析的使用可參考 [分析](/extras/analytics/) 。

### 棄用？（譯者加）

* **config**：
  * 預設值： `config.toml`
  * 解說： 設定檔路徑，允許 `toml`、`yaml`、`json` 等附檔名。 （在 `hugo new site` 時可透過 `-f, --format` 指定）
  * 結果： 依 `toml`、`yaml`、`json` 的順序讀取。 此結果在 `/data/` 資料夾下也一樣。
* **log**：
  * 預設值： `false`
  * 解說： 是否啟用日誌。
  * 結果： 沒 `logFile` 也無法保存日誌，淪為虛設。
* **editor**：
  * 預設值： `""`。
  * 解說： 使用此編輯器開啟新內容。
  * 結果： 沒有作用，反而 `newContentEditor` 有其所述的功能。

## Ignore various files when rendering

The following statement inside `./config.toml` will cause Hugo to ignore files
ending with `.foo` and `.boo` when rendering:

```toml
ignoreFiles = [ "\\.foo$", "\\.boo$" ]
```
The above is a list of regular expressions.
Note that the backslash (`\`) character is escaped, to keep TOML happy.

## Configure Blackfriday rendering

[Blackfriday](https://github.com/russross/blackfriday) is Hugo's
[Markdown](http://daringfireball.net/projects/markdown/)
rendering engine.

In the main, Hugo typically configures Blackfriday with a sane set of defaults.
These defaults should fit most use cases, reasonably well.

However, if you have unusual needs with respect to Markdown,
Hugo exposes some of its Blackfriday behavior options for you to alter.
The following table lists these Hugo options,
paired with the corresponding flags from Blackfriday's source code (for the latter, see
[html.go](https://github.com/russross/blackfriday/blob/master/html.go) and
[markdown.go](https://github.com/russross/blackfriday/blob/master/markdown.go)):

<table class="table table-bordered-configuration">
    <thead>
        <tr>
            <th>Flag</th>
            <th>Default</th>
            <th>Blackfriday flag</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><code><strong>taskLists</strong></code></td>
            <td><code>true</code></td>
            <td><code></code></td>
        </tr>
        <tr>
            <td class="purpose-description" colspan="3">
                <span class="purpose-title">Purpose:</span>
                <code>false</code> turns off GitHub-style automatic task/TODO
                list generation.
            </td>
        </tr>
        <tr>
            <td><code><strong>smartypants</strong></code></td>
            <td><code>true</code></td>
            <td><code>HTML_USE_SMARTYPANTS</code></td>
        </tr>
        <tr>
            <td class="purpose-description" colspan="3">
                <span class="purpose-title">Purpose:</span>
                <code>false</code> disables smart punctuation substitutions
                including smart quotes, smart dashes, smart fractions, etc.
                If <code>true</code>, it may be fine-tuned with the
                <code>angledQuotes</code>,
                <code>fractions</code>,
                <code>smartDashes</code> and
                <code>latexDashes</code> flags (see below).
            </td>
        </tr>
        <tr>
            <td><code><strong>angledQuotes</strong></code></td>
            <td><code>false</code></td>
            <td><code>HTML_SMARTYPANTS_ANGLED_QUOTES</code></td>
        </tr>
        <tr>
            <td class="purpose-description" colspan="3">
                <span class="purpose-title">Purpose:</span>
                <code>true</code> enables smart, angled double quotes.<br>
                <small>
                    <strong>Example:</strong>
                    <code>"Hugo"</code> renders to
                    «Hugo» instead of “Hugo”.
                </small>
            </td>
        </tr>
        <tr>
            <td><code><strong>fractions</strong></code></td>
            <td><code>true</code></td>
            <td><code>HTML_SMARTYPANTS_FRACTIONS</code></td>
        </tr>
        <tr>
            <td class="purpose-description" colspan="3">
                <span class="purpose-title">Purpose:</span>
                <code>false</code> disables smart fractions.<br>
                <small>
                    <strong>Example:</strong>
                    <code>5/12</code> renders to
                    <sup>5</sup>&frasl;<sub>12</sub>
                    (<code>&lt;sup&gt;5&lt;/sup&gt;&amp;frasl;&lt;sub&gt;12&lt;/sub&gt;</code>).<br>
                    <strong>Caveat:</strong>
                    Even with <code>fractions = false</code>,
                    Blackfriday still converts
                    1/2, 1/4 and 3/4 respectively to
                    ½ (<code>&amp;frac12;</code>),
                    ¼ (<code>&amp;frac14;</code>) and
                    ¾ (<code>&amp;frac34;</code>),
                    but only these three.</small>
            </td>
        </tr>
        <tr>
            <td><code><strong>smartDashes</strong></code></td>
            <td><code>true</code></td>
            <td><code>HTML_SMARTYPANTS_DASHES</code></td>
        </tr>
        <tr>
            <td class="purpose-description" colspan="3">
                <span class="purpose-title">Purpose:</span>
                <code>false</code> disables smart dashes; i.e., the conversion
                of multiple hyphens into en&nbsp;dash or em&nbsp;dash.
                If <code>true</code>, its behavior can be modified with the
                <code>latexDashes</code> flag below.
            </td>
        </tr>
        <tr>
            <td><code><strong>latexDashes</strong></code></td>
            <td><code>true</code></td>
            <td><code>HTML_SMARTYPANTS_LATEX_DASHES</code></td>
        </tr>
        <tr>
            <td class="purpose-description" colspan="3">
                <span class="purpose-title">Purpose:</span>
                <code>false</code> disables LaTeX-style smart dashes and
                selects conventional smart dashes. Assuming
                <code>smartDashes</code> (above), if this is:
                <ul>
                    <li>
                        <strong><code>true</code>,</strong> then
                        <code>--</code> is translated into “&ndash;”
                        (<code>&amp;ndash;</code>), whereas
                        <code>---</code> is translated into “&mdash;”
                        (<code>&amp;mdash;</code>).
                    </li>
                    <li>
                        <strong><code>false</code>,</strong> then
                        <code>--</code> is translated into “&mdash;”
                        (<code>&amp;mdash;</code>), whereas a
                        <em>spaced</em> single hyphen between two words
                        is translated into an en&nbsp;dash&mdash;e.g.,
                        <code>12 June - 3 July</code> becomes
                        <code>12 June &amp;ndash; 3 July</code>.
                    </li>
                </ul>
            </td>
        </tr>
        <tr>
            <td><code><strong>hrefTargetBlank</strong></code></td>
            <td><code>false</code></td>
            <td><code>HTML_HREF_TARGET_BLANK</code></td>
        </tr>
        <tr>
            <td class="purpose-description" colspan="3">
                <span class="purpose-title">Purpose:</span>
                <code>true</code> opens external links in a new window or tab.
            </td>
        </tr>
        <tr>
            <td><code><strong>plainIDAnchors</strong></code></td>
            <td><code>true</code></td>
            <td>
                <code>FootnoteAnchorPrefix</code> and
                <code>HeaderIDSuffix</code>
            </td>
        </tr>
        <tr>
            <td class="purpose-description" colspan="3">
                <span class="purpose-title">Purpose:</span>
                <code>true</code> renders any header and footnote IDs
                without the document ID.<br>
                <small>
                    <strong>Example:</strong>
                    renders <code>#my-header</code> instead of
                    <code>#my-header:bec3ed8ba720b9073ab75abcf3ba5d97</code>.
                </small>
            </td>
        </tr>
        <tr>
            <td><code><strong>extensions</strong></code></td>
            <td><code>[]</code></td>
            <td><code>EXTENSION_*</code></td>
        </tr>
        <tr>
            <td class="purpose-description" colspan="3">
                <span class="purpose-title">Purpose:</span>
                Enable one or more of Blackfriday's Markdown extensions
                (if they aren't Hugo defaults).<br>
                <small>
                    <strong>Example:</strong> &nbsp;
                    Include <code>"hardLineBreak"</code>
                    in the list to enable Blackfriday's
                    <code>EXTENSION_HARD_LINE_BREAK</code>.
                </small>
            </td>
        </tr>
        <tr>
            <td><code><strong>extensionsmask</strong></code></td>
            <td><code>[]</code></td>
            <td><code>EXTENSION_*</code></td>
        </tr>
        <tr>
            <td class="purpose-description" colspan="3">
                <span class="purpose-title">Purpose:</span>
                Disable one or more of Blackfriday's Markdown extensions
                (if they are Hugo defaults).<br>
                <small>
                    <strong>Example:</strong> &nbsp;
                    Include <code>"autoHeaderIds"</code>
                    in the list to disable Blackfriday's
                    <code>EXTENSION_AUTO_HEADER_IDS</code>.
                </small>
            </td>
        </tr>
        </tbody>
</table>

**Notes**

* These flags are **case sensitive** (as of Hugo v0.15)!
* These flags must be grouped under the `blackfriday` key
and can be set on **both the site level and the page level**.
Any setting on a page will override the site setting
there. For example:

<table class="table">
    <thead>
        <tr>
            <th>TOML</th>
            <th>YAML</th>
        </tr>
    </thead>
    <tbody>
        <tr style="vertical-align: top;">
            <td style="width: 50%;">
<pre><code>[blackfriday]
  angledQuotes = true
  fractions = false
  plainIDAnchors = true
  extensions = ["hardLineBreak"]
</code></pre>
            </td>
            <td>
<pre><code>blackfriday:
  angledQuotes: true
  fractions: false
  plainIDAnchors: true
  extensions:
    - hardLineBreak
</code></pre>
            </td>
        </tr>
    </tbody>
</table>
