# Google Trends 總體商業環境關鍵字儀表板

每月 Google Trends 熱搜關鍵字，經過白名單過濾後，只保留與**總體商業環境**相關的詞彙，並以互動式儀表板呈現跨月趨勢。

🔗 **線上儀表板**：https://resatseng.github.io/google-trends-dashboard/

---

## 功能

- **時間區間選擇**：自由選取起始／結束月份，統計數字與圖表同步更新
- **類別篩選**：點選類別標籤快速過濾
- **關鍵字搜尋**：即時篩選
- **Top 20 長條圖**：依區間累計次數排名
- **月份趨勢 Sparkline**：每個關鍵字的月別出現狀況，選取範圍外的月份自動淡出
- **可排序表格**：依關鍵字、類別、次數排序

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
| 貿易/地緣 | 國際貿易與地緣政治 | 關稅、美國製造、中東、g7 |
| AI/科技趨勢 | 人工智慧與科技動態 | ai、anthropic、claude、h200、computex、spacex |
| 半導體/製造業 | 半導體產業與先進製造 | 半導體、晶圓代工、先進封裝、dram、伺服器 |
| 能源 | 能源轉型與新興技術 | 能源、低軌衛星、smr、hvdc |
| 產業活動/研調 | 展覽活動與市場研調 | computex、trendforce、ai expo、海底電纜 |

---

## 更新方式

### 1. 新增月份資料

將新的月份 CSV 放入 `google_trending/` 目錄，命名格式：

```
monthly_trends_summary_YYYY-MM.csv
```

CSV 格式（兩欄，無 header 或第一行為 `關鍵字,出現次數`）：

```
關鍵字,出現次數
ai,3
降息,2
...
```

### 2. 更新 MONTHS 清單

編輯 `generate_dashboard.py`，在 `MONTHS` 清單加入新月份：

```python
MONTHS = ['2025-11', '2025-12', '2026-01', '2026-02', '2026-03', '2026-04', '2026-05']
```

### 3. 重新產生儀表板

```bash
cd google_trending
python generate_dashboard.py
```

### 4. 部署更新

```bash
cp google_trending/keyword_dashboard.html ../dashboard-deploy/index.html
cd ../dashboard-deploy
git add index.html
git commit -m "update: 更新至 YYYY-MM 月份資料"
git push
```

GitHub Pages 收到 push 後約 1 分鐘自動更新。

---

## 檔案結構

```
google_trending/
├── generate_dashboard.py        # 主程式：過濾 + 產生 HTML
├── filter_keywords.py           # 黑名單過濾（舊版，供參考）
├── keyword_dashboard.html       # 產生的儀表板（本地預覽用）
├── monthly_trends_summary_*.csv # 各月原始資料
└── filtered_*.csv               # 黑名單過濾後的結果

dashboard-deploy/                # GitHub Pages 部署目錄
├── index.html                   # 儀表板（即 keyword_dashboard.html）
└── README.md
```

---

## 技術說明

- 純靜態 HTML，無需後端，可直接用瀏覽器開啟
- 圖表使用 [Chart.js 4.4](https://www.chartjs.org/)（CDN）
- 所有資料內嵌於 HTML，單一檔案即可分享
