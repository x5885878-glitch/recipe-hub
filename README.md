# 廚房作戰手冊 · Kitchen Playbook

標準化、可複製的個人食譜系統。每道菜是一個獨立 HTML 頁(離線可看、可列印),
統一入口把所有菜與「戰鬥日」串成一個目錄。專為在廚房用平板邊看邊做菜設計。

## 結構

```
recipe-hub/
├── index.html              ← 統一入口(改 CATALOG 就能新增項目)
├── assets/
│   └── style.css           ← 共用設計系統(改這裡 = 全站改版)
├── dishes/                 ← 單菜頁(每道菜一個檔)
│   ├── gongbao-chicken.html
│   └── tomato-egg-soup.html
├── battle-days/            ← 戰鬥日(多菜時間軸)
│   └── 2026-06-28.html
├── _templates/             ← 空白模板,新增時複製用
│   ├── dish.template.html
│   └── battle-day.template.html
└── README.md
```

兩種瀏覽路徑:
- 統一入口 → 某道菜 → 完整食譜 + 流程
- 統一入口 → 某個戰鬥日 → 時間軸,每個步驟對標到具體餐點

## 新增一道菜

1. 複製 `_templates/dish.template.html` 到 `dishes/{英文-id}.html`
2. 填入內容(`{...}` 是要替換的佔位符)。**標準化指標**區塊必填——這是「精緻化」的核心。
3. 到 `index.html` 的 `CATALOG.dishes` 加一筆:
   ```js
   { id:"...", name:"...", category:"...", time:30, difficulty:"中", tags:["..."], url:"dishes/英文-id.html" }
   ```

## 新增一個戰鬥日

1. 複製 `_templates/battle-day.template.html` 到 `battle-days/{日期}.html`
2. 每個時間點一張 `tl-item` 卡,用 `tl-ref` 把它**對標**回某道菜的流程第幾步。
3. 到 `index.html` 的 `CATALOG.battleDays` 加一筆。

## 標準化欄位(每道菜都要有)

| 欄位 | 意義 |
|------|------|
| 份量 / 總時間 / 實作時間 / 難度 | 規格 |
| 食材(分組 + 量 + 處理方式) | 可複製的配方 |
| 流程(火候 / 溫度 / 時間 / 關鍵點) | 可複製的操作 |
| 標準化指標(熟度 / 味型鹹度 / 成敗判定) | **精緻化**:讓每次做出來一致 |

## 部署到 GitHub Pages

```bash
cd recipe-hub
git init
git add .
git commit -m "init: 廚房作戰手冊"
git branch -M main
git remote add origin https://github.com/<你的帳號>/<repo名>.git
git push -u origin main
```

然後在 GitHub repo → **Settings → Pages** → Source 選 `main` branch、`/ (root)` → Save。
幾分鐘後網址會是 `https://<你的帳號>.github.io/<repo名>/`。

> 平板在廚房直接開這個網址即可。建議用瀏覽器「加入主畫面」做成 App 圖示。

## 本機預覽

直接雙擊 `index.html` 即可(本系統不依賴 fetch,單純連結跳轉)。
若想模擬正式環境:`python3 -m http.server` 後開 `http://localhost:8000`。

## 設計重點(廚房友善)

- 大字、高對比、大點擊區;數字(時間/溫度/火候)用等寬字,像儀表讀數
- 點步驟可打勾(記住進度)、右下「下廚模式」一鍵放大
- 可列印成 station card(列印時自動隱藏導覽)
