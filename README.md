# BUMP! 校園排球 線上版：設定教學

做完以下 6 步，家人就能用 Google 帳號登入遊玩，存檔放在雲端，換手機、換電腦都接得上，還有家族排行榜。
全程使用免費方案，大約 30～60 分鐘，不需要寫程式。

資料夾裡有 4 個檔案：

| 檔案 | 用途 | 要改嗎 |
|---|---|---|
| `index.html` | 遊戲本體 | 不用改。之後更新遊戲只要換這個檔 |
| `firebase-config.js` | 你的 Firebase 設定 | **第 4 步改一次** |
| `firestore.rules` | 資料庫安全規則 | 不用改，第 3 步複製貼上 |
| `README.md` | 這份教學 | — |

---

## 第 1 步：建立 Firebase 專案

1. 用電腦打開 <https://console.firebase.google.com>，用你的 Google 帳號登入。
2. 按「建立專案」（或「新增專案」），名稱可取 `bump-volleyball`。
3. Google Analytics 可以關掉，不影響遊戲。按到「建立專案」完成。

## 第 2 步：開啟 Google 登入

1. 左側選單「建構 → Authentication」，按「開始使用」。
2. 在「登入方式」分頁選 **Google** → 打開「啟用」→「專案支援電子郵件」選你的信箱 → 儲存。

## 第 3 步：建立資料庫並貼上安全規則

1. 左側選單「建構 → Firestore Database」，按「建立資料庫」。
2. 位置選 **asia-east1（台灣）**，模式選「正式版模式」，建立。
3. 進到資料庫的「規則」分頁，把裡面的內容全部刪掉，貼上 `firestore.rules` 的全部內容，按「發布」。

> 這份規則保證：每個人只能讀寫自己的存檔；排行榜大家都看得到，但只能改自己那一筆；聊天室只能用自己的身分發言、只能刪自己的訊息。

## 第 4 步：取得設定並填進 `firebase-config.js`

1. 左上角齒輪 →「專案設定」→「一般」分頁，往下找「你的應用程式」。
2. 按網頁圖示 **`</>`**，暱稱隨意（例如 `bump-web`），**不用**勾 Firebase Hosting，按「註冊應用程式」。
3. 畫面會出現一段 `const firebaseConfig = { apiKey: "...", authDomain: "...", ... }`。
4. 用記事本（Windows）或文字編輯（Mac）打開 `firebase-config.js`，把 6 個欄位對應的值貼進引號裡，存檔。

完成後大概長這樣（值是你自己的）：

```js
window.FIREBASE_CONFIG = {
  apiKey: "AIzaSy....",
  authDomain: "bump-volleyball.firebaseapp.com",
  projectId: "bump-volleyball",
  storageBucket: "bump-volleyball.firebasestorage.app",
  messagingSenderId: "1234567890",
  appId: "1:1234567890:web:abcd1234"
};
```

> `apiKey` 不是密碼，放在公開網站上是正常的；真正保護資料的是第 3 步的安全規則。

## 第 5 步：放上 GitHub Pages（免費網址）

1. 到 <https://github.com> 註冊並登入。
2. 右上角「+ → New repository」，名稱填 `bump-volleyball`，選 **Public**，按「Create repository」。
3. 在新的 repository 頁面按「uploading an existing file」，把 `index.html` 和 `firebase-config.js` 兩個檔案拖進去，按「Commit changes」。
4. 進「Settings → Pages」，Source 選「Deploy from a branch」，Branch 選 `main`、資料夾選 `/ (root)`，按 Save。
5. 等 1～2 分鐘，頁面上方會出現網址：`https://你的帳號.github.io/bump-volleyball/`。

## 第 6 步：把網址加進 Firebase 授權網域

1. 回到 Firebase：「Authentication → 設定 → 已授權網域」，按「新增網域」。
2. 填入 `你的帳號.github.io`（不要加 `https://`，也不要加後面的 `/bump-volleyball/`），新增。

完成！打開網址，封面上方會出現「用 Google 登入・雲端存檔」。

---

## 給家人的使用說明

- 打開網址 → 按「用 Google 登入」→ 選自己的 Google 帳號。
- 登入後，生涯模式和經理人模式的進度會自動存到雲端；在別的裝置登入同一個帳號就能接著玩。
- 封面的「家族排行榜」可以看到大家的生涯評價和經理人最高聯賽，也能在那裡改暱稱。
- 登入後左上角有「聊天」按鈕，可以跟家人即時聊天，有新訊息會出現紅色數字；比賽進行中會自動隱藏。自己的訊息可以刪除。
- 沒登入也能玩，只是存檔只在那台裝置。登入後，這台裝置原本的存檔會自動上傳。

## 常見問題

**按登入沒反應，或出現「disallowed_useragent」**
在 LINE、Facebook 等 App 裡直接點開連結時，Google 會擋登入。請點右上角選單「用 Safari／Chrome 開啟」。

**出現「這個網址還沒加進 Firebase 的授權網域」**
第 6 步的網域沒填對，確認是 `你的帳號.github.io`。

**排行榜顯示「讀取失敗」**
第 3 步的規則沒有發布成功，回去再貼一次並按「發布」。

**遊戲更新時要怎麼做？**
1. 到 GitHub 的 `bump-volleyball` repository →「Add file → Upload files」。
2. 把新的 `index.html` 拖進去（會自動覆蓋舊的），按「Commit changes」。
3. 等 1～2 分鐘，重新整理遊戲網址就是新版。手機如果還是舊版，關掉分頁重開。
`firebase-config.js` 不用動，大家的存檔也不受影響。
如果更新說明有提到「規則有變」，再把新的 `firestore.rules` 貼到 Firebase 的 Firestore →「規則」並發布。

**之前在 Claude 上玩的進度可以搬過來嗎？**
兩邊是不同網址，存檔不會自動帶過來，需要重新開始。

**會花錢嗎？**
不會。Firebase 免費方案（Spark）的額度，家庭幾個人玩綽綽有餘，也不需要綁信用卡。

## 進階：只讓家人登入（選用）

目前任何拿到網址的人都能登入，並出現在排行榜上。如果只想讓家人用，可以把 `firestore.rules` 裡兩處的
`request.auth != null` 改成下面這樣（換成家人的 Gmail），再發布：

```
request.auth != null && request.auth.token.email in ['爸爸@gmail.com', '媽媽@gmail.com', '小孩@gmail.com']
```

不在名單上的人一樣能登入看到遊戲，但無法使用雲端存檔和排行榜。
