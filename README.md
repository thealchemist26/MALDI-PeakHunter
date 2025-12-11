# 🧪 MALDI PeakHunter

![Python](https://img.shields.io/badge/Python-3.8%2B-blue?style=for-the-badge&logo=python&logoColor=white)
![Platform](https://img.shields.io/badge/Platform-Windows%20%7C%20Linux%20%7C%20macOS-lightgrey?style=for-the-badge)
![License](https://img.shields.io/badge/License-Academic%20Research-green?style=for-the-badge)

**MALDI PeakHunter** is a specialized, Python-based desktop application designed for mass spectrometry analysis. It streamlines the workflow of visualizing mass spectra, searching for compound identifications via PubChem, and managing a local spectral library.

It bridges the gap between raw `m/z` data and chemical identification, offering real-time structure fetching, adduct calculations, and instrument-specific tolerance settings.

---

## 📸 Screenshots

| **Main Interface** | **Structure Analysis** |
|:---:|:---:|
| ![Main Interface](assets/screenshot_main.png) <br> *Interactive plotting and PubChem search* | ![Structure View](assets/screenshot_db.png) <br> *Chemical structure preview and local DB* |

*(Note: Please replace the paths above with your actual screenshots)*

---

## 🌟 Key Features

### 📊 Interactive Visualization
* **Stem Plotting:** visualizes mass spectra using `Matplotlib`.
* **Zoom & Pick:** Interactive zooming and peak picking. Click any peak to instantly trigger a database search.
* **Dynamic Labeling:** Automatically labels the most intense peaks within the current zoom view.

### 🔍 Dual-Engine Search
1.  **PubChem API Integration:** Real-time searching of the NCBI PubChem database.
    * Fetches Monoisotopic Mass, Name, Formula, and 2D Structure.
    * **Smart Filtering:** Filter results by class (Lipids, Peptides, Metabolites, Drugs, etc.).
    * **Adduct Handling:** Automatically calculates neutral mass based on selected adducts (e.g., `[M+H]+`, `[M+Na]+`, `[M+K]+`).
2.  **Local Library (SQLite):**
    * Build your own proprietary database.
    * Full CRUD (Create, Read, Update, Delete) capabilities.
    * Store MS/MS fragment data alongside precursor m/z.

### 🛠 Laboratory Workflow Tools
* **Instrument Presets:** One-click tolerance settings for common instruments:
    * MALDI MRT (High Res)
    * Cyclic IMS
    * Synapt G2/G2-Si
    * Standard TOF
* **Data Export:** Export analyzed results to `.csv` for further processing in Excel or R.
* **Structure Viewer:** Fetches and displays the 2D chemical structure of selected compounds.

---

## 🧩 Logic Flow



```mermaid
graph TD
    A[Input Spectrum List] --> B{Select Action}
    B -->|Generate Plot| C[Interactive Matplotlib Graph]
    B -->|Search| D[Adduct Calculation]
    D --> E{Source?}
    
    E -->|PubChem| F[REST API Call]
    F --> G[Filter by Class/PPM]
    G --> H[Display Results]
    
    E -->|Local DB| I[SQLite Query]
    I --> H
    
    H --> J[Select Result]
    J --> K[Fetch 2D Structure]
