# Replication Package — UR10 RTX Acoustic Thesis Pipeline

**目的：** 口試委員／未來自己可重跑主結果  
**主機（原始產生環境）：** DGX `spark-68ef`，路徑 `/home/lab109/song/isaacsim6.0`  
**審計文件：** [`../REPRODUCIBILITY_AUDIT.md`](../REPRODUCIBILITY_AUDIT.md)、[`../DATA_MANIFEST.md`](../DATA_MANIFEST.md)

---

## 1. 環境

```bash
cd <repo_root>
source scripts/env_host_isolated.sh
# 必要：APP_ROOT, IsaacLab/_isaac_sim
```

**Experience（必用）：** `${APP_ROOT}/apps/isaacsim.exp.base.python.kit`

**版本釘選（統一寫法）：**

| 元件 | 版本 |
|------|------|
| Isaac Sim | **host standalone 6.0.0-rc.59** |
| Isaac Lab | 3.0.0-beta2 |
| rsl-rl-lib | 5.0.1 |
| Python | 3.12（kit 內建） |
| GPU | NVIDIA GB10 |

> 勿與 Docker `nvcr.io/nvidia/isaac-sim:6.0.0` 混寫；論文與本 repo 以 **host rc.59** 為準。

---

## 2. 輸出目錄三層語意

| 層級 | 路徑 | 進 git？ |
|------|------|----------|
| **Raw repeat** | `runtime/outputs/fixed_tcp_repeatability_v1/` | ❌ |
| **Feature extract** | `runtime/outputs/phase3_rtx_features/fixed_tcp_repeatability_v1_distance_features.csv` | ❌ |
| **Canonical comparison** | `runtime/outputs/phase3_rtx_pra_comparison_fixed_tcp_repeatability_v1/` | ✅ |

---

## 3. 主結果重現順序

### Phase 3 — Sim 靜態 30/30（論文主貢獻）

```bash
bash scripts/run_phase3_repeatability_and_analysis.sh
```

- **Batch ID：** `fixed_tcp_repeatability_v1`
- **距離：** 0.5, 1.0, 1.5, 2.0, 2.5, 3.0 m
- **Repeats：** 5
- **預期：** 30/30 PASS → 見 raw `batch_summary.txt`
- **Canonical 摘要（已進 git）：**  
  `runtime/outputs/phase3_rtx_pra_comparison_fixed_tcp_repeatability_v1/`

### Phase 4 — Lab 動態 smoke

```bash
bash lab/run_lab_smoke.sh
# 輸出：runtime/outputs/lab_dynamic_smoke_v1/
bash lab/plot_lab_smoke_results.py --output-dir runtime/outputs/lab_dynamic_smoke_v1
```

### Phase 4.6 — Sim→Lab 監督學習

```bash
bash lab/run_sl_lab_distance.sh
# 輸出：runtime/outputs/lab_sl_distance_v1/
```

### Phase 5 — In-sim RSL-RL（延伸，非主貢獻）

```bash
# Smoke（2 iter）
bash lab/run_rl_distance_in_sim.sh

# 長訓練 v5（500 iter，塑形獎勵）
bash lab/run_rl_distance_in_sim_long_v5.sh

# 評估
cd IsaacLab && source ../scripts/env_host_isolated.sh
export PYTHONPATH="../lab:../scripts:${PYTHONPATH:-}"
./isaaclab.sh -p ../lab/eval_rl_distance_in_sim.py --headless \
  --experience "${APP_ROOT}/apps/isaacsim.exp.base.python.kit" \
  --steps 64 --checkpoints ../runtime/outputs/lab_rl_distance_in_sim_long_v5/model_499.pt
```

---

## 4. 論文圖表 ↔ 檔案對照

| 圖表 | 路徑 |
|------|------|
| 圖4.1–4.3（Sim RTX×PRA） | `runtime/outputs/phase3_rtx_pra_comparison_fixed_tcp_repeatability_v1/*.png` |
| 圖4.5–4.6 | `runtime/outputs/lab_dynamic_smoke_v1/*.png` |
| 圖4.7–4.8 | `runtime/outputs/lab_sl_distance_v1/*.png` |
| 表4.3 | `.../fixed_tcp_rtx_pra_comparison.csv` |
| 表4.6 | `runtime/outputs/lab_sl_distance_v1/sl_distance_summary.json` |
| 表4.7（RL） | 見 `thesis/THESIS_PHASE5_INSIM_RL_2026-06-28.md` |

---

## 5. 統計分析腳本

| 分析 | 腳本 |
|------|------|
| RTX×PRA Spearman | `scripts/analyze_fixed_tcp_rtx_pra.py` |
| Lab E vs GT ρ | `lab/plot_lab_smoke_results.py` |
| SL 指標 | `lab/train_sl_distance_regressor.py` |
| RL eval ρ, MAE | `lab/eval_rl_distance_in_sim.py` |
| 離線 RL smoke | `lab/train_rl_distance_smoke.py` |

---

## 6. Claim boundary（重現時請對照）

| 可宣稱 | 不可宣稱 |
|--------|----------|
| 30/30 PASS；early_energy 距離趨勢 ρ≈−0.66 (n=6) | 厘米級部署測距 |
| RTX×PRA early energy **中度正相關趨勢**（ρ≈+0.66, **p≈0.16, n=6**）— pilot only | 波形等價、統計顯著跨模型一致 |
| Lab 動態 ρ≈−0.48；Sim→Lab SL r≈0.47 | MAE 0.41 m 可上實機 |
| in-sim RL 閉環可跑通 | RL 優於 SL |

---

## 7. 交付清單（口試前）

- [x] `REPRODUCIBILITY_AUDIT.md` + `DATA_MANIFEST.md`
- [ ] `thesis/THESIS_DRAFT_FCU_v1.docx` 更新 §3.9–§4.8
- [ ] 圖4.1–4.10 嵌入 Word
- [x] `CITATION_BANK.md` 補 DOI（口試前對照 Word 參考文獻）
- [ ] 本檔 + `THESIS_OUTLINE_FCU_2026-06-29.md` 一併存檔