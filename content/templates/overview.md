---
aliases:
- /doc/templates/
- /layout/templates/
- /layout/overview/
lastmod: 2015-05-22
date: 2013-07-01
linktitle: 概述 Overview
menu:
  main:
    parent: layout
next: /templates/go-templates
prev: /themes/creation
title: Hugo Templates
weight: 10
toc: true
---

雨果使用優秀的 Go html/template 庫作為模板引擎。
其為一款使用少量邏輯且非常輕量的引擎。
在我們的經驗裡，其保有創建靜態網站所需基礎的邏輯。

Hugo uses the excellent Go html/template library for its template engine.
It is an extremely lightweight engine that provides a very small amount of
logic. In our experience it is just the right amount of logic to be able
to create a good static website.

雖然雨果有許多不同作用的模板，但大多數的網站只需使用少量模板就能構建完成。
因此別害怕種類眾多的模板，它們使雨果能構建複雜的網站，但更多時候只需要創建
[/layouts/\_default/single.html](/templates/content/)
和 [/layouts/\_default/list.html](/templates/list/)
。

While Hugo has a number of different template roles, most complete
websites can be built using just a small number of template files.
Please don’t be afraid of the variety of different template roles. They
enable Hugo to build very complicated sites. Most sites will only
need to create a [/layouts/\_default/single.html](/templates/content/) & [/layouts/\_default/list.html](/templates/list/)

如果你初次接觸 Go 模板，
[Go Template Primer](/layout/go-templates/)
會是一個好的開始。

If you are new to Go's templates, the [Go Template Primer](/layout/go-templates/)
is a great place to start.

如果你熟悉 Go 模板，也能學習更多關於雨果提供的
[附加模板功能](/templates/functions/)
t complete websites can be built using just a small number of template files [變數](/templates/variables/)
。

If you are familiar with Go’s templates, Hugo provides some [additional
template functions](/templates/functions/) and [variables](/templates/variables/) you will want to be familiar
with.

## 主要模板 Primary Template roles

雨果運行有三種主要的模板類型。 There are 3 primary kinds of templates that Hugo works with.

### [Single](/templates/content/)
顯示特有的內容片段。

Render a single piece of content

### [List](/templates/list/)
Page that list multiple pieces of content

### [Homepage](/templates/homepage/)
The homepage of your site

## 輔助模板（可選） Supporting Template Roles (optional)

Hugo also has additional kinds of templates all of which are optional

### [Partial Templates](/templates/partials/)
Common page parts to be included in the above mentioned templates

### [Content Views](/templates/views/)
Different ways of rendering a (single) content type

### [Taxonomy Terms](/templates/terms/)
A list of the terms used for a specific taxonomy, e.g. a Tag cloud

## 其他模板（通常非必要） Other Templates (generally unnecessary)

### [RSS](/templates/rss/)
Used to render all rss documents

### [Sitemap](/templates/sitemap/)
Used to render the XML sitemap

### [404](/templates/404/)
此模板將創建一個在輯頁面上託管時使用的 404.html 頁面。

This template will create a 404.html page used when hosting on GitHub Pages

### [Alias](/extras/aliases/#customizing)
This template will override the default page used to create aliases of pages.

