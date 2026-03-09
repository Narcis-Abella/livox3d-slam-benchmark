# Theoretical Foundation: 3D SLAM for 2D Mapping

Generating 2D maps directly from 3D LiDAR data often leads to suboptimal results. This document outlines why a 3D-first pipeline is necessary for achieving precision and reliability.

## 1. Hardware Characteristics: Non-Repetitive Scanning

The Livox Mid-360 utilizes a non-repetitive scanning pattern that accumulates point density over time. Traditional spinning LiDARs sample a fixed horizontal plane, whereas the Mid-360 covers a 3D volume.

*   **2.D Limitations:** A 2D pipeline discards vertical data, losing critical geometric features like ceilings and floor transitions that are essential for accurate registration.
*   **3.D Advantage:** Accumulating the scanning pattern in a 3D volume provides a richer feature set for scan matching, which significantly reduces odometry drift.

## 2. Mitigation of Geometric Degeneracy

SLAM algorithms frequently suffer from degeneracy in monotone environments, such as long corridors, where the lack of features in the direction of travel causes the estimated pose to "slide."

*   **2.D Failure Mode:** If walls are planar and vertical features are sparse, 2D algorithms lack sufficient constraints to stop pose drift along the corridor axis.
*   **3.D Solution:** 3D SLAM utilizes floor and ceiling planes as additional mathematical constraints. The constant distance to these horizontal surfaces provides the necessary stability for 6-DoF estimation.

## 3. Compensation for Pitch and Roll

Ground robots rarely operate on a perfectly flat Euclidean plane. Small tilts caused by floor unevenness or acceleration impact sensor orientation.

*   **Systematic Error:** In 2D pipelines, pitch and roll are often misinterpreted as wall distortions, resulting in blurred maps or "ghost" obstacles.
*   **6-DoF Modeling:** 3D SLAM integrated with an IMU explicitly models the robot's orientation. This allows for an orthogonal projection of points onto the 2D plane based on the gravity vector.

## 4. Backend Optimization Fidelity

Backends like GTSAM distribute residual errors across a factor graph. Optimizing loop closures in a 3D space allows the system to absorb non-planar errors that would otherwise violate 2D constraints. The resulting 3D map, when flattened into an Occupancy Grid Map (OGM), exhibits much sharper boundaries due to the full geometric awareness during optimization.

---

### References

*   **Sodhi et al. (2019):** *Online and Consistent Occupancy Grid Mapping*. [cs.cmu.edu](https://www.cs.cmu.edu/~kaess/pub/Sodhi19iros.pdf)
*   **Liu et al. (2024):** *Voxel-SLAM: Accurate and Versatile LiDAR-Inertial SLAM*. [arXiv:2410.08935](https://arxiv.org/html/2410.08935v1)
*   **Xu et al. (2022):** *FAST-LIO2: Fast Direct LiDAR-Inertial Odometry*. [arXiv:2107.04413](https://arxiv.org/abs/2107.04413)
*   **Labbé & Michaud (2019):** *RTAB-Map as an Open-Source Lidar and Visual SLAM Library for Large-Scale and Long-Term Online Operation*. [Wiley Online Library](https://onlinelibrary.wiley.com/doi/abs/10.1002/rob.21831)
*   **Dellaert (2012):** *Factor Graphs and GTSAM: A Hands-On Introduction*. [Georgia Tech](https://borg.cc.gatech.edu/sites/default/files/gtsam.pdf)
*   **Livox HAP/Mid-360 Algorithms:** *Advancements in Non-Repetitive Scanning*. [livoxtech.com](https://www.livoxtech.com/news/hap_algorithms)
