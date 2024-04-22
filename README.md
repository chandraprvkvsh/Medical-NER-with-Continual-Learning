# Medical NER with Continual Learning

## Overview

In this exploration of Medical Named Entity Recognition (NER) using continual learning techniques, we delve into training on three distinct datasets (G1, G2, and G3) sourced from Meta Clinical Trials. Each dataset focuses on different entities within its respective domain (T1, T2, and T3). Employing the BERT-Base model with 110M parameters, integration of Elastic Weight Consolidation (EWC) addresses catastrophic forgetting.

## Approach

- **Continual Learning**: The model learns sequentially, tackling tasks T1, T2, and T3 one after the other.
- **Model Capacity Management**: We maintain a fixed memory size throughout the training process.
- **Evaluation**: Performance metrics are reported based on test sets reserved for each task, assessing knowledge retention, forward transfer, and backward transfer.
- **Mitigating Catastrophic Forgetting**: Elastic Weight Consolidation (EWC) is employed to ensure retention of knowledge from previous tasks.

## Model Details

- **Model**: BERT-Base (110M parameters)
- **Model Size**: <500MB
- **GPU Used**: P100 
- **Training Time**: Approximately an hour

## Elastic Weight Consolidation (EWC)

Elastic Weight Consolidation is a technique aimed at enabling continual learning while mitigating catastrophic forgetting. It assigns importance to parameters learned during previous tasks and penalizes large deviations from these parameters while learning new tasks. This ensures that important parameters for previously learned tasks are preserved while adapting to new tasks.

## Training Steps

1. **Preprocessing**: Organizing datasets into a dataframe with sentences and labels.
2. **Tokenization**: Ensuring no labeled entities are lost during tokenization.
3. **Custom Functions**: Developed for preprocessing, dataset creation, and training.
4. **Fine-tuning**: Full-parameter fine-tuning of the Bert-Base model for Tasks T1, T2, T3 in a sequential manner while preserving knowledge from previous tasks.
5. **Fisher Information Computation**: Calculated importance of each model parameter, focusing on regularization of those parameters based on their importances with EWC.
6. **Validation**: Conducted on validation sets of previous tasks alongside the current task.

## Performance Metrics

| Entity           | T1              | T1 and T2       | T1, T2, and T3  | G1+G2+G3 Combined |
|------------------|-----------------|-----------------|-----------------|-------------------|
| allergy_name     | 0.738386        | 0.824524        | 0.901130        | 0.797745          |
| cancer           | 0.726179        | 0.793356        | 0.833593        | 0.742930          |
| chronic_disease  | 0.779630        | 0.805484        | 0.854898        | 0.783958          |
| treatment        | 0.777369        | 0.840783        | 0.881324        | 0.801621          |
| micro avg        | 0.769029        | 0.820854        | 0.865570        | 0.786875          |
| macro avg        | 0.755391        | 0.816037        | 0.867736        | 0.781564          |
| weighted avg     | 0.768511        | 0.820849        | 0.865853        | 0.786826          |

## Conclusion

Our approach outperforms models trained on all data simultaneously. By addressing continual learning challenges in NLP, we aim to contribute to the advancement of adaptable models in medical text analysis. Also, We can get better performance with a bigger model and more training. Like even using bert-large and training it for more number of epochs with early stopping could do the trick. This is just for demonstration of approach, bert-base is the smallest popular transformer model and we trained for just 10 epochs.
