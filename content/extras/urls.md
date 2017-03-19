---
aliases:
- /doc/urls/
lastmod: 2016-05-07
date: 2014-01-03
menu:
  main:
    parent: extras
next: /community/mailing-list
notoc: true
prev: /extras/localfiles
title: 網址 URLs
---

## 漂亮的網址 Pretty URLs

預設的情況，雨果用「漂亮的」網址來創建內容。
例如： 內容被建立於 `/content/extras/urls.md`
將被渲染於 `/public/extras/urls/index.html` 位置。
因此可在瀏覽器中訪問透過 http://example.com/extras/urls/
不需要非標準的服務器端配置也能運行這些漂亮的網址。

By default, Hugo creates content with 'pretty' URLs. For example,
content created at `/content/extras/urls.md` will be rendered at
`/public/extras/urls/index.html`, thus accessible from the browser
at http://example.com/extras/urls/.  No non-standard server-side
configuration is required for these pretty URLs to work.

若你願意使用我們所稱的「醜陋的網址」，例如
http://example.com/extras/urls.html 。
你很幸運，雨果是能夠使用醜陋的網址創建整個網站。
簡單地增加 `uglyurls = true` 在網站的設定檔 `config.toml`
中，或者在命令行中使用 `--uglyURLs=true` 旗標。

If you would like to have what we call "ugly URLs",
e.g.&nbsp;http://example.com/extras/urls.html, you are in luck.
Hugo supports the ability to create your entire site with ugly URLs.
Simply add `uglyurls = true` to your site-wide `config.toml`,
or use the `--uglyURLs=true` flag on the command line.

如果你想要特定的內容片段擁有確切的網址，可以在前提內容中的 `url` 參數指定路徑。
更多細節見 [Content Organization](/content/organization/) 。

If you want a specific piece of content to have an exact URL, you can
specify this in the front matter under the `url` key. See [Content
Organization](/content/organization/) for more details.

## 標準化 Canonicalization

預設情況下，在輸入中遇到的所有相對的網址（站內的連結）會保持不變， 例如
`/css/foo.css` 依然維持 `/css/foo.css`， 即 `canonifyURLs: false`。

By default, all relative URLs encountered in the input are left unmodified,
e.g. `/css/foo.css` would stay as `/css/foo.css`,
i.e. `canonifyURLs` defaults to `false`.

若把 `canonifyURLs` 設定為 `true`，所有相對的網址將使用 `baseURL` 進行 *標準化* 。
例如： 假設你在網站的設定檔 `config.toml` 配置
`baseURL = http://yoursite.example.com/`
選項，則相對網址 `/css/foo.css`
將轉變為絕對的網址 `http://yoursite.example.com/css/foo.css`。

By setting `canonifyURLs` to `true`, all relative URLs would instead
be *canonicalized* using `baseURL`.  For example, assuming you have
`baseURL = http://yoursite.example.com/` defined in the site-wide
`config.toml`, the relative URL `/css/foo.css` would be turned into
the absolute URL `http://yoursite.example.com/css/foo.css`.

標準化的好處有可以修定所有網址改為絕對路徑，這可能有助於一些解析任務。
Note though that all real browsers handle this
client-side without issues.

Benefits of canonicalization include fixing all URLs to be absolute, which may
aid with some parsing tasks.  Note though that all real browsers handle this
client-side without issues.

未標準化的好處可維持與資源相對的結構。
因此 http 和 https 能基於頁面檢索方式而決定。

Benefits of non-canonicalization include being able to have resource inclusion
be scheme-relative, so that http vs https can be decided based on how this
page was retrieved.

> 注意： 在 2014 年 5 月釋出的雨果 v0.11 版本中，`canonifyURLs` 的預設值從 `true` 切換為 `false`。
> 我們認為這是更好的默認值且應該繼續保持這種情況。
> 因此，如果你是由 v0.10 或舊版本中升級，請根據此情況來確認和調整您的網站。

> Note: In the May 2014 release of Hugo v0.11, the default value of `canonifyURLs` was switched from `true` to `false`, which we think is the better default and should continue to be the case going forward. So, please verify and adjust your website accordingly if you are upgrading from v0.10 or older versions.

若要找出網站中 `canonifyURLs` 的當前值，你能使用 v0.13 所增加更便利的命令 `hugo config`：

To find out the current value of `canonifyURLs` for your website, you may use the handy `hugo config` command added in v0.13:

    hugo config | grep -i canon

或者，如果你使用沒有安裝 `grep` 的視窗系統：

Or, if you are on Windows and do not have `grep` installed:

    hugo config | FINDSTR /I canon

## 相對的網址 Relative URLs

預設情況下，雨果不會更改所有相對的網址，當你想要從本地文件系統中瀏覽網站時，這可能會有問題。

By default, all relative URLs are left unchanged by Hugo,
which can be problematic when you want to make your site browsable from a local file system.

在網站配置中將 `relativeURLs` 設為 `true` 會使雨果重寫所有相對的網址以相對於當前內容。

Setting `relativeURLs` to `true` in the site configuration will cause Hugo to rewrite all relative URLs to be relative to the current content.

例如，如果 `/post/first/` 頁面包含相對的網址 `/about/` 的鏈接，雨果將該網址重寫為 `../../about/`。

For example, if the `/post/first/` page contained a link with a relative URL of `/about/`, Hugo would rewrite that URL to `../../about/`.
