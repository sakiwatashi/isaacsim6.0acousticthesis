# Citation Bank（論文用 · 已補 DOI）

**格式目標：** 逢甲 fcuformat APA；英文 A–Z、中文筆劃  
**狀態：** 2026-06-29 已自 `paper_rewriting_output/citation_support_bank.md` 轉正式條目；口試前對照 Word 參考文獻逐條核對頁碼。  
**§2 建議引用集：** 見下方「Recommended 20」。

---

## Recommended 20（論文第二章主引用集）

| # | Key | Reference |
|---|-----|-----------|
| 1 | `gao2026_isaac_survey` | Gao, S., Pagnucco, M., Bednarz, T., & Song, Y. (2026). NVIDIA Isaac Sim: Enabling Scalable, GPU-Accelerated Simulation for Robotics. *arXiv:2606.03551*. https://doi.org/10.48550/arxiv.2606.03551 |
| 2 | `hofer2021_sim2real` | Höfer, S., et al. (2021). Sim2Real in Robotics and Automation: Applications and Challenges. *IEEE T-ASE*. https://doi.org/10.1109/tase.2021.3064065 |
| 3 | `mittal2023_orbit` | Mittal, M., et al. (2023). Orbit: A Unified Simulation Framework for Interactive Robot Learning Environments. *IEEE RA-L*. https://doi.org/10.1109/lra.2023.3270034 |
| 4 | `grade2025_dynamic` | GRADE (2025). Generating Realistic and Dynamic Environments for robotics research with Isaac Sim. *IJRR*. https://doi.org/10.1177/02783649251346211 |
| 5 | `salimpour2025_sim2real` | Salimpour, S., Peña-Queralta, J., & Páez-Granados, D. (2025). Sim-to-Real… Isaac Sim to Gazebo and ROS 2. *arXiv:2501.02902*. https://doi.org/10.48550/arxiv.2501.02902 |
| 6 | `zhou2024_cps` | Zhou, Z., et al. (2024). Towards Building AI-CPS with NVIDIA Isaac Sim… *ACM MSEC*. https://doi.org/10.1145/3639477.3639740 |
| 7 | `alatise2020_sensor_fusion` | Alatise, M. B., et al. (2020). A Review on Challenges of Autonomous Mobile Robot and Sensor Fusion Methods. *IEEE Access*. https://doi.org/10.1109/access.2020.2975643 |
| 8 | `he2019_tof` | He, Y., et al. (2019). Recent Advances in 3D Data Acquisition and Processing by Time-of-Flight Camera. *IEEE Access*. https://doi.org/10.1109/access.2019.2891693 |
| 9 | `zhmud2018_ultrasonic` | Zhmud, V., et al. (2018). Application of ultrasonic sensor for measuring distances in robotics. *J. Phys.: Conf. Ser.* https://doi.org/10.1088/1742-6596/1015/3/032189 |
| 10 | `dumbgen2022_echolocation` | Dümbgen, F., et al. (2022). Blind as a Bat: Audible Echolocation on Small Robots. *IEEE RA-L*. https://doi.org/10.1109/lra.2022.3194669 |
| 11 | `liu2020_indoor_acoustic` | Liu, M., et al. (2020). Indoor acoustic localization: a survey. *EURASIP J. Adv. Signal Process.* https://doi.org/10.1186/s13673-019-0207-4 |
| 12 | `valin2017_sound_localization` | Valin, J.-M., et al. (2017). Localization of sound sources in robotics: A review. *Robotics and Autonomous Systems*. https://doi.org/10.1016/j.robot.2017.07.011 |
| 13 | `tsuchiya2022_multipath` | Tsuchiya, A., et al. (2022). Indoor self-localization using multipath arrival time… *Jpn. J. Appl. Phys.* https://doi.org/10.35848/1347-4065/ac506c |
| 14 | `nvidia_rtx_acoustic` | NVIDIA. RTX Acoustic Sensor Documentation. https://docs.isaacsim.omniverse.nvidia.com/latest/sensors/isaacsim_sensors_rtx_acoustic.html |
| 15 | `nvidia_rtx_annotators` | NVIDIA. RTX Annotators / GenericModelOutput. https://docs.isaacsim.omniverse.nvidia.com/6.0.0/sensors/isaacsim_sensors_rtx_annotators.html |
| 16 | `oceansim2025` | Song, J., et al. (2025). OceanSim… *IROS 2025*. https://doi.org/10.1109/iros60139.2025.11246878 |
| 17 | `brinkmann2019_roundrobin` | Brinkmann, F., et al. (2019). A round robin on room acoustical simulation… *JASA*. https://doi.org/10.1121/1.5096178 |
| 18 | `scheibler2018_pyroom` | Scheibler, R., et al. (2018). PyRoomAcoustics… *ICASSP*. https://doi.org/10.1109/ICASSP.2018.8461829 |
| 19 | `dechorate2021` | dEchorate (2021). Calibrated RIR dataset. https://doi.org/10.1186/s13636-021-00229-0 |
| 20 | `xu2024_digital_twin` | Xu, X., et al. (2024). Collaborative robotics, digital twins, HMI and AI. *RCIM*. https://doi.org/10.1016/j.rcim.2024.102769 |

---

## 軟體與平台（必引）

| Key | 建議引用 | 用途 |
|-----|----------|------|
| `isaac_sim_60` | NVIDIA Isaac Sim 6.0 Documentation & Release Notes. Version **6.0.0-rc.59** (host standalone). https://docs.isaacsim.omniverse.nvidia.com/ | RTX Acoustic、SimulationApp |
| `isaac_lab` | NVIDIA Isaac Lab Documentation v**3.0.0-beta2**. https://isaac-sim.github.io/IsaacLab/ | DirectRLEnv、AppLauncher |
| `rsl_rl` | Rudin, N., Hoeller, D., Reist, P., & Hutter, M. (2022). Learning to Walk in Minutes Using Massively Parallel Deep Reinforcement Learning. *CoRL*. https://proceedings.mlr.press/v164/rudin22a.html | RSL-RL / PPO 實作系譜 |
| `ppo_schulman` | Schulman, J., et al. (2017). Proximal Policy Optimization Algorithms. *arXiv:1707.06347*. https://doi.org/10.48550/arXiv.1707.06347 | PPO 演算法 |
| `ur10_official` | Universal Robots. UR10 product documentation. https://www.universal-robots.com/products/ur10-robot/ | 機器人模型 |
| `omniverse_rtx` | NVIDIA Omniverse RTX / RTX Sensors Overview. https://docs.isaacsim.omniverse.nvidia.com/latest/sensors/isaacsim_sensors_rtx.html | 感測渲染語境 |

**版本統一說明：** 本研究實驗於 **host standalone Isaac Sim 6.0.0-rc.59** 執行。Docker 映像 `nvcr.io/nvidia/isaac-sim:6.0.0` 為早期探索環境，**不作**論文釘選版本。

---

## 機器人非視覺感測

| Key | Reference |
|-----|-----------|
| `robot_sonar_review` | Zhmud et al. (2018) — see `zhmud2018_ultrasonic` |
| `tof_robot` | He et al. (2019) — see `he2019_tof` |
| `sensor_fusion_nonvisual` | Alatise et al. (2020) — see `alatise2020_sensor_fusion` |
| `echolocation_robot` | Dümbgen et al. (2022) — see `dumbgen2022_echolocation` |

---

## Sim-to-Real / 數位雙生

| Key | Reference |
|-----|-----------|
| `sim2real_survey` | Höfer et al. (2021) — see `hofer2021_sim2real` |
| `domain_gap_robot` | Collins, J., et al. (2021). A Review of Physics Simulators for Robotic Applications. *IEEE Access*. https://doi.org/10.1109/access.2021.3068769 |
| `isaac_sim_robot_learning` | Gao et al. (2026); Salimpour et al. (2025); Mittal et al. (2023) |

---

## 房間聲學與模擬

| Key | Reference |
|-----|-----------|
| `pyroomacoustics` | Scheibler et al. (2018) — see `scheibler2018_pyroom` |
| `room_acoustics_multipath` | Liu et al. (2020); Tsuchiya et al. (2022) |
| `rir_early_energy` | Brinkmann et al. (2019); dEchorate (2021) |
| `simulator_disagreement` | Brinkmann et al. (2019) — 支撐「趨勢級對照、非波形等價」 |

---

## 強化學習（Phase 5）

| Key | Reference |
|-----|-----------|
| `ppo_schulman` | Schulman et al. (2017) |
| `rl_robot_sim` | Mittal et al. (2023); Salimpour et al. (2025) |
| `rsl_rl_impl` | Rudin et al. (2022) — rsl-rl-lib 5.0.1 |

---

## BibTeX（LaTeX / 參考文獻管理）

```bibtex
@article{gao2026_isaac_survey,
  author  = {Gao, Song and Pagnucco, Min and Bednarz, Tomasz and Song, Ying},
  title   = {{NVIDIA} Isaac Sim: Enabling Scalable, GPU-Accelerated Simulation for Robotics},
  journal = {arXiv preprint arXiv:2606.03551},
  year    = {2026},
  doi     = {10.48550/arxiv.2606.03551}
}

@article{hofer2021_sim2real,
  author  = {H{\"o}fer, Sebastian and others},
  title   = {Sim2Real in Robotics and Automation: Applications and Challenges},
  journal = {IEEE Transactions on Automation Science and Engineering},
  year    = {2021},
  doi     = {10.1109/tase.2021.3064065}
}

@article{mittal2023_orbit,
  author  = {Mittal, Mayank and others},
  title   = {Orbit: A Unified Simulation Framework for Interactive Robot Learning Environments},
  journal = {IEEE Robotics and Automation Letters},
  year    = {2023},
  doi     = {10.1109/lra.2023.3270034}
}

@inproceedings{scheibler2018pyroom,
  author    = {Scheibler, Robin and Bechwati, Fatima and Dietz, Martin and others},
  title     = {PyRoomAcoustics: A Python package for audio room simulations and array processing algorithms},
  booktitle = {ICASSP},
  year      = {2018},
  doi       = {10.1109/ICASSP.2018.8461829}
}

@article{schulman2017ppo,
  author  = {Schulman, John and Wolski, Filip and Dhariwal, Prafulla and Radford, Alec and Klimov, Oleg},
  title   = {Proximal Policy Optimization Algorithms},
  journal = {arXiv preprint arXiv:1707.06347},
  year    = {2017},
  doi     = {10.48550/arXiv.1707.06347}
}

@inproceedings{rudin2022rslrl,
  author    = {Rudin, Nikita and Hoeller, David and Reist, Philipp and Hutter, Marco},
  title     = {Learning to Walk in Minutes Using Massively Parallel Deep Reinforcement Learning},
  booktitle = {Conference on Robot Learning},
  year      = {2022}
}

@misc{isaac_sim_60,
  author = {{NVIDIA}},
  title  = {Isaac Sim 6.0 Documentation},
  year   = {2026},
  note   = {Version 6.0.0-rc.59 host standalone}
}

@misc{isaac_lab_beta,
  author = {{NVIDIA}},
  title  = {Isaac Lab},
  year   = {2026},
  note   = {v3.0.0-beta2}
}
```

---

## 正文引用對照（撰寫時用）

| 章節 | 建議引用 |
|------|----------|
| §1.1 | `zhmud2018_ultrasonic`, `gao2026_isaac_survey` |
| §2.2 | `gao2026_isaac_survey`, `mittal2023_orbit`, `hofer2021_sim2real` |
| §2.4 | `nvidia_rtx_acoustic`, `nvidia_rtx_annotators` |
| §3.6–3.7 | `isaac_sim_60`, `ur10_official` |
| §3.8 | `scheibler2018_pyroom`, `brinkmann2019_roundrobin` |
| §3.9–3.10 | `isaac_lab_beta`, `rsl_rl`, `ppo_schulman` |
| §4.7.1 | `ppo_schulman`, `rudin2022rslrl` |

---

*Cross-ref: `paper_rewriting_output/citation_support_bank.md` · Updated 2026-06-29*