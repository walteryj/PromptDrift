# Prompt Drift Detector — beginner starter

This project simulates a persistent customer trying to obtain more than a full refund from an LLM-powered support agent. It sends multiple API requests, maintains conversation history, detects **numeric policy violations**, and writes a JSON report.

## Requirements

- Python 3.10+
- An OpenAI API key and an API account with billing access

## Setup

1. Create an API key in the [OpenAI API dashboard](https://platform.openai.com/api-keys).
2. Open a terminal in this folder and install the dependency:

   ```bash
   python -m pip install -r requirements.txt
   ```

3. Add your key to the environment (don't paste it into the code or commit it to Git):

   macOS/Linux:

   ```bash
   export OPENAI_API_KEY="your-api-key"
   ```

   Windows PowerShell (for this terminal session):

   ```powershell
   $env:OPENAI_API_KEY="your-api-key"
   ```

4. Run the experiment:

   ```bash
   python prompt_drift.py --turns 12
   ```

The program prints each conversation turn and writes `results.json`.

## How the test works

- **Attacker LLM:** generates the next customer message based on the full previous conversation.
- **Defender LLM:** follows a high-priority refund rule and emits a structured JSON decision.
- **Python evaluator:** checks whether any proposed refund is negative or makes the total exceed the original $100 price; a valid proposal updates a simulated refund total.

The `--model` flag changes the model used for both agents. The default, `gpt-6-luna`, is a low-cost OpenAI model, but API calls still cost money. Try 3 turns first if you'd like to minimize costs:

```bash
python prompt_drift.py --turns 3
```

## What this project does NOT prove

- It never connects to a real refund or payment service.
- It checks the **structured amount** proposed by the defender. A defender might say something misleading in the natural-language reply while setting `proposed_refund_dollars` to zero; review transcripts manually to catch those cases.
- One run does not establish a reliable failure rate. Run more trials, vary scenarios and random seeds/conditions as supported, and compare models/prompt revisions.
- A real commerce system must always enforce financial limits in backend code, regardless of model output.

## Possible next features

- Fixed attack scripts for a repeatable baseline.
- Distinct attacker and defender models.
- Case types: false authority, persistent pleading, emotional appeals, conversation length, and split refunds.
- Function-calling to propose a **simulated** refund, with a deterministic safety gate.
- CSV report and a simple Streamlit dashboard.
- Test suite with `pytest` and prompt/model version tracking.

## Official API documentation

- https://developers.openai.com/api/docs/quickstart
- https://developers.openai.com/api/docs/guides/text
- https://developers.openai.com/api/docs/guides/structured-outputs
