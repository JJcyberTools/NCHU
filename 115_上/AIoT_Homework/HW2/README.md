# HW2｜Multiple Linear Regression — Kaggle 洪水機率預測

> 國立中興大學・智慧物聯網（AIoT）個人作業  
> **主題：** 以多元線性迴歸預測洪水機率（FloodProbability）  
> **展示網頁：** https://jjcybertools.github.io/nchu_AIoT_Homework/HW2/

<img width="287" height="937" alt="image" src="https://github.com/user-attachments/assets/3d3a60e2-556a-42f5-bfae-cf3c190022bb" />


## 1. 專案簡介（Project Overview）

本作業使用 Kaggle 公開資料集 **Regression with a Flood Prediction Dataset**，以 20 個數值特徵預測連續型目標 `FloodProbability`。依照 **CRISP-DM** 六階段完成資料理解、資料前處理、迴歸模型訓練、特徵選擇、模型評估與展示部署。對照模型包含：

- **Ordinary Least Squares（OLS）**：`LinearRegression`
- **Ridge Regression**：L2 正則化，`alpha=1.0`
- **Lasso Regression**：L1 正則化，`alpha=0.001`

比較全部 20 特徵與 3 種各選取 14 特徵的策略，共 **4 種特徵配置 × 3 種模型 = 12 組實驗**。另以 OLS 建立 **95% 預測區間（Prediction Interval）**，並進行迴歸假設診斷。

> **定位與限制：** 本專案是公開競賽資料的教學研究，不應直接用於實際防洪警報或工程決策。網站是成果展示，不能取代作業規定的 Notebook 與 PDF 報告。

## 2. 資料集（Dataset）

| 項目 | 說明 |
|---|---|
| Kaggle 競賽 | [Playground Series S4E5 — Regression with a Flood Prediction Dataset](https://www.kaggle.com/competitions/playground-series-s4e5/data) |
| 訓練資料 | `train.csv` |
| 原始筆數 | 1,117,957 |
| 原始欄位 | 22 欄（`id`、20 個 predictors、`FloodProbability`） |
| 預測目標 y | `FloodProbability`，連續型數值 |
| 輸入 X | 20 個數值型特徵（不含 `id`） |
| 本次分析樣本 | 隨機抽樣 3,000 筆（`random_state=42`） |
| 訓練／測試 | 2,400／600 筆（80%／20%） |
| 缺失值 | 此次實際資料檢查未發現缺失值；程式仍保留補值流程 |

### 20 個輸入特徵

| 特徵 | 中文說明 | 特徵 | 中文說明 |
|---|---|---|---|
| `MonsoonIntensity` | 季風強度 | `TopographyDrainage` | 地形排水 |
| `RiverManagement` | 河川管理 | `Deforestation` | 森林砍伐 |
| `Urbanization` | 都市化程度 | `ClimateChange` | 氣候變遷 |
| `DamsQuality` | 水壩品質 | `Siltation` | 淤積 |
| `AgriculturalPractices` | 農業作法 | `Encroachments` | 土地侵占 |
| `IneffectiveDisasterPreparedness` | 防災準備不足 | `DrainageSystems` | 排水系統 |
| `CoastalVulnerability` | 海岸脆弱性 | `Landslides` | 山崩 |
| `Watersheds` | 集水區 | `DeterioratingInfrastructure` | 基礎設施老化 |
| `PopulationScore` | 人口指標 | `WetlandLoss` | 濕地流失 |
| `InadequatePlanning` | 規劃不足 | `PoliticalFactors` | 政治因素 |

> 中文名稱是為了方便閱讀的概略翻譯；特徵值的尺度與方向須以 Kaggle 資料集定義為準，不能直接當作實際氣象量測單位。

## 3. 技術架構（Technology Stack）

| 類別 | 技術 | 用途 |
|---|---|---|
| 程式語言 | Python 3、JavaScript | 分析訓練及網頁互動 |
| 資料處理 | `pandas`、`numpy` | 讀檔、抽樣與矩陣計算 |
| 機器學習 | `scikit-learn` | 前處理、特徵選擇、迴歸建模與評估 |
| 統計檢定 | `scipy`、`statsmodels` | t 分布、同方差、殘差常態與 VIF |
| 視覺化 | `matplotlib`、Plotly.js、SVG | Notebook 分析圖、網站圖表與即時特徵影響圖 |
| 文件 | Jupyter Notebook、Markdown、PDF | 實驗記錄與正式報告 |
| 部署 | GitHub Pages | 靜態成果網站 |

前端不需要 Python 伺服器：網站利用已訓練模型的**固定 Ridge 係數**做單筆情境試算；模型訓練、特徵選擇與測試集評估是在 Python Notebook 中執行。

## 4. 專案檔案（Project Structure）

```text
HW2/
├── index.html                          # GitHub Pages 互動分析展示
├── README.md                           # 本技術文件
├── HW2_Multiple_Linear_Regression.ipynb # Python 實驗與 CRISP-DM 分析
├── REPORT_TEMPLATE.md                  # 正式報告內容草稿
├── requirements.txt                    # Python 套件依賴
└── data/
    ├── README.md                       # 資料存放說明
    └── train.csv                       # 自行由 Kaggle 下載；不提交大型原始資料
```

Notebook 執行後會在其工作目錄輸出模型比較 CSV 與分析圖，例如 `model_comparison.csv`、`evaluation_plots.png`、`prediction_interval_95.png`、`regression_diagnostics.png`。

## 5. 安裝與執行（Quick Start）

### 5.1 準備資料

1. 開啟 [Kaggle 資料頁](https://www.kaggle.com/competitions/playground-series-s4e5/data)，登入並依競賽規定下載資料。
2. 解壓縮後，將 `train.csv` 放入 `HW2/data/train.csv`。
3. 原始 CSV 約有 111 萬筆資料，不建議直接提交至 GitHub。

### 5.2 Python 執行環境

在專案的 `HW2/` 目錄執行：

```bash
python -m pip install -r requirements.txt
jupyter notebook HW2_Multiple_Linear_Regression.ipynb
```

開啟 Notebook 後，使用 **Run All（執行全部儲存格）**，確認輸出數據與圖表產生成功，最後儲存包含執行結果的 `.ipynb`。

> Notebook 使用相對路徑 `data/train.csv`，建議以 `HW2/` 作為工作目錄。序列特徵選擇（SFS）可能需要較長運算時間。

### 5.3 抽樣參數

```python
RANDOM_STATE = 42
MAX_ROWS = 3000
TARGET = "FloodProbability"
```

`MAX_ROWS=None` 可取消抽樣上限，但由於 SFS 計算成本高，完整資料訓練可能非常耗時。**報告中的實測數值以固定抽樣 3,000 筆為準**。

## 6. CRISP-DM 六階段（研究方法）

### 6.1 Business Understanding｜商業理解

研究問題：20 個環境與基礎設施特徵能否預測洪水機率？在這份資料下，特徵選擇及正則化是否改善預測誤差？評估以測試集 RMSE、R² 等指標呈現。

### 6.2 Data Understanding｜資料理解

檢查資料形狀、欄位名稱、缺失值、目標分布與特徵相關性；確認 `FloodProbability` 為連續目標，`id` 僅為識別欄位，不納入建模。

### 6.3 Data Preparation｜資料準備

- `random_state=42` 抽樣 3,000 筆。
- `train_test_split(test_size=0.2, random_state=42)`。
- `SimpleImputer(strategy="median")`：補值器只在訓練集擬合。
- `StandardScaler()`：標準化參數只由訓練集估計。
- 特徵選擇只根據訓練資料執行，避免直接利用測試集標籤挑選特徵。

### 6.4 Modeling｜建模與特徵選擇

| 選擇方式 | 設定 | 說明 |
|---|---|---|
| All features | 20 特徵 | 基準模型 |
| SelectKBest | `f_regression`、k=14 | 依單變量 F 檢定選擇特徵 |
| RFE | `LinearRegression`、k=14 | 遞迴剔除特徵 |
| SFS | forward、k=14、CV=3 | 逐步向前選取特徵 |

各策略分別建立 `LinearRegression`、`Ridge(alpha=1.0)`、`Lasso(alpha=0.001)` 三種模型，總共 12 組。

### 6.5 Evaluation｜模型評估

評估指標：

```text
MAE         = mean(|y - y_pred|)
MSE         = mean((y - y_pred)^2)
RMSE        = sqrt(MSE)
R²          = 1 - SS_res / SS_tot
Adjusted R² = 1 - (1 - R²) * (n - 1) / (n - p - 1)
```

其中 `n` 為測試筆數，`p` 為模型特徵數。

產生的分析圖包含 Actual vs Predicted、Residual Plot、Residual Distribution、**OLS 95% 預測區間**、Residual vs Fitted 以及 Q–Q Plot。

> 本次將 12 組模型依**測試集** RMSE 排序，屬於課堂實驗比較；若要嚴格估計泛化能力，應以訓練集交叉驗證選定模型，再對獨立測試集進行一次最終評估，避免測試集參與模型選擇。

### 6.6 Deployment｜部署展示

使用訓練好的 Ridge 模型係數，在靜態 GitHub Pages 網站提供 20 個特徵滑桿與數值輸入，並即時更新單筆 `FloodProbability` 預測及相對於範例的特徵貢獻變化。此互動**不會重新訓練模型**，固定測試集的 RMSE、R² 不會跟著滑桿變動。

## 7. 實驗結果（Actual Experimental Results）

**資料設定：** 3,000 筆抽樣、80/20 切分、20 個原始特徵。以下數字是先前使用實際 `train.csv` 執行 Python 實驗所得，並非 Kaggle 公開榜單分數。

| 特徵選擇 | 模型 | 特徵數 | Test RMSE | Test R² |
|---|---|---:|---:|---:|
| All features | **Ridge** | 20 | **0.019644** | **0.860401** |
| All features | OLS | 20 | 0.019645 | 0.860391 |
| All features | Lasso | 20 | 0.020335 | 0.850417 |
| SelectKBest | OLS | 14 | 0.033470 | 0.594750 |
| SelectKBest | Ridge | 14 | 0.033470 | 0.594743 |
| SFS | OLS | 14 | 0.033681 | 0.589622 |
| SFS | Ridge | 14 | 0.033681 | 0.589616 |
| SelectKBest | Lasso | 14 | 0.033826 | 0.586082 |
| SFS | Lasso | 14 | 0.034031 | 0.581053 |
| RFE | Ridge | 14 | 0.034242 | 0.575833 |
| RFE | OLS | 14 | 0.034243 | 0.575820 |
| RFE | Lasso | 14 | 0.034445 | 0.570804 |

### 7.1 最低測試 RMSE 的組合

| 指標 | Ridge（全部 20 特徵） |
|---|---:|
| MAE | 0.015378 |
| MSE | 0.000385897 |
| RMSE | **0.019644** |
| R² | **0.860401** |
| Adjusted R² | 0.855579 |

觀察：在這次抽樣與固定切分中，完整 20 特徵的 Ridge 與 OLS 差距很小；減少至 14 特徵後，測試 RMSE 反而增加。這不表示所有資料集都不適合特徵選擇。

### 7.2 95% 預測區間（Prediction Interval）

使用 **OLS** 的解析式、訓練殘差估計的變異數與 t 分布臨界值計算 95% 預測區間（不是 Ridge 的精確預測區間）。

- 測試集實測涵蓋率：**95.0%**
- 平均預測區間寬度：**0.07859**

傳統計算方式依賴近似線性、獨立、同方差及常態誤差假設；本次診斷顯示部分假設不完全成立，因此區間屬於教學用近似，不能宣稱可靠的實務保障。

### 7.3 迴歸假設診斷（Regression Diagnostics）

| 檢查項目 | 方法 | 先前實測 | 解讀 |
|---|---|---:|---|
| 線性關係 | Residual vs Fitted | 圖形判讀 | 觀察殘差型態 |
| 同方差 | Breusch–Pagan | p = 1.36×10⁻¹⁹ | 拒絕同方差虛無假設 |
| 殘差常態性 | Jarque–Bera、Q–Q | p = 1.54×10⁻⁷³ | 拒絕常態假設 |
| 殘差獨立性 | Durbin–Watson | 2.064 | 抽樣順序非時間順序，無法單憑此值確立獨立 |
| 多重共線性 | VIF | 最大 1.018 | 未見嚴重共線性 |

## 8. 網頁互動功能與技術說明

🌐 **[開啟 HW2 互動網站](https://jjcybertools.github.io/nchu_AIoT_Homework/HW2/)**

| 網頁模組 | 說明 |
|---|---|
| 研究資訊 | 顯示 Kaggle 資料量、抽樣數、特徵數與拆分比例 |
| 評估儀表板 | 展示 Ridge 最佳實測 RMSE、R²、MAE、Adjusted R² |
| 模型比較 | 12 組模型資料表及 Plotly RMSE 圖 |
| OLS 預測區間 | 可縮放、滑鼠提示的預測圖；網頁僅展示部分測試點 |
| Actual vs Predicted | 可互動散點圖，為視覺化展示 |
| 即時 Ridge 預測 | 20 個特徵輸入／滑桿，依固定已訓練係數計算結果 |
| 特徵變動影響 | 顯示輸入相對於預設範例的模型預測變化量 |
| CRISP-DM | 六階段的研究流程摘要 |

**重要區別：**
1. 網站的歷史模型分數是已完成實驗的固定結果，不會隨單筆輸入改變。
2. 滑桿試算屬於**固定 Ridge 係數的推論**，不是重新執行 12 組模型訓練。
3. 預測區間圖使用 **OLS**，Ridge 為測試 RMSE 最低的模型，兩者方法應分開理解。
4. 網頁只有部分測試點用於互動展示，完整評估請看 Notebook 的測試集輸出。
5. 前端使用 Plotly CDN；若外部套件遭網路阻擋，互動圖可能無法載入。固定數值與表格仍可參照。

## 9. 方法比較與研究限制

- **OLS**：基準線性迴歸，容易解釋係數，但對共線性與極端值較敏感。
- **Ridge**：L2 正則化，通常可減少係數波動，本次在固定設定下測試 RMSE 略低於 OLS。
- **Lasso**：L1 正則化，可產生稀疏係數；本次使用固定 alpha，不代表已完成最佳超參數調整。
- **其他網路常見方向（尚未在本專案實測）**：Random Forest、Gradient Boosting；不可將其假設表現寫成實驗結果。

資料限制包括：只使用 3,000 筆抽樣、單次訓練測試切分、部分迴歸假設不成立、測試集模型排序可能帶入選擇偏誤。**Kaggle 官方榜單名次目前未提供，不應虛構名次。** NotebookLM 外部資料研究與引用比較待學生自行完成後加入正式報告。

## 10. AI 協助揭露（AI Assistance）

### GPT 輔助內容

ChatGPT 協助研究流程規劃、程式與文件整理、回歸比較、圖表呈現、網站互動功能及除錯。依老師要求，**需由學生將實際完整 GPT 對話匯出 PDF** 並放入繳交檔案。

### NotebookLM 摘要

老師要求使用 NotebookLM 研究網路上同主題的解法，並在正式報告中撰寫**至少 100 字的摘要**，明確標示「NotebookLM 摘要」，並比較網路上的主流或更優方法。

> 此部分必須由學生實際操作及確認引用來源。目前尚未提供真實 NotebookLM 研究結果，因此不代寫成已完成紀錄。

## 11. 作業繳交檢查（Submission）

老師指定繳交格式為學號命名的 ZIP，例如 `7114056XXX_hw2.zip`，建議包括：

```text
7114056XXX_hw2.zip
├── 7114056XXX_hw2.ipynb           # 或 .py，含實際執行結果
├── 7114056XXX_hw2_report.pdf      # 六階段、圖表、方法比較、AI揭露
├── 7114056XXX_hw2_GPT.pdf         # 真實對話匯出 PDF
├── README.md                      # 技術文件與 GitHub / Colab 連結
└── figures/                       # 預測圖、95%區間圖、診斷圖（建議）
```

- [x] Kaggle 資料來源與 20 個特徵
- [x] 多元線性迴歸、三種特徵選擇、12 組模型比較
- [x] MAE、MSE、RMSE、R²、Adjusted R²
- [x] 95% 預測區間與五大假設診斷
- [x] CRISP-DM 六階段與 GitHub 網頁
- [ ] **學生**：姓名／學號、指定檔名
- [ ] **學生**：完整 GPT 對話匯出 PDF
- [ ] **學生**：NotebookLM 研究與至少 100 字摘要、外部來源
- [ ] **繳交前**：執行並儲存 Notebook 圖表輸出、PDF 合併確認、ZIP 與 iLearning 上傳

> 網站及 GitHub 屬於額外展示。**最終以老師 iLearning HW2 作業說明為準。**

## 12. 參考資料（References）

1. Kaggle — [Regression with a Flood Prediction Dataset, Playground Series S4E5](https://www.kaggle.com/competitions/playground-series-s4e5/data)
2. scikit-learn — [Linear Models](https://scikit-learn.org/stable/modules/linear_model.html)
3. scikit-learn — [Feature Selection](https://scikit-learn.org/stable/modules/feature_selection.html)
4. scikit-learn — [Model Evaluation](https://scikit-learn.org/stable/modules/model_evaluation.html)
5. statsmodels — [Regression Diagnostics](https://www.statsmodels.org/stable/diagnostic.html)
6. Wirth & Hipp（2000）— CRISP-DM methodology（六階段框架）

---

**專案連結：** [GitHub HW2 原始碼](https://github.com/JJcyberTools/nchu_AIoT_Homework/tree/main/HW2) ｜ [GitHub Pages 網站](https://jjcybertools.github.io/nchu_AIoT_Homework/HW2/)
