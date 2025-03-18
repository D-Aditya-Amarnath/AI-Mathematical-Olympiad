# An AI Agent for Solving Math Olympiad Problems

MathOlympiadAgent is an AI assistant built with vLLM and PyTorch. It is designed to solve challenging mathematical problems by generating detailed chain-of-thought reasoning before producing a final answer. This project is specifically tailored for the AI Mathematical Olympiad (AI|MO) competitions hosted on Kaggle.

## Table of Contents

- [About the Competition](#about-the-competition)
- [About the DeepSeek Model](#about-the-deepseek-model)
- [Code Details](#code-details)
  - [Environment & Configuration](#environment--configuration)
  - [Prompt Generation & Chain-of-Thought](#prompt-generation--chain-of-thought)
  - [Ensemble Response Generation and Consensus](#ensemble-response-generation-and-consensus)
  - [Kaggle Inference Integration](#kaggle-inference-integration)
## About the Competition

The **AI Mathematical Olympiad – Progress Prize 2** is a Kaggle-hosted competition that challenges participants to solve complex math problems. Here are some key details:

- **Problem Set:**  
  The competition features 110 problems covering topics such as algebra, combinatorics, geometry, and number theory. These problems are typically formatted in LaTeX and require solutions to be submitted as non-negative integers.

- **Evaluation Method:**  
  Submissions are evaluated not only on the correctness of the final answer but also on the clarity of the chain-of-thought (CoT) reasoning provided by the participant. Answers must be enclosed within `\boxed{...}` and then processed modulo 1000.

- **Purpose and Impact:**  
  The competition is designed to push the boundaries of mathematical reasoning in AI. By emphasizing step-by-step reasoning, it encourages the development of systems that not only provide the right answer but also explain their thought process. This approach advances the field of autonomous problem-solving and has attracted participants from around the world.

For more details, please visit the [AI Mathematical Olympiad – Progress Prize 2 Overview](https://www.kaggle.com/competitions/ai-mathematical-olympiad-progress-prize-2/overview)

## About the DeepSeek Model

QwenAgent leverages the **DeepSeek R1** model, a refined and optimized version of the Qwen model designed specifically for mathematical problem solving. Key aspects of the DeepSeek model include:

- **Distilled Architecture:**  
  The DeepSeek model is a distilled version of Qwen 7B, offering improved inference speed while maintaining strong reasoning capabilities.

- **AWQ Quantization:**  
  It incorporates AWQ (a weight quantization technique) to reduce memory footprint and accelerate computations without significant loss of accuracy. This is particularly valuable in environments like Kaggle where resources are limited.

- **Optimized for Math Problems:**  
  DeepSeek has been fine-tuned on mathematical reasoning tasks, making it well-suited for competitions like AI|MO. Its training includes examples that emphasize chain-of-thought reasoning, ensuring that it can generate detailed step-by-step solutions.

- **Kaggle Integration:**  
  In the Kaggle environment, the model is loaded from a dataset path (e.g., `/kaggle/input/deepseek-r1/transformers/deepseek-r1-distill-qwen-7b-awq-casperhansen/1`), ensuring seamless integration with competition infrastructure.

## Code Details

### Environment & Configuration

- **Setting Up:**  
  The code configures environment variables to allocate GPU resources, manage tokenizer parallelism, and set the correct CUDA binary path. This ensures the agent runs efficiently both locally and on Kaggle.

- **Timing Configuration:**  
  A series of cutoff times is defined to adjust parameters (like maximum token generation) as runtime thresholds are reached, helping to manage long evaluation periods.

### Prompt Generation & Chain-of-Thought

- **Chain-of-Thought Prompts:**  
  The agent generates multiple conversation templates that instruct the model to reason step-by-step. For example:
  
  > "You are a smart assistant. Think through the problem step-by-step, clearly outlining your chain-of-thought before arriving at the answer. Finally, provide your final answer inside `\boxed{}` after taking modulo 1000."

- **Template Diversity:**  
  To balance consistency and variation, the same primary prompt is repeated multiple times (e.g., 13 copies) along with a few alternative versions. This ensemble approach leverages the model's inherent randomness to generate diverse outputs.

### Ensemble Response Generation and Consensus

- **Multiple Candidates:**  
  Each prompt yields a candidate answer with detailed reasoning. The agent extracts the final answer (text within `\boxed{...}`) from each response.

- **Consensus Mechanism:**  
  Candidate answers are aggregated using a simple counting method. The most common answer (with a slight random tie-breaker) is selected and then returned modulo 1000.

### Kaggle Inference Integration

- **CSV-Based Inference Server:**  
  MathOlympaidAgent integrates with Kaggle’s evaluation framework through a CSV-based inference server. The agent reads questions from a CSV file (e.g., `reference.csv`) and outputs answers in the expected format.

- **Logging:**  
  Detailed logging is used throughout the process (from seeding and prompt generation to final answer selection) to facilitate debugging and track the reasoning process.

