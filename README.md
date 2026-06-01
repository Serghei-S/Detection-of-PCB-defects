# Detection of PCB Defects

Computer vision project for detecting defects on printed circuit boards using the Kaggle dataset [`akhatova/pcb-defects`](https://www.kaggle.com/datasets/akhatova/pcb-defects).

The final notebook recomputes the full pipeline from scratch on Kaggle **Tesla T4**: dataset loading, EDA, preprocessing, YOLO/Faster R-CNN training, compact-model experiments, metrics, plots, correct detections, errors, and final conclusions.

## Final Notebook

[`pcb_defects_recompute_from_scratch_READY.ipynb`](pcb_defects_recompute_from_scratch_READY.ipynb)

## Dataset Classes

| Class | Meaning |
|---|---|
| `missing_hole` | Required hole is missing. |
| `mouse_bite` | Small bite-like damage on a trace or copper area. |
| `open_circuit` | Broken trace, so the electrical path is interrupted. |
| `short` | Unwanted connection between traces or copper regions. |
| `spur` | Extra copper protrusion on a trace. |
| `spurious_copper` | Unwanted copper fragment on the board. |

## Model Comparison

| Model | Accuracy | Macro-F1 | mAP50 | mAP50-95 | Speed, ms/img | Notes |
|---|---:|---:|---:|---:|---:|---|
| FasterRCNN-optimized | 0.9856 | 0.9856 | 0.9197 | 0.4684 | 73.3 | Best overall quality |
| FasterRCNN-ResNet50-FPN | 0.9784 | 0.9785 | 0.9078 | 0.4266 | 114.0 | Strong baseline |
| YOLOv8s_960_balanced | 0.9640 | 0.9810 | 0.9307 | 0.4809 | 78.8 | Best YOLO quality |
| YOLOv8n_960_efficient | 0.8345 | 0.8737 | 0.7358 | 0.3648 | 66.2 | Best compact model, about 6 MB |
| YOLO11n_960_efficient | 0.7914 | 0.8251 | 0.6781 | 0.3334 | 64.5 | Compact YOLO11 |
| YOLOv8n_640_fast | 0.6763 | 0.7636 | 0.5388 | 0.2534 | 55.9 | Fast nano baseline |
| YOLO11n_640_fast | 0.5252 | 0.5811 | 0.4466 | 0.2098 | 57.0 | Fast YOLO11 baseline |

## Visual Results

### PCB examples with annotations

![PCB examples](assets/pcb_examples.png)

### Final model comparison

![Model comparison](assets/model_comparison.png)

### Example errors

![Prediction errors](assets/prediction_errors.png)

## Main Conclusions

- **Best quality model:** `FasterRCNN-optimized`.
- **Best compact model:** `YOLOv8n_960_efficient`.
- **Best YOLO by quality:** `YOLOv8s_960_balanced`.
- Increasing image size from `640` to `960` helped YOLO models because PCB defects are small.
- Most errors happen between visually similar copper/trace defects such as `mouse_bite`, `open_circuit`, `spur`, and `spurious_copper`.

## Run on Kaggle

```powershell
kaggle kernels push -p . --accelerator NvidiaTeslaT4
```
