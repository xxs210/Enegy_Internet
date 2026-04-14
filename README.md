# Enegy_Internet
# 交直流混合微电网容量优化配置

黑龙江大庆地区交直流混合微电网系统储能容量优化配置研究项目。采用双层优化框架（外层启发式算法 + 内层线性规划），对5种储能技术进行容量和功率的最优配置。

---

## 项目结构总览

```
d:\能源互联网综合实验\
│
├── Data.csv / Data.xlsx                  ← 原始光伏数据
├── sensitivity_analysis.py               ← 敏感性分析脚本
├── 项目回顾.md                           ← 项目整体回顾文档
├── 容量优化配置实验.pdf / .pptx           ← 实验报告
├── 能源互联网项目：数据清洗与治理技术文档  ← 数据清洗技术文档
├── 随机场景下交直流混合微电网...研究.md    ← 研究论文草稿
│
├── 实验结果_v1/                          ← v1 基线实验
├── 实验结果_v2/                          ← v2 数据方法对比
└── 实验结果_v3/                          ← v3 系统完善（4个Phase + 压力测试）
```

---

## 根目录文件

| 文件                                                | 说明                                                 |
| --------------------------------------------------- | ---------------------------------------------------- |
| `Data.csv`                                          | 原始光伏出力数据（CSV格式），全年15分钟间隔          |
| `Data.xlsx`                                         | 原始光伏出力数据（Excel格式）                        |
| `sensitivity_analysis.py`                           | 敏感性分析脚本，分析典型日权重和聚类方法对结果的影响 |
| `项目回顾.md`                                       | 项目整体回顾文档，包含思路、约束演进、数据对比       |
| `容量优化配置实验.pdf`                              | 实验报告PDF版                                        |
| `容量优化配置实验.pptx`                             | 实验报告PPT版                                        |
| `能源互联网项目：数据清洗与治理技术文档.md`         | 数据清洗与治理的技术文档（Markdown版）               |
| `能源互联网项目：数据清洗与治理技术文档.pdf`        | 数据清洗与治理的技术文档（PDF版）                    |
| `随机场景下交直流混合微电网系统容量优化配置研究.md` | 研究论文草稿                                         |

---

## 实验结果_v1/ — 基线实验

v1是首轮基线实验，搭建完整的双层优化框架并验证可行性。采用3σ数据清洗 + K-means聚类，无电网交互、无碳排放约束、无老化模型。

### 文档

| 文件                                                | 说明                                             |
| --------------------------------------------------- | ------------------------------------------------ |
| `实验记录_v1.md`                                    | v1完整实验记录，包含设计思路、问题解决、优化结果 |
| `实验说明.md`                                       | v1系统架构、数学模型、算法设计的详细说明         |
| `随机场景下交直流混合微电网系统容量优化配置研究.md` | 基于v1的研究论文草稿                             |

### 核心代码

| 文件                    | 说明                                                   |
| ----------------------- | ------------------------------------------------------ |
| `bilevel_solver.py`     | 外层GA求解器（DEAP库），定义搜索空间、评估函数、GA参数 |
| `optimization_model.py` | 内层Pyomo优化模型，定义变量、约束、目标函数及求解      |
| `data_processor.py`     | 数据预处理，3σ清洗 + K-means聚类生成典型日数据         |
| `run_solver.py`         | 运行入口脚本，含日志记录和检查点机制                   |
| `k-means.py`            | K-means聚类独立脚本                                    |
| `analyze_results.py`    | 结果分析脚本，输出成本分解和运行指标                   |
| `check_progress.py`     | 优化运行进度检查脚本                                   |
| `quick_test.py`         | 快速测试脚本（小种群少迭代）                           |

### 数据文件

| 文件                    | 说明                        |
| ----------------------- | --------------------------- |
| `Cleaned_Data.csv`      | 3σ清洗后的光伏数据          |
| `Typical_Days_Data.csv` | K-means聚类生成的典型日数据 |
| `typical_pv_data.csv`   | 典型日光伏出力曲线数据      |
| `typical_load_data.csv` | 典型日负载曲线数据          |
| `typical_pv_days.png`   | 典型日光伏出力曲线图        |
| `typical_load_days.png` | 典型日负载曲线图            |

### 日志与结果

| 文件                                              | 说明             |
| ------------------------------------------------- | ---------------- |
| `optimization_results.txt`                        | 最终优化结果     |
| `bilevel_optimization_output.log`                 | 主运行日志       |
| `bilevel_optimization_output_debug.log`           | 调试日志         |
| `bilevel_optimization_output_full_debug.log`      | 完整调试日志     |
| `bilevel_optimization_output_new.log`             | 新一轮运行日志   |
| `final_run_manual_loop.log`                       | 手动循环运行日志 |
| `final_run_output.log` / `final_run_output_2.log` | 最终运行日志     |
| `run_output.log`                                  | 运行输出日志     |
| `quick_test.log`                                  | 快速测试日志     |
| `verify_test.log`                                 | 验证测试日志     |

---

## 实验结果_v2/ — 数据方法对比实验

v2重点研究不同数据清洗策略和聚类方法对优化结果的影响。采用Z-score清洗（保留极端天气日），对比K-means、层次聚类、DBSCAN三种方法。

### 文档

| 文件                                                | 说明                                             |
| --------------------------------------------------- | ------------------------------------------------ |
| `实验记录_v2.md`                                    | v2完整实验记录，包含清洗对比、聚类对比、结果分析 |
| `实验说明.md`                                       | v2实验框架说明                                   |
| `随机场景下交直流混合微电网系统容量优化配置研究.md` | 基于v2的研究论文草稿                             |

### 核心代码

| 文件                    | 说明                                          |
| ----------------------- | --------------------------------------------- |
| `data_processor_v2.py`  | Z-score清洗 + K-means/层次/DBSCAN三种聚类实现 |
| `data_processor.py`     | v1数据处理器（保留供参考）                    |
| `bilevel_solver.py`     | 外层GA求解器（与v1相同）                      |
| `optimization_model.py` | 内层LP模型（与v1相同）                        |
| `run_comparison.py`     | 可配置的双层优化求解器，支持不同聚类方法      |
| `run_dbscan.py`         | DBSCAN聚类独立运行脚本                        |
| `run_remaining.py`      | 断点续传运行脚本                              |
| `run_solver.py`         | 基础运行脚本                                  |
| `compare_results.py`    | 三种方法结果综合对比脚本                      |
| `analyze_results.py`    | 结果分析脚本                                  |
| `check_progress.py`     | 进度检查脚本                                  |
| `quick_test.py`         | 快速测试脚本                                  |
| `rl_data_assessment.py` | 数据质量评估脚本                              |
| `k-means.py`            | K-means聚类独立脚本                           |

### 聚类结果子目录

| 目录            | 说明                |
| --------------- | ------------------- |
| `K-means/`      | K-means聚类优化结果 |
| `Hierarchical/` | 层次聚类优化结果    |
| `DBSCAN/`       | DBSCAN聚类优化结果  |

每个子目录包含：

- `optimization_results.txt` — 该聚类方法的优化结果
- `run_log.txt` — 运行日志

### 数据文件

| 文件                                                         | 说明                       |
| ------------------------------------------------------------ | -------------------------- |
| `Cleaned_Data.csv`                                           | 3σ清洗数据（供对比）       |
| `Cleaned_Data_Zscore.csv`                                    | Z-score清洗后的光伏数据    |
| `Typical_Days_Data.csv`                                      | 典型日汇总数据             |
| `clustering_weights.json`                                    | 各聚类方法的权重数据       |
| `typical_pv_kmeans.csv` / `typical_pv_hierarchical.csv` / `typical_pv_dbscan.csv` | 各聚类方法的光伏典型日数据 |
| `typical_load_kmeans.csv` / `typical_load_hierarchical.csv` / `typical_load_dbscan.csv` | 各聚类方法的负载典型日数据 |
| `typical_pv_data.csv` / `typical_load_data.csv`              | 默认典型日数据             |

### 图表

| 文件                                            | 说明                       |
| ----------------------------------------------- | -------------------------- |
| `clustering_comparison.png`                     | 三种聚类方法对比图         |
| `daily_pv_distribution.png`                     | 日光伏出力分布图           |
| `dbscan_pca_scatter.png`                        | DBSCAN PCA降维散点图       |
| `dendrogram.png`                                | 层次聚类树状图             |
| `k_distance_plot.png`                           | K-距离图（DBSCAN参数选择） |
| `typical_pv_days.png` / `typical_load_days.png` | 默认典型日曲线图           |
| `typical_days_dbscan.png`                       | DBSCAN典型日曲线图         |
| `typical_days_hierarchical.png`                 | 层次聚类典型日曲线图       |

### 对比结果

| 文件                              | 说明                         |
| --------------------------------- | ---------------------------- |
| `comparison_results.json`         | 三种方法完整对比结果         |
| `comparison_results_partial.json` | 部分对比结果（运行中间保存） |
| `comparison_kmeans.json`          | K-means单独对比结果          |

---

## 实验结果_v3/ — 系统完善实验

v3在v2基础上逐步引入电网交互、碳排放约束、电池老化模型，并通过GA/PSO/DE三种算法交叉验证。包含4个Phase和跨区域压力测试。

### 文档

| 文件             | 说明                                           |
| ---------------- | ---------------------------------------------- |
| `实验记录_v3.md` | v3完整实验记录，包含4个Phase的设计、问题、结果 |

### 核心代码

| 文件                          | 说明                                        |
| ----------------------------- | ------------------------------------------- |
| `bilevel_solver_v3.py`        | Phase 1基础求解器（电网交互 + TOU分时电价） |
| `bilevel_solver_v3_carbon.py` | Phase 2碳排放约束求解器（LP简化版）         |
| `bilevel_solver_v3_aging.py`  | Phase 3电池老化求解器（动态寿命模型）       |
| `solvers_pso_de.py`           | Phase 4 PSO和DE算法实现                     |
| `data_processor_v2.py`        | Z-score清洗 + 三种聚类（与v2共用）          |
| `kaggle_pressure_test.py`     | 跨区域压力测试脚本                          |

### 运行脚本

| 文件                          | 说明                                      |
| ----------------------------- | ----------------------------------------- |
| `run_phase1_all.py`           | Phase 1：三种聚类方法对比运行             |
| `run_phase2_all.py`           | Phase 2：四种碳预算 × 三种聚类            |
| `run_phase2_all_v2.py`        | Phase 2改进版（串行执行，避免多进程竞争） |
| `run_phase2_cb.py`            | Phase 2单场景运行（指定碳预算和聚类方法） |
| `run_phase2_cb_subprocess.py` | Phase 2子进程运行脚本                     |
| `_run_cb800_kmeans.py`        | 碳预算800 + K-means专用运行脚本           |
| `run_phase3_all.py`           | Phase 3：固定/动态寿命对比运行            |
| `run_phase4_all.py`           | Phase 4：三种算法 × 三种聚类运行          |
| `run_dbscan_dynamic.py`       | DBSCAN动态寿命单独运行脚本                |
| `summarize_phase3.py`         | Phase 3结果汇总脚本                       |
| `summarize_phase4.py`         | Phase 4结果汇总脚本                       |

### 测试/调试脚本

| 文件                                     | 说明                        |
| ---------------------------------------- | --------------------------- |
| `quick_test_phase1.py`                   | Phase 1快速测试             |
| `quick_test_phase2.py`                   | Phase 2快速测试             |
| `quick_phase2_test.py`                   | Phase 2快速测试（另一版本） |
| `test_aging_solver.py`                   | 老化求解器测试              |
| `test_cb800.py`                          | 碳预算800测试               |
| `test_coo.py`                            | 稀疏矩阵格式测试            |
| `test_dense.py`                          | 密集矩阵格式测试            |
| `test_inner_cb.py`                       | 内层碳预算测试              |
| `test_lp_speed.py` / `test_lp_speed2.py` | LP求解速度测试              |
| `test_memory.py`                         | 内存使用测试                |
| `test_phase4_ga.py`                      | Phase 4 GA测试              |
| `test_phase4_quick.py`                   | Phase 4快速测试             |
| `test_scipy.py`                          | SciPy兼容性测试             |
| `test_speed.py`                          | 求解速度测试                |

### 聚类结果子目录

| 目录            | 说明                      |
| --------------- | ------------------------- |
| `K-means/`      | K-means聚类各阶段优化结果 |
| `Hierarchical/` | 层次聚类各阶段优化结果    |
| `DBSCAN/`       | DBSCAN聚类各阶段优化结果  |

每个子目录包含：

| 文件                               | 说明                             |
| ---------------------------------- | -------------------------------- |
| `optimization_results.txt`         | Phase 1基础优化结果              |
| `run_log.txt`                      | Phase 1运行日志                  |
| `phase2_results_nocb.txt`          | Phase 2无碳预算结果              |
| `phase2_results_cb800.txt`         | Phase 2碳预算800结果             |
| `phase2_results_cb500.txt`         | Phase 2碳预算500结果             |
| `phase2_results_cb300.txt`         | Phase 2碳预算300结果             |
| `phase2_log_*.txt`                 | Phase 2各碳预算运行日志          |
| `results_phase3_fixed_cb500.txt`   | Phase 3固定寿命结果（碳预算500） |
| `results_phase3_dynamic_cb500.txt` | Phase 3动态寿命结果（碳预算500） |
| `log_phase3_*.txt`                 | Phase 3运行日志                  |
| `phase4_ga_results.txt`            | Phase 4 GA算法结果               |
| `phase4_pso_results.txt`           | Phase 4 PSO算法结果              |
| `phase4_de_results.txt`            | Phase 4 DE算法结果               |
| `pso_results.txt` / `pso_log.txt`  | PSO早期运行结果和日志            |
| `de_results.txt` / `de_log.txt`    | DE早期运行结果和日志             |

### 压力测试子目录

| 目录           | 说明                   |
| -------------- | ---------------------- |
| `kaggle_test/` | 跨区域合成数据压力测试 |

| 子目录                  | 说明                    |
| ----------------------- | ----------------------- |
| `Desert_kmeans/`        | 沙漠高辐照区 + K-means  |
| `Desert_hierarchical/`  | 沙漠高辐照区 + 层次聚类 |
| `Nordic_kmeans/`        | 北欧低辐照区 + K-means  |
| `Nordic_hierarchical/`  | 北欧低辐照区 + 层次聚类 |
| `Monsoon_kmeans/`       | 季风多变区 + K-means    |
| `Monsoon_hierarchical/` | 季风多变区 + 层次聚类   |

每个区域子目录包含：

- `optimization_results.txt` — 优化结果
- `typical_pv.csv` / `typical_load.csv` — 典型日数据
- `weights.json` — 聚类权重
- `kmeans/` 或 `hierarchical/` — 含运行日志和结果

| 文件                                     | 说明             |
| ---------------------------------------- | ---------------- |
| `kaggle_test/pressure_test_summary.json` | 压力测试汇总结果 |

### 数据文件

| 文件                                                         | 说明                    |
| ------------------------------------------------------------ | ----------------------- |
| `Cleaned_Data_Zscore.csv`                                    | Z-score清洗后的光伏数据 |
| `clustering_weights.json`                                    | 聚类权重数据            |
| `typical_pv_kmeans.csv` / `typical_pv_hierarchical.csv` / `typical_pv_dbscan.csv` | 各聚类方法光伏典型日    |
| `typical_load_kmeans.csv` / `typical_load_hierarchical.csv` / `typical_load_dbscan.csv` | 各聚类方法负载典型日    |

### 对比结果

| 文件                        | 说明                         |
| --------------------------- | ---------------------------- |
| `phase1_comparison.json`    | Phase 1三种聚类对比结果      |
| `phase2_cb_comparison.json` | Phase 2碳预算对比结果        |
| `phase3_comparison.json`    | Phase 3固定/动态寿命对比结果 |
| `phase4_comparison.json`    | Phase 4三种算法对比结果      |
| `progress_checkpoint.json`  | 运行进度检查点               |

### 图表

| 文件                            | 说明                 |
| ------------------------------- | -------------------- |
| `clustering_comparison.png`     | 聚类方法对比图       |
| `daily_pv_distribution.png`     | 日光伏出力分布图     |
| `dbscan_pca_scatter.png`        | DBSCAN PCA散点图     |
| `dendrogram.png`                | 层次聚类树状图       |
| `k_distance_plot.png`           | K-距离图             |
| `typical_days_dbscan.png`       | DBSCAN典型日曲线图   |
| `typical_days_hierarchical.png` | 层次聚类典型日曲线图 |

### 日志与临时文件

| 文件                                            | 说明                     |
| ----------------------------------------------- | ------------------------ |
| `phase2_cb_progress.txt`                        | Phase 2碳预算运行进度    |
| `phase2_progress.txt`                           | Phase 2运行进度          |
| `phase4_progress.txt`                           | Phase 4运行进度          |
| `phase2_cb_stdout.log` / `phase2_cb_stderr.log` | Phase 2标准输出/错误日志 |
| `phase3_stdout.log` / `phase3_stderr.log`       | Phase 3标准输出/错误日志 |
| `dbscan_dynamic_output.txt`                     | DBSCAN动态寿命运行输出   |
| `scipy_test_result.txt`                         | SciPy测试结果            |
| `speed_result.txt`                              | 求解速度测试结果         |
| `test_lp_speed_results.txt`                     | LP速度测试结果           |

---

## .idea/ — IDE配置

PyCharm项目配置目录，包含IDE设置，无需手动修改。

---

## 版本演进关系

```
v1 基线实验                    v2 数据方法对比              v3 系统完善
┌──────────────────┐    ┌──────────────────┐    ┌──────────────────────────┐
│ 3σ数据清洗        │    │ Z-score数据清洗    │    │ Phase 1: 电网交互+TOU电价  │
│ K-means聚类       │ →  │ 三种聚类方法对比    │ →  │ Phase 2: 碳排放约束+碳税   │
│ 双层优化(GA+LP)   │    │ 同一优化模型       │    │ Phase 3: 电池老化模型      │
│ 基础约束          │    │                   │    │ Phase 4: GA/PSO/DE对比     │
│ LER惩罚函数       │    │                   │    │ 压力测试: 跨区域验证        │
└──────────────────┘    └──────────────────┘    └──────────────────────────┘
```
