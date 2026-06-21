# DOI 申請流程(取得可引用的永久識別碼)

> 作者:Morris (Yeh-Hsing) Lu｜ORCID 0009-0006-5373-0586。
> 你們已有經驗(GenAI Consulting Methodology Toolkit 已上 Zenodo 並有 concept/version DOI),本檔把流程整理成可照做的步驟。

---

## 0. 先分清:DOI ≠ 同行評審
- **DOI** = 永久識別碼,**幾分鐘就能拿**(Zenodo/OSF/figshare),**不需審查**。給你:可引用 + 時間戳(優先權)。
- **同行評審期刊** = 另一回事,門檻高、要實證(見 `06`/`07`)。
- 本套**現階段最適合**:以「framework-synthesis working paper」deposit 取得 DOI,**先卡優先權、可被引用**;實證(`15`)完成後再出 v2 或投期刊。

---

## 1. 選平台(三選一,建議 Zenodo)

| 平台 | 適合 | 特點 |
|---|---|---|
| **Zenodo**(建議,你們已用)| 論文 + 軟體 + 資料 | CERN 營運;支援 **concept DOI(永遠指最新版)+ version DOI**;可掛 GitHub release;免費 |
| **OSF** | 預印本 + 專案 | 適合做 preprint + 開放專案頁 |
| **SSRN** | 社科/管理工作論文 | 管理/商學圈能見度高;審核較久 |

> 你們已有 Zenodo 流程 → **沿用 Zenodo** 最省事,且能和既有 toolkit deposit 用「Is supplement to / Is part of」cross-link。

---

## 2. 上傳前要備好的(metadata 清單)
- **標題**:Consulting Thinking Capability System: An Operationalized Framework for Consulting Thinking Development and Assessment
- **作者 + ORCID**:Morris (Yeh-Hsing) Lu,ORCID 0009-0006-5373-0586(**個人,非機構**)
- **摘要**(用 `06 §5.5` 的誠實主張當骨架:不是新框架,是把既有框架操作化成可訓練/評分/驗證的系統)
- **關鍵字**:consulting methodology;capability framework;3C;TOWS;Kano;diagonal synthesis;assessment rubric;AI-native ebook
- **授權**:**Apache License 2.0**(與你們既有一致)
- **版本**:v0.1(或 v1.0 release candidate)
- **檔案**:**PDF —— LaTeX 排版(由 `13` ebook 用 `pandoc + xelatex` 匯出,非瀏覽器另存)**;DOI 是永久學術產物,排版要像 preprint。指令與注意事項見 skill `consulting-thinking-coach`〈出版 / DOI deposit 輸出規範〉。+ 互動 `17_Interactive_Site.html` + 可選 `CITATION.cff`
- **關聯**:Related identifiers → 「Is supplement to」既有 toolkit DOI;「Is derived from」本 GitHub repo

---

## 3. Zenodo 步驟(照做)
1. 用 ORCID 登入 Zenodo(綁定 ORCID,作者自動帶出)。
2. New upload → 拖入 PDF/HTML。
3. 填 metadata:Resource type = **Publication › Preprint**(或 Working paper);填上 §2 清單。
4. Authors 填 Morris (Yeh-Hsing) Lu + ORCID;**不要填機構為作者**。
5. License 選 **Apache License 2.0**。
6. (建議)綁 GitHub:Zenodo↔GitHub 開啟後,**在 GitHub 發一個 release**,Zenodo 自動 archive 並給 version DOI;同時自動產生 **concept DOI(永遠指最新版)**。
7. Publish → 立刻得到 DOI。
8. 把 DOI 寫回 `13 附錄 C` 與本檔。

> **concept DOI vs version DOI**:對外引用主推 **concept DOI**(永遠最新);特定版本用 version DOI。

---

## 4. 上線後
- 在 LinkedIn / 網站 / repo README 放「Cite this: DOI」。
- 在 `06`/`13` 補上 DOI;`19` 製作流程也可引用。
- 出 v2 時:同一個 upload 按「New version」,**concept DOI 不變**,新增一個 version DOI。

---

## 5. 升級到期刊(未來,選做)
1. 先跑 `15` 實證 → 有數據。
2. 投合適的期刊(依研究成熟度與讀者選 venue),定位「operationalized framework + pilot validation」。
3. 投稿前跑 `hype-scrub` + `claim-audit`(見 `21`),守誠實。
4. 收到審稿意見用 `reviewer-response` 流程回覆。
> 提示:**正式投稿前,先研究並比較目標期刊的範圍與命名慣例,再定標題與定位**(別隨手取名)。具體 venue 選擇與投稿節奏屬內部規劃,不寫進對外文件。

---
## 6. 防呆提醒
- ⚠️ **作者是個人(Morris Lu),不是 TigerAI**(機構可放 affiliation 或致謝,但作者欄是人)。
- ⚠️ 摘要**不得**出現「世界首創/真空白」(會被審稿打);用 `06 §5.5` 誠實版。
- ⚠️ 對外文件不得出現內部工具實作細節(只稱「OpenAI 相容端點」)。

📌 配套:成品 `13`/`17`、誠實定位 `06`、實證 `15`、防酸民審查 `21`。
