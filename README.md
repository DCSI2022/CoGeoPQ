# CoGeoPQ

Bandwidth-constrained multimodal cooperative perception via geometry-aware probabilistic queries.

The guide follows one V2X-Real vehicle. Watch the bracketed target in both panels. The left panel disables remote queries and the right panel uses full CoGeoPQ.

![An 11-second V2X-Real guide. The same target and ground truth are shown with remote queries disabled and with full CoGeoPQ. The selected frame has BEV IoU 0.64 and 0.90, respectively.](assets/preview.gif)

## V2X-Real VC

One ego vehicle and three collaborators observe the same sequence. The paired replay compares predictions from the same frozen checkpoint, ego queries and ground truth. A second vehicle from the sequence illustrates the three stages of cooperation.

<!-- Insert the GitHub-generated v2x-real-inline.mp4 attachment URL here on a line by itself. -->

## CooperScene V+2V+I

Two other vehicles and a roadside unit collaborate with the ego vehicle at an intersection. The video follows a target through consecutive ego frames and pauses on one paired example. A recorded fusion trace then shows the reference position before and after fusion, separately from the regressed detection box.

<!-- Insert the GitHub-generated cooperscene-inline.mp4 attachment URL here on a line by itself. -->

## How cooperation works

- **Local geometry enrichment.** Nearby LiDAR bird’s-eye-view features add spatial context to the queries.
- **Utility-based query communication.** Objectness and localization uncertainty guide which queries fit the sender’s budget.
- **Geometry-constrained probabilistic fusion.** The receiver associates compatible queries and updates the ego reference position and uncertainty before 3D detection.

The full test-set ablations below use a 128-query limit per sender. Each column compares a separate evaluation condition with the full model.

| Setting | BEV metric | Without local BEV context | Objectness-only selection | Query concatenation | Full CoGeoPQ |
| --- | --- | ---: | ---: | ---: | ---: |
| V2X-Real VC (1,253 ego samples) | mAP@0.5 | 16.18% | 37.69% | 33.28% | **43.55%** |
| CooperScene V+2V+I (2,087 ego samples) | AP@0.5 | 54.80% | 72.78% | 65.86% | **73.66%** |

## Code release

Code release is planned after paper acceptance.

## Data acknowledgment

- **V2X-Real** by Xiang et al. (ECCV 2024), UCLA Mobility Lab. The replay renders LiDAR observations, annotations and CoGeoPQ predictions from the VC test split. [Dataset project page](https://mobility-lab.seas.ucla.edu/v2x-real/) · [Paper](https://doi.org/10.1007/978-3-031-72943-0_26)
- **CooperScene** by Wu et al. (ECCV 2026), University of California, Riverside. The replay adapts the original LiDAR and annotations into aligned views and overlays CoGeoPQ predictions. [Dataset project page](https://cisl.ucr.edu/CooperScene/) · [Paper](https://doi.org/10.1007/978-3-032-37393-9_7) · [Dataset license](https://github.com/UCR-CISL/CooperScene/blob/main/DATA_LICENSE)
