# CoGeoPQ

Bandwidth-constrained multimodal cooperative perception via geometry-aware probabilistic queries.

CoGeoPQ shares compact object queries so nearby vehicles can contribute their observations under a fixed communication budget. Follow one vehicle through the CooperScene guide, from the two LiDAR views to the fused query and final detection.

![CooperScene guide following the same vehicle. At the selected frame, the ego vehicle records 60 LiDAR returns inside its ground-truth box and vehicle 2 records 2,497. After fusion the reference point is closer to the target center. The full detector also fits the vehicle more closely.](assets/cooperscene-guide.gif)

*Selected example, frame 488101.* Fusion reduces the reference-point error from **0.70 m to 0.29 m** before box regression. The final box has **BEV IoU 0.22 with remote queries disabled and 0.91 with full CoGeoPQ**.

## CooperScene V+2V+I

The full replay follows the same vehicle for 29 consecutive frames at an intersection, with two other vehicles and a roadside unit available to the ego vehicle. The target’s mean BEV IoU over the clip is 0.594 with remote queries disabled and 0.759 with full CoGeoPQ.

https://github.com/user-attachments/assets/a6d5899b-a68e-45d9-8522-2d3ecf69eb2e

Across **2,087 test samples**, the same frozen checkpoint gives **37.81% BEV AP@0.5** with remote queries disabled and **73.66%** with full CoGeoPQ.

## V2X-Real VC

**Full test set (1,253 ego samples).** Using the same checkpoint, enabling remote queries raises BEV mAP@0.5 from 33.32% to 43.55% across the three evaluated classes.

**Selected replay (one vehicle, 41 frames).** At frame 000055, the ego vehicle has 18 LiDAR returns inside the target box, and enabling remote queries raises its BEV IoU from 0.64 to 0.90. Over the full clip, remote queries improve the target’s BEV IoU in some frames and reduce it in others. The mechanism close-ups follow a second vehicle in the same sequence.

https://github.com/user-attachments/assets/162dfb33-3d8b-491d-8349-e4344b18bc57

## How cooperation works

- **Local geometry enrichment.** Nearby LiDAR bird’s-eye-view features add spatial context to the queries.
- **Utility-based query communication.** Objectness and localization uncertainty guide which queries fit the sender’s budget.
- **Geometry-constrained probabilistic fusion.** The receiver associates compatible queries and updates the ego reference position and uncertainty before 3D detection.

The following ablations keep cooperation enabled with a limit of 128 queries per sender. Each ablation changes one component of the full model. For example, query concatenation receives remote queries and passes them to the detection head alongside the ego queries.

| Setting | BEV metric | Without local BEV context | Objectness-only selection | Query concatenation | Full CoGeoPQ |
| --- | --- | ---: | ---: | ---: | ---: |
| V2X-Real VC (1,253 ego samples) | mAP@0.5 | 16.18% | 37.69% | 33.28% | **43.55%** |
| CooperScene V+2V+I (2,087 ego samples) | AP@0.5 | 54.80% | 72.78% | 65.86% | **73.66%** |

## Code release

Code release is planned after paper acceptance.

## Data acknowledgment

- **V2X-Real** by Xiang et al. (ECCV 2024), UCLA Mobility Lab. The replay renders LiDAR observations, annotations and CoGeoPQ predictions from the VC test split. [Dataset project page](https://mobility-lab.seas.ucla.edu/v2x-real/) · [Paper](https://doi.org/10.1007/978-3-031-72943-0_26)
- **CooperScene** by Wu et al. (ECCV 2026), University of California, Riverside. The replay adapts the original LiDAR and annotations into aligned views and overlays CoGeoPQ predictions. [Dataset project page](https://cisl.ucr.edu/CooperScene/) · [Paper](https://doi.org/10.1007/978-3-032-37393-9_7) · [Dataset license](https://github.com/UCR-CISL/CooperScene/blob/main/DATA_LICENSE)
