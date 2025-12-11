# 🧪 MALDI PeakHunter

![Python](https://img.shields.io/badge/Python-3.8%2B-blue?style=for-the-badge&logo=python&logoColor=white)
![Platform](https://img.shields.io/badge/Platform-Windows%20%7C%20Linux%20%7C%20macOS-lightgrey?style=for-the-badge)
![License](https://img.shields.io/badge/License-Academic%20Research-green?style=for-the-badge)

**MALDI PeakHunter** is a specialized, Python-based desktop application designed for mass spectrometry analysis. It streamlines the workflow of visualizing mass spectra, searching for compound identifications via PubChem, and managing a local spectral library.

It bridges the gap between raw `m/z` data and chemical identification, offering real-time structure fetching, adduct calculations, and instrument-specific tolerance settings.

---

## 📸 Screenshots

| **Main Interface** | **Structure View** |
|:---:|:---:|
| ![Main Interface](<img width="1916" height="1023" alt="image" src="https://github.com/user-attachments/assets/616cae72-c82a-4551-a87b-ccad8d9d2e9b" />
) <br> *Interactive plotting and PubChem search* | ![Structure View](<img width="1915" height="1031" alt="image" src="https://github.com/user-attachments/assets/edf75d29-37bb-494d-a3c9-91c175ae1ad4" />
) <br> *Chemical structure preview and local DB* |

## 📸 Screenshots

### Interface & Plotting

| **Main Spectrum View** | **Peak Selection & Zoom** |
|:---:|:---:|
| <img alt="Main Spectrum View" src="<img width="1284" height="523" alt="image" src="https://github.com/user-attachments/assets/0645aad6-4120-408b-8e61-a67edd3768b1" />
" width="100%"> | <img alt="Peak Selection" src="<img width="1284" height="523" alt="image" src="<img width="1059" height="1001" alt="image" src="https://github.com/user-attachments/assets/6a54b06c-24a5-49fa-beb2-355977d05ffd" />
" />
" width="100%"> |
| *Overview of the main window with a plotted spectrum.* | *Zooming in and selecting a specific peak for analysis.* |

### Database & Results

| **PubChem Search Results** | **Local Database Manager** |
|:---:|:---:|
| <img alt="PubChem Results" src="<img width="1910" height="1031" alt="image" src="https://github.com/user-attachments/assets/106aa636-f6ba-473f-b28e-f2517e5cc520" />
" width="100%"> | <img alt="Database Manager" src="<img width="1212" height="572" alt="image" src="https://github.com/user-attachments/assets/a41cf317-8bbf-448c-b56c-a1a9a95d1219" />
" width="100%"> |
| *Viewing search results and corresponding chemical structure.* | *Managing the local SQLite compound library.* |

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
