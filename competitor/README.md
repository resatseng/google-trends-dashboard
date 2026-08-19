# 競品情報看板

從 Google News 監控 ERP/CRM/企業管理軟體競爭對手的相關新聞，經 Gemini 逐則判斷「是否真的與該公司相關」與「是否為負面事件」，依競爭者分區呈現，作為業務負面情報的初篩訊號；同時監控本公司（鼎新數智）自身新聞的正負面動態。

🔗 **線上看板**：https://resatseng.github.io/google-trends-dashboard/competitor/

---

## 這是什麼

業務同仁平時很難逐一追蹤每家競爭對手的負面消息（當機、資安漏洞、裁罰、違約、倒閉、客訴等）。這個看板把這件事自動化：從業務日報萃取出的競爭者名單出發，逐家查 Google News，用 LLM 過濾掉「同名不同產業」的雜訊，只留下真正跟競爭對手有關的新聞，並標記是否為負面事件。

## 監控範圍

- **競爭者來源**：業務日報 `L5_competitors` 欄位萃取出的客戶備註（`競爭者資訊_展開.xlsx`），只要客戶備註中提及過一次即納入監測候選（不設家數上限、不設最低提及次數門檻）
- **名單清理**：兩輪 LLM 清理去除雜訊——
  1. 過濾「其他既有ERP系統廠商」「他牌ERP廠商」等空泛描述（非具體公司名稱）
  2. 過濾同賽道詞彙但實際非 ERP/CRM 競品的公司（如中華電信、VMware 等）
- **分類**：LLM 依客戶備註產品線關鍵字，分為 **ERP専屬** / **ERP同類・BPM** / **ERP同類・MES** / **ERP同類・系統整合(SI)**，加上獨立呈現的 **本公司**（鼎新數智）分組
- **本公司新聞**：另外查詢「鼎新數智／鼎新電腦／Digiwin」，不限負面，用 LLM 標記正面／負面／中性，掌握公關動態與潛在風險

## 判斷方式

- 查詢詞：競爭者簡稱 +「當機/裁罰/違約/倒閉/停業/求償/資安/客訴」等負面關鍵字
- 每家競爭者一次抓取的新聞，一次性交給 Gemini（`gemini-2.5-flash`）批次判斷，避免短公司名（如「正航」「凌越」）誤命中無關新聞——例如同名的上市工業股、營造公司、地名、人名、遊戲、諧音哏，都會被排除
- 此看板僅供內部業務參考，負面事件仍建議人工複核連結後之原文再行判斷

## 看板功能

- **競爭者索引**：依「本公司／ERP専屬／ERP同類」分組列出所有偵測到相關新聞的競爭者，點選可直接跳到該區塊
- **本週新聞**：獨立區塊呈現近 7 天內的新聞，分「本公司」與「競爭者」兩組
- **搜尋**：依競爭者名稱或新聞標題篩選
- **只顯示有負面事件的競爭者**：勾選後只列出有負面新聞的競爭者，且卡片內只展開負面新聞項目
- **新聞日期區間篩選**：自由選取起訖日期
- **全部展開／收合**：一鍵切換所有卡片的展開狀態

---

## 自動更新流程

資料更新完全自動化，由 Windows Task Scheduler「業務情報看板_每日更新」於每日 07:00 觸發：

```
Task Scheduler（每日 07:00）
  → daily_intel_pipeline.bat
      ├── python competitor_news_monitor.py   # 抓新聞 + Gemini分類 → dashboard_data.json
      ├── python regulation_news_monitor.py   # 法規商機看板（見該看板 README）
      ├── python build_pages.py               # 套進頁面範本，輸出到 dashboard-deploy
      └── git commit + git push → GitHub Pages 自動發布
```

網址約在排程執行後 **1 分鐘**內更新。執行紀錄寫在 `google_trending/log_intel.txt`。

## 手動補跑

```bash
cd D:\yujui\Labeling\auto-labeling\google_trending
python competitor_news_monitor.py   # 重新抓新聞並分類
python build_pages.py               # 套進範本，輸出到 dashboard-deploy
cd D:\yujui\dashboard-deploy
git add competitor
git commit -m "update: 競品情報看板更新"
git push
```

## 檔案結構

```
google_trending/
├── competitor_news_monitor.py         # 抓新聞 + Gemini分類主程式
├── competitor_dashboard_template.html # 看板頁面範本
├── build_pages.py                     # 套資料進範本，輸出到 dashboard-deploy
├── dashboard_data.json                # 最新一次執行結果（看板資料來源）
├── vague_names_excluded.json          # LLM判定為空泛描述而排除的名單
├── non_erp_excluded.json              # LLM判定為非ERP相關而排除的名單
├── competitor_categories.json         # 競爭者→分類對照表
└── competitor_news_alerts_YYYY-MM-DD.csv  # 每日執行的原始明細紀錄

dashboard-deploy/competitor/
├── index.html                         # 看板（build_pages.py 產生）
└── README.md
```
