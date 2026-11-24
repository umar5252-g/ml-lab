# Random Forest Experiments: Results and Discussion

## Breiman’s main ideas
Breiman’s 2001 paper presents Random Forests as collections of randomized decision trees. Bootstrap samples give trees different training examples, while randomly selecting features at each split helps keep trees less correlated. Strong trees make useful predictions, and lower correlation means their errors are less likely to repeat together. Combining their votes can reduce variance and improve generalization compared with relying on one tree. Breiman also describes out-of-bag examples as an internal way to estimate error.

## Framework and preprocessing
- Heart Disease: the Cleveland subset has patient measurements and a diagnosis label. We used the local CSV, removed missing rows, converted diagnosis to binary, and used a stratified 80/20 split.
- Intel Images: six outdoor-scene classes. We resized RGB images to 24 × 24, scaled pixel values to 0–1, sampled up to 150 images per class, and used a stratified 80/20 split. Flattening pixels keeps the exercise simple, but it does not capture image structure as well as a deep-learning method.
- Models: Decision Tree, Random Forest with scikit-learn defaults, and RBF SVM with standardized inputs. Accuracy uses held-out test data; time is model fitting time.

## Heart Disease UCI: model comparison

| Model | Test accuracy | Training time (seconds) |
| --- | --- | --- |
| Decision Tree | 0.7833 | 0.0122 |
| Random Forest (default) | 0.8833 | 0.1859 |
| SVM (RBF) | 0.9000 | 0.0109 |

The highest test accuracy was from **SVM (RBF)** (0.900). The Random Forest scored 0.883 and took 0.186 seconds to fit; the Decision Tree scored 0.783 in 0.012 seconds, and the SVM scored 0.900 in 0.011 seconds. Compared with one tree, the forest improved accuracy by 0.100 but took longer to fit. The SVM was more accurate than the forest on this split.

## Intel Image Classification: model comparison

| Model | Test accuracy | Training time (seconds) |
| --- | --- | --- |
| Decision Tree | 0.3944 | 1.0259 |
| Random Forest (default) | 0.5333 | 1.6985 |
| SVM (RBF) | 0.5389 | 0.2666 |

The highest test accuracy was from **SVM (RBF)** (0.539). The Random Forest scored 0.533 and took 1.698 seconds to fit; the Decision Tree scored 0.394 in 1.026 seconds, and the SVM scored 0.539 in 0.267 seconds. Compared with one tree, the forest improved accuracy by 0.139 but took longer to fit. The SVM was more accurate than the forest on this split.

## Heart Disease UCI: number of trees

| n_estimators | Test accuracy | Training time (seconds) |
| --- | --- | --- |
| 1 | 0.8167 | 0.0078 |
| 10 | 0.8333 | 0.0266 |
| 50 | 0.8500 | 0.0938 |
| 100 | 0.8833 | 0.1747 |
| 300 | 0.8667 | 0.5301 |

Accuracy was 0.817 with 1 tree, 0.867 with 300 trees, and highest (0.883) at 100 trees in this test. The 300-tree fit took 0.530 seconds. In general, accuracy improved as the forest grew and then leveled off or fluctuated slightly. A finite test set can make accuracy fluctuate, so not every step is expected to improve.

## Intel Image Classification: number of trees

| n_estimators | Test accuracy | Training time (seconds) |
| --- | --- | --- |
| 1 | 0.3167 | 0.0185 |
| 10 | 0.4944 | 0.1670 |
| 50 | 0.5222 | 0.8341 |
| 100 | 0.5333 | 1.7260 |
| 300 | 0.5278 | 4.9586 |

Accuracy was 0.317 with 1 tree, 0.528 with 300 trees, and highest (0.533) at 100 trees in this test. The 300-tree fit took 4.959 seconds. In general, accuracy improved as the forest grew and then leveled off or fluctuated slightly. A finite test set can make accuracy fluctuate, so not every step is expected to improve.

## Stability across random seeds

| Dataset | Accuracies across seeds | Mean accuracy | Standard deviation |
| --- | --- | --- | --- |
| Heart Disease | [0.85, 0.85, 0.85] | 0.8500 | 0.0000 |
| Intel Images | [0.5111111111111111, 0.5111111111111111, 0.5277777777777778] | 0.5167 | 0.0079 |

The standard deviation summarizes how much the three forest test scores varied across random seeds; a smaller value indicates more consistent scores in this check. Results suggest that the larger ensemble is less sensitive to its random choices, although this is a small illustrative check, not a formal statistical test.