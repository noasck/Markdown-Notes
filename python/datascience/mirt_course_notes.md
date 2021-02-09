# Classification metrics

### Working with binary classification

- Accurracy - <img src="https://render.githubusercontent.com/render/math?math=\frac{1}{n} \displaystyle\sum_{i=1}^{n} [ y^t_i = y_i^p]">
- Balanced accuracy - mean of accuracy above each class.
- Precision = <img src="https://render.githubusercontent.com/render/math?math=\dfrac{\text{TP}}{TP plus FP}">
- Recall = <img src="https://render.githubusercontent.com/render/math?math=\dfrac{\text{TP}}{TP plus FN}">
- F-score = **min (precision, recall)** - not differentiable.
- F1-score = harmonic mean.
- F-<img src="https://render.githubusercontent.com/render/math?math=\beta "> - weighted F1.
- ROC
- ROC-AUC
- Precision-Recall Curve

### Working with multiclass classification

![Multiclass metrics](assets/mirt_course_notes-7cd15bcf.png)

- Confusion matrix