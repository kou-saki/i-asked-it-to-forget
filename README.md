# I Asked It to Forget, but It Didn't — A Case of Miscommunication Between AI and Humans

This repository contains a technical investigation into a phenomenon observed during interactions with inference-based AI systems such as ChatGPT (GPT-4 series). Specifically, it documents and analyses instances where past prompts appear to be recalled and acted upon, despite the user's explicit instructions to discard or ignore them.

## 📌 Overview

In this report, the author explores how inference-based AIs process prompts, how historical context is internally managed, and how this can lead to misunderstandings—even when the user believes the AI has “forgotten” earlier instructions.

The central case study involves a prompt instructing the AI to cancel a reminder, which was apparently ignored due to unintended reactivation of past context.

## 🧠 Key Sections

- **Introduction**  
  Motivation and purpose of the investigation.

- **Observed Behaviour in ChatGPT**  
  A breakdown of architectural principles such as stateless operation, token compression, and intent estimation.

- **Structural Inference**  
  Analysis of how and why prompts are recalled or reconstructed internally.

- **Recommendations**  
  Practical advice for users, and design suggestions for developers.

- **Glossary**  
  Simplified definitions of technical terms for non-expert readers.

## 📂 Files

- `I Asked It to Forget, but It Didn't — A Case of Miscommunication Between AI and Humans.md` — English version of the full technical report.  
- `忘れてと言ったのに、忘れなかった-AIと人の間のミスコミュニケーション.md` — Japanese version of the original document.

## ⚠️ Disclaimer

This document is based on user-side observations and dialogue with ChatGPT, and does not represent official disclosures from OpenAI. Verification by AI researchers is strongly encouraged.

## 📄 License

This work is provided under a **CC BY 4.0** licence unless otherwise stated. Please cite the author if used in derivative works or academic references.
