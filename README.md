# MALDI-PeakHunter

Version: 1.0
Author: Rohith Krishna
Dependencies: tkinter, numpy, matplotlib, sqlite3, requests, Pillow (PIL), csv, json, threading

Overview
MALDI PeakHunter is a desktop GUI application for analyzing MALDI (Matrix-Assisted Laser Desorption/Ionization) mass spectrometry data. It enables researchers to:

Load and visualize mass spectra interactively.

Search compound masses in PubChem using their monoisotopic mass.

Filter search results by chemical class (e.g., lipids, peptides, metabolites).

Preview compound structures retrieved from PubChem.

Maintain a local SQLite database for custom compound libraries.

Export search results to .csv.

The program combines scientific tools like NumPy and Matplotlib with an intuitive Tkinter GUI, making it ideal for both research labs and educational environments.

Key Features
Interactive stem plot of spectrum peaks.

Automated PubChem compound mass search (with retry and timeout handling).

Chemical class keyword filters.

Automatic image retrieval and display of compound structures.

Local SQLite storage and retrieval.

Custom search tolerances and adduct adjustments.

Multi-threaded networking to keep the UI responsive.

Application Architecture
The main code defines a single class:

class MassSpecApp
The core of the application, managing both logic and GUI layout.

Initialization (__init__)

Sets up the main Tkinter window and all interface components:

Left panel: settings, filters, and controls.

Right panel: matplotlib-based interactive plot, results table, and structure viewer.

Status bar for progress updates.

Initializes SQLite database and network session.

Creates numpy arrays for spectrum data and handles threading-safe GUI updates.

Function Reference
Each function is documented below for developers extending or maintaining the code.

Database Handling
init_db()
Initializes the SQLite database my_ms_library.db.
Creates a table compounds if it doesn’t exist, containing columns:

Column	Type	Description
id	INTEGER	Primary key
name	TEXT	Compound name
formula	TEXT	Formula string
precursor_mz	REAL	Observed precursor m/z
collision_energy	TEXT	Collision energy text
peaks_json	TEXT	Serialized JSON of peaks
add_to_db_window()
Spawns a small modal window allowing users to add a custom compound to the local library.
Entries include name, formula, collision energy, and peaks. Data are stored in the SQLite table.

search_local_db()
Searches the local database for matching compounds within the user-defined tolerance around the selected precursor m/z.
Displays results in the right-side treeview.

Spectrum Management
parse_input_spectrum()
Parses user-provided text input containing m/z and intensity pairs, separated by commas, spaces, or tabs.
Returns a list of tuples sorted by descending intensity.

plot_spectrum()
Generates an interactive stem plot of m/z peaks against intensities using Matplotlib.
Automatically labels the top 20 visible peaks and refreshes on zoom/pan operations.

update_visible_labels(ax)
Updates visible m/z labels dynamically whenever the zoom level changes.

on_peak_click(event)
Triggered when the user clicks a specific data point on the plot.
Automatically initiates a PubChem search for the selected peak’s m/z.

PubChem Search Logic
setup_network_session()
Configures the requests.Session() with retry policies using urllib3.Retry.
Prevents crashes from flaky network conditions.

start_search_thread(override_mz=None)
Spawns a background thread for PubChem search operations, preventing UI freezing.

search_pubchem(override_mz, search_id)
Main PubChem search logic:

Calculates the neutral mass based on the selected adduct.

Defines the search range (based on tolerance).

Queries PubChem’s REST API for CIDs within that mass range.

Passes CIDs to fetch_filter_rank() for detail retrieval and ranking.

fetch_filter_rank(cids, theoretical_neutral, input_precursor, search_id)
Given a CID list, retrieves molecular properties (title, formula, monoisotopic mass).
Applies keyword-based class filters and ranks compounds by mass error (ppm).

update_tree_gui(results, input_precursor)
Populates result table with:

Rank

Source (PubChem or local DB)

Compound name

Formula

Observed m/z

Computed error

Class match label

Hidden CID column (used for structure fetching)

get_tolerance_range(mass)
Calculates the allowable window around the neutral mass based on tolerance settings (in Da or ppm).

Structure Retrieval
on_tree_select(event)
Triggered when a user selects a row in the Treeview.
If a PubChem compound is selected, begins asynchronous structure image fetching.

fetch_structure_image(cid)
Downloads a PNG image of the selected compound structure from PubChem.
Updates the structure preview frame with the compound image.

GUI Management and Helpers
safe_gui_call(callback, *args)
Executes a given callback safely within the Tkinter event loop (thread-safe).

safe_msg(title, msg, icon="info")
Displays a Tkinter message box in a thread-safe way with selection of icons (info/warning/error).

update_status(msg)
Updates the bottom status bar text through the GUI thread safely.

export_csv()
Exports the current results table to a .csv file.
Includes headers and all compound data (both local and PubChem).

apply_instrument_preset(event=None)
Sets the tolerance based on the selected instrument preset (from INSTRUMENTS dictionary).

on_close()
Gracefully closes the application, ensuring all threads and the database connection are terminated.

Configuration Reference
Constant	Description
PUBCHEM_URL	Base REST endpoint for PubChem monoisotopic mass search
STRUCTURE_URL	Endpoint for retrieving structure PNG images by CID
ADDUCTS	Common MS adducts and their mass adjustments
INSTRUMENTS	Instrument-specific mass tolerance presets
CLASS_KEYWORDS	Keyword sets for automatic class filtering of compounds
Dependencies
Make sure the following Python modules are installed:

bash
pip install numpy matplotlib pillow requests
Optional (for packaging or advanced handling):

bash
pip install pyinstaller
Running the Application
From the command line:

bash
python maldi_peakhunter.py
The main interface will open, allowing you to paste spectrum data and begin analysis.

File Structure
text
maldi_peakhunter.py
my_ms_library.db          # Automatically created SQLite database
data/
  └─ exported_results.csv # Example output (if exported)
Example Input
text
411.22046, 10000
611.09, 50000
760.585, 25000
Example Workflow
Launch the program.

Paste spectrum peaks into the input field.

Choose your instrument and adduct.

Press Generate Plot to view the spectrum.

Click on a peak to trigger an online PubChem mass search.

Review candidate results in the table and see structural images.

Optionally export results as .csv.

Contributing
When contributing:

Ensure your code follows PEP8 style guidelines.

Use descriptive docstrings for new methods.

Test network and database operations for race conditions.

Document any added compound classification keywords.

License
This project is for academic and research use only.
Uses the PubChem PUG REST API under their public data terms.
