# 3D 模型資產 / 3D model assets

甲蟲模型放於：

```
xylotrupes_pachycera.glb
```

## 目前狀態

此檔目前是**自製的示範模型**（由 `npm run make:sample-model` 以基本幾何產生），
**不是下載或未授權的資產**。它含有依命名規範命名的 mesh，供構造系統（X-ray／
分層／分解／熱點）運作與測試。把獲授權的真實 GLB 覆蓋同名檔即可替換。

若檔案不存在或損壞，展品會顯示教育版佔位模型 + 可恢復診斷（不白屏）。

## 3D 資產規格

- 格式：**glTF-Binary (.glb)**，Y-up，公制，甲蟲朝 **+Z**（頭部指向 +Z）。
- 壓縮：可用 **KTX2(basis)**、**Meshopt**、**Draco**（載入器已支援，解碼器離線內附）。
- 建議：三角形數 ≤ ~150k、貼圖 ≤ 2048²、材質用 PBR metal-rough。

### Mesh 命名規範（構造系統以名稱前綴分類，見 `src/modules/anatomy/anatomyRegistry.ts`）

| 前綴 / 名稱 | 構造 | 說明 |
| ---------- | ---- | ---- |
| `shell_*`、`horn_*`、`leg_*`、`antenna_*` | 外殼 | 外骨骼（X-ray 時變透明） |
| `hindwing_*` | 飛行翅 | 收於翅鞘下的飛行翅 |
| `muscle_*` | 飛行肌肉 | 胸腔飛行肌 |
| `trachea*`、`spiracle_*` | 氣門與氣管 | 呼吸系統 |
| `digestive*` | 消化道 | 腸道 |
| `brain`、`nerve_cord`、`nerve*` | 腦與腹神經索 | 神經系統 |

未匹配任何前綴的 mesh 會被當作外殼處理。缺少某構造時，該構造顯示「此資產尚未
提供」，其他功能照常。

### 可選動畫 clip（沒有時使用程式化安全後備，不假裝科學準確）

`elytra_open`、`hindwings_unfold`、`anatomy_explode`、`anatomy_assemble`
（階段 2 目前以程式化位移／透明處理分解與透視；具名 clip 支援於後續完善）。
