# images/ 資料夾

你的作品圖片放這裡。

---

## 檔案命名規則

檔名必須對應 HTML 中的 `data-slot` 屬性值。共 27 個位置：

### 遊戲美術（9 張）

**Streamwalker Tribes**
- `streamwalker-01.jpg` — 1600×1200 · Key Art 主視覺
- `streamwalker-02.jpg` — 1600×1200 · Character Sheet 角色設計
- `streamwalker-03.jpg` — 2400×1350 · Banner 卡池橫幅

**Ali Baba**
- `alibaba-01.jpg` — 1600×1200 · Protagonist Trio 三位主角
- `alibaba-02.jpg` — 1600×1200 · Genie Jackpot 神燈精靈演出
- `alibaba-03.jpg` — 2400×1350 · Game Screen 遊戲介面全景

**Legend of Pirate 鬼盜傳奇**
- `pirate-01.jpg` — 1600×1200 · Underwater Environment 海底環境
- `pirate-02.jpg` — 1600×1200 · Creature Roster 生物圖鑑
- `pirate-03.jpg` — 2400×1350 · Capture Sequence 捕獲演出

---

### 專案管理（9 張）

**XA Data Push**
- `xa-01.jpg` — 1600×1200 · System Architecture 系統架構
- `xa-02.jpg` — 1600×1200 · Push Dashboard 推送後台
- `xa-03.jpg` — 2400×1350 · Analytics Visualization 數據視覺化

**Custom Sales & Orders**
- `sales-01.jpg` — 1600×1200 · Product Configurator 產品配置器
- `sales-02.jpg` — 1600×1200 · Order Flow 訂單流程
- `sales-03.jpg` — 2400×1350 · Sales Dashboard 業務儀表板

**Brand & Talent**
- `talent-01.jpg` — 1600×1200 · Artist Roster 藝人陣容
- `talent-02.jpg` — 1600×1200 · Course Catalog 課程總覽
- `talent-03.jpg` — 2400×1350 · Cooperation Inquiry 合作窗口

---

### 個人作品畫廊（9 張，Bento 不規則排版）

- `personal-01.jpg` — 1600×1600 · Featured 主打（大方形）
- `personal-02.jpg` — 2400×1350 · Wide Piece 橫幅
- `personal-03.jpg` — 1200×1600 · Portrait 直式
- `personal-04.jpg` — 1200×1600 · Portrait 直式
- `personal-05.jpg` — 1200×1200 · Square 方形
- `personal-06.jpg` — 1600×1200 · Landscape 橫式
- `personal-07.jpg` — 1600×1200 · Landscape 橫式
- `personal-08.jpg` — 1200×1200 · Square 方形
- `personal-09.jpg` — 2400×1350 · Finale 壓軸橫幅

---

## 檔案格式

- **推薦**：`.webp`（比 JPG 小 25-35%）
- **通用**：`.jpg`（品質 82）
- 檔名要與 HTML 中的 src 路徑一致

## 尺寸與大小規範

詳見專案根目錄的 `IMAGES.md`。

## 加入圖片到 HTML

編輯 `index.html`，找到對應的 `data-slot="XXX"` 位置，在該 div 內加入：

```html
<img src="images/XXX.jpg" alt="描述" loading="lazy" />
```

CSS 會自動隱藏原本的佔位符文字，圖片會填滿容器並使用 `object-fit: cover` 裁切。
