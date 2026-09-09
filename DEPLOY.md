# 羽球拍配色模擬器 — 部署說明

單一靜態 HTML，沒有後端、沒有外部套件、不需要建置流程。
圖片處理全在瀏覽器端（Canvas），使用者的照片不會離開他們的裝置，
所以不用處理上傳、儲存、隱私政策這些事，流量成本也接近零。

檔案：`index.html`（約 31 KB）

---

## 部署方式（擇一）

### 1. Cloudflare Pages — 最推薦
免費、全球 CDN、自動 HTTPS、支援自訂網域。

1. 到 dash.cloudflare.com → Workers & Pages → Create → Pages
2. 選 **Upload assets**（不用接 Git 也可以）
3. 把 `index.html` 拖進去，命名專案，Deploy
4. 幾秒後拿到 `專案名.pages.dev` 的網址

更新就是再拖一次新檔案。

### 2. GitHub Pages
適合你想順便做版本控管。

```bash
git init
git add index.html
git commit -m "init"
git branch -M main
git remote add origin git@github.com:<帳號>/<repo>.git
git push -u origin main
```
到 repo 的 Settings → Pages → Source 選 `main` / `root` → 存檔。
網址是 `https://<帳號>.github.io/<repo>/`。

### 3. Netlify Drop
最快，開 app.netlify.com/drop，把檔案拖進網頁就上線了，適合先給球友試用。

### 4. 自己的主機
丟進 Nginx / Apache 的網站根目錄即可，不需要任何設定。

---

## 上線前建議檢查

- **iPhone 的 HEIC 照片**：Safari 開得起來，但 Android 和桌機 Chrome 解析不了。
  程式已經會跳提示教使用者改設定或轉成 JPG，這是實務上最常遇到的客訴。
- **手機版面**：寬度小於 920px 會自動變成上下排列，觸控已支援（定位點按住才顯示）。
- **效能**：握把是逐像素運算，拖曳定位點時每一格都會重畫。
  一般手機沒問題，但如果收到低階機種卡頓的回報，
  可以在 `pointermove` 裡加 `requestAnimationFrame` 節流。
- **設定記憶**：用 `localStorage`，每個使用者存在自己的瀏覽器，
  彼此不會互相影響，也不需要帳號系統。無痕模式下不會記憶，但不影響使用。

## 之後想加功能的話

目前是純前端，如果要加「分享配色連結」，
不需要資料庫也能做：把參數編碼成網址的 query string，
開啟時讀回來即可。要做「作品牆」那種需要存圖的功能才會需要後端。
