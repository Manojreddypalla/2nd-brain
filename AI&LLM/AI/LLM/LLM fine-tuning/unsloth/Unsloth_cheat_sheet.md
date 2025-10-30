# Unsloth AI Cheat Sheet

## 1. What is Unsloth AI?

Unsloth AI is an open-source Python framework for fast and memory-efficient fine-tuning of Large Language Models (LLMs). It's built on top of Hugging Face Transformers and offers significant speed and memory improvements.

*   **Performance:** 2x faster training, 70% less VRAM usage.
*   **Supported Models:** Llama, Mistral, Gemma, Qwen, DeepSeek, and more.
*   **Training Types:** Full-finetuning, pretraining, and quantized training (4-bit, 8-bit, 16-bit).

## 2. Why Use Unsloth?

*   **Speed:** Up to 30x faster on multiple GPUs compared to Flash Attention 2.
*   **Memory Efficiency:** Enables fine-tuning on consumer-grade hardware.
*   **Accessibility:** Works on Google Colab and Kaggle.
*   **Simplified API:** Streamlines the fine-tuning process.
*   **Inference Integration:** Integrates with Ollama, llama.cpp, and vLLM.

## 3. Installation

```bash
pip install unsloth
```
*Requires PyTorch and is compatible with Python 3.13 or lower.*

## 4. Core Fine-tuning Workflow

1.  **Load Model & Tokenizer:**
    Use `unsloth.FastLanguageModel.from_pretrained` to load a 4-bit quantized model.

    '''python
    from unsloth import FastLanguageModel
    model, tokenizer = FastLanguageModel.from_pretrained(
        model_name = "unsloth/llama-3-8b-bnb-4bit",
        max_seq_length = 2048,
        load_in_4bit = True,
    )
    '''

2.  **Load & Process Dataset:**
    Prepare your dataset in an instruction or conversation format.

3.  **Setup Model for LoRA:**
    Configure LoRA adapters for efficient fine-tuning using `FastLanguageModel.get_peft_model`.

4.  **Train the Model:**
    Use Hugging Face's `SFTTrainer` and `TrainingArguments`.

5.  **Save & Export:**
    Merge LoRA adapters and save the model to the Hugging Face Hub or as a GGUF file for local inference.

    '''python
    # Save to Hub
    model.push_to_hub("your-repo/your-model", token = "...")
    tokenizer.push_to_hub("your-repo/your-model", token = "...")

    # Save as GGUF
    model.save_pretrained_gguf("model.gguf", tokenizer, quantization_method = "q4_k_m")
    '''

## 5. Key Concepts

*   **QLoRA (Quantized Low Rank Adaptation):** Quantizes the base model and trains only a small set of additional parameters.
*   **GGUF:** A file format optimized for CPU inference of LLMs.

## 6. Use Cases

*   **Supervised Fine-tuning (SFT):** Creating LoRA adapters for completion or chat models.
*   **Reinforcement Learning:** Implementing various RL algorithms.
*   **Model Conversion:** Easily convert models to GGUF, vLLM-compatible, or push to the Hub.
