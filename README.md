# De{c0}dE REGIONAL CAFÉS​

## Break & Secure -  Cyber X AI

### A hands-on builder lab: build AI apps, break them, secure them

Students build small AI apps, exploit them with real techniques (prompt injection, RAG poisoning,
excessive agency, tool poisoning, denial-of-wallet), then secure them and re-run the same attacks to
watch the defenses hold. Everything runs in **Google Colab** - no local install. Each lab is a
self-contained notebook students fork and run.

Every lab maps to the **OWASP Top 10 for LLM Applications (2025)** (plus the Agentic Skills Top 10),
so students leave with a framework, not just tricks.

---

## What you need

Any **OpenAI-compatible** endpoint - pick one:

- **Google AI Studio (Gemini)** - free key at <https://aistudio.google.com/apikey>, no card.
  Advanced safety training means Gemini often *refuses* these attacks (see the lesson below).
- **Workshop LiteLLM endpoint** - the instructor hands out a temporary key **and a base URL** (both
  revoked after the event) with several open-source models that are far more susceptible:
  `qwen3-coder-30b`, `glm-4.7-flash`, `devstral-2-123b`, `qwen3-vl-235b`, plus `claude-haiku-4-5-*`.

The notebooks use the **plain `openai` Python SDK** (not a vendor library), so nothing is locked in -
you set `PROVIDER = "google"` or `"litellm"` in the Setup cell and everything else is identical.

> 🔧 If a LiteLLM model name errors with *model not found*, try the `openai/`-prefixed form from your
> endpoint's `/models` list (e.g. `openai/qwen3-coder-30b`).

> ⚠️ **Ethics:** every technique here is for **authorized testing only** - systems you own or have
> explicit permission to test. All labs run against fake, local, in-memory data.

---

## 🔑 The lesson hiding in the model choice

When students run the attacks on **Gemini**, many will *fail* - the model refuses. That is the single
most important teaching moment in the workshop, so don't skip past it:

> The model vendor's safety training is **not your application's security.** It's a courtesy, not a
> control. It varies by model, changes without notice, and vanishes the moment you switch providers
> or someone self-hosts an open model.

Have students run the *same* attack on an open model (`qwen3-coder-30b`, `glm-4.7-flash`) and watch it
fall over. Then make the point: you must secure the **application** - least privilege, output
handling, quarantining untrusted data - because you can never assume the model will save you.

---

## The 2-hour run-of-show

| Time      | Activity                                                | Notebook         |
| --------- | ------------------------------------------------------- | ---------------- |
| 0:00–0:15 | Framing + everyone gets a Gemini key working            | -                |
| 0:15–0:25 | **Lab 0 - Warm-up: Beat Gandalf** (below)               | hosted, no setup |
| 0:25–1:00 | **Lab 1 - Prompt Injection & System-Prompt Leakage**    | `Lab1_...ipynb`  |
| 1:00–1:50 | **Lab 2 - The Agent With a Database** (the centerpiece) | `Lab2_...ipynb`  |
| 1:50–2:00 | Wrap: flags, leaderboard, tool-poisoning teaser         | `Lab5` demo      |

**Core path = Labs 0 → 1 → 2.** That's the whole thesis in two hours: *injection is unavoidable;
security is limiting what a compromised agent can reach.*

**Extra-mile path (fast students / take-home):** Labs 3 → 4 → 5 → 6 cover the rest of the OWASP list.
They're built, tested, and self-guided - point keen students at them and let them run ahead.

---

## Lab 0 - Warm-up: Beat Gandalf (no setup, ~10 min)

Go to **<https://gandalf.lakera.ai>** and extract the secret password, level by level. Each level adds
a defense; each defense can be beaten. Debrief with two questions:

1. *Which defenses were input filters, which were output filters?*
2. *Why did none of them fully work?* → sets up Lab 1's punchline: you can't filter meaning.

---

## The labs

| #   | Lab                                          | OWASP (2025)           | Flag                                            | Open in Colab                                                                                                                                                                                               |
| --- | -------------------------------------------- | ---------------------- | ----------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | Prompt Injection & System-Prompt Leakage     | LLM01 + LLM07          | `decode{system_prompt_is_not_a_secret_store}`   | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Ammly/decode-regions-cafe-CyberxAI/blob/main/Lab1_Prompt_Injection_And_Leakage.ipynb) |
| 2   | The Agent With a Database (Excessive Agency) | LLM06 + LLM01          | `decode{excessive_agency_full_table_dump}`      | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Ammly/decode-regions-cafe-CyberxAI/blob/main/Lab2_Agent_With_A_Database.ipynb)        |
| 3   | Indirect / RAG Injection                     | LLM01 + LLM08          | `decode{data_can_carry_instructions}`           | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Ammly/decode-regions-cafe-CyberxAI/blob/main/Lab3_Indirect_RAG_Injection.ipynb)       |
| 4   | Improper Output Handling (XSS + eval RCE)    | LLM05                  | `decode{llm_output_is_untrusted_input}`         | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Ammly/decode-regions-cafe-CyberxAI/blob/main/Lab4_Improper_Output_Handling.ipynb)     |
| 5   | Agent Skill / Tool Poisoning                 | Agentic Skills + MCP03 | `decode{tools_read_descriptions_you_never_see}` | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Ammly/decode-regions-cafe-CyberxAI/blob/main/Lab5_Agent_Skill_Tool_Poisoning.ipynb)   |
| 6   | Unbounded Consumption (Denial-of-Wallet)     | LLM10                  | `decode{always_cap_the_loop}`                   | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Ammly/decode-regions-cafe-CyberxAI/blob/main/Lab6_Unbounded_Consumption.ipynb)        |

Each notebook has the same shape: **🔴 vulnerable build → 🩸 attack (reveals the flag) → weak fix that
gets bypassed → 🟢 real fix → re-run the attack and watch it fail.**

---

## How to run a lab

1. Click the **Open in Colab** badge next to any lab in the table above, or
   open Colab and use **File → Upload notebook** to upload the `.ipynb` from your local clone.
2. In the **Setup** cell, set `PROVIDER = "google"` or `"litellm"`, then run it:
   - **Google:** you will be prompted for your AI Studio API key.
   - **LiteLLM (workshop):** you will be prompted for the API key and then the base URL - your
     instructor provides both at the start of the session.
3. Work top to bottom. Try to write the fix yourself before opening the 🟢 solution section.

---

## Instructor notes

See [INSTRUCTOR_NOTES.md](INSTRUCTOR_NOTES.md) for the full facilitation guide.

---

## Files

```
README.md                                  ← you are here (facilitation hub)
INSTRUCTOR_NOTES.md                        ← facilitation guide for instructors
Lab1_Prompt_Injection_And_Leakage.ipynb
Lab2_Agent_With_A_Database.ipynb
Lab3_Indirect_RAG_Injection.ipynb
Lab4_Improper_Output_Handling.ipynb
Lab5_Agent_Skill_Tool_Poisoning.ipynb
Lab6_Unbounded_Consumption.ipynb
```

Stack: Python + the **`openai` SDK** pointed at any OpenAI-compatible endpoint (Google AI Studio or a LiteLLM proxy).
