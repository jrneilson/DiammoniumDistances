# DiammoniumDistances

Code and data for computing end-to-end N–N distances of diammonium organic cations from crystal structure CIF files.

## Contents

### `structure_analysis_forPub.ipynb`

Processes a set of CIF crystal structures to extract the end-to-end nitrogen–nitrogen (N–N) distance for diaminoalkane (diammonium) cations. For each structure, the notebook:

1. Reads the CIF file and generates the full unit cell using the Atomic Simulation Environment (ASE).
2. Constructs a molecular bond graph using a 1.6 Å distance cutoff under the minimum image convention, accounting for periodic boundary conditions.
3. Identifies diammonium cations by finding connected components that contain exactly two nitrogen atoms and the expected number of carbon atoms (*C*n).
4. Computes the N–N distance by summing displacement vectors along the shortest bonded path between the two nitrogens, correctly handling cations that span a unit cell boundary.
5. Deduplicates distances from symmetry-equivalent molecules within the unit cell.

Calculations are parallelized across all available CPU cores using `joblib`. The notebook expects CIF files to be organized in folders named `c4_structures/` through `c12_structures/`, where the folder name indicates the number of methylene carbons in the cation chain (*C*n = 4–12).

### `analyzed_NN_exploded.csv`

Results table with one row per unique N–N distance measurement (731 rows/molecules). Columns:

| Column | Description |
|---|---|
| `Cn` | Number of methylene carbons in the diammonium cation alkyl chain |
| `code` | Six-letter CSD refcode of the crystal structure |
| `N-N distance` | End-to-end N–N distance (Å) |

## Dependencies

- [ASE](https://wiki.fysik.dtu.dk/ase/) — crystal structure I/O and distance calculations
- [NetworkX](https://networkx.org/) — molecular graph construction and shortest-path search
- [joblib](https://joblib.readthedocs.io/) — parallel execution
- numpy, pandas
