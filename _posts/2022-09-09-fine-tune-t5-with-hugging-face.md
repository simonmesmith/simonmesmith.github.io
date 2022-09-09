---
layout: single
title: Fine-tune T5 with Hugging Face (as of September 9, 2022)
tags: machine-learning hugging-face transformers t5
---

[Hugging Face](https://huggingface.co/) is a great resource for streamlining the use of machine learning in applications. It can be challenging, however, to know what documentation and examples are the most up-to-date. 

Take the case of fine-tuning a [T5](https://ai.googleblog.com/2020/02/exploring-transfer-learning-with-t5.html) model. If you search online for "fine tune a T5 model with Hugging Face" you'll get thousands of results. Many of these are outdated, referring to older versions of the Hugging Face API, which has rapidly evolved. But it's hard to know which results are outdated.

If you're in the same boat, you can hopefully [save some trouble with this Gist](https://gist.github.com/simonmesmith/0334cef17d06d23ca5fa50c78a956d57). This should be accurate as of September 9, 2022. Alternatively, here are the steps I used:

# 1. Install dependencies

```console
pip install datasets pandas transformers
```

# 2. Import libraries and modules

```python
from datasets import Dataset, DatasetDict
import pandas as pd
from transformers import T5Tokenizer, T5ForConditionalGeneration, DataCollatorForSeq2Seq, Seq2SeqTrainingArguments, Seq2SeqTrainer
```

# 3. Set model, tokenizer, and data_collator variables

```python
tokenizer = T5Tokenizer.from_pretrained("t5-base")
model = T5ForConditionalGeneration.from_pretrained("t5-base")
data_collator = DataCollatorForSeq2Seq(tokenizer=tokenizer, model=model)
```

# 4. Get data and divide into train, eval, and test sets

Note: Replace the dataframe with your own, but make sure it has "source_text" and "target_text" columns or you'll need to modify other code below.

```python
df = pd.DataFrame({"source_text": [], "target_text": []}) 
train_df = df.sample(frac = 0.8)
eval_df = df.drop(train_df.index).sample(frac = 0.5)
test_df = df.drop(train_df.index).drop(eval_df.index)
```

# 5. Create a dataset dict from the dataframes

```python
dataset = DatasetDict({
    "train": Dataset.from_pandas(train_df),
    "eval": Dataset.from_pandas(eval_df),
    "test": Dataset.from_pandas(test_df),
    })
```

# 6. Tokenize the dataset

Note: Change the max_length to whatever makes the most sense for your data.

```python
def tokenize(source_texts, target_texts):
    model_inputs = tokenizer(source_texts, max_length=512, truncation=True)
    with tokenizer.as_target_tokenizer():
        labels = tokenizer(target_texts, max_length=512, truncation=True)
    model_inputs["labels"] = labels["input_ids"]
    return model_inputs
tokenized_dataset = dataset.map(tokenize, input_columns=["source_text", "target_text"], remove_columns=["source_text", "target_text"])
```

# 7. Set training arguments

Note: Change "output_directory" to where you want, and update other parameters as makes sense. [Here's the documentation for this](https://huggingface.co/docs/transformers/v4.21.3/en/main_classes/trainer#transformers.TrainingArguments).

```python
training_arguments = Seq2SeqTrainingArguments(
    "output_directory",
    learning_rate=0.0001,
    weight_decay=0.01,
    fp16=True,
    per_device_train_batch_size=4,
    per_device_eval_batch_size=4,
    gradient_accumulation_steps=2,
    num_train_epochs=20,    
    evaluation_strategy="epoch",
    report_to="all"
)
```

# 8. Create a trainer

```python
trainer = Seq2SeqTrainer(
    model,
    training_arguments,
    train_dataset=tokenized_dataset["train"],
    eval_dataset=tokenized_dataset["eval"],
    data_collator=data_collator,
    tokenizer=tokenizer
)
```

# 9. Train the model

```python
trainer.train()
```

# 10. Save the tokenizer and model

Note: Udate the "output_directory" to wherever you want to save everything.

```python
tokenizer.save_pretrained("output_directory")
model.save_pretrained("output_directory")
```

And that's it! Again, [all the code is in this Gist](https://gist.github.com/simonmesmith/0334cef17d06d23ca5fa50c78a956d57). And if you have any issues, you should probably check to make sure nothing has changed with Hugging Face's API since I wrote this.