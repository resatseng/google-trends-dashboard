# 法規商機看板

監控 ESG、資安、碳費、碳關稅、個資、食安等法規/揭露義務動態，用 Gemini 萃取「法規名稱／生效時間／適用對象」，作為「因法規要求而可能需要導入/升級 ERP、合規管理系統」的商機初篩訊號；並具備自動偵測機制，能發現清單以外沒預先想到的新興法規。

🔗 **線上看板**：https://resatseng.github.io/google-trends-dashboard/regulation/

---

## 這是什麼

法規上路往往會逼企業導入或升級系統（例如強制電子發票、碳盤查申報、資安法適用範圍擴大），這對 ERP 供應商而言是商機訊號。這個看板把「有哪些法規正在發生、誰會受影響、什麼時候生效」自動整理出來，讓業務可以在法規剛上新聞時就掌握潛在客戶名單的輪廓。

## 監控方式

### 固定主題查詢
針對已知會影響 ERP 需求的法規領域做固定關鍵字查詢，包含：永續報告書強制申報、資安法適用對象、ESG揭露強制、電子發票強制、碳盤查申報、碳費徵收對象、CBAM碳關稅、個資法裁罰、產品碳足跡揭露、食品安全新規定、工程業ISO強制等。

### 自動偵測（發現清單以外的新興法規）
除了固定主題，另外用「強制實施」「新制上路」「主管機關公告」「ISO認證強制」等**不綁定特定法規名稱**的廣泛查詢語句，讓 LLM 自己從新聞裡辨識出「這是什麼法規」，藉此發現清單裡沒預先想到的新興法規（例如某天冒出的營建業新規、勞動法規修法、國際準則如 CRA／UNECE 車用資安法規等），依 LLM 萃取出的法規名稱分組，自動併入看板，不需要人工事先列出主題清單。

## 判斷方式

每則新聞標題交給 Gemini（`gemini-2.5-flash`）判斷：
- 是否在講一項具體的法規/揭露義務要求（而非泛泛而談）
- 法規/制度名稱、生效日/申報期限、適用對象（如上市櫃公司、實收資本額20億以上、特定行業）

所有主題（含新發現的）再統一分類到 **ESG/永續揭露** / **資安/網路安全** / **碳排/環保法規** / **勞動法規** / **食品安全** / **稅務/財務** / **採購/其他法規** 七大類，純英文或英文夾雜的主題名稱會另外補上簡短中文標籤方便閱讀。

## 看板功能

- **主題索引**：依七大分類分組列出所有偵測到的法規主題，點選可直接跳到該區塊
- **本週新聞**：獨立區塊呈現近 7 天內的新聞
- **搜尋**：依法規名稱或新聞標題篩選
- **新聞日期區間篩選**：自由選取起訖日期
- **全部展開／收合**：一鍵切換所有卡片的展開狀態

---

## 自動更新流程

資料更新完全自動化，由 Windows Task Scheduler「業務情報看板_每日更新」於每日 07:00 觸發（與[競品情報看板](../competitor/README.md)共用同一支排程）：

```
Task Scheduler（每日 07:00）
  → daily_intel_pipeline.bat
      ├── python competitor_news_monitor.py   # 競品情報看板（見該看板 README）
      ├── python regulation_news_monitor.py   # 抓新聞 + Gemini萃取 + 分類 → regulation_data.json
      ├── python build_pages.py               # 套進頁面範本，輸出到 dashboard-deploy
      └── git commit + git push → GitHub Pages 自動發布
```

網址約在排程執行後 **1 分鐘**內更新。執行紀錄寫在 `google_trending/log_intel.txt`。

## 手動補跑

```bash
cd D:\yujui\Labeling\auto-labeling\google_trending
python regulation_news_monitor.py   # 重新抓新聞、萃取並分類
python build_pages.py               # 套進範本，輸出到 dashboard-deploy
cd D:\yujui\dashboard-deploy
git add regulation
git commit -m "update: 法規商機看板更新"
git push
```

## 檔案結構

```
google_trending/
├── regulation_news_monitor.py         # 抓新聞 + Gemini萃取 + 分類主程式
├── regulation_dashboard_template.html # 看板頁面範本
├── build_pages.py                     # 套資料進範本，輸出到 dashboard-deploy
└── regulation_data.json               # 最新一次執行結果（看板資料來源）

dashboard-deploy/regulation/
├── index.html                         # 看板（build_pages.py 產生）
└── README.md
```
