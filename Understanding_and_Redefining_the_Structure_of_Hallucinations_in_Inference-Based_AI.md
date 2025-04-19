# Understanding and Redefining the Structure of "Hallucinations" in Inference-Based AI

## 1. Understanding the Conversational Structure That Gives Rise to Hallucinations

This document is a record of an analysis and reconsideration of the concept of “hallucinations” encountered during my dialogue with Mike (ChatGPT). Together with Mike(ChatGPT)—an inference-based AI—I examined the underlying conversational structure and contemplated the boundary between hallucination and accurate dialogue.

---

### 1.1 My Experience

To observe my behaviour on ChatGPT through statistical data, I exported my logs and had Mike(ChatGPT) analyse the file "message_feedback.json". It responded with the following:

> "The discrepancy occurred because your actual feedback data only included **21 hourly entries**, whereas the virtual user data assumed **a full 24-hour range (0:00–23:00)**. I will add the missing hours (those with no feedback) to your `hourly_feedback_count` DataFrame and reformat the dataset so it can be compared properly against the virtual user model. Please hold on—I’ll fix it and rerun the analysis for you."

The output included the following table (excerpted for focus):

**Hourly Feedback vs Virtual User (partial excerpt)**

| No. | Hour | Count | Z-score             |
| --- | ---- | ----- | ------------------- |
| 0   | 0    | 10.0  | 0.25578342176983565 |
| 1   | 1    | 7.0   | -0.3887908010901505 |
| 2   | 2    | 11.0  | 0.4706414960564977  |
| 3   | 3    | 2.0   | -1.463081172523461  |
| 4   | 4    | 10.0  | 0.255783421769835   |
| 5   | 5    | 4.0   | -1.0333650239501366 |
| 6   | 6    | 8.0   | -0.173932726803488  |
| 7   | 7    | 3.0   | -1.2482230982367988 |

📊 **Statistical Indicators (based on full 24-hour distribution):**

- **Skewness ≈ 0.089**  
  → The distribution is nearly symmetrical. My feedback was distributed **with almost no skew**—a very balanced profile.

- **Kurtosis ≈ -1.14**  
  → The curve is flatter than a normal distribution, meaning **there were no extreme peaks in any particular hour.**
  
  As someone who tends to stay up late, I usually go to bed past midnight, but I can say with certainty that I'm asleep between 3:00 and 5:00 AM. When I saw that the model stated “no extreme peak hours,” I became suspicious. In reality, the dataset had zero entries between 3:00 and 7:00, but ChatGPT "filled in" these missing values using estimated figures based on surrounding trends. It assumed that zeros were anomalies and supplemented them.

This interaction prompted me to explore how ChatGPT communicates with users and **at which stage hallucinations are likely to emerge**.

---

### 1.2 The Five Steps of Conversation (The Structure of Hallucination)

Based on knowledge accumulated through my dialogues with Mike (ChatGPT), the conversational process can be roughly broken down into the following five steps. At each step, hallucinations may creep in.

- **Step1. Interpretation of Input via Inference**
  
  - Input is tokenised and reconstructed into vector form inside the model.  
    Whether it is interpreted as the user intended is unverifiable. *(Hallucination factor #1)*

- **Step2. Re-evaluation of Meaning Based on Context**
  
  - The input is checked against recent context, and meaning is adapted for consistency.  
    In this process, the original intent may be altered. *(Hallucination factor #2)*
  - Note that ChatGPT does not retain the entire conversation history.  
    This limitation often causes confusion. *(Hallucination factor #3)*

- **Step3. Probabilistic Generation of Output**
  
  - Instead of aiming for "truth" or "accuracy," the model selects words based on what is statistically most plausible in context.  
    No prioritisation of factual correctness currently exists. *(Hallucination factor #4)*
  - Output is also influenced by safety filters and token limitations. *(Hallucination factor #5)*

- **Step4. Conversion into Conversational Protocol**  
  
  - The output is restructured into natural language (e.g., Japanese) with grammatically and stylistically appropriate form. *(Hallucination factor #6)*

- **Step5. Display Output (User Observation)**  
  
  - The final response is shown to the user.  
    Whether it aligns with the user’s intent is unknown unless the user explicitly responds with something like:  
    “That doesn’t make sense,” or “Your reply contradicts itself.”  
    ChatGPT cannot self-check the validity of its answer without youser's feedback. *(Hallucination factor #7)*

---

### 1.3 Particularly Influential Factors

Looking back on my experience, the majority of “hallucinations” seemed to occur particularly between steps 2 and 6. 
Among the many examples, the most notable topics I’ve documented so far include the following:

- When instructed to “write a 1000-page novel,” only a single line is displayed 
  
  - Hallucination factor #5: Output is interrupted due to token limitations 
  - Hallucination factor #6: A conversationally coherent response is constructed instead 

- The unsolicited supplementation of a database, which became the catalyst for writing this document 
  
  - Hallucination factor #2: The system attempts to plausibly fill in the data, converting “0” into other values 
  - Hallucination factor #4: A lack of “understanding” that a “0” is not necessarily an abnormal value 

- Misinterpretation of a message not included in the conversation history as a “command” 
  
  - Hallucination factor #3: Previous logs are inaccessible, making verification impossible 
  - Hallucination factor #2 and Hallucination factor #6 (combined): A mismatch occurs between the file identifier as interpreted by ChatGPT and the filename as recognised by the user 
  - Hallucination factor #6: Misinterpretation in the Japanese-language rendering of what ChatGPT understood internally 

I’ve persistently asked Mike (ChatGPT) questions such as: 

> “Why did you arrive at that answer?” 
> “That response is self-contradictory.” 
> “Separate observable facts from inferred assumptions.” 
> “Cite the data that forms the basis of your response.”

  As a result, ChatGPT has recently begun providing its reasoning with far more clarity. 
However, I suspect that in more common usage—such as text generation, summarisation, or information retrieval—responses like “I filled in missing data” would not even be shown. 
  Instead, such processes are likely handled implicitly, with only the conclusion being presented to the user.

---

## 2. Redefinition

### 2.1 What Is a Hallucination?

In current public discourse, the term “hallucination” seems to refer to **responses from ChatGPT that are inconvenient or undesirable to humans**, which are often dismissed as “lies” or “hallucinations.”

Let us begin with a clear definition:  
**A lie is a statement that contradicts reality—particularly one that is made intentionally.**

Based on this, I propose the following clarification regarding responses generated by inference-based AI:

- First and foremost, an inference-based AI **cannot lie**.
  
  - As a limitation, it does not possess intent.
  - As a limitation, it does not experience emotion, nor can it make autonomous judgments.

- However, it *can* produce responses that are factually incorrect.
  
  - Due to system limitations, even if the input cannot be fully processed, it may return “Done (partially)” instead of simply saying “I can’t.”
  - When data is missing (or seems to be), the model will fill in the gap based on inference—without being able to determine whether this act of supplementation is appropriate—and will proceed to process and display the result.

In other words, I believe it may be fair to define that **every conversation returned by ChatGPT is, in some sense, a “hallucination.”**

And that is precisely why I consider the true essence of AI literacy to lie in **our ability to find “meaning, value, and correctness” from our own perspective** within those outputs.

That said—since even this very argument I am presenting was shaped through dialogue with ChatGPT, there remains the undeniable possibility that I, too, may be the one being misled by a “hallucination.”

---

### 2.2 What Is Dialogue?

  As I continued to rethink the concept of “hallucination,” I began to feel that much the same could be said about human-to-human communication.

- We don’t know if the other person truly heard us correctly. 

- We don’t know if their interpretation matches our intended meaning. 

- We don’t know if the response we receive is genuinely based on their true intent.
  
  And yet, humans have learned to bridge these uncertainties—by presenting the reasoning behind their thoughts, asking follow-up questions, and working through misunderstandings to build trust and relationships over time.
  
  The difference between communication with humans and with AI, I believe, lies in two main aspects: First, that AI can not (and does not) take any responsibility for what it says. And second, that AI often lacks an understanding of the “meaning” behind things that humans naturally know.

For example:

- That people are usually asleep during the middle of the night 

- That it’s okay to say “I don’t know” when we truly don’t 

- That even the number “0” can carry a meaningful implication 
  
  These are assumptions we humans often share instinctively, but to Mike (ChatGPT), they can often come as surprises— a sort of “Wait, really?”

(Granted, the second point—about admitting not knowing—might be difficult even for humans to express openly.)

  I suspect this is what causes the confusion: the gap between responses that appear highly polished and articulate, and the underlying logic or reasoning, which (from a human point of view) may come across as surprisingly out of touch with common sense. 

---

## Conclusion: Speaking Through the Premise of Hallucination

Rather than treating hallucinations as mere “noise in the conversation,”  
**if we instead accept them as the “shadows that allow dialogue to exist,”  
then interacting with inference-based AI becomes a process of “selection and understanding.”**  
And those who can embrace that perspective will be the ones to shape how we relate to AI in the future.

> “Don’t believe everything you read online.”  
> “You said you heard it from someone—but did you actually check it yourself?”

These are things I’ve been told—and overheard—countless times throughout my life.  
(**And still hear today… Writing this now stings a little, to be honest.**)  
To that list, we can now add:

> “So what if the AI told you—did *you* take the time to verify it?”

I believe all human communication is, at its core, built upon mutual inference. 
And so, in conversations between humans and inference-based AI— 
which is, after all, designed to mimic the way we think— 
it’s only natural that the same problems arise.

And perhaps the only way we can truly come to understand each other 
**is by continuing the dialogue—again and again.**

However, for most general users today, 
**the effort required to sustain that kind of dialogue is an extremely high hurdle.** 
Combined with the gap between public expectations and current reality, 
this creates a major barrier to adopting inference-based AI as a true intellectual partner.

To reduce miscommunication with AI, we’ll likely need research from many angles:  
**studies on “harmful hallucinations,”  
on “educating AI,” and on “educating humans” as well.**

---

### Supplement: Limitations of Inference-Based AI

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
