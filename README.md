# 生物掃描實驗室 · BioScan Lab — 沙田生物多樣性展品

呢個 repo **淨係放砌好嘅網站檔案**（`vite build` 嘅輸出），用嚟喺 GitHub Pages 免費寄存。

- 原始碼唔喺呢度：喺本機 `gravity-park-live-timing-centre/beetle-scanner-exhibit/`。
- **唔好手改呢度啲檔案** — 下次重新部署會覆蓋晒。
- 更新方法：喺原始碼跑 `npm run build`，將 `dist/` 嘅內容覆蓋落呢度，再 commit、push。

## 點解要分開一個 repo

原始碼放喺一個同「轆轆小車大挑戰」計時系統共用嘅 repo 入面，嗰度有
`data/students.csv`、`data/results.csv` — 真實活動嘅班別、學號同成績。
免費嘅 GitHub Pages **一定要 public repo**，所以嗰個 repo 唔可以推上嚟。
呢度只有展品網站本身，冇任何學生資料。

## 內容授權

**逐項出處：[CREDITS.md](CREDITS.md)** — 每個 3D 模型同每張相片嘅作者、授權、原始頁。

展出嘅 19 隻入面 15 隻係 CC0、**4 隻係 CC BY 4.0**（姬兜、金斑蝶、烏頭、珠頸斑鳩）。
CC BY 嘅署名係授權條件，所以：

- 署名印咗喺展品畫面上（每隻動物嘅左下角）
- 同時喺呢度公開列明 —— 一個外界攞唔到嘅本機檔案唔算履行咗署名義務

呢份 `CREDITS.md` 係由原始碼嘅 `PHOTO_CREDITS.md` 複製過嚟，更新網站嗰陣要一齊更新。
