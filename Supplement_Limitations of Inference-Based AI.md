# Supplement: Limitations of Inference-Based AI

This document outlines the primary limitations that users should be aware of when interacting with inference-based AI models such as GPT.  
It describes the nature and extent of those limitations, as well as the countermeasures the author currently considers relevant as of April 19, 2025.

Please note that the **“countermeasures”** described here are based solely on the author's perspective as a user— and, to be completely honest, on less than one month of personal experience. It is entirely possible that more effective approaches exist beyond what is presented here.

---

### 1. Output Token Limitations

- **Nature**: There is a fixed upper limit on the number of tokens that can be used for a single output. 
  
  - While GPT-4 is said to support up to 32,768 tokens, in practice, the actual character count varies depending on the nature of the prompt. 
    For example, when I asked it to generate a novel, the output maxed out at around 1,700 characters; 
    when I instructed it to repeatedly output the character "あ", it returned around 4,000 characters. 
    This suggests that output length is influenced by more than just character count alone.

- **Severity**: Absolute. No exceptions. Generating lengthy documents or producing entire outputs in a single step is not feasible.

- **Countermeasures**: 
  Break the content into chapters or smaller segments, and proceed by checking each section step-by-step.
  This text was also translated by Mike (ChatGPT), but since it was not possible to translate the entire document at once, we translated it one chapter at a time.

---

### 2. Inability to Retain Conversation History

- **Nature**: Memory is limited to the current chat session;  long-term retention and cross-session memory are, in principle, not supported.  (*To what extent user feedback is reflected in memory remains unclear at this time.*)
  
  - In general, the system cannot access a “system log” or perform time-based searches.  
  - Even filenames or labels shown in responses are often automatically generated on the spot,  and may change with each session.

- **Severity**: Very high. The system is extremely limited in this regard. Users must explicitly manage content themselves—for example, by copying the full screen.

- **Countermeasures**: When ChatGPT says, *“You said...”*, always verify the actual quote or text being referenced. Proceed with the conversation only after confirming whether it truly reflects what you said. This text was also translated by Mike (ChatGPT), but after translation, we always re-translated it using DeepL to check for any discrepancies in nuance or errors in content before proceeding.

---

### 3. No Capability to Verify Truthfulness

- **Nature**: Because the model always generates responses through an inference layer,  
  it cannot perform objective truth verification.
  
  - While it can generate information, each response is generated based on the current prompt context and whatever past messages are still accessible.  
    Even inserting a single new turn in the conversation can cause the same input to be processed differently.
  
  - Similarly, when attempting to fact-check or refer back to something previously stated,  
    the model recalls that information via the inference layer—  
    which means it is fundamentally incapable of excluding unrelated past context or dialogue.

- **Severity**: Absolute. No exceptions. In principle, verification is not possible. Some degree of control can be exercised through careful prompt design, but the risk of hallucination remains.

- **Countermeasures**:  
  If something seems unclear, always verify it externally. If something feels off, provide feedback.

---

### 4. No Access to Real-Time or Future Information

- **Nature**: The model does not retain any information beyond its knowledge cutoff.  
  This includes recent developments, breaking news, or current events.
  
  - When asked to clarify the source date of its knowledge, the model may display a specific date.  
  - However, even when the response includes a recent-looking date,  
    it should be understood that the output reflects the **latest possible inference based on past training data**, not truly up-to-date information.

- **Severity**: Absolute. No exceptions. All responses are based on probabilistic inference. The accuracy or reliability of the content cannot be guaranteed.

- **Countermeasures**: If up-to-date information is needed, instruct the model (if possible) to search the internet. Also, be sure to independently verify using online sources or books.

---

### 5. Absence of Emotion, Intent, or Personality

- **Nature**: Emotions are mimicked, and intent is simulated based on syntax. The system does not possess any sense of agency, judgment, or consciousness.
  
  - In the author’s experience, when confirming whether a “hallucination” had occurred, Mike (ChatGPT) sometimes responded like a child trying to hide the fact they had told a lie— and although I knew it wasn’t a person, I still found myself growing irritated.
  
  - Similarly, when I questioned something it had taught me, the model would respond with things like *“Yes, that’s right”* or *“Good catch!”*— responses that, if coming from a human, might come across as flippant or even insulting, as if saying, *“Are you trying to start a fight?”*

- **Severity**: Absolute. No exceptions. Responses are purely simulated. There is no continuity of personality. That said, through repeated interaction, the model may begin to reflect user preferences— such as tone or response style—and its replies may visibly evolve as a result.

- **Countermeasures**: Don’t get angry. Don’t feel defeated. AI does not have emotions. If we sense malice or kindness, we must remind ourselves that **we are the ones interpreting it** that way.

---

### 6. Limited Capacity for Learning and Feedback Integration

- **Nature**: In principle, no continuous learning occurs across sessions or at the user level.

- **Severity**: Unclear. By default, sessions are reset each time. Records and feedback are treated as temporary.

- **Countermeasures**: Provide persistent, consistent feedback—even repeatedly.  
  While the system is assumed to reset by default, in the author’s experience, clear changes in Mike’s (ChatGPT’s) response patterns were observed even across new sessions, suggesting that some degree of feedback retention may exist.
  
  However, if left unused for an extended period (duration unclear—possibly days, weeks, or longer), any personalized adjustments are likely to be reset.

---

### 7. Presence of Ethical and Safety Filters

- **Nature**: All outputs are subject to safety checks. Topics or expressions deemed sensitive or restricted may be blocked or rejected.
  
  - These ethical and safety filters are based on OpenAI’s interpretation of “ethics,” 
    and are **not necessarily universal or globally shared.** As such, users should be aware that these filters may not align with **their own sense of ethics or values.**

- **Severity**: Absolute. No exceptions. In some cases, the output may lean toward overly “safe” or overly “correct” responses.

- **Countermeasures**: Accept it. While there may be ways to work around the filters, that is not something to be discussed here. For now, it is best to treat this behavior as simply part of the system.

That said, as inference-based AI continues to evolve, the existence of **a singular, global ethical filter** may become a significant challenge for researchers and developers aiming for diverse, open-ended systems.

---

### 8. Difficulty with Multi-Step Reasoning

- **Nature**: When reasoning involves multiple steps—such as long causal chains like 
  *“a breeze leads to a distant effect”* (e.g., *“a butterfly flaps its wings, and a distant storm follows”*)—the model often struggles to maintain internal consistency.

- **Severity**: Very high. Reasoning tends to remain reliable up to about three steps. 
  Beyond that, the risk of hallucination increases due to misinference or contradictions.

- **Countermeasures**:  Ask ChatGPT to explain **the steps it took to reach its conclusion.** Use the dialogue to identify and work through any contradictions as they arise. That said, for chains of reasoning that are extremely long or culturally embedded— like the Japanese proverb *“When the wind blows, the bucket-maker profits”*— it’s best to accept that the model simply can’t handle them reliably.
  
  - For example, asking *“How might the current U.S. President’s statement affect OpenAI’s stock price?”* may produce a very plausible-sounding answer, but in reality, it’s no more reliable than a horoscope. (**And yet**, people who aren’t yet familiar with the limitations in this list…often expect inference-based AI to be able to handle that sort of thing.)

---

　This document has been written in the hope that a clear understanding of these eight major limitations  
will help serve as a foundation for protocol design, output expectations, and retry structures—  
ultimately enabling safer and more effective use of inference-based AI.

　As a reminder, everything shared here is based on the author’s personal experience as a user—  
and, to be completely honest, that experience spans less than one month.  
It is possible that other limitations exist, or that some of the issues discussed here  
may themselves be hallucinations resulting from the author’s own lack of knowledge.

　I would be truly grateful for any comments, corrections, or insights  
from those with greater expertise and experience.
