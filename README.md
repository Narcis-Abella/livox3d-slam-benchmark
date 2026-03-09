# Livox 3D SLAM Benchmark

> **WORK IN PROGRESS**
> Development of the Ign Gazebo plugin for the Livox Mid-360 non-repetitive scanning pattern is currently underway. Benchmarking of 3D SLAM pipelines and their projection to 2D Occupancy Grid Maps (OGM) will follow.

## Overview

This project evaluates different 3D SLAM architectures for robots equipped with Livox Mid-360 LiDARs. The core hypothesis is that mapping in 3D and subsequently projecting to 2D provides better robustness than direct 2D SLAM. This is particularly relevant when handling non-repetitive scanning patterns and environments prone to geometric degeneracy.

## Experimental Setup

The benchmark compares three distinct workflows in a simulated environment:

1.  **Standard Hybrid:** fast-lio2 as the frontend coupled with rtabmap (SuperPoint + GTSAM) as the backend.
2.  **Modern Hybrid:** GLIM for odometry frontend with rtabmap (SuperPoint + GTSAM) for the backend.
3.  **Unified Architecture:** Pure GLIM implementation.

## Technical Challenges

*   **Non-Repetitive Pattern Simulation:** A custom Ignition (Ign) Gazebo plugin is required to accurately model the "flower" scan pattern of the Livox Mid-360.
*   **Metrological Integrity:** The simulation must reflect the physical FoV asymmetry (+52° to -7°) and the point accumulation density over time.

## Theoretical Foundation

A detailed technical justification for why 3D mapping is necessary for high-quality 2D results can be found in [THEORY.md](./THEORY.md).

---
**Narcís Abella** - 2026
