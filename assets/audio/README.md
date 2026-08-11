# 旁白音訊資產 / Narration audio assets

每個語言一個資料夾，每個任務一個音檔（檔名 = 任務 id）：

```
assets/audio/yue-Hant-HK/find-parts.mp3
assets/audio/yue-Hant-HK/open-wings.mp3
assets/audio/yue-Hant-HK/xray-shell.mp3
assets/audio/yue-Hant-HK/layers.mp3
assets/audio/yue-Hant-HK/protect-forest.mp3
```

支援 `.mp3`（首選）或 `.ogg`。

**目前刻意不包含錄音檔** —— 專案不會偽造真人錄音。
- **開發模式**：若音檔不存在，可用 `lang=zh-HK` 的 `SpeechSynthesisUtterance` 試聽時序。
- **正式 build**：若音檔不存在，只顯示字幕並在診斷面板記錄「缺少資產」，
  絕不假裝已有真人錄音。

正式展出前，請放入獲授權的預錄廣東話旁白（每段核心旁白約 8 秒內）。
