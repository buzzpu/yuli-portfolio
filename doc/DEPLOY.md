# 上線與維護手冊

本站的部署現況、日常更新流程、以及遇到問題時的查修清單。

---

## 現況

| 項目 | 值 |
|---|---|
| 正式網址 | https://yuli.playplay.games/ |
| GitHub repo | [buzzpu/yuli-portfolio](https://github.com/buzzpu/yuli-portfolio) · public |
| 部署方式 | GitHub Pages · `main` 分支 · `/` (root) |
| 網域 DNS | Cloudflare · `yuli` CNAME → `buzzpu.github.io` · **DNS only（灰雲）** |
| HTTPS | Let's Encrypt 自動簽發 · `Enforce HTTPS` 已開啟 |
| 舊網址 | `buzzpu.github.io/yuli-portfolio/` → 301 轉至正式網址 |
| commit 身分 | 僅此 repo：`yuli <buzz.pu@gmail.com>` |

不需要 Vercel 或任何 build 工具 —— 純靜態 HTML，Pages 直接吃。

---

## 日常更新流程

改完檔案後：

```bash
cd ~/Downloads/yuli-portfolio
git add -A
git commit -m "換掉 Ali Baba 的主視覺"
git push
```

Push 後約 1 分鐘網站更新。看不到變化先按 `Cmd`+`Shift`+`R` 強制重新整理，瀏覽器很愛快取圖片。

Commit 訊息寫「改了什麼」而不是「改了檔案」。未來想找「主視覺是哪次換的」時，`換掉 Ali Baba 的主視覺` 有用，`update` 沒用。

### 換圖片的完整步驟

1. 依 `doc/IMAGES.md` 的規格表確認尺寸與比例
2. 用 [squoosh.app](https://squoosh.app) 壓縮（JPG 品質 82 或 WebP 品質 80）
3. 檔名依 `data-slot` 命名，覆蓋 `images/` 裡的同名檔案
4. `git add -A && git commit && git push`

檔名清單見 `doc/IMAGE-NAMING.md`。

---

## Cloudflare DNS 設定

目前只有一筆紀錄在服務這個站：

| 類型 | 名稱 | 目標 | Proxy 狀態 |
|---|---|---|---|
| CNAME | `yuli` | `buzzpu.github.io` | **DNS only（灰雲）** |

子網域只需要一筆 CNAME，不需要 4 筆 A record，也用不到 CNAME flattening（那是根網域才有的問題）。

### 兩個不能踩的雷

**橘雲必須關著。** 開啟 Proxy 的話，GitHub 查 DNS 只會看到 Cloudflare 的 IP、無法通過網域驗證，Let's Encrypt 憑證會簽不出來。

**若之後要開橘雲，SSL 模式必須是 Full (strict)。** 想用 Cloudflare 快取而打開橘雲時，同時要到 **SSL/TLS → Overview** 設成 `Full (strict)`。留在預設的 `Flexible` 會造成無限轉址（`ERR_TOO_MANY_REDIRECTS`）。不確定就一直留灰雲 —— Pages 本身已經有 CDN。

### 驗證指令

```bash
dig +short CNAME yuli.playplay.games        # 應回 buzzpu.github.io.
dig +short A yuli.playplay.games            # 應回 185.199.108-111.153
curl -I https://yuli.playplay.games/        # 應回 HTTP 200
curl -I http://yuli.playplay.games/         # 應回 301 → https://
```

`dig` 若回傳 `104.21.x.x` 這類 IP，代表橘雲被打開了。

---

## 踩雷清單

### 本機好好的，線上破圖

幾乎都是**大小寫**。macOS 檔名不分大小寫，GitHub Pages 跑在 Linux 上會分 —— `Streamwalker-01.jpg` 和 `streamwalker-01.jpg` 在線上是兩個不同檔案。目前 27 張圖全部小寫且對得上，之後加圖要留意。

### Push 被拒：rejected / non-fast-forward

遠端有本機沒有的 commit。先拉再推：

```bash
git pull --rebase origin main
git push
```

### 本機 DNS 快取造成的假象

改完 Cloudflare DNS 後，macOS 的解析器可能還記著舊紀錄，導致 `curl` 連到舊 IP、看到舊的憑證。清快取：

```bash
sudo dscacheutil -flushcache
sudo killall -HUP mDNSResponder
```

或用 `curl --resolve` 繞過快取直接測：

```bash
curl -sI --resolve yuli.playplay.games:443:185.199.108.153 https://yuli.playplay.games/
```

### Commit 沒掛在 GitHub 帳號上

GitHub 用 **email** 認人，不是 commit 裡的名字。`buzz.pu@gmail.com` 必須加進 [Settings → Emails](https://github.com/settings/emails) 並完成驗證。

### 底線開頭的檔案在線上消失

Pages 底層是 Jekyll，會忽略 `_` 開頭的檔名。真的遇到就在根目錄放一個空的 `.nojekyll` 關掉這個行為。

---

## 待辦

- [ ] **圖片壓縮**：目前 `images/` 共 36 MB，最重的 `streamwalker-03.jpg` 單張 3.5 MB。轉 WebP 品質 80 約可降到 6–8 MB 且肉眼無差，手機體感差異明顯。
- [ ] **`personal-05.jpg` 比例不合**：1200×1600 直式塞在 1:1 方形格（`gallery-item--5`），`object-fit: cover` 會裁掉上下各約 1/4。主體不在正中間的話建議換到 item--3 / item--4（3:4 直式位）或重新裁切。
- [ ] **`index-demo.html` 的處置**：目前公開可從 `/index-demo.html` 開啟，內容是 picsum 假圖，且含已在 `index.html` 修掉的畫廊 CSS bug。建議刪除或一併修正。

---

## 版控範圍

`.gitignore` 排除了這些：

| 項目 | 原因 |
|---|---|
| `web_img/` | 原始圖檔備份（36 MB），網站不使用，正式圖片在 `images/` |
| `web_files/` | 開發過程的歷史版本 HTML |
| `.DS_Store` · `*.bak` | macOS 與本機備份 |
| `.claude/settings.local.json` | 含本機絕對路徑，不需分享 |
