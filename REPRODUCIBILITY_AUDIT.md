# Reproducibility Audit — Phase 3 Main Experiment

**Repo:** [sakiwatashi/isaacsim6.0acousticthesis](https://github.com/sakiwatashi/isaacsim6.0acousticthesis)  
**Audit date:** 2026-06-29  
**Purpose:** 口試委員可審計之主實驗協定；對照 `DATA_MANIFEST.md` 與 `thesis/REPLICATION_PACKAGE.md`。

---

## 1. 主實驗識別

| 項目 | 值 |
|------|-----|
| **實驗名稱** | Phase 3 — Fixed-TCP RTX repeatability + RTX×PRA cross-model characterization |
| **Batch ID** | `fixed_tcp_repeatability_v1` |
| **材質條件** | B（medium absorption, α≈0.35） |
| **機器人** | UR10 official USD asset，固定 TCP |
| **距離點** | 0.5, 1.0, 1.5, 2.0, 2.5, 3.0 m |
| **Repeat 數** | 5（`repeat_001` … `repeat_005`） |
| **預期 run 總數** | **30 / 30 PASS**（6 距離 × 5 repeats） |

---

## 2. 重現指令（單一入口）

```bash
cd <repo_root>
source scripts/env_host_isolated.sh
bash scripts/run_phase3_repeatability_and_analysis.sh
```

此腳本依序執行：

1. `run_host_fixed_tcp_repeatability_batch.sh` — 30 次 Isaac Sim 擷取  
2. `extract_fixed_tcp_rtx_features.py` — 由 raw summary 聚合特徵  
3. `run_phase3_rtx_pra_comparison.sh` — RTX×PRA 趨勢分析與圖表  

**環境釘選（論文與 repo 統一寫法）：**

| 元件 | 版本 |
|------|------|
| Isaac Sim | **host standalone 6.0.0-rc.59**（非 Docker `6.0.0` 映像） |
| Isaac Lab | 3.0.0-beta2 |
| rsl-rl-lib | 5.0.1 |
| Experience | `${APP_ROOT}/apps/isaacsim.exp.base.python.kit` |

---

## 3. 輸出目錄三層語意

| 層級 | 路徑 | 進 git？ | 說明 |
|------|------|----------|------|
| **Raw repeat** | `runtime/outputs/fixed_tcp_repeatability_v1/` | ❌ | 每 repeat×距離：`summary.json` + `timeseries.csv`；見 `DATA_MANIFEST.md` |
| **Feature extract** | `runtime/outputs/phase3_rtx_features/fixed_tcp_repeatability_v1_distance_features.csv` | ❌ | 由 raw 重跑腳本產生（30 rows） |
| **Canonical comparison** | `runtime/outputs/phase3_rtx_pra_comparison_fixed_tcp_repeatability_v1/` | ✅ | 6 點平均比較表、Spearman CSV、3 圖、`PHASE3_RTX_PRA_REPORT.json` |

**勿混淆：** `phase3_rtx_pra_comparison_*` 是**分析摘要**；`fixed_tcp_repeatability_v1` 是**原始 repeat 樹**。

---

## 4. 預期產物清單

### Raw（重跑後應存在，未進 git）

- `fixed_tcp_repeatability_v1/batch_summary.txt` — 應顯示 `pass=30 fail=0`
- 5 × 6 = 30 個 `distance_*m/official_asset_ur10_fixed_tcp_distance_sweep_summary.json`
- 30 個 `*_timeseries.csv`（被 `.gitignore` 排除）
- 合計約 **61** 個檔案（含 `batch_summary.txt`）

### Canonical（已進 git）

| 檔案 | 預期 row / 內容 |
|------|-----------------|
| `fixed_tcp_rtx_pra_comparison.csv` | **6** rows（每距離 1 列，5 repeats 平均） |
| `fixed_tcp_rtx_pra_correlations.csv` | **16** data rows + header |
| `PHASE3_RTX_PRA_REPORT.json` | `comparison_rows: 6`；含 `*_relative` 路徑欄位 |
| `rtx_*.png`, `pra_features_vs_distance.png` | 3 張趨勢圖 |

**Checksum（canonical，2026-06-27 產生）：**

- `fixed_tcp_rtx_pra_correlations.csv` → `8402175258493500fbae37a70f2c9596456052793954e443892c638202d8c228`
- `fixed_tcp_rtx_pra_comparison.csv` → `30982e566ff16b04dacdad122a773c8257d03445f2551581ef56008e246356a4`

---

## 5. Claim boundary（可講 / 不可講）

### 可宣稱

| Claim | 依據 |
|-------|------|
| 30/30 可重複性 PASS | `batch_summary.txt`（raw，重跑可驗） |
| `primary_sgw_early_energy` 隨距離呈下降趨勢（ρ≈−0.66, n=6） | `fixed_tcp_rtx_pra_correlations.csv` row: `distance_vs_rtx` × `primary_sgw_early_energy_mean` |
| 平坦 `amplitude_max` 在 ≥1.0 m 飽和，不適合作距離特徵 | comparison CSV + 圖 |
| PRA `early_energy_50ms` 6 點強單調下降（ρ=−1.0, n=6） | correlations CSV |
| Sim→Lab 延伸：動態 smoke ρ≈−0.48；SL r≈0.47 | `lab_dynamic_smoke_v1/`, `lab_sl_distance_v1/`（已進 git） |

### 不可宣稱（或須加強烈限定）

| 不可宣稱 | 原因 |
|----------|------|
| 厘米級部署測距 | 僅趨勢級 feasibility |
| RTX 與 PRA **波形等價** | `claim_boundary` 明確排除 |
| RTX×PRA early energy **統計顯著一致** | ρ≈+0.657, **p≈0.156**, **n=6** — 未達顯著 |
| 「強證據 ρ≈+0.66」 | 同上；僅 **pilot cross-model characterization** |
| RL 優於監督基線 | eval MAE 0.441 m > SL 0.41 m |
| 已完成 CH201 實機驗證 | 未執行 |

### RTX×PRA 建議表述（論文 / README 統一）

> 部分 RTX early-energy feature（`primary_sgw_early_energy`）與 PRA `early_energy_50ms` 呈中度正相關趨勢（Spearman ρ≈+0.66, n=6），但 p≈0.16 未達顯著，僅作 pilot cross-model characterization，不代表波形驗證。

---

## 6. 已知風險與未完成項

| 風險 | 狀態 | 緩解 |
|------|------|------|
| Raw repeat 未上傳 git | 開放 | `DATA_MANIFEST.md` 說明重跑步驟與預期檔案數 |
| `PHASE3_RTX_PRA_REPORT.json` 含 DGX 絕對路徑 | 已緩解 | 新增 `*_relative` 欄位；絕對路徑保留為產生環境紀錄 |
| `thesis/CITATION_BANK.md` 引用完整性 | 進行中 | 已補 DOI；口試前對照 Word 參考文獻逐條核對 |
| Isaac Sim 版本混寫（Docker 6.0.0 vs host rc.59） | 已統一 | 全文以 **host 6.0.0-rc.59** 為準 |
| PRA reference 未進主 comparison git 樹 | 低 | 材質敏感度目錄 `phase3_material_sensitivity_sgw/pra_cond_B/` 已含 PRA CSV |

---

## 7. 延伸實驗（非主審計範圍）

| 實驗 | 腳本 | 輸出 |
|------|------|------|
| 材質敏感度 A/B/C | `run_phase3_material_sensitivity.sh` | `phase3_material_sensitivity_sgw/` ✅ |
| Lab 動態 smoke | `lab/run_lab_smoke.sh` | `lab_dynamic_smoke_v1/` ✅ |
| Sim→Lab SL | `lab/run_sl_lab_distance.sh` | `lab_sl_distance_v1/` ✅ |
| In-sim RSL-RL v5 | `lab/run_rl_distance_in_sim_long_v5.sh` | checkpoints 未進 git |

---

## 8. 相關文件

- [`DATA_MANIFEST.md`](DATA_MANIFEST.md) — raw / summary 檔案契約  
- [`thesis/REPLICATION_PACKAGE.md`](thesis/REPLICATION_PACKAGE.md) — 全管線重現步驟  
- [`thesis/CITATION_BANK.md`](thesis/CITATION_BANK.md) — 論文引用庫  
- [`README.md`](README.md) — repo 導覽與 claim 摘要