MuCAN: Multi-Correspondence Aggregation Network for Video Super-Resolution
=====================================

| **Year:** Jul 2020
| **Authors:** Wenbo Li, Xin Tao, Taian Guo, Lu Qi, Jiangbo Lu, Jiaya Jia
| **Affiliations:** The Chinese University of Hong Kong, Kuaishou Tech, Tsinghua University, SmartMore

Video super-resolution (VSR) aims to utilize multiple low-resolution frames to generate a high-resolution prediction for each frame. The authors argues that there are two main limitations for existing VSR methods:

- Flow estimation is error-prone and affects recovery results.
- Similar patterns in natural images are rarely exploited.

In this work, the authors propose the multi-correspondence aggregation network (MuCAN) for VSR. This method achieves SOTA results on multiple benchmark datasets.

In contrast to previous methods that model VSR as separate alignment and regression stages, the authors view this problem as an inter- and intra-frame correspondence aggregation task.

- **Inter-frame correspondence:** Motion estimation may suffer from inevitable errors so the authors resort to simultaneously consider multiple correspondence candidates for each pixel. They propose a **temporal multi-corresopndence aggregation module** (TM-CAM) for alignment.
    .. image:: figures/mucan-2.png
       :width: 420pt

- **Intra-frame correspondence:** Similar pattern within each frame can benefit detail restoration. The authors design a new **cross-scale nonlocal-correspondence aggregation module** (CN-CAM) to exploit multi-scale self-similarity property.
   .. image:: figures/mucan-1.png
      :width: 280pt
