# SmolVLA Pilot Evaluation on LIBERO Spatial

## Goal

Evaluate a pretrained Vision-Language-Action (VLA) policy on a small
selection of simulated robot tasks and record its successes and failures.

## Setup

- **Policy:** `HuggingFaceVLA/smolvla_libero`
- **Environment:** LIBERO Spatial
- **Tasks:** IDs 0, 1, and 2
- **Episodes:** 5 per task
- **Training or fine-tuning:** None; this was evaluation of a pretrained policy

## Results

| Task ID | Instruction | Successes / Episodes | Success rate | Reported 95% interval |
|---:|---|---:|---:|---:|
| 0 | Pick up the black bowl between the plate and ramekin and place it on the plate | 3/5 | 60% | 23.1%–88.2% |
| 1 | Pick up the black bowl next to the ramekin and place it on the plate | 4/5 | 80% | 37.6%–96.4% |
| 2 | Pick up the black bowl from the table center and place it on the plate | 5/5 | 100% | 56.6%–100% |

Across these selected episodes, the policy succeeded in **12 of 15 attempts (80%)**.

## Review of unsuccessful episodes

| Task ID | Episode | Observed behavior | Failure category |
|---:|---:|---|---|
| 0 | 1 | The robot moved near the bowl but stopped slightly too far away. It then attempted to grip, but did not pick up the bowl, so it could not complete the placement. | Approach/alignment; grasp/contact |
| 0 | 2 | The robot again approached the bowl but was slightly misaligned. Its grasp attempt missed, and the bowl was not picked up. | Approach/alignment; grasp/contact |
| 1 | 4 | The robot had a positioning problem and attempted to grasp near the center of the bowl, but missed the bowl. | Grasp/contact |

In the failed episodes reviewed, the visible problem occurred before successful pickup: the robot approached or targeted the bowl imprecisely and failed to establish a grasp. This is an observation from these three videos, not proof that all failures have the same cause.

## Interpretation

The policy completed all five evaluated episodes for task 2, while it had one or more failures on tasks 0 and 1. The reviewed failures suggest that precise positioning and grasping may be challenging in these particular episodes. More episodes and a broader video review would be needed to determine whether this pattern is consistent.

## Limitations

- Only three selected LIBERO Spatial tasks were evaluated.
- Each task had only five episodes, so the success-rate estimates are uncertain.
- The reported confidence intervals are wide.
- This was an evaluation of a pretrained policy; no training or fine-tuning was performed.
- The observations describe only the three unsuccessful videos reviewed.
- These results should not be interpreted as performance across all LIBERO tasks or real-world robots.

## Next steps

1. Evaluate more episodes for these tasks if compute time permits.
2. Keep a per-episode record of success and observed failure stage.
3. Review any additional failures using the same categories.
4. Record the exact software versions and evaluation settings so the experiment is reproducible.