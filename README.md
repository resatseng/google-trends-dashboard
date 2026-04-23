# Google Trends 總體商業環境關鍵字儀表板

每週自動爬取 Google Trends 熱搜關鍵字，經過白名單過濾後，只保留與**總體商業環境**相關的詞彙，並以互動式儀表板呈現跨月趨勢與主題聚合分析。

🔗 **線上儀表板**：https://resatseng.github.io/google-trends-dashboard/

---

## 功能

### 關鍵字視角
- **時間區間選擇**：自由選取起始／結束月份，所有統計與圖表同步更新
- **洞察摘要列**：自動顯示活躍關鍵字數、新興／上升數、熱度最高主題
- **Top 排名圖表**：長條圖可切換「關鍵字 Top 20」與「主題排名」
- **類別篩選**：點選類別標籤快速過濾
- **趨勢篩選**：依 ★ 新興 / ↑ 上升 / → 持平 / ↓ 下降 篩選，附計算方式說明（ℹ 展開）
- **關鍵字搜尋**：即時篩選
- **首現標記**：關鍵字在選取區間內首次出現時顯示「初現 YYYY-MM」標籤
- **月份趨勢 Sparkline**：月別出現狀況，選取範圍外自動淡出
- **CSV 匯出**：下載當前篩選結果（含首現月份欄位）
- **可排序表格**：依關鍵字、類別、次數、趨勢排序
- **URL 狀態記憶**：篩選條件寫入網址，可直接分享特定視角

### 主題視角
- **主題熱度折線圖**：各主題月別熱度折線，選取區間高亮，範圍外虛線淡化
- **主題排名表格**：依區間合計次數排序，含趨勢標籤、Sparkline、成員關鍵字 chips
- **趨勢計算說明卡**：常駐顯示四種趨勢標籤的判斷條件
- **關鍵字去重**：每個關鍵字只計入第一個匹配的主題，避免重複計算

---

## 資料期間

| 月份 | 資料來源 |
|------|---------|
| 2025-11 | monthly_trends_summary_2025-11.csv |
| 2025-12 | monthly_trends_summary_2025-12.csv |
| 2026-01 | monthly_trends_summary_2026-01.csv |
| 2026-02 | monthly_trends_summary_2026-02.csv |
| 2026-03 | monthly_trends_summary_2026-03.csv |
| 2026-04 | monthly_trends_summary_2026-04.csv |

---

## 關鍵字類別（白名單）

| 類別 | 說明 | 範例 |
|------|------|------|
| 總經指標 | 宏觀經濟數據與政策 | cpi、ppi、gdp、降息、聯準會、非農就業數據 |
| 匯率/貨幣 | 主要貨幣與匯率 | 台幣匯率、日圓、人民幣、美元指數 |
| 大宗商品 | 能源與工業原物料 | 原油、銅價、黃金、天然氣、布蘭特原油 |
| 貿易/地緣 | 國際貿易與地緣政治 | 關稅、美國製造、中東、伊朗、無人機、飛彈 |
| AI/科技趨勢 | 人工智慧與科技動態 | ai、anthropic、claude、h200、computex、spacex |
| 半導體/製造業 | 半導體產業與先進製造 | 半導體、晶圓代工、先進封裝、dram、伺服器 |
| 能源 | 能源轉型與新興技術 | 能源、低軌衛星、smr、hvdc、bloom energy |
| 產業活動/研調 | 展覽活動與市場研調 | computex、trendforce、ai expo、海底電纜 |

---

## 趨勢標籤計算方式

將選取的時間區間切為**前後兩半**，比較各半段的出現次數合計：

| 標籤 | 條件 | 說明 |
|------|------|------|
| ★ 新興 | 前半段 = 0，後半段 > 0 | 本期從無到有 |
| ↑ 上升 | 後半段 > 前半段 × 1.2 + 0.5 | 熱度顯著提升 |
| → 持平 | 差異不顯著 | 熱度維持穩定 |
| ↓ 下降 | 前半段 > 後半段 × 1.2 + 0.5 | 熱度明顯退燒 |

> × 1.2：需超過 20% 差距才判定趨勢，避免小幅波動誤判。  
> + 0.5：低頻關鍵字緩衝值。趨勢會隨選取區間變動，區間越短判斷越敏感。

---

## 主題聚合（主題視角）

多個語意相關的關鍵字會被聚合為同一主題，合計計算熱度：

| 主題 | 說明 | 種子關鍵字（部分） |
|------|------|------|
| 地緣衝突 | 中東局勢、伊朗、無人機、飛彈 | 伊朗、中東、無人機、飛彈、荷姆茲、烏克蘭 |
| AI科技浪潮 | 大模型、AI硬體、太空科技 | anthropic、claude、spacex、nasa、groq |
| 半導體供應鏈 | 晶圓代工、先進封裝、記憶體 | 半導體、晶圓、dram、砷化鎵、銅箔 |
| AI伺服器需求 | 伺服器、液冷、CPO | 伺服器、液冷、cpo、水冷 |
| Fed政策與通膨 | 利率決策、通膨指標 | cpi、ppi、降息、聯準會、非農、美債 |
| 油價震盪 | 國際油價、天然氣 | 原油、油價、天然氣、布蘭特、wti |
| 能源轉型 | 再生能源、SMR、HVDC | 能源、smr、hvdc、bloom energy、衛星 |
| 關稅與貿易戰 | 美中貿易、川普政策 | 關稅、美國製造、g7、川普 |
| 匯率波動 | 台幣、日圓、人民幣走勢 | 匯率、日圓、人民幣、美元指數 |
| 黃金與銅 | 避險資產與工業金屬 | 黃金、金價、銅價 |

---

## 爬取條件

| 參數 | 設定 |
|------|------|
| 地區 | 台灣（geo=TW） |
| 時間範圍 | 過去 7 天（date=today 1-m, hours=168） |
| 類別 | 商業與經濟（category=3） |
| 資料來源 | Google Trends 熱搜榜 |

每週執行一次，當月結束時彙整為月報（`monthly_trends_summary_YYYY-MM.csv`），再經白名單過濾後呈現於儀表板。

---

## 自動更新流程

資料更新完全自動化，由 Windows Task Scheduler 定期觸發：

```
Task Scheduler
  → google_trending.bat
      ├── papermill 執行 google_trending.ipynb   # 爬取 Google Trends
      ├── python generate_dashboard.py           # 過濾 + 產生 HTML
      ├── copy keyword_dashboard.html → index.html
      └── git commit + git push → GitHub Pages 自動發布
```

網址約在排程執行後 **1 分鐘**內更新。

---

## 手動更新方式（補跑）

### 1. 執行爬蟲 + 彙整

在 Jupyter 跑 `google_trending.ipynb` 兩個 cell：
- `fetch_all_trends_fullpage()` — 爬取當月資料
- `summarize_monthly_trends()` — 彙整成月報 CSV

### 2. 重新產生儀表板

```bash
cd D:\yujui\Labeling\auto-labeling\google_trending
python generate_dashboard.py
```

> **注意**：不需要手動修改 `MONTHS` 清單，程式會自動偵測資料夾內的 CSV 檔案。

### 3. 部署更新

```bash
copy google_trending\keyword_dashboard.html dashboard-deploy\index.html
cd dashboard-deploy
git add index.html
git commit -m "update: 更新至 YYYY-MM 月份資料"
git push
```

---

## 檔案結構

```
google_trending/                         # 爬蟲與儀表板產生
├── google_trending.ipynb                # 爬蟲主程式（papermill 執行）
├── google_trending_output.ipynb         # 最近一次執行結果
├── google_trending.bat                  # 自動化排程腳本
├── generate_dashboard.py                # 白名單過濾 + 產生 HTML
├── keyword_dashboard.html               # 產生的儀表板（本地預覽用）
├── weekly_trends_log_YYYY-MM.csv        # 每週爬蟲原始資料
└── monthly_trends_summary_YYYY-MM.csv   # 彙整後月報（儀表板資料來源）

dashboard-deploy/                        # GitHub Pages 部署目錄
├── index.html                           # 儀表板（即 keyword_dashboard.html）
└── README.md
```

---

## 技術說明

- 純靜態 HTML，無需後端，可直接用瀏覽器開啟或分享
- 圖表使用 [Chart.js 4.4](https://www.chartjs.org/)（CDN）
- 所有資料內嵌於 HTML，單一檔案即可分享
- 爬蟲使用 Selenium headless Chrome，透過 [papermill](https://papermill.readthedocs.io/) 排程執行
