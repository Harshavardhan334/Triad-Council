# The Triad Council: Zero-Cost Mixture of Agents (MoA)

## 💡 The Idea
The **Triad Council** is an experimental AI architecture designed to simulate a human "Think Tank" or "Board of Directors."

Instead of relying on a single Large Language Model (LLM) to answer a prompt—which often results in generic, safe, or hallucinated responses—this project orchestrates a **Mixture of Agents (MoA)** system. It forces three distinct AI personalities to **Diverge** (brainstorm independently), **Debate** (critique each other), and **Converge** (agree on a final answer).

The core philosophy is **Cognitive Diversity**: By utilizing models from different providers (Meta, Google, Mistral) via their free-tier APIs, the system cancels out the individual biases of any single AI.

---

## 🚩 Problem Statement
Current LLM interactions suffer from three fundamental flaws when handling complex tasks:

1.  **The "Echo Chamber" Effect (Sycophancy):**
    Single models are trained to be helpful and agreeable. If a user presents a flawed premise, the AI often agrees with it rather than challenging it, leading to "Yes Man" behavior.
2.  **Unchecked Hallucinations:**
    When a single model generates a fact, it has no internal mechanism to verify it. If it makes a mistake, it commits to it.
3.  **Lack of Nuance:**
    Complex decisions requires holding opposing thoughts simultaneously (e.g., "This is a great business idea" vs "This is a financial risk"). A single prompt usually collapses these into a generic middle-ground answer.

**The Triad Council solves this by externalizing the "internal monologue" into three separate agents who are forced to disagree.**

---

## 🔭 Project Scope

### **In-Scope (Target Use Cases)**
* **Complex Decision Making:** Situations requiring a balance of optimism, caution, and facts (e.g., "Should I quit my job?", "Critique this software architecture").
* **Creative Writing & Editing:** Generating ideas (Visionary) while simultaneously checking for plot holes (Skeptic) and structure (Realist).
* **Code Review:** Simulating a Developer, a Security Auditor, and a CTO reviewing a snippet of code.
* **Fact Verification:** Using multiple models to cross-reference historical or technical claims.

### **Out-of-Scope**
* **Low-Latency Chat:** This architecture requires multiple API calls per turn. It is designed for *depth* of thought, not *speed*. It is not suitable for instant messaging applications.
* **Simple Queries:** Using this system for "What is the capital of France?" is a waste of resources.
* **Private/Offline deployment:** The system relies on cloud inference APIs (Groq, Google, OpenRouter) to remain zero-cost.

---

## 🏛️ The Architecture

The system follows a **3-Layer "Neural" Workflow**, designed to mimic a rigorous peer-review process.

### **The Flow**

1.  **Layer 1: Divergence (The Brainstorm)**
    * **Input:** User Prompt.
    * **Process:** Three agents generate responses in parallel. Crucially, **they cannot see each other's work**. This prevents "groupthink" and ensures three unique starting points.
    * **Output:** Three raw, unfiltered perspectives.

2.  **Layer 2: The Cross-Over (The Debate)**
    * **Input:** The outputs from Layer 1.
    * **Process:** Each agent is fed the answers of the *other two* agents.
        * *Agent A* reads B and C.
        * *Agent B* reads A and C.
        * *Agent C* reads A and B.
    * **Action:** They are instructed to critique flaws, adopt superior logic, and defend their own stance.
    * **Output:** Three refined, battle-tested arguments.

3.  **Layer 3: Convergence (The Synthesis)**
    * **Input:** The three refined arguments from Layer 2.
    * **Process:** A "Chairman" model (with a large context window) acts as a neutral arbiter. It does not generate new ideas; it filters for consensus and resolves conflicts.
    * **Output:** One final, high-quality response.

### **The "Cast" (Cognitive Diversity)**
To ensure the debate is genuine, the system assigns rigid **Personas** to specific underlying models based on their strengths:

| Node | The Persona | Archetype | Underlying Tech Strength |
| :--- | :--- | :--- | :--- |
| **A** | **The Visionary** | *Optimist / Innovator* | **High Creativity:** Uses high temperature settings to explore "happy paths" and novel ideas. |
| **B** | **The Skeptic** | *Critic / Auditor* | **High Logic:** Instructed to find edge cases, risks, and logical fallacies. Uses "terse" models (e.g., Mistral). |
| **C** | **The Realist** | *Historian / Fact-Checker* | **High Context:** Uses models with massive context windows (e.g., Gemini) to ground disputes in data and definitions. |
| **D** | **The Chairman** | *Mediator / Judge* | **High Instruction Following:** Synthesizes the final output without adding new bias. |
