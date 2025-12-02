# Global Chess Challenge - Starter Kit

[![banner image](https://images.aicrowd.com/raw_images/challenges/social_media_image_file/1166/a4a6aaf8ab15af56cc81.png)](https://www.aicrowd.com/challenges/global-chess-challenge-2025)

# [Global Chess Challenge](https://www.aicrowd.com/challenges/global-chess-challenge-2025)

This repository is the **Submission template and Starter kit** for the Global Chess Challenge! Clone the repository to compete now!

**This repository contains**:
* **Documentation** on how to submit your agent to the leaderboard
* **The procedure** for best practices and information on how we evaluate your agent
* **Starter code** for you to get started!

# Table of Contents

1. [Competition Overview](#-competition-overview)
2. [Challenge Description](#-challenge-description)
3. [Tracks](#-tracks)
4. [Evaluation Metrics](#-evaluation-metrics)
5. [Getting Started](#-getting-started)
   - [How to write your own agent?](#️-how-to-write-your-own-agent)
   - [How to start participating?](#-how-to-start-participating)
      - [Setup](#setup)
      - [How to make a submission?](#-how-to-make-a-submission)
      - [What hardware does my code run on?](#-what-hardware-does-my-code-run-on)
      - [Baseline](#baseline)
6. [Frequently Asked Questions](#-frequently-asked-questions)
7. [Important Links](#-important-links)

# 📖 Competition Overview

Most chess players don't have regular access to a top coach. What they do have are their own games and a recurring question: "What should I have played here?" The Global Chess Challenge imagines a tool that looks at those positions, suggests a strong move, and explains the idea in simple language, so players can coach themselves using the games they already play.

This challenge asks you to build models that play legal chess moves and briefly explain their choices in natural language, while a world-class engine checks how well those moves hold up on the board. The challenge turns a familiar game into a testbed to see whether reasoning models can think clearly, play good moves, and talk about them in a way humans can follow.

# ♟️ Challenge Description

The Global Chess Challenge asks participants to build a **text-only chess agent** that does two things at once: play a legal move and explain the idea behind it in simple language.

On each turn, your model receives a chess position as text and must respond with:
- A one-sentence rationale explaining the idea behind the move
- A legal move in UCI format

The environment verifies legality, evaluates move quality using Stockfish, and runs full games in tournaments to measure overall playing strength.

## Input Format
For every turn, your agent receives:
- Position encoded as a FEN string
- Side to move (White or Black)
- List of legal moves in UCI format

## Output Format
Your agent must return:
- A one-sentence rationale: `<think>...</think>`
- Exactly one move in UCI format: `<uci_move>...</uci_move>`

# 🏁 Getting Started

1. **Sign up** to join the competition [on the AIcrowd website](https://www.aicrowd.com/challenges/global-chess-challenge-2025/).
2. **Clone** this repo and start developing your agent.
3. **Develop** your agent(s) following the template in [how to write your own agent](#-how-to-write-your-own-agent) section.
4. [**Submit**](#-how-to-make-a-submission) your trained models using huggingface for evaluation.

# ✍️ How to write your own agent?

Please follow the instructions in [player_agents/README.md](player_agents/README.md) for instructions and examples on how to write your own chess agent for this competition.

# 🚴 How to start participating?

**Clone** the repository recursively:
    ```bash
    git clone --recursive git@github.com:aicrowd/global-chess-challenge-2025-starter-kit.git
    cd global-chess-challenge-2025-starter-kit
    ```

**Install** competition specific dependencies
    ```bash
    pip install -r requirements.txt
    ```

## Running LLM locally

Before running local evaluation, you need to start either a vLLM server or a Flask server in a **separate terminal** from the `player_agents` directory.

### Option 1: Using vLLM (for LLM-based agents)
```bash
cd player_agents
bash run_vllm.sh
```

### Option 2: Using Flask for rule-based agents - (Note that rule based agents cannot be submitted, its only for local testing)
```bash
cd player_agents
# For random agent
python random_agent_flask_server.py
# OR for Stockfish agent
python stockfish_agent_flask_server.py
```

Keep this server running in the background while you run local evaluation.

## Local testing
Test your agent locally using `python local_evaluation.py`.

**Note:** Make sure you have started either the vLLM server or Flask server (see [Running LLM locally](#running-llm-locally)) in a separate terminal before running local evaluation.


## Before you submit
Accept the Challenge Rules on the main [challenge page](https://www.aicrowd.com/challenges/global-chess-challenge-2025) by clicking on the **Participate** button.

# 📮 How to Make a Submission

This guide walks you through the process of submitting your chess agent to the Global Chess Challenge 2025.

## Prerequisites

Before making a submission, ensure you have:

1. ✅ **Accepted the Challenge Rules** on the [challenge page](https://www.aicrowd.com/challenges/global-chess-challenge-2025) by clicking the **Participate** button
2. ✅ **Installed AIcrowd CLI** (included in `requirements.txt`)
3. ✅ **Logged in to AIcrowd** via the CLI
4. ✅ **Prepared your model** on Hugging Face
5. ✅ **Created a prompt template** for your agent

## Step 1: Login to AIcrowd

First, authenticate with AIcrowd:

```bash
aicrowd login
```

You'll be prompted to enter your AIcrowd API key. You can find your API key at: https://www.aicrowd.com/participants/me

## Step 2: Prepare Your Model on Hugging Face

Your model must be hosted on Hugging Face. You can use:
- A public model (e.g., `Qwen/Qwen3-0.6B`)
- Your own fine-tuned model
- A private/gated model (requires additional setup - see below)

### Using Private or Gated Models

If your model is private or gated, you need to grant AIcrowd access. See [docs/huggingface-gated-models.md](docs/huggingface-gated-models.md) for detailed instructions.

## Step 3: Create Your Prompt Template

Your prompt template should be a Jinja file that formats the chess position and legal moves for your model. Examples are available in the `player_agents/` directory:

- `llm_agent_prompt_template.jinja` - For general LLM agents
- `sft_agent_prompt_template.jinja` - For supervised fine-tuned agents
- `random_agent_prompt_template.jinja` - Minimal template example

## Step 4: Configure Your Submission

Edit the `aicrowd_submit.sh` file with your submission details:

```bash
# Configuration variables
CHALLENGE="global-chess-challenge-2025"
HF_REPO="YOUR_HF_USERNAME/YOUR_MODEL_NAME"  # e.g., "Qwen/Qwen3-0.6B"
HF_REPO_TAG="main"  # or specific branch/tag
PROMPT_TEMPLATE="player_agents/YOUR_PROMPT_TEMPLATE.jinja"
```

### Configuration Parameters:

- **CHALLENGE**: The challenge identifier (keep as `global-chess-challenge-2025`)
- **HF_REPO**: Your Hugging Face model repository (format: `username/model-name`)
- **HF_REPO_TAG**: The branch or tag to use (typically `main`)
- **PROMPT_TEMPLATE**: Path to your prompt template file

## Step 5: Submit Your Model

Once configured, run the submission script:

```bash
bash aicrowd_submit.sh
```

Or submit directly using the AIcrowd CLI:

```bash
aicrowd submit-model \
    --challenge "global-chess-challenge-2025" \
    --hf-repo "YOUR_HF_USERNAME/YOUR_MODEL_NAME" \
    --hf-repo-tag "main" \
    --prompt-template-path "player_agents/YOUR_PROMPT_TEMPLATE.jinja"
```

# ❓ Frequently Asked Questions

## How are games evaluated?
Games are played in round-robin tournaments with ACPL ratings determining the final rankings. Each move is also checked for legality and compared against Stockfish for calculating CPL.

## Can I use external tools?
Your agent must be self-contained and run without network access during evaluation. You can use Stockfish locally during training. During inference, we only run the LLM.

**Best of Luck** :tada: :tada: