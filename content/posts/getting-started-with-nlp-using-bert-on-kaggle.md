---
title: "Getting started with NLP using Bert on Kaggle"
date: 2025-01-29
---

## 1、Import and EDA

```
import os
iskaggle = os.environ.get('KAGGLE_KERNEL_RUN_TYPE', '')
from pathlib import Path
if iskaggle:
    path = Path('/kaggle/input/us-patent-phrase-to-phrase-matching')
```

```
import pandas as pd
df = pd.read_csv(path/'train.csv')
df['input'] = 'TEXT1: ' + df.context + '; TEXT2: ' + df.target + '; ANC1: ' + df.anchor
df.input.head()
```

## 2、Tokenization

```
from datasets import Dataset, DatasetDict
ds = Dataset.from_pandas(df)
import warnings,logging,torch
warnings.simplefilter('ignore')
logging.disable(logging.WARNING)
model_nm = 'anferico/bert-for-patents'
# Load model directly
from transformers import AutoModelForSequenceClassification, AutoTokenizer
model = AutoModelForSequenceClassification.from_pretrained(model_nm, num_labels=1)
tokenizer = AutoTokenizer.from_pretrained('anferico/bert-for-patents')
```

```
def tok_func(x):
    return tokenizer(x['input'])
# Tokenize all the sentences using the tokenizer
tok_ds = ds.map(tok_func, batched=True)
tok_ds = tok_ds.rename_columns({'score':'labels'})
```

## 3、Test and Validation sets

```
eval_df = pd.read_csv(path/'test.csv')
dds = tok_ds.train_test_split(0.25, seed=42)
eval_df['input'] = 'TEXT1: ' + eval_df.context + '; TEXT2: ' + eval_df.target + '; ANC1: ' + eval_df.anchor
eval_ds = Dataset.from_pandas(eval_df).map(tok_func, batched=True)
```

## 4、Metrics and Correlation

```
import numpy as np
def corr(x,y): 
    ## change the 2-d array into 1-d array
    return np.corrcoef(x.flatten(), y)[0,1]
def corr_d(eval_pred): return {'pearson': corr(*eval_pred)}
```

## 5、Training our model

```
from transformers import TrainingArguments,Trainer
bs = 128
epochs = 4
lr = 8e-5
args = TrainingArguments('outputs', learning_rate=lr, warmup_ratio=0.1, lr_scheduler_type='cosine', fp16=True,
    evaluation_strategy="epoch", per_device_train_batch_size=bs, per_device_eval_batch_size=bs*2,
    num_train_epochs=epochs, weight_decay=0.01, report_to='none')
trainer = Trainer(model, args, train_dataset=dds['train'], eval_dataset=dds['test'],
                  tokenizer=tokenizer, compute_metrics=corr_d)
trainer.train()
```

![Image description](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fials6lciimf07ahsxw39.png)

## 6、Get the predictions on the test set

```
preds = trainer.predict(eval_ds).predictions.astype(float)
preds = np.clip(preds, 0, 1)
import datasets

submission = datasets.Dataset.from_dict({
    'id': eval_ds['id'],
    'score': preds
})

submission.to_csv('submission.csv', index=False)
```

Go to Source
