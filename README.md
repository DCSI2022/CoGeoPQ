# CoGeoPQ

Bandwidth-constrained multimodal cooperative perception via geometry-aware probabilistic queries.

CoGeoPQ lets vehicles share compact object queries under a bandwidth limit. In the guide below, follow the bracketed vehicle. Both panels use the same ego observation and ground truth. The right panel also receives queries from collaborators.

![An 11-second V2X-Real guide. The same target and ground truth are shown with remote queries disabled and with full CoGeoPQ. The selected frame has BEV IoU 0.64 and 0.90, respectively.](assets/preview.gif)

*Selected V2X-Real frame 000055.* The ego LiDAR records 18 returns inside this vehicle’s ground-truth box. Its BEV IoU rises from 0.64 to 0.90 with remote queries enabled. The [V2X-Real replay](#v2x-real-vc) follows this vehicle across all 41 frames.

## CooperScene V+2V+I

Watch the **same vehicle** from sensor observations through fusion to the final detection. At frame 488101, the ego LiDAR records 60 returns inside the target box, and one collaborating vehicle records 2,497. Fusion reduces the reference-point error from **0.70 m to 0.29 m** before box regression. The final box overlaps the ground truth more closely with remote queries enabled (**BEV IoU 0.22 to 0.91**).

https://github.com/user-attachments/assets/a6d5899b-a68e-45d9-8522-2d3ecf69eb2e

Across **2,087 test samples**, the paired diagnostic shown in the video gives **37.81% BEV AP@0.5** with remote queries disabled and **73.66%** with full CoGeoPQ, using the same model weights.

## V2X-Real VC

The 41-frame replay includes both gains and losses on the bracketed vehicle. Its mean BEV IoU over the clip is **0.805 with remote queries disabled** and **0.776 with full CoGeoPQ**. The mechanism close-ups follow a second vehicle in the same sequence.

https://github.com/user-attachments/assets/162dfb33-3d8b-491d-8349-e4344b18bc57

Across **1,253 test samples**, enabling remote queries at the same checkpoint raises **BEV mAP@0.5 from 33.32% to 43.55%**. This metric covers detections across all three evaluated classes.

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
