# WAM is capable in BEHAVIOR-1K: the 2nd-place solution to the 2026 BEHAVIOR Challenge

**Team IF1001 · 2nd place in the 2026 BEHAVIOR Challenge at CoRL 2026**

[Paper](https://arxiv.org/abs/2611.13818) · [PDF](https://arxiv.org/pdf/2611.13818) · [Official Leaderboard](https://huggingface.co/spaces/behavior-1k/2026-challenge-leaderboard) · [Validation Records](https://huggingface.co/datasets/YelinHe/behavior_2026_self_evaluation_simplify) · [Official Test Recordings](https://huggingface.co/datasets/YelinHe/behavior_2026_official_private_test)

**Authors:** Yelin He, Jiaquan Ye, Xinming Wei, Wenjie Xiao, Gengming Zhang, Guoting Wei, Xianbiao Qi, and Rong Xiao.

This repository accompanies our technical report and provides links to our public evaluation materials. Due to our company's confidentiality requirements, the implementation code and model weights are not publicly available at this time.

## Overview

We present a Cosmos 3-based world action model (WAM) solution for long-horizon household tasks. Team IF1001 ranked **2nd on the official 2026 private-test leaderboard**, achieving a **Q-score of 0.615** and a **full-task success rate of 41.2%**, without using external closed-source models.

Our method combines:

- **History and progress conditioning:** visual history and 4 conditioning tokens produced by stage-task fusion help resolve observational ambiguity.
- **Policy specialization:** task-group LoRA fine-tuning and separate Pick, Place, Close, and Open skill adapters adapt the policy to different tasks and manipulation operations.
- **Progress estimation and routing:** stage estimation supplies progress conditioning, while subtask prediction guides policy routing and provides alternative stage IDs through skill-to-stage lookup for selected tasks.
- **Inference and execution strategies:** action-candidate aggregation, rolling inpainting, temporal resampling, TOPP-RA retiming, and demonstration-derived gripper rules support execution.

See the [technical report](https://arxiv.org/abs/2611.13818) for the architecture, training procedure, evaluation comparisons, and limitations.

## About the challenge

The [2026 BEHAVIOR Challenge](https://behavior.stanford.edu/challenge/index.html), held in conjunction with CoRL 2026, evaluates embodied agents on **100 full-length household tasks across 7 scenes** in BEHAVIOR-1K. These tasks require navigation and bimanual manipulation over extended sequences. The challenge provides **20,000 human demonstrations** with visual observations, robot states and actions, and skill annotations.

Q-score measures partial completion of task goals, while success rate measures full-task completion. The official ranking is based on the mean private-test Q-score. Competition details and results are available on the [challenge website](https://behavior.stanford.edu/challenge/index.html) and [official leaderboard](https://huggingface.co/spaces/behavior-1k/2026-challenge-leaderboard).

## Results

| Evaluation | Tasks | Instances per task | Q-score | Success rate (%) |
|---|---:|---|---:|---:|
| Validation: 3-round mean ± std. | 100 | 10 per round | 0.613 ± 0.008 | 40.9 ± 1.0 |
| Official private test | 100 | 20 | **0.615** | **41.2** |

The validation results report mean ± sample standard deviation across 3 rounds, named `round0`, `round1`, and `round2` in the released records, with tasks weighted equally within each round. Per-task results are reported in Appendix C.1 of the paper.

The validation results evaluate the method presented in the report, which simplifies the deployment configuration used for the official private-test submission. Validation and private-test results use different instances as well as different deployment configurations.

## Public evaluation materials

### Validation records

[Hugging Face: behavior_2026_self_evaluation_simplify](https://huggingface.co/datasets/YelinHe/behavior_2026_self_evaluation_simplify)

This collection provides evaluation metrics and rollout videos for the 3 validation rounds in Appendix C.1. Each round contains 1,000 episodes covering all 100 tasks. Use it to inspect individual task outcomes alongside the aggregate results in the report.

### Official private-test recordings

[Hugging Face: behavior_2026_official_private_test](https://huggingface.co/datasets/YelinHe/behavior_2026_official_private_test)

During the official evaluation, the organizers ran the simulator and queried our inference service over a public IP endpoint. This collection contains service-side observation recordings from those requests.

The official client used **K = 10**: each request returned 10 consecutive actions, which the client executed before sending the next observation. K is not the number of model predictions: requests could be served from an existing action buffer. At the 30 Hz control rate, the recordings contain approximately 3 observation frames per second.

Some calls were interrupted and rerun, so individual tasks may have more than 20 recordings. Video counts should not be interpreted as counts of unique evaluated instances. The collection does not include official instance IDs or per-episode Q-scores and success labels; refer to the [official leaderboard](https://huggingface.co/spaces/behavior-1k/2026-challenge-leaderboard) for the official results.

## Citation

If you find our report or evaluation materials useful, please cite:

```bibtex
@article{he2026wam,
  title         = {{WAM} is capable in {BEHAVIOR-1K}: the 2nd-place solution to the 2026 {BEHAVIOR} Challenge},
  author        = {He, Yelin and Ye, Jiaquan and Wei, Xinming and Xiao, Wenjie and Zhang, Gengming and Wei, Guoting and Qi, Xianbiao and Xiao, Rong},
  journal       = {arXiv preprint arXiv:2611.13818},
  year          = {2026},
  eprint        = {2611.13818},
  archivePrefix = {arXiv},
  primaryClass  = {cs.RO},
  url           = {https://arxiv.org/abs/2611.13818}
}
```

## Contact

For questions about the report or released evaluation materials, please contact:

- **Yelin He:** [heyelin58@gmail.com](mailto:heyelin58@gmail.com)
- **Xinming Wei (corresponding author):** [xinming.wei20@gmail.com](mailto:xinming.wei20@gmail.com)

## Acknowledgments

We thank the BEHAVIOR Challenge organizers for the benchmark and evaluation infrastructure, and prior challenge participants for the foundations on which this work builds.
