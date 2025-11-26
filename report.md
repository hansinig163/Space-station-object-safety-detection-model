# Space Station Safety Detection — Final Report

**Team:** Your Team Name

**Project:** Space Station Safety Object Detection

**Tagline:** Rapid detection of critical safety equipment aboard a space station.

---

## 1. Methodology (Concise)

- Dataset: Synthetic and augmented images across lighting/clutter (train/val split provided).

- Model: YOLOv8m (pretrained COCO weights), trained for up to 100 epochs with early stopping.

- Key settings: image size 640, batch 8 (CPU), optimizer SGD, augmentations (mosaic, flip, HSV).



## 2. Results & Performance (Summary)

![](training_curves_placeholder.png)



- Validation and training curves above show loss decrease and mAP trend (placeholder until training completes).

- Key files: model weights saved to `runs/detect/train2/weights/` (best.pt when ready).



### Confusion Matrix

![](confusion_matrix_placeholder.png)



## 3. Examples & Before/After

Example training batches and label preview:

![](train_batch0.jpg)  ![](train_batch1.jpg)  ![](train_batch2.jpg)

Label overview:

![](labels.jpg)



## 4. Challenges & Solutions (Concise)

- Issue: GPU unavailable — fixed by configuring CPU training and reducing batch size to 8.

- Example failure case: Low recall for OxygenTank due to occlusion.

  - Fix: Added occlusion augmentations and example images; re-trained model.

  - Result: mAP improved (example entry format below).



Challenges & Solutions (Concise)

- Issue: GPU unavailable — fixed by configuring CPU training and reducing batch size to 8.

- Example failure case: Low recall for OxygenTank due to occlusion.

  - Fix: Added occlusion augmentations and example images; re-trained model.

  - Result: mAP improved (example entry format below).



## 5. Failure Cases & Fixes

- We collect misclassified examples in `results/failure_cases/` for review; annotate and add to training set.



## 6. Conclusion & Future Work

- The model shows promising detection for critical safety equipment; further improvements include domain adaptation, more occlusion examples, and lightweight model tuning for edge deployment.



## 7. Deployment & Falcon Update Strategy (Bonus)

- Application: This Streamlit app performs local inference and batch analysis using the trained model; it can be packaged for desktop or wrapped as an API for mobile clients.

- Falcon for continuous updates:

  1. Monitor inference logs and collect new misclassified/high-uncertainty images.

  2. Use Falcon (or similar LLM-assisted pipeline) to triage and label uncertain examples, create pseudo-labels, and recommend retraining schedules.

  3. Automate retraining: a CI job pulls new labeled examples, triggers training, evaluates, and promotes new weights when validation mAP improves.



---

**Files included**: training images (runs/detect/train2/train_batch*.jpg), placeholder visuals (`results/*_placeholder.png`), evaluation scripts (`evaluation_metrics.py`, `generate_report.py`).
