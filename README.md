# Git command

###### tags: `Git`

專案規範與範例：
[Git Commit 規範](https://wadehuanglearning.blogspot.com/2019/05/commit-commit-commit-why-what-commit.html)

---
設置目錄連結：[github-markdown-toc](https://github.com/ekalinin/github-markdown-toc#installation)


> 現在電腦使用 windows，日後更新

[TOC]

---

## Commit Message 很重要

Git 在每次 Commit 時，需要寫下 Git Commit Message（提交說明），用來記錄提交版本更動的摘要。

> 任何專案都至少由兩個以上的開發者共同合作開發。

除了專案開發者，任何專案都會是跟其他開發者、以及未來的自己共同開發維護的。當不同開發者接手專案時，能藉由瀏覽 Commit Message 內容快速進入狀況，瞭解程式異動的原因，如此也利於後續的維護。

## 好的 Commit Message

一個好的 Git Commit Message 必須兼具 What & Why & How，能幫助開發者瞭解這個提交版本：

1. 做了什麼事情（What）
2. 為什麼要做這件事情（Why）
3. 用什麼方法做到的（How）

## Commit Message 的規範與準則
在團隊之間，撰寫 commit log 的方式應一致，也就是定義風格與內容，可透過遵守現有的慣例來實現。

一個 Commit Message 主要由 Header + Body + Footer 組成：

```python=
<type>(<scope>): <subject>
<BLANK LINE>
<body>
<BLANK LINE>
<footer>
```

- Message Header:
<type> (<scope>): <subject>
    - type（必要）：commit 的類別
        如：feat, fix, docs, style, refactor, test, chore
    - scope（可選）：commit 影響的範圍
        如：資料庫、控制層、模板層等，視專案不同改變
    - subject（必要）：commit 的簡短描述
        不超過 50 個字元，結尾不加句號
        盡量讓 Commit 單一化，一次只更動一個主題

- Message Body
    - 對本次 Commit 的詳細描述，解釋 What & Why & How
    - 可以分成多行，每一行不超過 72 個字元
        說明程式碼變動的項目與原因，還有與先前行為的對比

- Message Footer
    - 填寫任務編號 issue #1246
    - BREAKING CHANGE（可略），記錄不兼容的變動，後面是對變動的描述、以及變動原因和遷移方法

- Header：<type> 類別規範
     type 代表提交 Commit 的類別，以下為使用慣例：
    - feat：新增或修改功能（feature）
    - fix：修補 bug（bug fix）
    - docs：文件（documentation）
    - style：格式
    不影響程式碼運行的變動，例如：white-space, formatting, missing semi colons
    - refactor：重構
    不是新增功能，也非修補 bug 的程式碼變動
    - perf：改善效能（improves performance）
    - test：增加測試（when adding missing tests）
    - chore：maintain
    不影響程式碼運行，建構程序或輔助工具的變動
    例如：修改 config、Grunt Task 任務管理工具
    - revert：撤銷回覆先前的 commit
        例如：revert：type(scope):subject



| 類型     | 說明                                                             | 有無程式碼改動 |
| -------- | ---------------------------------------------------------------- | -------------- |
| Feat     | 新功能                                                           | 有             |
| Fix      | 錯誤修正                                                         | 有             |
| Docs     | 更新文件，如 README.md                                           | 沒有           |
| Style    | 程式碼格式調整(formatting)、缺少分號(missing semi colons)等      | 沒有           |
| Test     | 測試，新增測試、重構測試等                                       | 沒有           |
| Chore    | 更新專案建置設定、更新版本號等                                   | 沒有           |
| Refactor | 重構，針對已上線的功能程式碼調整與優化                           | 有             |
| Revert   | 撤銷之前的commit。 revert: type(scope): subject (回覆版本：xxxx) | 有             |

註(需求異動)：
```python=
[feat Type] modify：功能上的修正（非 bug）
[feat Type] change：功能或規格變更
[feat Type] disable：拿掉某功能（comment out）
[feat Type] remove：刪除檔案
[chore Type] upgrade：版本更新
```


### **Commit Message 範例**
```python=
feat: message 新增信件通知功能
feat(優惠券): 加入搜尋按鈕，調整畫面

fix: 圓餅圖圖例跑版
fix: 意見反應，信件看不到圖片問題

style: 統一換行符號 CRLF to LF

docs: 更新 README 相關資訊
docs: 修正型別註解

chore(submoudle): 變更 git url
chore: 調整單元測試環境

refactor(每日通知信件): 重構程式結構
```
> fix 範例
```pythn=
fix: 自訂表單新增/編輯頁面，修正離開頁面提醒邏輯

問題：
1. 原程式碼進入新增頁面後，沒做任何動作之下，離開頁面會跳提醒
2. 原程式碼從新增/編輯頁面回到上一頁後（表單列表頁面），離開頁面會跳提醒

原因：
1. 新增頁面時，頁面自動建立空白題組會調用 sort_item，造成初始化 unload 事件處理器。
2. 回到上一頁後，就不需要監聽 unload 事件，應該把 unload 事件取消。

調整項目：
1. 初始化 unload 事件處理器：排除新增表單時，頁面自動建立空白題組調用 sort_item 的情境
2. 回到上一頁後，復原表單被異動狀態且清除 unload 事件處理器

issue #1335
```
> chore 範例

```python=
chore:調整單元測試環境

調整項目：
1. MX/Modules
將客製化 Testing 的邏輯移除，否則在測試環境中無法正確存取檔案。
2. 加入 tests/unit 與 tests/integration 目錄，並將測試檔案移至合宜的位置。
3. AdminTestCase.php，繼承 TestCase，實作登入邏輯、setUp 與 tearDown，供其他測試案例繼承使用。
4. Bootstrap.php，引入 AdminTestCase.php 共測試案例繼承用。
5. Login.php，因測試案例中不能有 header 的設定，更動系統登入邏輯，在測試環境中改用 redirect 轉址。
6. phpunit.xml，取消嚴謹宣告覆蓋模式，避免造成測試不通過（若需知道你的測試案例覆蓋了哪些類別或邏輯，可自行打開）。

## 備註： unit 與 integration 目錄
分別為「單元測試目錄」與「整合測試目錄」，單元測試目錄負責測試 Api 與 Model，整合測試目錄則負責測試 Controller。

issue #709
```