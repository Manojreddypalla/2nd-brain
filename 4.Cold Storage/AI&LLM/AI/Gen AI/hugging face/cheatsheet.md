# Hugging Face Cheatsheet

This cheatsheet provides a quick reference for beginners working with the Hugging Face ecosystem.

## Core Concepts

*   **Hub:** The central place to find and share models, datasets, and spaces.
*   **Transformers:** The core library for working with pre-trained models.
*   **Pipelines:** The easiest way to use a model for a specific task.
*   **Tokenizers:** Used to preprocess text for the model.
*   **Datasets:** The library for accessing and processing datasets.
*   **Accelerate:** Simplifies training on distributed setups (multi-GPU, TPU).

## Getting Started

1.  **Create an Account:** Sign up on the [Hugging Face website](https://huggingface.co/).
2.  **Install the Library:**
    ```bash
    pip install transformers
    pip install datasets accelerate
    ```
3.  **Log in (optional, for uploading models):**
    ```bash
    huggingface-cli login
    ```

## Pipelines: The Quickest Way to Get Started

Pipelines are the easiest way to use a pre-trained model for a specific task. You can use a pipeline for many common tasks, including:

*   `sentiment-analysis`
*   `text-generation`
*   `fill-mask`
*   `question-answering`
*   `summarization`
*   `translation`

**Example: Sentiment Analysis**

```python
from transformers import pipeline

classifier = pipeline("sentiment-analysis")
result = classifier("I love using Hugging Face!")
print(result)
```

## Using Models and Tokenizers Directly

For more control, you can load models and tokenizers directly.

**Example: Text Generation**

```python
from transformers import AutoTokenizer, AutoModelForCausalLM

# Load the tokenizer and model
tokenizer = AutoTokenizer.from_pretrained("gpt2")
model = AutoModelForCausalLM.from_pretrained("gpt2")

# Prepare the input
input_text = "Hello, my name is"
inputs = tokenizer(input_text, return_tensors="pt")

# Generate text
outputs = model.generate(**inputs, max_length=20)
print(tokenizer.decode(outputs[0], skip_special_tokens=True))
```

## Fine-Tuning a Model

Fine-tuning allows you to adapt a pre-trained model to your specific task.

**High-Level Steps:**

1.  **Load a Dataset:** Use the `datasets` library to load a dataset from the Hub or your own files.
2.  **Preprocess the Data:** Use the tokenizer to prepare the dataset for the model.
3.  **Load a Pre-Trained Model:** Load a model with a head for your specific task (e.g., `AutoModelForSequenceClassification`).
4.  **Train the Model:** Use the `Trainer` class to fine-tune the model on your dataset.
5.  **Share Your Model:** Push your fine-tuned model to the Hub to share with the community.

## Important Tips

*   **Start with Pipelines:** They are the easiest way to get started and experiment with different models.
*   **Explore the Hub:** The Hub is your best friend. Spend time browsing models, datasets, and spaces.
*   **Use the Right Model for the Job:** Different models are good at different tasks. Check the model cards for information about a model's architecture and intended use.
*   **Leverage the Community:** The Hugging Face forums and Discord are great places to ask questions and get help.
*   **Don't Be Afraid to Experiment:** The best way to learn is by doing. Try different models, datasets, and hyperparameters to see what works best for your task.
