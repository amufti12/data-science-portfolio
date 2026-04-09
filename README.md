# Aydan Mufti — Data Science & Engineering Portfolio

Hi! I'm Aydan, a data scientist and software engineer with a Master's in Data Science from Boston University (4.0 GPA) and a background in Computer Science from Kennesaw State University. My work spans machine learning, systems programming, and full-stack data pipelines. I'm passionate about building things that are both rigorous and useful — from predictive safety systems for the NFL to real-time sorting algorithm visualizers.

Currently working as a Quality Assurance Engineer at Carrier Global Corporation, where I build PowerBI dashboards and automate testing processes. Actively seeking Data Scientist roles.

aydanmufti@gmail.com &nbsp;|&nbsp; [github.com/amufti12](https://github.com/amufti12)

---

### Contents

1. [NFL Injury Prediction and Risk Analysis System](#nfl-injury-prediction)
2. [HuffPost News Classification (DistilBERT)](#huffpost-classification)
3. [NHTSA Vehicle Complaints — Azure Cloud Pipeline & Power BI Dashboard](#nhtsa-powerbi)
4. [AlgoViz — Sorting Algorithm Visualizer](#algoviz)
5. [N-Body Gravitational Simulator](#nbody-simulator)
6. [Spotify Song Prediction](#song-prediction)
7. [Video-to-Video Synthesis (C-Day 2021)](#vid2vid)

---

## [NFL Injury Prediction and Risk Analysis System](https://github.com/amufti12/NFL-Injury-Prediction-and-Risk-Analysis-System) <a name="nfl-injury-prediction"></a>

#### Overview
A comprehensive machine learning capstone analyzing NFL player injury patterns across multiple injury mechanisms to identify environmental and contextual risk factors for evidence-based safety policy decisions.

#### Technical Details
- **Datasets**: Three NFL datasets spanning 2016–2020 seasons (surface analytics, punt analytics, impact detection)
- **Models**: Logistic Regression (L1/L2/ElasticNet), Decision Trees, Random Forests, Gradient Boosting, SVMs (linear, RBF, polynomial), KNN, K-Means, DBSCAN, Hierarchical Agglomerative Clustering, Ridge/Lasso/SVR regression
- **Data Pipeline**: Missing value handling (−999 temperature codes), categorical consolidation, polynomial feature engineering (degree 2), stratified 75-25 splits, 3-fold CV, StandardScaler normalization
- **Overfitting Prevention**: Regularization penalties, tree pruning (`max_depth=5`, `min_samples_split=10`), ensemble subsampling, cross-validation gap monitoring
- **Tools**: Python (scikit-learn, pandas, NumPy, matplotlib, seaborn), Jupyter Notebook

#### Model Performance

| Task | Best Model | Accuracy | Notes |
|---|---|---|---|
| Concussion Occurrence (Dataset 2) | Logistic Regression (L2) | **91.7%** | 100% precision |
| Lower Extremity Severity (Dataset 1) | KNN (Euclidean) / SVM (RBF) | **70.0%** | Baseline: 60% |
| Impact Count Prediction (Dataset 3) | SVR (RBF) | MAE: 5.53 | R² = −0.055 |

#### Key Findings
- **Synthetic surfaces increase lower extremity injury rates by ~62%** (rate ratio 1.62, p=0.043) but show **no association with concussions** (rate ratio 1.00, p=1.000) — a mechanism-specific discovery with major policy implications
- Wide receivers, cornerbacks, and linebackers account for 72.7% of lower extremity injuries; returners and gunners show 2.5× elevated concussion risk
- Game context (time, score, field position) accounts for 47% of feature importance in concussion models; temperature interactions drive lower extremity predictions

#### Business Impact
- NFL teams spend ~$521M annually on injured players; preventing 10 injuries offsets significant intervention costs
- Mechanism-specific findings prevent wasteful uniform policies — surface interventions protect lower extremities but won't reduce concussions
- Position-targeted programs address 500 high-risk players vs. 1,696 league-wide

#### Deliverables
- [EDA Notebook](https://github.com/amufti12/NFL-Injury-Prediction-and-Risk-Analysis-System/blob/main/Capstone_EDA_Analysis.ipynb) · [Modeling Notebook](https://github.com/amufti12/NFL-Injury-Prediction-and-Risk-Analysis-System/blob/main/Capstone_Modeling.ipynb)
- Technical Report & Non-Technical Report (APA-formatted) available in repository

---
 
## [HuffPost News Classification (DistilBERT)](https://github.com/amufti12/HuffPost-News-Classification/tree/main) <a name="huffpost-classification"></a>
 
#### Overview
A natural language processing project fine-tuning DistilBERT on HuffPost news headlines to classify articles into topic categories, demonstrating transformer-based text classification on a real-world multi-class problem.
 
#### Technical Details
- **Model**: DistilBERT (distilbert-base-uncased), fine-tuned for multi-class classification
- **Dataset**: HuffPost News Category Dataset (Kaggle)
- **Performance**: 74.4% accuracy, 0.661 macro-F1
- **Tools**: Python (HuggingFace Transformers, PyTorch, pandas, scikit-learn)
 
#### Key Findings
- Transformer-based embeddings significantly outperform classical bag-of-words approaches for headline classification
- Macro-F1 of 0.661 reflects meaningful performance across imbalanced category distribution
- Short headline text presents inherent ambiguity — performance ceiling is constrained by label overlap across categories
 
---

## NHTSA Vehicle Complaints — Azure Cloud Pipeline & Power BI Dashboard <a name="nhtsa-powerbi"></a>

#### Overview
An end-to-end cloud data engineering and business intelligence project using NHTSA's public vehicle complaints dataset — a real-world record of consumer-reported automobile issues collected via web and phone. The pipeline spans ingestion, transformation, distributed storage, serverless querying, and interactive visualization across the full Microsoft Azure stack.

#### Technical Details
- **Data Source**: NHTSA Complaints Dataset (public record of vehicle complaints across U.S. manufacturers)
- **Cloud Services**: Azure Data Factory (ADF), Azure Data Lake Storage Gen2, Azure Synapse Analytics (Serverless SQL Pool), Azure CosmosDB, Microsoft Power BI
- **Pipeline Architecture**:
  - Raw `.txt` complaint file ingested via ADF and loaded into ADLS Gen2
  - ADF Data Flows used to transform and convert to `.parquet` format, partitioned by manufacturer name
  - Parquet files mounted as an external table in Azure Synapse using PolyBase / serverless SQL
  - A second dataset (TMDB movie data) loaded into CosmosDB via ADF for NoSQL querying
- **SQL Analysis**: Queried Ford F-150 crash records (1990–2010) using Synapse SQL — filtering by model, year range, and crash indicator, then aggregating crash counts by year
- **Power BI Report**:
  - Connected Power BI Desktop to the Synapse serverless SQL endpoint via Azure Synapse Analytics connector
  - Imported NHTSA Synapse external table and NHTSA Ford-specific safety ratings CSV (`Safercar_data_FORD.csv`)
  - Engineered a composite key (`YEAR_MODEL_ID = YEARTXT & "_" & MODELTXT`) to establish a many-to-one relationship between datasets in Model View
  - Built an interactive dashboard with bar/line charts, a data table, and dynamic slicers — all visuals cross-filter on selection

#### Key Features
- Full ELT pipeline (Extract → Load → Transform) rather than traditional ETL, reflecting modern cloud data architecture patterns
- Parquet partitioning by manufacturer enables efficient predicate pushdown in distributed queries (simulating production-scale data lake design)
- Cross-dataset joins in Power BI link complaint volume data with safety ratings for richer vehicle-level analysis
- Dashboard slicers allow filtering by model year, vehicle model, and crash indicator dynamically across all visuals

#### Tools & Technologies
Azure Data Factory · Azure Data Lake Storage Gen2 · Azure Synapse Analytics · Azure CosmosDB · Microsoft Power BI · SQL · Python · Parquet

---

## [AlgoViz — Sorting Algorithm Visualizer](https://github.com/amufti12/AlgoViz) <a name="algoviz"></a>

#### Overview
A real-time interactive visualizer for classic sorting algorithms, built in C++ with a custom OpenGL rendering pipeline and an ImGui control panel. Watch Selection, Bubble, Insertion, Merge, and Quick Sort animate step-by-step.

#### Technical Details
- **Language**: C++
- **Rendering**: OpenGL, GLFW, GLM, GLAD
- **UI**: ImGui (immediate-mode GUI)
- **Algorithms Implemented**: Selection Sort, Bubble Sort, Insertion Sort, Merge Sort, Quick Sort
- **Planned**: Additional sorting algorithms, graph traversal visualizations (BFS/DFS, Dijkstra's)

#### How to Build and Run
```bash
git clone https://github.com/amufti12/AlgoViz
cd AlgoViz
mkdir build && cd build
cmake -G "YOUR_GENERATOR" ..
cmake --build .
# Run the executable from the build directory
```

---

## [N-Body Gravitational Simulator](https://github.com/amufti12/NBodySimulator) <a name="nbody-simulator"></a>

#### Overview
A C++ simulation of the gravitational N-Body Problem — a dynamical system of particles interacting under Newtonian gravity, rendered in real time with interactive controls via ImGui and ImPlot.

#### Technical Details
- **Language**: C++
- **Physics**: Numerical integration of gravitational forces across N particles
- **Libraries**: Eigen (linear algebra), ImGui (UI), ImPlot (real-time plotting), GLFW (windowing)

#### How to Build and Run
```bash
cd <project root directory>
mkdir build
cd build
cmake -G "YOUR_GENERATOR" ..
cmake --build .
# Copy imgui.ini from the root directory into the executable directory, then run
```

---

## [Spotify Song Prediction](https://github.com/amufti12/song_prediction) <a name="song-prediction"></a>

#### Overview
A song recommendation algorithm built for the Spotify Million Playlist Dataset Challenge — given a seed playlist title and/or initial tracks, predict the subsequent tracks a user would add.

#### Technical Details
- **Dataset**: Spotify Million Playlist Dataset (1M playlists, 2M+ unique tracks, ~300K artists; 2010–2017)
- **Task**: Automatic playlist continuation (open-ended challenge on AIcrowd)
- **Approach**: Collaborative filtering and sequence-based recommendation modeling
- **Note**: The dataset is no longer publicly downloadable — contact Spotify Research directly for access

#### Background
Playlists represent 54% of consumer listening habits (Digital Music Alliance, 2018 Annual Music Report). By learning playlist structure — genre, mood, occasion, intent — recommendation systems can surface music that fits not just a user's taste, but a specific listening context. This challenge explores what makes songs "go together."

---

## [Video-to-Video Synthesis with Semantically Segmented Video](https://github.com/amufti12/Video-To-Video-Synthesis-C-Day-2021-Materials) <a name="vid2vid"></a>

*C-Day 2021 Research Presentation*

#### Abstract
This project studies the use of generative adversarial networks (GANs) to translate semantically segmented video masks into photo-realistic video — a task known as video-to-video synthesis. A conditional GAN architecture (derived from pix2pix) learns a mapping from semantic labels to realistic imagery. The model synthesizes translated video that replicates low-frequency scene details from the source, with adjacent frames maintaining rough coherence despite the absence of temporal context across frames. This study demonstrates how simplistic conditional GANs can perform surprisingly well at video-to-image translation tasks.

**Index Terms**: Machine Vision · Conditional GAN · Generative Adversarial Networks · pix2pix · Semantic Segmentation · vid2vid · Video-to-Video · Image-to-Image

---

*This portfolio is a living document. More projects are added as they're completed.*
