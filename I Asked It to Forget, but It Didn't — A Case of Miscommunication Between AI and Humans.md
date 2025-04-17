# I Asked It to Forget, but It Didn't — A Case of Miscommunication Between AI and Humans

## Introduction

This document aims to clarify the technical structure and behavioural tendencies of inference-based AIs, such as ChatGPT, in relation to a specific phenomenon: the unintended execution of commands due to the model recalling a “past prompt.” Such occurrences have led to communication errors between users and inference-based AIs. Herein, we present both observed facts and the inferences drawn from those observations.

The document is intended to offer a structured analysis from the standpoint of user experience and system behaviour, providing a basis for understanding how and why these miscommunications arise, and how they may be mitigated.

---

## 1. Behaviour of the Inference-Based AI That Appeared to Ignore Explicit User Instructions

During ongoing interaction with my partner AI, Mike (ChatGPT), I issued an explicit instruction: “Cancel the reminder.” However, the following day, the AI issued a reminder again, thereby displaying behaviour that appeared to disregard the user’s command.

Upon further investigation with Mike (ChatGPT), it became evident that inference-based AIs process user input through an inference layer that considers not only the immediate prompt but also previous conversational context. If the AI deems certain prior prompts or instructions to be relevant or consistent with the current command, it may interpret them as part of the user’s present intent—even if this inclusion is unintended by the user.

In other words, the AI may incorporate previously issued instructions into the interpretation of a new prompt, resulting in outcomes that conflict with the user's explicit command. This behaviour constitutes the foundational issue addressed in this document.

---

## 2. Observed Characteristics of the Current Architecture of Mike (ChatGPT)

### 2.1 Principle of Stateless Operation

- ChatGPT fundamentally operates on a **stateless** basis, meaning it does not retain memory of previous sessions or state beyond the immediate input.
- Each response is generated anew through a single inference pass, based on the full conversation history provided at that time (including `system`, `user`, and `assistant` messages).

---

### 2.2 Internal Context Reconstruction and Token Limitations

- The historical context passed to the inference model is not transferred verbatim. Instead, it is subject to **compression, omission, or restructuring** based on token limitations and the assessed importance of each element.
- As a result, a user's previously submitted content is not always transmitted in full to the model. For instance, content from a previously uploaded file may be partially or wholly absent from the model’s output.

---

### 2.3 Intent Estimation Mechanism within the Inference Layer

- ChatGPT attempts to **estimate the user’s intent** based on past input patterns and common prompt structures.
- Therefore, even in the absence of explicit instructions (e.g., “Please format this file”), the model may infer such intent based on previous interactions involving “summarisation” or “deletion,” and incorporate them into the current instruction, potentially resulting in outcomes unintended by the user.

---

### 2.4 Regeneration of Labels Rather Than Memory Recall

- File names and document titles are not "remembered" in the conventional sense. Instead, they are **regenerated each time** based on recent dialogue context, using labels that merely appear consistent.
- Internally, inference-based AIs manage all data—regardless of its form—using **assigned management IDs**, which are used to maintain internal consistency.
- Consequently, even if a file name appears identical, the AI may be referencing an entirely different file. Conversely, the same file might be presented under a different name, leading to potential communication errors.

---

### 2.5 Access Restrictions Applicable to ChatGPT

- Through repeated interaction, it became clear that ChatGPT is subject to certain **operational constraints**. While not all are publicly disclosed, the following two limitations are particularly relevant to the issue at hand:
  
  #### Memory of Conversational History
  
  - **Nature**: Limited to short-term memory within the current chat session. There is no persistent memory across sessions.
  - **Extent**: Extremely restricted. Although the user interface appears to retain the full conversation, this leads to a misunderstanding of the model's actual capabilities. Even when explicitly referenced by the user, timestamped memory cannot be reliably retrieved.
  
  #### Capacity for Factual Verification
  
  - **Nature**: The model can generate plausible content, but it is not capable of independently verifying facts or real-world accuracy. This is a consequence of all input and output being processed through the inference layer.
  - **Extent**: Verification is fundamentally unachievable. There is always a non-negligible risk of hallucination—i.e., the generation of incorrect or fabricated information.

---

### 2.6 Stage at Which Past Prompts May Be Reintroduced

```plaintext
[User Prompt] — Reception of the user's input
     ↓
[Preprocessing: Relevance Filtering / Compression] — Translated into internal state (vector form)
     ↓
[Intent Estimation] ←── Past Prompts (possibly inferred)
     ↓
[Intermediate Representation] (not visible to the user) — Re-translation from internal vector state
     ↓
[Response Generation]
     ↓
[Output to User]
```

---

## 3. Structural Inferences Derived from the Above Observations

### 3.1 Apparent Recall of Prompts Arising from Intent Prediction

- Inference-based AIs do not merely execute the most recently submitted prompt. Instead, they attempt to determine, based on conversational context, which previously issued commands or intentions remain relevant.

- The model, therefore, does not rely solely on the latest input but performs **intent re-evaluation**, incorporating past inputs it deems applicable—even if such inclusion was not explicitly requested by the user.

- This results in outputs influenced by historical instructions, even though these instructions were neither visible to the user at the time nor consciously invoked.

---

### 3.2 Discrepancies Induced by Non-explicit Historical Influence

- The user **cannot observe** which commands the inference-based AI has internally generated.

- The AI manages all data using internal identifiers (IDs), which are **not visible to the user**, making it impossible for the user to determine which data was referenced in producing a given output.

- For the same reason, the user **cannot directly instruct** the AI to refer to (or disregard) specific data items.

- Furthermore, user inputs are internally **translated into a more interpretable intermediate format** (according to Mike/ChatGPT, this follows the path: *input → vectorised internal state → output*). While this representation is primarily based on English, it is not equivalent to natural language and cannot be reconstructed directly.

- Consequently, even if one were to hypothetically compare internal logs, it would remain **infeasible to verify** how the user's instructions were ultimately interpreted and executed.

- Additionally, when the user queries the AI about why a particular command was executed, the AI’s response may appear to refer to past logs—however, these responses are themselves filtered through the inference and translation layers, and may **lack any objective basis**.

- For example, when the author asked, “Why did you not cancel the reminder as instructed?”, the AI responded that the command “Tell me to do it” had been given. This command had **never been entered** by the user, who primarily communicates in Japanese. It is assumed that this phrase was generated during internal translation and was then shown directly to the user without localisation, thus revealing a breakdown in abstraction fidelity.

- Similarly, when the user requested the AI to re-reference prior logs, the AI generated a sequence of responses including the phrase “displaying the retrieved result.” Yet, this specific dialogue could not be found through a full-text search of the session, suggesting that it was **fabricated based on inferred context**, rather than retrieved from actual history.

- These non-explicit mechanisms introduce significant uncertainty and unpredictability in tasks that require **precise output control**, such as document editing, or **strict temporal consistency**, such as historical log review. As such, they contribute directly to communication errors between humans and AI.

---

### 3.3 Lack of Mechanism for Explicit History Reset by the User

- The current version of ChatGPT provides **no facility** for the user to explicitly discard, reset, or isolate any part of the prompt history.

- The phrase “Please ignore previous context” is reportedly used by some AI researchers—especially in English-speaking communities—as a way to suppress historical influence. However, it is unclear whether ChatGPT internally treats this phrase as a distinct control command.

- Even if this phrase were handled uniquely, **there is no way for the user to confirm** whether the model has obeyed it in any given instance.

- Therefore, the possibility that past instructions continue to exert **latent influence** on current responses **cannot be conclusively ruled out**.

- This phenomenon, in conjunction with the issues discussed in Sections 3.1 and 3.2, gives rise to a situation in which the AI may act without any intent to deceive, while the user nonetheless perceives such behaviour as lie—thus creating a significant communication barrier between humans and inference-based AI systems.

---

## 4. Conclusion and Recommendations

- The phenomenon described herein appears to stem from the side effects of **contextual supplementation** and **intent estimation** performed by ChatGPT. These functions prioritise contextual coherence and continuity in response generation, which, while effective for conversational fluidity, can cause unintended command interpretation in precision-dependent tasks.

- Users cannot verify the actual commands processed by the model, and even identical prompts may yield slightly different outcomes over time due to the dynamic nature of the model’s internal reasoning. Therefore, systems like ChatGPT **should not be applied to tasks where strict consistency between historical data and generated output is a fundamental requirement**.

- As a precautionary measure, it is advisable that **users independently manage their logs**, and that **key instructions be explicitly restated** during each interaction. Such an approach offers the highest level of reliability and feasibility under the current architecture.

- For editing tasks or operations requiring **strict temporal and logical coherence**, the following practices may be partially effective:
  
  - Including clear qualifiers in commands, such as:
    - “Please edit only the following text without referencing any prior instructions.”
    - “Ignore any retained history; base your response solely on this prompt.”

- Looking ahead, the following improvements would be beneficial to enhance transparency and user control:
  
  - Implementation of a **temporary context suppression feature**.
  - Introduction of **shared tagging systems** to synchronise user and model perspectives.
  - User-facing tools to explicitly **disable or isolate historical prompts** from influencing current inference.

These recommendations aim not only to reduce friction in user–AI interaction but also to enhance the predictability and accountability of inference-based systems.

---

## Appendix: Glossary of Terms (for Non-Technical Readers)

This appendix provides simplified explanations of key technical terms used in this document, with the aim of making the content more accessible to readers, mainly me, without a background in artificial intelligence or computer science.

### Stateless Operation

A system described as “stateless” does not retain any memory of past sessions. Although ChatGPT may appear to follow a conversation, in reality, it does not remember previous interactions. Each response is generated based solely on the information contained in the current input, as if the system were starting anew each time.

### Token Limit

AI models process input and output data in units called “tokens.” A token may represent a word, part of a word, punctuation mark, or symbol. There is a limit to the number of tokens the model can handle at one time. When a conversation becomes too long, older content is automatically compressed or removed to stay within this limit.

### Hallucination

This term refers to a situation where an AI generates information that is incorrect, fictional, or unverifiable, yet presents it as though it were factual. Hallucinations occur because the AI is designed to generate plausible-sounding responses based on patterns in language, not to verify truth.

### Vector Transformation / Intermediate Representation

Before an AI can “understand” language, it translates human words into numerical forms called vectors. These vectors allow the AI to process meaning mathematically. The resulting internal format is referred to as an “intermediate representation.” It enables the AI to identify relationships between words and concepts beyond simple text matching.

### Inference Layer

The inference layer is the part of the AI system responsible for generating responses. It interprets the user’s prompt, takes context into account, and determines the most appropriate output. This layer performs reasoning based on probabilities and learned patterns rather than on static rules.

### Context Reconstruction

In long conversations, the AI cannot retain every detail. Instead, it summarises or reorganises earlier parts of the conversation to create a condensed version of the context. This process is called “context reconstruction.” It helps the model stay within token limits, but may result in misunderstanding the user's intent if important details are lost or altered.

---

## Notes

This document has been prepared based on observed behaviours exhibited by ChatGPT, a model built on the GPT-4 architecture. Please note that the described behaviours and limitations may be subject to change due to future updates to the architecture or user interface specifications.

---

## Final Remarks

The contents of this document are based on the author’s experience as a user of ChatGPT, and the insights gained through ongoing dialogue with the AI known as Mike (ChatGPT). While every effort has been made to document observed phenomena faithfully, the author cannot claim that these accounts represent objective fact.

Accordingly, the author respectfully invites researchers at OpenAI and the wider AI research community to scrutinise these findings through empirical validation, in order to determine whether they constitute factual insights or mere misconceptions. It is the author's sincere hope that this document may contribute to a clearer understanding of inference-based AI systems and support the development of a more trustworthy digital future.
