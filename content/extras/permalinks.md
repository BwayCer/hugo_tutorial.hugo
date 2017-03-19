---
aliases:
- /doc/permalinks/
lastmod: 2015-01-19
date: 2013-11-18
menu:
  main:
    parent: extras
next: /extras/scratch
notoc: true
prev: /extras/pagination
title: 固定鏈接
---

預設情況下，內容被放置於 `publishdir`（預設：public）的目標命名空間並與 `contentdir` 的階層相匹配。
網站配置選項 `permalinks` 允許你去調整每部分的基礎。
它將改變文件被寫入的位置和改變頁面內部「標準的（"canonical"）」位置。
參考 `.RelPermalink` 的模板會遵守此選項的映射結果做調整。

By default, content is laid out into the target `publishdir` (public)
namespace matching its layout within the `contentdir` hierarchy.
The `permalinks` site configuration option allows you to adjust this on a
per-section basis.
This will change where the files are written to and will change the page's
internal "canonical" location, such that template references to
`.RelPermalink` will honour the adjustments made as a result of the mappings
in this option.

例如，如果你有一個部分是 `post`，然後你想要基於年月份去調整標準的路徑階層，那麼你可以這樣使用：

For instance, if one of your sections is called `post`, and you want to adjust
the canonical path to be hierarchical based on the year and month, then you
might use:

```yaml
permalinks:
  post: /:year/:month/:title/
```

只有 `post/` 下的內容會被重寫。
包含 `date: 2013-11-18T19:20:00-05:00`
且被命名為 `content/post/sample-entry`
的文件最終的渲染頁會出現在 `public/2013/11/sample-entry/index.html`
且到達網址改為透過
<http://yoursite.example.com/2013/11/sample-entry/>
。

Only the content under `post/` will be so rewritten.
A file named `content/post/sample-entry` which contains a line
`date: 2013-11-18T19:20:00-05:00` might end up with the rendered page
appearing at `public/2013/11/sample-entry/index.html` and be reachable via
the URL <http://yoursite.example.com/2013/11/sample-entry/>.

以下為 permalink 定義的可用值列表。
其中引用的時間都取決於內容的 `date`。

The following is a list of values that can be used in a permalink definition.
All references to time are dependent on the content's date.

  * **:year** 四碼數字的年份。 the 4-digit year
  * **:month** 兩碼數字的月份。 the 2-digit month
  * **:monthname** 月份的英文名。 the name of the month
  * **:day** 兩碼數字的日期。 the 2-digit day
  * **:weekday** 一碼數字的星期（如：Sunday = 0）。 the 1-digit day of the week (Sunday = 0)
  * **:weekdayname** 星期的英文名。 the name of the day of the week
  * **:yearday** 一至三碼的數字，一年當中的第幾日。 the 1- to 3-digit day of the year
  * **:section** 內容的部分（`content/"section"`）。 the content's section
  * **:title** 內容的標題。 the content's title
  * **:slug** the content's slug (or title if no slug)
  * **:filename** 內容的檔名（不含副檔名）。 the content's filename (without extension)

