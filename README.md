# SmolVLA LIBERO Evaluation

A reproducible pilot evaluation of the pretrained **SmolVLA** model on selected tasks from the **LIBERO Spatial** manipulation benchmark.

This project focuses on setting up SmolVLA with LeRobot, running simulated robot episodes, recording task success, and inspecting failed rollout videos.

> This is a **pilot evaluation**, not a full LIBERO benchmark or a claim of general manipulation performance.

## Results Summary

The pretrained `HuggingFaceVLA/smolvla_libero` policy was evaluated for five episodes on each of three LIBERO Spatial tasks.

| Task ID | Task description | Successes | Episodes | Success rate |
|---:|---|---:|---:|---:|
| 0 | Pick up the black bowl between the plate and ramekin and place it on the plate | 3 | 5 | 60% |
| 1 | Pick up the black bowl next to the ramekin and place it on the plate | 4 | 5 | 80% |
| 2 | Pick up the black bowl from the table center and place it on the plate | 5 | 5 | 100% |

**Overall:** 12 successful episodes out of 15, or **80%** across the selected episodes.

These results are from a small pilot. Confidence intervals are wide because only five episodes were evaluated per task.

## Failure Observations

The unsuccessful episodes are included in the repository:

- **Task 0, episodes 1 and 2:** the robot approached the bowl but remained slightly misaligned. Its grasp attempt missed the bowl, and it could not complete pickup or placement.
- **Task 1, episode 4:** the robot targeted the bowl imprecisely and attempted to grasp near the center, but missed the bowl.

These observations describe the reviewed videos only. They should not be interpreted as proof that every failure has the same cause.

## Repository Structure

```text
SmolVLA-LIBERO-Evaluation/
├── notebooks/
│   └── 01_baseline_evaluation.ipynb
└── Videos/
    ├── spatial_task_00/
    │   ├── eval_info.json
    │   ├── task_descriptions.json
    │   └── videos/libero_spatial_0/
    ├── spatial_task_01/
    │   ├── eval_info.json
    │   ├── task_descriptions.json
    │   └── videos/libero_spatial_1/
    └── spatial_task_02/
        ├── eval_info.json
        ├── task_descriptions.json
        └── videos/libero_spatial_2/
```

- [`eval_info.json`](Videos/spatial_task_00/eval_info.json) contains the episode reward, success, and timing information.
- [`task_descriptions.json`](Videos/spatial_task_00/task_descriptions.json) contains the natural-language task instructions.
- MP4 files contain the recorded rollout videos.
- [`01_baseline_evaluation.ipynb`](notebooks/01_baseline_evaluation.ipynb) contains the Colab setup and evaluation commands.

## Evaluation Setup

- **Platform:** Google Colab
- **GPU:** NVIDIA T4
- **Model:** `HuggingFaceVLA/smolvla_libero`
- **Framework:** Hugging Face LeRobot
- **Benchmark:** LIBERO Spatial
- **MuJoCo rendering backend:** EGL
- **Episodes per task:** 5
- **Evaluation batch size:** 1
- **Fine-tuning:** None

## Running the Evaluation

Open [`01_baseline_evaluation.ipynb`](notebooks/01_baseline_evaluation.ipynb) in Google Colab and run its cells in order.

The notebook installs LeRobot and the SmolVLA/LIBERO extras:

```python
!git clone https://github.com/huggingface/lerobot.git
%cd lerobot
!pip install -e ".[smolvla,libero]"
```

It configures MuJoCo for headless Colab execution:

```python
import os
os.environ["MUJOCO_GL"] = "egl"
```

It evaluates task IDs 0, 1, and 2:

```python
policy = "HuggingFaceVLA/smolvla_libero"
task_ids = [0, 1, 2]
episodes_per_task = 5
output_root = "/content/drive/MyDrive/smolvla_libero_pilot"
```

An individual evaluation command can be run as follows:

```bash
lerobot-eval \
  --policy.path=HuggingFaceVLA/smolvla_libero \
  --env.type=libero \
  --env.task=libero_spatial \
  --env.task_ids='[0]' \
  --eval.batch_size=1 \
  --eval.n_episodes=5 \
  --output_dir=/content/drive/MyDrive/smolvla_libero_pilot/spatial_task_00
```

Mount Google Drive before evaluating if you want to preserve checkpoints, logs, and videos after the Colab session ends.

## Limitations

- Only three selected LIBERO Spatial tasks were evaluated.
- Five episodes per task provide limited statistical confidence.
- The evaluation used a pretrained model without fine-tuning.
- Episode initialization and runtime differences may affect individual rollouts.
- The project does not measure performance on every LIBERO task, in real-world settings, or across other VLA models.
- Failure classifications are based on manual review of a small number of videos.

## Next Steps

1. Increase episodes per task to at least 10.
2. Evaluate additional LIBERO Spatial tasks.
3. Record repeated runs with fixed seeds where supported.
4. Add a structured failure-analysis table.
5. Compare SmolVLA with a baseline or fine-tuned policy.
6. Move from pure evaluation toward one controlled research question.

## References

- [LeRobot](https://github.com/huggingface/lerobot)
- [SmolVLA](https://huggingface.co/HuggingFaceVLA/smolvla_libero)
- [LIBERO](https://libero-project.github.io/)
