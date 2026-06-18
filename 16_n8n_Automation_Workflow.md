# n8n 自動化工作流:顧問教練 Bot(零阻力推廣)

> 目的:**把「手動複製 prompt」的推廣阻力降到零**。員工開完會把會議雜記丟進 Slack/Teams/Line/信箱 → n8n 自動套 `11_AI_Prompt_Pack` → 回傳 **C1-C5 評分 + 缺的對角線 + `02` 顧問式摘要草稿**。
> 對齊:TigerAI n8n Skill Pack 產品線;**原生 node 優先**(少用 Code,對齊 n8n 原生化紀律)。
> ⚠️ 安全:會議雜記可能含客戶機密 → LLM 節點指向**內部/私有 OpenAI 相容端點**(可地端),不外送敏感;**對外文件只稱「OpenAI 相容端點」,不點名內部橋接。**

---

## 1. 使用者體驗(目標)
```
員工:在 #consulting-coach 頻道貼會議雜記(或 @LineBot)
Bot(10-30 秒後回 thread):
  📊 C2(5/12)
  🔻 缺的對角線:你只寫客戶現在,沒接對手未來。建議補「對手 DEF 現在盲點 × 客戶未來需求」
  📝 顧問式摘要草稿(02 六段):…(可直接編修交件)
```
→ 員工**不必懂方法論、不必複製 prompt**,丟進去就有教練回饋。

---

## 2. 架構(node-by-node,原生為主)

```
[① Trigger] → [② IF 過濾] → [③ Set 取雜記] → [④ AI 節點:結構化輸出] → [⑤ Set 排版] → [⑥ 回覆] → [⑦(選)Log 到 Sheet]
```

| # | Node(原生) | 設定重點 |
|---|---|---|
| ① | **Trigger**(擇一)| Slack Trigger(指定頻道)／ Teams Trigger ／ Line:Webhook 接 Messaging API ／ Email(IMAP)|
| ② | **IF / Filter** | 只處理該頻道或含關鍵字(如以 `/coach` 開頭),避免誤觸 |
| ③ | **Edit Fields (Set)** | 把正文存成 `notes`(去掉指令前綴)|
| ④ | **AI 節點(OpenAI 相容)** + **Structured Output Parser** | 一次呼叫回 JSON(摘要+評分+對角線),見 §3;用 parser 避免寫 JS |
| ⑤ | **Edit Fields (Set)** | 用 expression 組回覆文字,純表達式不寫 Code |
| ⑥ | **回覆 node** | Slack Post Message(回原 thread)／ Line Reply ／ Email Send |
| ⑦ | **Google Sheets / DB Append**(選)| 寫 `員工/日期/總分/對角線數/L` 一列 → **自動累積實證數據**,直接餵 `15_Empirical_Run_Kit` |

> 第 ⑦ 是隱藏價值:workflow 每跑一次就**自動收一筆 rubric 資料**,你想做的前後測(`15`)母體自己長出來。

---

## 3. AI 節點 Prompt(一次呼叫拿全部,省 token)

**System**:
```
你是顧問思考能力系統教練。把會議雜記轉成顧問判斷。核心:填格子不等於思考,價值在對角線(跨主體×跨時態)。不編造客戶沒透露的事;訊號 vs 推測要分;沒證據標「待確認」。
```
**User**(帶 `{{$json.notes}}`):
```
依「顧問式會議摘要」六段改寫並評分。只輸出 JSON:
{
 "summary": {"原話":"","真痛點":"","現需求":"","未來與沒說出口":"","競爭與環境":"","我們怎麼切入":"","下一步":""},
 "diagonal": "明寫一條對角線(對手或環境某時態 × 客戶某時態 → 建議);連不出就說缺什麼",
 "rubric": {"紀錄":0,"痛點":0,"未來":0,"競爭":0,"環境":0,"商業":0},
 "total": 0, "level": "C1-C5",
 "missing": "最該補的一格 + 升一級的下一步"
}
規則:沒有成立的對角線→level 封頂 C3;只回答客戶問的→最多 C4。
```
> 對應 `11` 的 P1+P2+P3 合併。AI 節點開「JSON / Structured Output」,⑤ 直接讀欄位,**免 Code node**。

---

## 4. 回覆排版(⑤ Set 的 expression 範例)
```
📊 {{$json.level}}({{$json.total}}/12)
🔻 對角線:{{$json.diagonal}}
🧩 最該補:{{$json.missing}}
📝 草稿:真痛點 {{$json.summary.真痛點}}｜未來/沒說出口 {{$json.summary.未來與沒說出口}}｜我們怎麼切入 {{$json.summary.我們怎麼切入}}｜下一步 {{$json.summary.下一步}}
```

---

## 5. 治理 / 隱私(必做)
- LLM 端點用**內部/私有 OpenAI 相容端點**(可地端),避免客戶機密上公有雲。
- 頻道權限控管;必要時加「去識別化」前處理(遮罩公司/人名),對應 `14` SOP。
- Log(⑦)含客戶資訊就存內部、設權限。

---

## 6. 落地(Pilot)
1. 先接**一個**觸發源(建議 Slack 或 Line,門檻最低)。
2. 開 `#consulting-coach`,公告「開完會把雜記貼這,Bot 給你評分+草稿」。
3. 跑兩週看採用率 + Log 分數趨勢 → 穩了再接 Teams/Email、加 ⑦、接 `15` 前後測。

---

## 7. 產品化延伸(TigerAI)
本 workflow 可做成 **n8n Skill Pack 可賣模組**:「**AI 顧問教練 Bot**」—— 顧問公司/業務團隊接自家 Slack/Line 就能把全員會議產出升級。呼應 `13` 的「主動」精神:方法論不只被讀,是被自動套用。

---
📌 配套:prompt 源 `11`、摘要格式 `02`、評分 `03`、資料回收餵 `15`。
> 下一步可做:產出**可匯入的 n8n workflow JSON**(Slack 版或 Line 版)當起手範本 —— 說「出 n8n JSON」即做。
