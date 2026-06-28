# Data Manifest — Phase 3 & Canonical Outputs

**Repo:** [sakiwatashi/isaacsim6.0acousticthesis](https://github.com/sakiwatashi/isaacsim6.0acousticthesis)  
**Last updated:** 2026-06-29  
**Companion:** [`REPRODUCIBILITY_AUDIT.md`](REPRODUCIBILITY_AUDIT.md)

本檔說明哪些實驗資料**未**納入 git、如何重跑產生、以及 canonical 摘要的預期規模。

---

## 1. 為何 raw data 未上傳

`.gitignore` 刻意排除：

- `runtime/outputs/**/repeat_*/` — 完整 repeat 目錄樹  
- `runtime/outputs/**/*_timeseries.csv` — GMO 時序（體積大）  
- `runtime/outputs/**/batch_summary.txt`、`*_run.log` — 批次日誌  

理由：單次 Phase 3 主實驗 raw 樹約 **61** 個檔案；timeseries 逐檔可達數 MB。公開 repo 保留**可審計摘要**（comparison CSV、圖、JSON），raw 由重現腳本在本機 DGX 再生。

---

## 2. 主實驗 raw 契約

### 識別

| 欄位 | 值 |
|------|-----|
| Batch ID | `fixed_tcp_repeatability_v1` |
| 輸出根目錄 | `runtime/outputs/fixed_tcp_repeatability_v1/` |
| 距離 | 0.5, 1.0, 1.5, 2.0, 2.5, 3.0 m |
| Repeats | 5（`repeat_001` … `repeat_005`） |
| 材質 | B |

### 目錄結構（每 repeat）

```text
fixed_tcp_repeatability_v1/
├── batch_summary.txt
├── repeat_001/
│   ├── distance_0p5m/
│   │   ├── official_asset_ur10_fixed_tcp_distance_sweep_summary.json
│   │   └── official_asset_ur10_fixed_tcp_distance_sweep_timeseries.csv
│   ├── distance_1p0m/
│   │   └── ...
│   └── ... (共 6 距離)
├── repeat_002/
│   └── ...
└── repeat_005/
    └── ...
```

### 預期檔案數

| 類型 | 數量 |
|------|------|
| `batch_summary.txt` | 1 |
| `*_summary.json` | **30**（6 距離 × 5 repeats） |
| `*_timeseries.csv` | **30** |
| **合計** | **61** |

### `batch_summary.txt` 預期內容

```text
batch_id=fixed_tcp_repeatability_v1
material_condition=B
total_runs=30
pass=30
fail=0
```

### 重跑指令

```bash
source scripts/env_host_isolated.sh
bash scripts/run_phase3_repeatability_and_analysis.sh
```

僅跑 raw batch（不跑分析）：

```bash
BATCH_ID=fixed_tcp_repeatability_v1 REPEAT_COUNT=5 \
  bash scripts/run_host_fixed_tcp_repeatability_batch.sh
```

### 驗證 checklist

- [ ] `batch_summary.txt` 顯示 `pass=30 fail=0`
- [ ] 存在 `repeat_001` … `repeat_005` 各 6 個 `distance_*m/` 子目錄
- [ ] 每子目錄有 `*_summary.json`（timeseries 可選保留）
- [ ] `phase3_rtx_features/fixed_tcp_repeatability_v1_distance_features.csv` 有 **30** data rows（`feature_row_count: 30`）

---

## 3. Feature extract 層（未進 git）

| 路徑 | 說明 | 預期 rows |
|------|------|-----------|
| `runtime/outputs/phase3_rtx_features/fixed_tcp_repeatability_v1_distance_features.csv` | 由 30 個 summary 聚合 | 30 |
| `..._distance_features.summary.json` | 提取元資料 | — |

產生方式：含於 `run_phase3_repeatability_and_analysis.sh` 第二步。

---

## 4. Canonical comparison 層（已進 git）

**目錄：** `runtime/outputs/phase3_rtx_pra_comparison_fixed_tcp_repeatability_v1/`

| 檔案 | 說明 | 預期 |
|------|------|------|
| `fixed_tcp_rtx_pra_comparison.csv` | 6 距離點 RTX 平均 + PRA 對齊 | 6 rows |
| `fixed_tcp_rtx_pra_correlations.csv` | Spearman 趨勢表 | 16 rows |
| `PHASE3_RTX_PRA_REPORT.json` | 分析報告元資料 | `comparison_rows: 6` |
| `rtx_amplitude_max_vs_distance.png` | RTX peak 趨勢圖 | — |
| `rtx_signal_way_peak_vs_distance.png` | Signal-way 特徵圖 | — |
| `pra_features_vs_distance.png` | PRA 參考趨勢圖 | — |

**SHA-256（2026-06-27 canonical run）：**

```
8402175258493500fbae37a70f2c9596456052793954e443892c638202d8c228  fixed_tcp_rtx_pra_correlations.csv
30982e566ff16b04dacdad122a773c8257d03445f2551581ef56008e246356a4  fixed_tcp_rtx_pra_comparison.csv
```

### PRA 參考來源

主實驗 cond B 的 PRA 特徵來自：

`runtime/outputs/experiment_4_pra_reference_passport_v1_cond_B/pra_reference_features.csv`

此路徑**未**單獨 whitelist 進 git；同等 PRA 資料可於  
`runtime/outputs/phase3_material_sensitivity_sgw/pra_cond_B/pra_reference_features.csv`（已進 git）取得。

---

## 5. 其他已進 git 的摘要資料

| 目錄 | 內容 |
|------|------|
| `phase3_material_sensitivity_sgw/` | 材質 A/B/C RTX×PRA 比較 |
| `lab_dynamic_smoke_v1/` | Lab 動態觀測圖與 summary JSON |
| `lab_sl_distance_v1/` | Sim→Lab SL 指標、圖、預測 CSV |

---

## 6. 明確未進 git

| 類型 | 路徑模式 | 備註 |
|------|----------|------|
| Isaac Sim 安裝 | `app/` | 本機安裝 |
| Isaac Lab clone | `IsaacLab/` | 獨立 git |
| Raw repeats | `fixed_tcp_repeatability_v1/repeat_*/` | 見 §2 |
| Timeseries | `*_timeseries.csv` | 全 repo 排除 |
| RL checkpoints | `model_*.pt` | 可重訓 |
| TensorBoard | `events.out.tfevents.*` | 訓練日誌 |
| Scene cache | `runtime/scenes/` | 執行時生成 |

---

## 7. 取得完整 raw 的選項

1. **自行重跑**（推薦）：具 NVIDIA GPU + Isaac Sim 6.0.0-rc.59 host 安裝  
2. **聯絡作者**：口試或審查需要時可索取 `fixed_tcp_repeatability_v1/` 壓縮檔（未預設公開）  
3. **勿**假設 clone repo 後 `fixed_tcp_repeatability_v1/` 存在 — 該目錄為重跑產物