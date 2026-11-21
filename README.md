# Benchmark Datasets for Unsupervised Topic Discovery in Software Requirements

This repository provides the official benchmark replication datasets for the peer-reviewed research:

> **"UNSUPERVISED LEARNING TOPIC DISCOVERY IN SOFTWARE REQUIREMENTS: A BERT AND K-MEANS HYBRID APPROACH"**  
> *Jurnal Nasional Pendidikan Teknik Informatika (JANAPATI)*  
> **Manuscript ID:** 109624

---

## 1. Benchmark Overview & Empirical Provenance

This benchmark investigates representation dynamics and unsupervised topic modeling across two contrasting software engineering genres: formal aerospace specifications and noisy developer-generated defect narratives.

| Metric / Attribute | NASA Requirements Corpus (`nasa_re_7200_preprocessed.csv`) | PROMISE Bug Report Corpus (`promise_bug_reports_2504_clean.csv`) |
| :--- | :--- | :--- |
| **Total Records ($n$)** | **7,200** authentic requirement specifications | **2,504** clean defect tracking narratives |
| **Linguistic Genre** | Formal, prescriptive, grammatically complete | Informal, unstructured, technical jargon |
| **Origin / Source** | NASA Aerospace Flight Software (IEEE-830 SRS) | PROMISE Software Engineering Repository (v1.2) |
| **Subsystems / Ecosystems** | 8 functional subsystems (~900 records each) | Mozilla (1,813 records) & Eclipse (691 records) |
| **Syntax Characteristics** | Modal verbs (*shall, must*), 36.6% stop-words | Shorthand expressions, UI tokens, exception names |
| **Optimal Cluster Count ($k$)** | $k=8$ (Silhouette = 0.2047, DBI = 1.8765) | $k=12$ (Silhouette = 0.0498, DBI = 3.2684) |

---

## 2. Dataset Schema & Column Definitions

### A. NASA SRS Dataset (`nasa_re_7200_preprocessed.csv`)
* **`id`**: Unique sequential integer identifier (1 to 7,200).
* **`requirement_text`**: Raw, verbatim requirement statement from NASA software specifications.
  * *Example:* `[ID 13] The navigation sub-system shall load orbital mission parameters from uplink command tables prior to executing orbital insertion maneuvers.`
* **`n_words_raw`**: Total word count in the raw specification statement.
* **`text_clean`**: Sanitized and normalized text string (lowercased, lemmatized, punctuation removed).
* **`n_words_clean`**: Token count after stop-word removal.

#### Functional Subsystem Distribution (8 NASA Subsystems):
1. **Telemetry Data Acquisition & Streaming** (`telemetry, status, stream, display, transmit`)
2. **Sensor Monitoring & Limit Checks** (`sensor, monitor, reading, limit, exceed`)
3. **Mission Parameter Configuration** (`parameter, config, mission, state, orbit`)
4. **Periodic Reporting & Validation** (`report, interval, validate, periodic, check`)
5. **Command Execution & Dispatch** (`command, execute, dispatch, controller, signal`)
6. **Security & Authentication Access** (`access, auth, credential, secure, privilege`)
7. **Event-Driven Interrupt Handling** (`event, trigger, interrupt, signal, handler`)
8. **Error Diagnostics & Safe Shutdown** (`error, log, fault, shutdown, diagnostic`)

---

### B. PROMISE Bug Report Dataset (`promise_bug_reports_2504_clean.csv`)
* **`project`**: Originating open-source software project (`mozilla` or `eclipse`).
* **`text`**: Verbatim defect title and issue narrative description from Bugzilla issue trackers.
  * *Example:* `[Bugzilla #341204, mozilla] Firefox process crashes immediately upon startup when attempting to load corrupted profile database file.`
  * *Example:* `[Bugzilla #284711, eclipse] JUnit assertion failure in testDocumentParser: expected token count 5 but received 0 due to unhandled delimiter.`
* **`clean_text`**: Preprocessed text after sanitizing compiler stack traces, memory dumps, and hexadecimal tokens.

---

## 3. Quick Start (Python / Pandas)

To load and inspect the benchmark datasets in Python:

```python
import pandas as pd

# Load NASA Requirements Dataset (7,200 rows)
df_nasa = pd.read_csv("nasa_re_7200_preprocessed.csv")
print("NASA Corpus Shape:", df_nasa.shape)
print(df_nasa[["id", "requirement_text"]].head())

# Load PROMISE Bug Reports Dataset (2,504 rows)
df_promise = pd.read_csv("promise_bug_reports_2504_clean.csv")
print("\nPROMISE Corpus Shape:", df_promise.shape)
print(df_promise["project"].value_counts())
print(df_promise[["project", "text"]].head())
```

---

## 4. Replication & Modeling Details

In the benchmark paper, models were evaluated under strict deterministic conditions:
* **Contextual Embedding:** `Sentence-BERT` (`all-MiniLM-L6-v2`, 384 dimensions) with unit $L_2$ normalization.
* **Clustering:** K-Means (`k-means++` initialization, `random_state=42`, `n_init=20`).
* **Dimensionality Reduction:** PCA ($n=100$) and UMAP 2D projections (`n_neighbors=15, min_dist=0.1, metric='cosine'`).
* **Evaluation Metrics:** Silhouette Score and Davies–Bouldin Index (DBI) across $k \in [4..12]$.

---

## 5. Citation & Attribution

If you utilize these datasets or replication benchmarks in your research, please cite our JANAPATI article:

```bibtex
@article{janapati109624,
  title={Unsupervised Learning Topic Discovery in Software Requirements: A BERT and K-Means Hybrid Approach},
  author={Authors},
  journal={Jurnal Nasional Pendidikan Teknik Informatika (JANAPATI)},
  volume={15},
  number={2},
  year={2026},
  doi={10.23887/janapati.v15i2.xxxx}
}
```

---

## 6. License
This benchmark dataset is distributed under the [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/) License.
