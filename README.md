# Attention Over Reservoir States

Code accompanying the MSc dissertation *Attention Over Reservoir States: Exploiting Temporal Dynamics in Echo State Networks for Musical Instrument Classification*.

The repository contains the five notebooks used for reservoir selection, the primary experiments, the matched non-recurrent control, dissertation figures and tables, and the post-hoc checks.

## Repository structure

Files tracked on GitHub include the notebooks and retained result artefacts from the definitive dissertation experiments:

```text
.
├── notebooks/
│   ├── 01_ESN_Reservoir_Selection.ipynb
│   ├── 02_ESN_Main_Experiments.ipynb
│   ├── 03_Nonrecurrent_Control.ipynb
│   ├── 04_Dissertation_Figures.ipynb
│   └── 05_Post_Hoc_Checks.ipynb
├── results_sweep_final/
├── results_final/
├── results_nonrecurrent_control/
├── results_robustness/
├── state_regime_diagnostics.json
├── .gitignore
├── README.md
└── requirements.txt
```

The NSynth dataset and large intermediate caches are kept locally and are ignored by Git:

```
data/
cache_final/
```

The retained result directories and diagnostic JSON files are tracked so that the numerical outputs underlying the dissertation can be inspected without regenerating the large intermediate state caches.


Notebook 04 regenerates the state-regime diagnostics reported in Tables F.1 to F.3 and F.5 using the fixed 500-recording training sample and writes them to `state_regime_diagnostics.json`. It also computes `conditioning_diagnostics.json` in the dissertation figures output directory, which is the source of Table F.4. These diagnostics support Appendix F and do not alter the primary experimental pipeline.

## Data

Download the official NSynth dataset and place the three splits at:

```
data/
└── nsynth/
    ├── nsynth-train/
    │   ├── examples.json
    │   └── audio/
    ├── nsynth-valid/
    │   ├── examples.json
    │   └── audio/
    └── nsynth-test/
        ├── examples.json
        └── audio/
```

The notebooks exclude `synth_lead`, whose 5,501 recordings all fall in the training partition, leaving no validation or test examples. The resulting ten-class task covers 300,478 recordings, split 283,704 for training, 12,678 for validation and 4,096 for test.

NSynth is released by Magenta under CC BY 4.0.

## Installation

Python 3.11.15 was used for the dissertation experiments. A CUDA-capable GPU is strongly recommended for Notebook 02.

```bash
conda create -n esn-nsynth python=3.11.15
conda activate esn-nsynth
pip install -r requirements.txt
jupyter lab
```

The dependency versions used for the final environment are pinned in `requirements.txt`.

## Run order

Run the notebooks in order from fresh kernels:

1. `01_ESN_Reservoir_Selection.ipynb`
2. `02_ESN_Main_Experiments.ipynb`
3. `03_Nonrecurrent_Control.ipynb`
4. `04_Dissertation_Figures.ipynb`
5. `05_Post_Hoc_Checks.ipynb`

Start Jupyter inside the repository. Each notebook finds the project root by searching upwards for `data/nsynth/`, so no machine-specific absolute paths are required. All five notebooks expect that folder to be present because project-root detection uses it, even where the later notebooks primarily consume retained caches and results.

Notebook 02 writes `results_final/latest_run.json`, which Notebooks 03, 04 and 05 read to locate the run directory.

Notebooks 04 and 05 also read the caches written by 01 to 03. The retained reservoir-state cache alone is 75.1 GB (Table D.6), on top of the NSynth audio and the acoustic feature cache.

The saved notebook outputs are retained as a record of the definitive dissertation run. Cache and run identifiers are hashes of the configuration, so re-running the notebooks unchanged resolves to the same identifiers, reuses any validated caches and updates the corresponding result artefacts under the same result paths. Changing the configuration produces new identifiers and, where applicable, new result locations.

## Dissertation signposts

| Notebook | Dissertation link |
|---|---|
| 01 - Reservoir selection | Methodology 3.1.3, Results 4.1.1, Appendix D |
| 02 - Main experiments | Methodology 3.2 to 3.4, Results 4.2 to 4.4, Appendix E |
| 03 - Non-recurrent control | Methodology 3.2, Results 4.2 |
| 04 - Figures and tables | Chapter 4 figures and tables, Appendix F, Appendix G.4 |
| 05 - Post-hoc checks | Results 4.2.4, 4.3.5 and 4.4.6, Appendix I |

The primary RQ1 to RQ3 experimental results are produced by Notebooks 01 to 03. Notebook 04 reports those retained outputs and regenerates the supporting Appendix F diagnostics. Notebook 05 contains the post-hoc checks added during final review and does not alter the primary model-selection procedure.

## Experimental environment

The definitive dissertation experiments were run on the following system. The exact Python package versions required by the notebooks are pinned in `requirements.txt`.

| Component | Experimental environment |
|---|---|
| Operating system | Pop!_OS 24.04 LTS, x86_64 |
| Linux kernel | 6.18.7-76061807-generic |
| CPU | AMD Ryzen 7 7800X3D, 8 physical cores / 16 logical cores |
| System memory | 46.19 GiB |
| GPU | NVIDIA GeForce RTX 4090 |
| GPU memory | 23,028 MiB reported by `nvidia-smi`; 21.99 GiB visible to PyTorch |
| NVIDIA driver | 580.126.18 |
| Driver-supported CUDA version | 13.0 |
| Local CUDA toolkit (`nvcc`) | 12.0 |
| Python | CPython 3.11.15, conda-forge |
| PyTorch | 2.11.0+cu130 |
| PyTorch CUDA build | 13.0 |
| cuDNN | 9.19.0 |
| ReservoirPy | 0.4.1 |
| NumPy | 2.3.5 |
| SciPy | 1.15.3 |
| scikit-learn | 1.8.0 |
| joblib | 1.5.3 |
| librosa | 0.11.0 |
| pandas | 3.0.1 |
| matplotlib | 3.10.8 |
| JupyterLab | 4.5.6 |


The installed `nvcc` toolkit was CUDA 12.0, while the PyTorch 2.11.0 build used CUDA 13.0 (`+cu130`). These are separate components. The notebooks use the CUDA runtime supplied through the PyTorch installation.

## Estimated Runtimes

| Notebook | Estimated runtime |
|---|---:|
| 01_ESN_Reservoir_Selection | ~10 hr |
| 02_ESN_Main_Experiments | ~25 hr |
| 03_Nonrecurrent_Control | ~6 hr |
| 04_Dissertation_Figures | ~15 min |
| 05_Post_Hoc_Checks | ~4 hr |
