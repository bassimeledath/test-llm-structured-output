# Testing LLM Structured Output Options

![Weave Dashboard](weave.png)

This repository is dedicated to evaluating 4 options for structured Large Language Model (LLM) output using Weave. All options use GPT 4o as the base model:

1. Zero-shot (with JSON mode)
2. Few-shot (with JSON mode)
3. [Instructor](https://github.com/jxnl/instructor)
4. [Marvin](https://github.com/PrefectHQ/marvin)

They are compared against the following 3 metrics:

1. Latency
2. Tokens consumed
3. Completeness: whether the output adheres to the Pydantic BaseModel structure specified.
4. Accuracy: whether the values extracted are based on the context provided.

## Getting Started

To get started with the notebooks in this repository, follow these steps:

1. **Clone the repository:**
   ```sh
   git clone https://github.com/bassimeledath/test-llm-structured-output.git
   cd test-llm-structured-output
   ```

2. **Set up a virtual environment:**
   ```sh
    python -m venv venv
    source venv/bin/activate  # On Windows, use `venv\Scripts\activate`
   ```

3. **Install the required packages:**
   ```sh
    pip install -r requirements.txt
   ```

4. **Run the Jupyter Notebook!**

## Findings

- GPT-4o's zero-shot prompting is impressive at adhering to structure. It can unpack dictionaries to create an instance of `ResumeInfo` without issues. However, it is inconsistent in the formatting of values. For instance, it might extract the education of a user as “Duke BS, 2012-2016” in one case and “B.S. Duke, 2012-2016” in another. This inconsistency can only be resolved with few-shot prompting.
- Instructor and Marvin work well out of the box, with added inference latency being negligible.
- Marvin is accurate but consumes many tokens. Instructor is efficient and very easy to implement.
- Using LLMs as judges requires improvement, as ground truth labels are needed for better accuracy comparison.

## Verdict

- For most use cases, working with plain GPT-4o with the enabled JSON schema forcing parameter and few-shot examples should suffice. You may need additional logic to process the output of the completion API to enforce or cast string types to your desired type (e.g., casting age to an integer). If you're comfortable adding Instructor as a dependency, it is super easy to implement and effective. Output structuring and forcing, which was a bigger headache with GPT-3.5-turbo, seems to be much better with these newer models, and this trend will likely continue.
- Weave is excellent for evaluations, especially for comparing different models. Highly recommended!

## Limitations of Analysis

- This analysis is limited to GPT-4o, a closed-source model. Several packages handle output structuring for open-source models in a more low-level manner. That is a topic for another day. GPT-4o is a very advanced model, and I am interested in replicating this analysis with a smaller model.

[Link to Weave Dashboard](https://wandb.ai/bassimfaizal/compare-output-structuring-models/weave/compare-evaluations?evaluationCallIds=%5B%226634cff9-3d63-4748-9cf8-5c8a93778dcf%22%2C%22ce15da54-b232-411d-ba40-f620f223e9f4%22%2C%220a2ca30a-1579-475e-a150-fed93be50422%22%2C%22d810f8de-c1b1-4f0e-8323-8dae223c8d9a%22%5D)
