# Pilot results

**Model:** `HuggingFaceVLA/smolvla_libero`  
**Environment:** LIBERO Spatial  
**Episodes per task:** 5  
**Evaluation type:** Pretrained-model evaluation; no fine-tuning

| Task ID | Instruction | Successes / Episodes | Success rate | Reported 95% interval |
|---:|---|---:|---:|---:|
| 0 | Pick up the black bowl between the plate and ramekin and place it on the plate | 3/5 | 60% | 23.1%–88.2% |
| 1 | Pick up the black bowl next to the ramekin and place it on the plate | 4/5 | 80% | 37.6%–96.4% |
| 2 | Pick up the black bowl from the table center and place it on the plate | 5/5 | 100% | 56.6%–100% |

Overall for these selected episodes: **12/15 successes (80%)**.

This is a small pilot across three selected tasks. It is not a full LIBERO benchmark result, and the intervals are wide because each task has only five episodes.