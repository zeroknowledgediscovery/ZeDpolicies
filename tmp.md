# Level 0 Screening Task – Basic Data Handling & Visualization

## Objective
This entry-level task tests whether you can:
1. Handle biological sequence data in Python.
2. Do basic tabular manipulations.
3. Produce a simple visualization.

---

## Task

1. **Clone this repository** locally.

2. Work with the provided file:
   ```
   ./data/INFB_2018_2019.fasta
   ```

3. Write a Python script (`basic_analysis.py`) that:
   - Reads the FASTA file with Biopython.
   - Extracts sequence identifiers and sequence lengths.
   - Builds a pandas DataFrame with:
     - `id`
     - `length`
   - Computes summary statistics: mean, median, max, and min sequence length.
   - Produces a histogram of sequence lengths (`length_hist.png`).

4. **Deliverables**:
   - Script `basic_analysis.py`
   - Output CSV (`sequence_lengths.csv`)
   - Histogram figure (`length_hist.png`)

---

## Expected Skills Tested
- Can you install Python dependencies (`biopython`, `pandas`, `matplotlib`)?
- Do you know how to parse a FASTA file?
- Can you manipulate tabular data?
- Can you generate and save a simple plot?

---

## Notes
- Keep your script clean and commented.
- Save outputs to an `outputs/` folder for clarity.
- Use only standard Python + common scientific libraries.
