# Theoretical Foundation: 3D SLAM for 2D Mapping

Generating 2D maps directly from 3D LiDAR data often leads to suboptimal results. This document outlines why a 3D-first pipeline is necessary for achieving precision and reliability.

## 1. Hardware Characteristics: Non-Repetitive Scanning

The Livox Mid-360 utilizes a non-repetitive scanning pattern that accumulates point density over time [6]. Traditional spinning LiDARs sample a fixed horizontal plane, whereas the Mid-360 covers a 3D volume.

*   **2.D Limitations:** A 2D pipeline discards vertical data, losing critical geometric features like ceilings and floor transitions that are essential for accurate registration.
*   **3.D Advantage:** Accumulating the scanning pattern in a 3D volume provides a richer feature set for scan matching, which significantly reduces odometry drift [2, 3].

## 2. Mitigation of Geometric Degeneracy

SLAM algorithms frequently suffer from degeneracy in monotone environments, such as long corridors, where the lack of features in the direction of travel causes the estimated pose to "slide."

*   **2.D Failure Mode:** If walls are planar and vertical features are sparse, 2D algorithms lack sufficient constraints to stop pose drift along the corridor axis.
*   **3.D Solution:** 3D SLAM utilizes floor and ceiling planes as additional mathematical constraints. The constant distance to these horizontal surfaces provides the necessary stability for 6-DoF estimation [2].

## 3. Compensation for Pitch and Roll

Ground robots rarely operate on a perfectly flat Euclidean plane. Small tilts caused by floor unevenness or acceleration impact sensor orientation.

*   **Systematic Error:** In 2D pipelines, pitch and roll are often misinterpreted as wall distortions, resulting in blurred maps or "ghost" obstacles.
*   **6-DoF Modeling:** 3D SLAM integrated with an IMU explicitly models the robot's orientation [2, 3]. This allows for an orthogonal projection of points onto the 2D plane based on the gravity vector.

## 4. Backend Optimization Fidelity

Backends like GTSAM [5] distribute residual errors across a factor graph. Optimizing loop closures in a 3D space allows the system to absorb non-planar errors that would otherwise violate 2D constraints [1, 4]. The resulting 3D map, when flattened into an Occupancy Grid Map (OGM), exhibits much sharper boundaries due to the full geometric awareness during optimization.

---

## Bibliography

[1] **Sodhi, P., Ho, B. J., & Kaess, M. (2019):** *Online and Consistent Occupancy Grid Mapping for Planning in Unknown Environments*. [cs.cmu.edu](https://www.cs.cmu.edu/~kaess/pub/Sodhi19iros.pdf)

[2] **Liu, Z., et al. (2024):** *Voxel-SLAM: A Complete, Accurate, and Versatile LiDAR-Inertial SLAM System*. [arXiv:2410.08935](https://arxiv.org/html/2410.08935v1)

[3] **Xu, W., et al. (2022):** *FAST-LIO2: Fast Direct LiDAR-Inertial Odometry*. [arXiv:2107.04413](https://arxiv.org/abs/2107.04413)

[4] **Labbé, M., & Michaud, F. (2019):** *RTAB-Map as an Open-Source Lidar and Visual SLAM Library for Large-Scale and Long-Term Online Operation*. [Journal of Field Robotics](https://onlinelibrary.wiley.com/doi/abs/10.1002/rob.21831)

[5] **Dellaert, F. (2012):** *Factor Graphs and GTSAM: A Hands-On Introduction*. [Georgia Institute of Technology](https://borg.cc.gatech.edu/sites/default/files/gtsam.pdf)

[6] **Livox Technology (2022):** *Which LiDAR Scanning Pattern is Better? Repetitive or Non-repetitive*. [livoxtech.com](https://www.livoxtech.com/news/hap_algorithms)
