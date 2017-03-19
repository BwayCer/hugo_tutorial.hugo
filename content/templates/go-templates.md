---
aliases:
- /layout/go-templates/
- /layouts/go-templates/
lastmod: 2015-11-30
date: 2013-07-01
menu:
  main:
    parent: layout
next: /templates/ace
prev: /templates/overview
title: Go 模板啟蒙讀本
weight: 15
toc: true
---

Hugo uses the excellent [Go][] [html/template][gohtmltemplate] library for
its template engine. It is an extremely lightweight engine that provides a very
small amount of logic. In our experience it is just the right amount of
logic to be able to create a good static website. If you have used other
template systems from different languages or frameworks, you will find a lot of
similarities in Go templates.

This document is a brief primer on using Go templates. The [Go docs][gohtmltemplate]
go into more depth and cover features that aren't mentioned here.

## Go 模板介紹 Introduction to Go Templates

Go templates provide an extremely simple template language. It adheres to the
belief that only the most basic of logic belongs in the template or view layer.
One consequence of this simplicity is that Go templates parse very quickly.

A unique characteristic of Go templates is they are content aware. Variables and
content will be sanitized depending on the context of where they are used. More
details can be found in the [Go docs][gohtmltemplate].

## 基本語法 Basic  Syntax

Go lang 模板是添加了變數和函數的 HTML 文件。

Go lang templates are HTML files with the addition of variables and
functions.

**Go 的變數和函數包覆在 {{ }} 中簡易的被使用。**

**Go variables and functions are accessible within {{ }}**

訪問預定義的變數 "foo"：

Accessing a predefined variable "foo":

    {{ foo }}

**參數使用空格分隔。**

**Parameters are separated using spaces**

調用 `add` 函數並輸入 1, 2：

Calling the `add` function with input of 1, 2:

    {{ add 1 2 }}

**方法和欄位通過「點」符號來訪問。**

**Methods and fields are accessed via dot notation**

訪問頁面參數 "bar"：

Accessing the Page Parameter "bar"

    {{ .Params.bar }}

**Parentheses can be used to group items together**

    {{ if or (isset .Params "alt") (isset .Params "caption") }} Caption {{ end }}


## 變數 Variables

Each Go template has a struct (object) made available to it. In Hugo, each
template is passed page struct. More details are available on the
[variables](/layout/variables/) page.

A variable is accessed by referencing the variable name.

    <title>{{ .Title }}</title>

Variables can also be defined and referenced.

    {{ $address := "123 Main St."}}
    {{ $address }}


## 函數 Functions

Go template ships with a few functions which provide basic functionality. The Go
template system also provides a mechanism for applications to extend the
available functions with their own. [Hugo template
functions](/layout/functions/) provide some additional functionality we believe
are useful for building websites. Functions are called by using their name
followed by the required parameters separated by spaces. Template
functions cannot be added without recompiling Hugo.

**Example 1: Adding numbers**

    {{ add 1 2 }}

**Example 2: Comparing numbers**

    {{ lt 1 2 }}

(There are more boolean operators, detailed in the
[template documentation](http://golang.org/pkg/text/template/#hdr-Functions).)

## 引入 Includes

當引入其他模板時，你將傳遞其能訪問的資料給它。
要傳遞當前的上下文請記得語法包括尾隨的點。
要載入的模板位置始終相對於在 Hugo 裡 /layout/ 目錄。

When including another template, you will pass to it the data it will be
able to access. To pass along the current context, please remember to
include a trailing dot. The templates location will always be starting at
the /layout/ directory within Hugo.

**Example:**

    {{ template "partials/header.html" . }}

然而，Hugo v0.12 開始你也能使用 `partial` 去調用
[partial templates](/templates/partials/)
：

And, starting with Hugo v0.12, you may also use the `partial` call
for [partial templates](/templates/partials/):

    {{ partial "header.html" . }}


## 邏輯 Logic

Go templates provide the most basic iteration and conditional logic.

### 迭代 Iteration

Just like in Go, the Go templates make heavy use of `range` to iterate over
a map, array or slice. The following are different examples of how to use
range.

**Example 1: Using Context**

    {{ range array }}
        {{ . }}
    {{ end }}

**Example 2: Declaring value variable name**

    {{range $element := array}}
        {{ $element }}
    {{ end }}

**Example 2: Declaring key and value variable name**

    {{range $index, $element := array}}
        {{ $index }}
        {{ $element }}
    {{ end }}

### 條件 Conditionals

`if`, `else`, `with`, `or` & `and` provide the framework for handling conditional
logic in Go Templates. Like `range`, each statement is closed with `end`.

Go Templates treat the following values as false:

* false
* 0
* any array, slice, map, or string of length zero

**Example 1: `if`**

    {{ if isset .Params "title" }}<h4>{{ index .Params "title" }}</h4>{{ end }}

**Example 2: `if` … `else`**

    {{ if isset .Params "alt" }}
        {{ index .Params "alt" }}
    {{else}}
        {{ index .Params "caption" }}
    {{ end }}

**Example 3: `and` & `or`**

    {{ if and (or (isset .Params "title") (isset .Params "caption")) (isset .Params "attr")}}

**Example 4: `with`**

An alternative way of writing "`if`" and then referencing the same value
is to use "`with`" instead. `with` rebinds the context `.` within its scope,
and skips the block if the variable is absent.

The first example above could be simplified as:

    {{ with .Params.title }}<h4>{{ . }}</h4>{{ end }}

**Example 5: `if` … `else if`**

    {{ if isset .Params "alt" }}
        {{ index .Params "alt" }}
    {{ else if isset .Params "caption" }}
        {{ index .Params "caption" }}
    {{ end }}

## 管道 Pipes

One of the most powerful components of Go templates is the ability to
stack actions one after another. This is done by using pipes. Borrowed
from Unix pipes, the concept is simple, each pipeline's output becomes the
input of the following pipe.

Because of the very simple syntax of Go templates, the pipe is essential
to being able to chain together function calls. One limitation of the
pipes is that they only can work with a single value and that value
becomes the last parameter of the next pipeline.

A few simple examples should help convey how to use the pipe.

**Example 1:**

    {{ shuffle (seq 1 5) }}

is the same as

    {{ (seq 1 5) | shuffle }}

**Example 2:**

    {{ index .Params "disqus_url" | html }}

Access the page parameter called "disqus_url" and escape the HTML.

`index` 函數是 [Go][] 的內置函數，你能讀關於它
[這裡][gostdlibpkgtexttemplate]
。 `index`：

The `index` function is a [Go][] built-in, and you can read about it [here][gostdlibpkgtexttemplate]. `index`:

> ...returns the result of indexing its first argument by the following arguments. Thus "index x 1 2 3" is, in Go syntax, `x[1][2][3]`. Each indexed item must be a map, slice, or array.

**Example 3:**

    {{ if or (or (isset .Params "title") (isset .Params "caption")) (isset .Params "attr") }}
    Stuff Here
    {{ end }}

Could be rewritten as

    {{ if isset .Params "caption" | or isset .Params "title" | or isset .Params "attr" }}
    Stuff Here
    {{ end }}

### 對 IE 瀏覽器使用條件式註解的方法 Internet Explorer conditional comments using Pipes

By default, Go Templates remove HTML comments from output. This has the unfortunate side effect of removing Internet Explorer conditional comments. As a workaround, use something like this:

    {{ "<!--[if lt IE 9]>" | safeHTML }}
      <script src="html5shiv.js"></script>
    {{ "<![endif]-->" | safeHTML }}

Alternatively, use the backtick (`` ` ``) to quote the IE conditional comments, avoiding the tedious task of escaping every double quotes (`"`) inside, as demonstrated in the [examples](http://golang.org/pkg/text/template/#hdr-Examples) in the Go text/template documentation, e.g.:

```
{{ `<!--[if lt IE 7]><html class="no-js lt-ie9 lt-ie8 lt-ie7"><![endif]-->` | safeHTML }}
```

## 上下文（又名： 「點」） Context (a.k.a. the dot)

The most easily overlooked concept to understand about Go templates is that `{{ . }}`
always refers to the current context. In the top level of your template, this
will be the data set made available to it. Inside of a iteration, however, it will have
the value of the current item. When inside of a loop, the context has changed:
`{{ . }}` will no longer refer to the data available to the entire page. If you need
to
access this from within the loop, you will likely want to do one of the following:

1. 設置一變數而不是依據上下文，例如：

    Set it to a variable instead of depending on the context.  For example:

        {{ $title := .Site.Title }}
        {{ range .Params.tags }}
          <li>
            <a href="{{ $baseURL }}/tags/{{ . | urlize }}">{{ . }}</a>
            - {{ $title }}
          </li>
        {{ end }}

    注意一旦進入迴圈，`{{ . }}` 的值就已經改變。
    我們有在迴圈外定義了變數，所以我們能在內部去訪問。

    Notice how once we have entered the loop the value of `{{ . }}` has changed. We
    have defined a variable outside of the loop so we have access to it from within
    the loop.

2. 使用 `$.` 在任意地方去訪問全域變數，這是與上述有相同效果的範例：

    Use `$.` to access the global context from anywhere.
    Here is an equivalent example:

        {{ range .Params.tags }}
          <li>
            <a href="{{ $baseURL }}/tags/{{ . | urlize }}">{{ . }}</a>
            - {{ $.Site.Title }}
          </li>
        {{ end }}

    This is because `$`, a special variable, is set to the starting value
    of `.` the dot by default,
    a [documented feature](http://golang.org/pkg/text/template/#hdr-Variables)
    of Go text/template.  Very handy, eh?

    > 然而，有個小魔法能使它停止工作，如果有人惡作劇去重新定義 `$`，如： `{{ $ := .Site }}`。
    > *（不，不要這麼做）*

    > However, this little magic would cease to work if someone were to
    > mischievously redefine `$`, e.g. `{{ $ := .Site }}`.
    > *(No, don't do it!)*
    > You may, of course, recover from this mischief by using `{{ $ := . }}`
    > in a global context to reset `$` to its default value.

## 空格 Whitespace

Go 1.6 後包含修剪 Go 標籤兩側空格的功能，透過包含連字符（-）和空格添加在緊鄰對應側的 `{{` 或 `}}` 分隔符。

Go 1.6 includes the ability to trim the whitespace from either side of a Go tag by including a hyphen (`-`) and space immediately beside the corresponding `{{` or `}}` delimiter.

例如，以下的 Go 模板：

For instance, the following Go template:

```html
<div>
  {{ .Title }}
</div>
```

HTML 將輸出包含換行和水平縮排：

will include the newlines and horizontal tab in its HTML output:

```html
<div>
  Hello, World!
</div>
```

而使用

whereas using

```html
<div>
  {{- .Title -}}
</div>
```

在這情況下將簡單地輸出 `<div>Hello, World!</div>`。

in that case will output simply `<div>Hello, World!</div>`.

Go 將以下字符視為空格： 空白、水平縮排、回車和換行。

Go considers the following characters as whitespace: space, horizontal tab, carriage return and newline.

# 雨果參數 Hugo Parameters

Hugo provides the option of passing values to the template language
through the site configuration (for sitewide values), or through the meta
data of each specific piece of content. You can define any values of any
type (supported by your front matter/config format) and use them however
you want to inside of your templates.


## 使用 Content（頁面）參數 Using Content (page) Parameters

在每個內容片段，你能提供參數給模板使用。
這發生在
[前提內容](/content/front-matter/)
。

In each piece of content, you can provide variables to be used by the
templates. This happens in the [front matter](/content/front-matter/).

這是一個被使用在本文檔中的範例。

An example of this is used in this documentation site. Most of the pages
benefit from having the table of contents provided. Sometimes the TOC just
doesn't make a lot of sense. We've defined a variable in our front matter
of some pages to turn off the TOC from being displayed.

這是前提內容的範例：

Here is the example front matter:

```
---
title: "Permalinks"
lastmod: 2015-11-30
date: "2013-11-18"
aliases:
  - "/doc/permalinks/"
groups: ["extras"]
groups_weight: 30
notoc: true
---
```

這是在模板中相對應的程式碼：

Here is the corresponding code inside of the template:

      {{ if not .Params.notoc }}
        <div id="toc" class="well col-md-4 col-sm-6">
        {{ .TableOfContents }}
        </div>
      {{ end }}



## 使用 Site（設定檔）參數 Using Site (config) Parameters
你能添加 site 參數在最上層設定文件（例如：`config.yaml`）當中，其可被用於 partials。

In your top-level configuration file (e.g., `config.yaml`) you can define site
parameters, which are values which will be available to you in partials.

例如，你可以聲明：

For instance, you might declare:

```yaml
params:
  CopyrightHTML: "Copyright &#xA9; 2013 John Doe. All Rights Reserved."
  TwitterUser: "spf13"
  SidebarRecentLimit: 5
```

在頁腳的布局，你能聲明 `<footer>` 僅在 `CopyrightHTML` 參數有提供時顯示。
然後如果它有值，你能使用 HTML-safe 來聲明它，因此 HTML 實體不會再次被轉譯。
這將讓你能在每年一月一號更簡單的更新透過最上層的設定文件而不是在你的模板中。

Within a footer layout, you might then declare a `<footer>` which is only
provided if the `CopyrightHTML` parameter is provided, and if it is given,
you would declare it to be HTML-safe, so that the HTML entity is not escaped
again.  This would let you easily update just your top-level config file each
January 1st, instead of hunting through your templates.

```
{{if .Site.Params.CopyrightHTML}}<footer>
<div class="text-center">{{.Site.Params.CopyrightHTML | safeHTML}}</div>
</footer>{{end}}
```

An alternative way of writing the "`if`" and then referencing the same value
is to use "`with`" instead. With rebinds the context `.` within its scope,
and skips the block if the variable is absent:

```
{{with .Site.Params.TwitterUser}}<span class="twitter">
<a href="https://twitter.com/{{.}}" rel="author">
<img src="/images/twitter.png" width="48" height="48" title="Twitter: {{.}}"
 alt="Twitter"></a>
</span>{{end}}
```

Finally, if you want to pull "magic constants" out of your layouts, you can do
so, such as in this example:

```
<nav class="recent">
  <h1>Recent Posts</h1>
  <ul>{{range first .Site.Params.SidebarRecentLimit .Site.Pages}}
    <li><a href="{{.RelPermalink}}">{{.Title}}</a></li>
  {{end}}</ul>
</nav>
```

# 模板示範： 顯示即將到來的事件 Template example: Show only upcoming events

Go allows you to do more than what's shown here.  Using Hugo's
[`where`](/templates/functions/#where) function and Go built-ins, we can list
only the items from `content/events/` whose date (set in the front matter) is in
the future:

    <h4>Upcoming Events</h4>
    <ul class="upcoming-events">
    {{ range where .Data.Pages.ByDate "Section" "events" }}
      {{ if ge .Date.Unix .Now.Unix }}
        <li><span class="event-type">{{ .Type | title }} —</span>
          {{ .Title }}
          on <span class="event-date">
          {{ .Date.Format "2 January at 3:04pm" }}</span>
          at {{ .Params.place }}
        </li>
      {{ end }}
    {{ end }}

[go]: http://golang.org/
[gohtmltemplate]: http://golang.org/pkg/html/template/
[gostdlibpkgtexttemplate]: http://golang.org/pkg/text/template/
