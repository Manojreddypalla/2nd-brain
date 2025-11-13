# LLM Fine-Tuning Interview Questions

### 1. What is fine-tuning in the context of LLMs?
**Answer:** Fine-tuning is the process of adapting a pre-trained Large Language Model (LLM) for a specific task or domain by continuing its training on a smaller, specialized dataset.

### 2. Why fine-tune an LLM instead of training one from scratch?
**Answer:** Fine-tuning is much more efficient. It requires significantly less data, time, and computational resources because it builds upon the general language understanding that the model has already learned during pre-training.

### 3. When should you choose fine-tuning over prompt engineering or RAG?
**Answer:**
*   Use **prompt engineering** for simple tasks.
*   Use **RAG** when you need to ground the model's responses in external documents or when the information is constantly changing.
*   Use **fine-tuning** when you need to change the model's behavior, style, or teach it new skills.

### 4. What is the difference between full fine-tuning and parameter-efficient fine-tuning (PEFT)?
**Answer:**
*   **Full fine-tuning** updates all the weights of the model. It can achieve the best performance but is very resource-intensive.
*   **PEFT** methods, like LoRA, only update a small subset of the model's parameters. This is much more efficient and reduces the risk of "catastrophic forgetting."

### 5. What is LoRA (Low-Rank Adaptation)?
**Answer:** LoRA is a PEFT technique that freezes the pre-trained model weights and injects trainable rank decomposition matrices (adapters) into the layers of the Transformer architecture. This significantly reduces the number of trainable parameters and makes fine-tuning more accessible.

### 6. What is "catastrophic forgetting" and how can you prevent it?
**Answer:** Catastrophic forgetting is when a model forgets its original knowledge after being fine-tuned on a new task. You can prevent it by:
*   Using PEFT methods like LoRA.
*   Freezing the lower layers of the model and only training the upper layers.
*   Using a smaller learning rate.

### 7. What is Reinforcement Learning from Human Feedback (RLHF)?
**Answer:** RLHF is a technique used to align LLMs with human preferences. It involves three steps:
1.  Collect human feedback on model outputs.
2.  Train a "reward model" on this feedback to predict human preferences.
3.  Use reinforcement learning to fine-tune the LLM to maximize the reward from the reward model.

### 8. How do you evaluate a fine-tuned LLM?
**Answer:**
*   **Standard metrics:** Accuracy, precision, recall, F1-score for classification tasks. ROUGE, BLEU for summarization/translation.
*   **Human evaluation:** Have humans rate the quality of the model's outputs.
*   **Domain-specific metrics:** Create metrics that are specific to your task.

### 9. What are the most important hyperparameters to consider when fine-tuning?
**Answer:**
*   **Learning rate:** The most important hyperparameter. Too high, and the model will forget its pre-trained knowledge. Too low, and it will take too long to train.
*   **Batch size:** The number of examples in each training step.
*   **Number of epochs:** The number of times the model sees the entire dataset.
