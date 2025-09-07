# Level 2 Screening Task – QuasiNet / LSM (Influenza B)

## Objective
Provision a **Google Cloud VM** (Ubuntu), set up a Python environment, clone this repo, and complete the following on the provided **Influenza B** sequences:
1. Randomly sample 200 sequences from the FASTA.
2. Parse sequences into a DataFrame.
3. Fit a **QuasiNet** model (use a NumPy array as input).
4. Compute a **distance matrix** from the inferred model.
5. Produce a **Seaborn clustermap**.
6. Email the artifacts (CSV, PNG, script).

This task evaluates cloud literacy, data handling, modeling alignment with Large Sequence Models (LSMs) / QuasiNet, and reproducibility.

---

## VM Setup (Google Cloud · Ubuntu · e2-medium recommended)

1. Go to https://console.cloud.google.com/ → create/choose a project and **enable billing** (use the 90-day free trial if new).
2. In the left menu: **Compute Engine → VM instances → Enable** (first run only).
3. Click **Create Instance**:
   - **Name:** `lsm-test-vm`
   - **Region/Zone:** choose a nearby region
   - **Machine type:** **e2-medium (2 vCPU, 4 GB RAM)** (recommended; the always-free e2-micro is too small)
   - **Boot disk:** Ubuntu **22.04 LTS**, 20–30 GB standard disk
   - **Firewall:** leave default; you’ll use SSH and a local tunnel for Jupyter if needed
4. Click **Create** and then **SSH** (browser-based) to open a shell on the VM.

---

## Environment Setup (once on the VM)

```bash
# refresh packages
sudo apt-get update -y && sudo apt-get upgrade -y

# essentials
sudo apt-get install -y python3 python3-venv python3-pip git

# workspace
mkdir -p ~/lsm-task && cd ~/lsm-task

# clone this repository (replace with your repo URL)
git clone https://github.com/<ORG>/<REPO>.git
cd <REPO>

# create & activate a virtual environment
python3 -m venv .venv
source .venv/bin/activate

# upgrade pip and install deps
pip install --upgrade pip
pip install pandas numpy matplotlib seaborn biopython scikit-learn
# install QuasiNet (as specified by the lab)
pip install quasinet
```

If `quasinet` is hosted privately or under a different name, use the exact pip path you were given (e.g., `pip install git+https://github.com/<org>/quasinet.git`).

---

## Data
Place your FASTA file at:
```
./data/INFB_2018_2019.fasta
```
If your file is named differently, update the paths in the script accordingly.

---

## Example Task Script (sampling 200 sequences)

> Save this as `sample200.py` (or include it in your main script).  
> This **only** demonstrates the sampling step. You still need to complete the parsing → QuasiNet → distance matrix → clustermap steps below.

```python
from Bio import SeqIO
import random

fastapath = './data/INFB_2018_2019.fasta'
outpath = "subset_influenzaB.fasta"

# Load all sequences
records = list(SeqIO.parse(fastapath, "fasta"))

# Pick 200 random entries (without replacement)
subset = random.sample(records, 200)

# Write to a new FASTA
SeqIO.write(subset, outpath, "fasta")

print(f"Selected {len(subset)} sequences and wrote to {outpath}")
```

Optional (reproducibility): set a seed at the top with `random.seed(42)`.

---

## Your Task (what you must implement)

1. **Parse FASTA → DataFrame**
   - Read `subset_influenzaB.fasta`.
   - Create a **pandas DataFrame** with at least:
     - `id` (sequence identifier)
     - `sequence` (uppercase A/C/G/T/N …)
     - any additional fields you extract (e.g., lineage parsed from headers).
   - **Index** the DataFrame by `id`.
   - Represent each sequence so that **each residue position** can be addressed (you may keep a second structure for model input if needed).

2. **Fit a QuasiNet model**
   - Install and import **QuasiNet**.
   - Convert your sequence representation to a **NumPy array** (do **not** feed a pandas DataFrame directly—convert/encode first).
   - Train/fit a **QuasiNet** model appropriate for sequence similarity/latent structure discovery.
   - Briefly document (in code comments) your encoding choice (e.g., one-hot, k-mers, learned embeddings) and any model hyperparameters.

3. **Distance Matrix**
   - Use the **inferred model** to compute a **pairwise distance matrix** across the **200 sampled sequences**.
   - Save to CSV as:
     ```
     outputs/distance_matrix.csv
     ```
   - The matrix must have sequence IDs as both row and column labels, and be symmetric.

4. **Seaborn Clustermap**
   - Produce a **clustermap** from the distance matrix using `seaborn.clustermap`.
   - Save the figure as:
     ```
     outputs/clustermap.png
     ```

5. **Deliverables**
   - Commit and push:
     - your final script (e.g., `analysis.py`)
     - `outputs/distance_matrix.csv`
     - `outputs/clustermap.png`
   - **Email** the CSV, PNG, and the script to **ishanu_ch@uky.edu**.
