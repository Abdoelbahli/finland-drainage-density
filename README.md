# Finland Drainage Density


## CPU-Accelerated KDE Evaluation with Dask Distributed

This project demonstrates how to speed up the evaluation of a Gaussian kernel density estimate (KDE) over a spatial grid by fully utilizing your machine’s CPU cores through parallel processing using Dask Distributed. The code partitions a geographic grid—derived from a Finland shapefile—into manageable chunks, evaluates the KDE concurrently on multiple CPU workers, and then reassembles the results into a final density map. The resulting visualization overlays the density map with the Finland boundaries, a full-spectrum color gradient, a scale bar (with units), and a north arrow.

## Features

- **Parallel CPU Processing:**  
  Utilizes Dask Distributed to split the grid evaluation among multiple CPU workers for faster processing.
  
- **Efficient Grid Evaluation:**  
  Generates a grid over the study area (Finland) using reprojected bounds (EPSG:3067) and a specified grid resolution.
  
- **Robust KDE Evaluation:**  
  Uses SciPy’s `gaussian_kde` (which expects data in shape `(d, n)`) to compute density values from the input stream of spatial points.
  
- **Enhanced Visualization:**  
  The final density map is visualized with:
  - A full-color spectrum (using a colormap spanning the entire range of colors),
  - An overlay of Finland’s boundaries,
  - A scale bar with explicit units (e.g. "100 km"), and
  - A north arrow (with customizable color).

## Requirements

- Python 3.7 or higher
- [NumPy](https://numpy.org/)
- [SciPy](https://scipy.org/)
- [Matplotlib](https://matplotlib.org/)
- [GeoPandas](https://geopandas.org/) (for handling shapefiles)
- [Dask Distributed](https://docs.dask.org/en/stable/distributed.html) (install via `pip install "dask[distributed]"`)
- [tqdm](https://github.com/tqdm/tqdm) (optional, for progress visualization)

## Installation

1. **Clone the Repository:**

   ```bash
   git clone https://github.com/Abdoelbahli/finland-drainage-density.git
   cd finland-drainage-density
   ```

2. **Install Dependencies:**

   Using pip:
   ```bash
   python -m pip install numpy scipy matplotlib geopandas "dask[distributed]" tqdm
   ```

## Usage

This project is structured as a Jupyter Notebook with cells that execute the following steps:

1. **Grid Generation:**  
   The notebook reads the total bounds from a reprojected Finland shapefile (EPSG:3067) and creates a grid based on a user-defined resolution (e.g. 4891 meters). This results in a manageable number of grid points (e.g. ~135 × 233).

2. **KDE Initialization:**  
   The notebook creates a CPU-based KDE object using SciPy’s `gaussian_kde`. The input spatial data (`stream_points`) is expected to have shape `(n_points, 2)`, and the KDE is initialized with data transposed to shape `(2, n_points)`.

3. **Partitioning & Parallel Processing:**  
   The grid is partitioned into row-wise chunks. Each chunk is processed in parallel on multiple CPU workers using Dask Distributed. The processing function evaluates the KDE over the grid chunk, and the tasks are scheduled concurrently.

4. **Reassembly and Visualization:**  
   The density values computed for each chunk are reassembled into the full grid. The final density map is then displayed using Matplotlib, with:
   - A colormap covering the full color spectrum,
   - Finland’s shapefile boundaries overlaid,
   - A scale bar with units (e.g. "100 km"),
   - A north arrow.

To run the notebook, open it in Jupyter Notebook or JupyterLab:

```bash
jupyter notebook drainage.ipynb
```

Then, execute the cells in order.

## Customization

- **Grid Resolution:**  
  Modify `grid_res` to adjust the spatial resolution of your grid.
  
- **Chunk Size:**  
  Adjust `chunk_size` to balance memory usage and parallel efficiency.
  
- **Visualization:**  
  You can change the colormap (e.g., switching to `nipy_spectral` for a full color spectrum), adjust the scale bar length, and modify the position or style of the north arrow.

## Troubleshooting

- **Memory Errors:**  
  Verify that your grid resolution and study area bounds are correct. Use GeoPandas to inspect `finland.total_bounds` and adjust `grid_res` accordingly.
  
- **Dask Issues:**  
  If tasks are not distributing correctly, ensure that your Dask cluster is properly configured (e.g., adjust the number of workers).

## Contributing

Contributions are welcome! Please open an issue or submit a pull request for any improvements or bug fixes.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

