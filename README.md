# Brain MRI Segmentation — Otsu vs Sauvola (Baseline Study)

Small, reproducible baseline experiment comparing **global thresholding (Otsu)** vs **adaptive thresholding (Sauvola)** for brain MRI mask segmentation.

The full workflow (loading, preprocessing, segmentation, metrics, and Sauvola hyperparameter search) lives in:
- [Brain_MRI_Segmentation_Otsu_vs_Sauvola.ipynb](Brain_MRI_Segmentation_Otsu_vs_Sauvola.ipynb)

## What this project does
- Loads paired grayscale MRI slices and binary masks
- Resizes images/masks to a fixed square resolution
- Runs two classical segmentation baselines:
	- **Otsu**: single global threshold per image
	- **Sauvola**: local thresholding with tunable window size and $k$
- Evaluates predictions with **Dice** and **Jaccard (IoU)**

## Dataset layout

**Recommended dataset:** [Brain Tumor Segmentation](https://www.kaggle.com/datasets/nikhilroxtomar/brain-tumor-segmentation)

Alternatively, you can use any dataset with the following structure:
```text
data/
  images/
    <name>.png
  masks/
    <name>.png
```

Notes:
- Filenames must match one-to-one between `images/` and `masks/` (same stem).
- Masks are binarized using `mask > 127`.

## Installation
This notebook uses common scientific Python packages:
```bash
pip install numpy pillow scikit-image tqdm
```

## How to run
1. Put your data in the folder structure shown above.
2. Open the notebook: [Brain_MRI_Segmentation_Otsu_vs_Sauvola.ipynb](Brain_MRI_Segmentation_Otsu_vs_Sauvola.ipynb)
3. Set the dataset root to your local `data/` folder (example):
	 - `root_dir="./data"`
4. Run all cells.

## Results (from the current run)
Sauvola hyperparameter search over:
- `window_sizes = [25, 51, 75, 101]`
- `k_values = [0.15, 0.2, 0.25, 0.3]`

Best Sauvola parameters found:
- **Window size = 101**, **k = 0.3**
- **Mean Dice = 0.0537**

Final comparison:

| Method  | Dice | Jaccard (IoU) |
|--------:|:----:|:-------------:|
| Otsu    | 0.0707 | 0.0376 |
| Sauvola | 0.0537 | 0.0281 |

Interpretation (high-level): on this dataset/configuration, **Otsu outperformed Sauvola** for both Dice and IoU, despite Sauvola tuning.

## Reproducibility tips
- Keep preprocessing identical across methods (resize + grayscale conversion).
- If your masks are encoded differently (not 0/255), adjust the binarization threshold.

## 👤 Author

**Soham Umbare**  
IIIT Raichur  
📧 cs23b1068@iiitr.ac.in

---
🧑‍💻 Happy Experimenting! 🔬
