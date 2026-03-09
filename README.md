# Livox 3D SLAM Benchmark 🚀

> 🚧 **WORK IN PROGRESS**
> Currently developing the Ign Gazebo plugin for the Livox Mid-360 non-repetitive scanning pattern.
> Upcoming: Benchmarking 3D SLAM pipelines and their projection to 2D Occupancy Grid Maps (OGM).

## 🎯 Overview

This project aims to evaluate and benchmark different 3D SLAM architectures specifically for robots equipped with **Livox Mid-360** LiDARs. The core hypothesis is that **mapping in 3D and then projecting to 2D** is significantly more robust than direct 2D SLAM, especially when dealing with non-repetitive scanning patterns and geometrically degenerate environments.

## 🧪 Experimental Setup

The benchmark will compare the following three workflows in a simulated environment:

1.  **Standard Hybrid:** `fast-lio2` (Frontend) + `rtabmap` (Backend with SuperPoint + GTSAM).
2.  **Modern Hybrid:** `GLIM` (Odometry Frontend) + `rtabmap` (Backend with SuperPoint + GTSAM).
3.  **Unified State-of-the-Art:** `GLIM` (Pure end-to-end architecture).

## 🛠️ Key Technical Challenges

- **Non-Repetitive Pattern Simulation:** Development of a custom Ignition (Ign) Gazebo plugin to accurately model the "flower" scan pattern of the Livox Mid-360.
- **Metrological Integrity:** Ensuring the simulation reflects the physical FoV asymmetery (+52° to -7°) and the accumulation density over time.

## 📚 Theoretical Foundation

For a detailed explanation of why 3D mapping is mandatory for high-quality 2D results, see [THEORY.md](./THEORY.md).

---
**Narcís Abella** - 2026
