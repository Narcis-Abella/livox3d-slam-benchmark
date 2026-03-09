# Theoretical Foundation: 3D SLAM for 2D Mapping

Generating 2D maps directly from 3D LiDAR data often leads to suboptimal results. This document outlines why a 3D-first pipeline is necessary for achieving precision and reliability, particularly when utilizing non-repetitive scanning sensors.

## 1. Hardware Characteristics: Non-Repetitive Scanning

The Livox Mid-360 utilizes a non-repetitive scanning pattern that accumulates point density over time [6]. Traditional spinning LiDARs sample a fixed horizontal plane, whereas the Mid-360 covers a 3D volume. Comparative studies of sensor variability emphasize that this density accumulation is critical for robust feature extraction [12].

*   **2D Limitations:** A 2D pipeline discards vertical data, losing critical geometric features like ceilings and floor transitions that are essential for accurate registration.
*   **3D Advantage:** Accumulating the scanning pattern in a 3D volume provides a richer feature set for scan matching, which significantly reduces odometry drift [2, 3, 7].

## 2. Mitigation of Geometric Degeneracy

SLAM algorithms frequently suffer from degeneracy in monotone environments, such as long corridors, where the lack of features in the direction of travel causes the estimated pose to "slide." 3D-aware pipelines actively detect and mitigate these ill-conditioned scenarios [9, 10].

*   **2D Failure Mode:** If walls are planar and vertical features are sparse, 2D algorithms lack sufficient constraints to stop pose drift along the corridor axis.
*   **3D Solution:** 3D SLAM utilizes floor and ceiling planes as additional mathematical constraints. The constant distance to these horizontal surfaces provides the necessary stability for 6-DoF estimation [2].

## 3. Compensation for Pitch and Roll

Ground robots rarely operate on a perfectly flat Euclidean plane. Small tilts caused by floor unevenness or acceleration impact sensor orientation [3].

*   **Systematic Error:** In 2D pipelines, pitch and roll are misinterpreted as wall distortions, resulting in blurred maps or ghost obstacles.
*   **6-DoF Modeling:** 3D SLAM integrated with an IMU explicitly models the robot's orientation [2, 3, 14]. This allows for an orthogonal projection of points onto the 2D plane based on the gravity vector.

## 4. Backend Optimization Fidelity

Backends distribute residual errors across a factor graph [5]. Optimizing loop closures in a 3D space allows the system to absorb non-planar errors that would otherwise violate 2D constraints [1, 4]. The resulting 3D map, when flattened into an Occupancy Grid Map (OGM), exhibits sharper boundaries due to the full geometric awareness during global optimization [11].

## 5. Computational Efficiency and Scaling

While 3D SLAM requires more resources, modern architectures address this through submap-based pose graph optimization and asynchronous backends [13, 14]. This allows for real-time operation while maintaining the precision of a full 3D pipeline.

---

## Bibliography

[1] **Sodhi, P., Ho, B. J., & Kaess, M. (2019):** *Online and Consistent Occupancy Grid Mapping for Planning in Unknown Environments*. [cs.cmu.edu](https://www.cs.cmu.edu/~kaess/pub/Sodhi19iros.pdf)

[2] **Liu, Z., et al. (2024):** *Voxel-SLAM: A Complete, Accurate, and Versatile LiDAR-Inertial SLAM System*. [arXiv:2410.08935](https://arxiv.org/abs/2410.08935v1)

[3] **Xu, W., et al. (2022):** *FAST-LIO2: Fast Direct LiDAR-Inertial Odometry*. [arXiv:2107.04413](https://arxiv.org/abs/2107.04413)

[4] **Labbé, M., & Michaud, F. (2019):** *RTAB-Map as an Open-Source Lidar and Visual SLAM Library for Large-Scale and Long-Term Online Operation*. [Journal of Field Robotics](https://onlinelibrary.wiley.com/doi/abs/10.1002/rob.21831)

[5] **Dellaert, F. (2012):** *Factor Graphs and GTSAM: A Hands-On Introduction*. [Georgia Institute of Technology](https://borg.cc.gatech.edu/sites/default/files/gtsam.pdf)

[6] **Livox Technology (2022):** *Which LiDAR Scanning Pattern is Better? Repetitive or Non-repetitive*. [livoxtech.com](https://www.livoxtech.com/news/hap_algorithms)

[7] **Koide, K., Yokozuka, M., Oishi, S., & Banno, A. (2024):** *GLIM: 3D range-inertial localization and mapping with GPU-accelerated scan matching factors*. [ScienceDirect](https://doi.org/10.1016/j.robot.2024.104750)

[8] **DeTone, D., Malisiewicz, T., & Rabinovich, A. (2018):** *SuperPoint: Self-Supervised Interest Point Detection and Description*. [arXiv:1712.07629](https://arxiv.org/abs/1712.07629)

[9] **Chandna, N., & Kaushal, A. (2025):** *DAMM-LOAM: Degeneracy Aware Multi-Metric LiDAR Odometry and Mapping*. [arXiv:2510.13287](https://arxiv.org/abs/2510.13287v1)

[10] **Li, Y., et al. (2025):** *Anti-Degeneracy Scheme for Lidar SLAM based on Particle Filter in Geometry Feature-Less Environments*. [arXiv:2502.11486](https://arxiv.org/abs/2502.11486v2)

[11] **"Transformation & Translation OGM" (2026):** *Experimental Validation of 3D-to-2D Mapping Pipelines for Increased Precision*. [ScienceDirect](https://www.sciencedirect.com/science/article/abs/pii/S0921889026000783)

[12] **Doumegna, M. K. F., et al. (2025):** *Lidar Variability: A Novel Dataset and Comparative Study of Solid-State and Spinning Lidars*. [arXiv:2507.04321](https://arxiv.org/abs/2507.04321v1)

[13] **Technical Report (TUM):** *SLAM2REF: Architecture for Submaps and Asynchronous Backend*. [mediatum.ub.tum.de](https://mediatum.ub.tum.de/doc/1769952/document.pdf)

[14] **Shan, T., & Englot, B. (2020):** *LIO-SAM: Tightly-coupled Lidar Inertial Odometry via Smoothing and Mapping*. [arXiv:2007.00258](https://arxiv.org/abs/2007.00258)
