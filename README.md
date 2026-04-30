# DUSt3R-Based Multi-View BEV Generation with LiDAR-Guided Optimization

This project studies bird's-eye-view (BEV) generation from multi-view driving images.  
The main goal is to test whether DUSt3R pointmaps can be directly projected into BEV maps, and whether sparse LiDAR guidance can further improve their metric alignment.

The project uses one selected frame from the Waymo Open Dataset. A small example frame is included in this repository, so the DUSt3R baseline can be run without downloading the full Waymo segment.

---

## Project Overview

The project has two main parts.

### 1. DUSt3R-Based BEV Baseline

I first use DUSt3R to reconstruct dense 3D pointmaps from selected Waymo camera images.  
The pointmaps are then directly projected into a BEV plane to generate:

- occupancy map
- density map
- height map
- RGB splat map

Two baseline settings are tested:

- **3-view baseline**: front-left, front, front-right
- **5-view baseline**: side-left, front-left, front, front-right, side-right

I also test confidence threshold filtering to study how DUSt3R confidence affects BEV quality.

### 2. LiDAR-Guided Optimization

The DUSt3R coordinate system is not naturally metric and is not aligned with the Waymo vehicle frame.  
To improve this, I use sparse Waymo LiDAR data as geometric guidance.

The optimization includes:

- global scale correction
- Bundle Adjustment, including: Sim(3)-based bundle adjustment, local residual correction

The optimized result is projected into the vehicle-frame BEV coordinate system.

## Data

A small selected Waymo frame is included in:

`data/selected_frame/frame_000030/`

It contains five camera images and `meta.json`. This allows `dust3r.ipynb` to run without downloading the full Waymo segment.

The full LiDAR-guided optimization notebook requires additional Waymo LiDAR, calibration, pose, and projection parquet files. These files are not included because they are large.

## How to Run

1. `data_input.ipynb`: used to inspect Waymo data and export the selected frame. This is optional if using the included example frame.
2. `dust3r.ipynb`: runs DUSt3R reconstruction, BEV baseline generation, and confidence threshold ablation.
3. `lidar_guided_optimization.ipynb`: runs LiDAR-guided scale correction, Sim(3) alignment, local residual correction, and final vehicle-frame BEV generation.
