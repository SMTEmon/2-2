---
title: "HUM 4441: Engineering Ethics - Practice Questions"
date: "2026-07-03"
tags: [ethics, exam, practice-questions, hum4441]
---

# 🧠 HUM 4441: Engineering Ethics - Practice Questions

These practice case studies are modeled after the sample midterm format (Case Study followed by analytical sub-questions) and cover the breadth of the syllabus.

Click the **"► View Answer / Hints"** dropdowns to reveal the solutions after you've tried answering them!

---

### 🟢 Case Study 1: The "CleanTech" Chemical Plant
**Topics Covered:** Environmental Ethics, Ethical Lenses, Whistle-Blowing (Chapters 3, 6, 7)

**The Case:**
Sarah is an environmental engineer working for CleanTech, a manufacturing company building a new plant upstream from a small farming town. During the design phase, Sarah realizes the plant will discharge a novel byproduct into the river. Because the chemical is entirely new, it is currently unregulated by the EPA, meaning discharging it is perfectly legal. However, Sarah’s internal tests suggest the chemical could accumulate in the soil over a decade, potentially harming the town's crops. 

When Sarah raises the issue, her manager tells her: *"We are meeting all legal requirements. Adding a specialized filtration system would cost $4 million and delay the opening by six months. We have a duty to our shareholders to open on time."* Sarah is considering taking her test data to the local newspaper.

**Questions:**
**(a) [8 marks]** Analyze CleanTech's decision to proceed without the filter using **Utilitarianism** and **Rights Ethics**. How might these two lenses produce different conclusions about the company's decision?
**(b) [8 marks]** Sarah is considering external whistle-blowing to the press. Evaluate her situation against the **four conditions** required for whistle-blowing. Based on these conditions, is her whistle-blowing currently *permissible*, *obligatory*, or *unjustified*?
**(c) [4 marks]** Explain how the **Cost-Oblivious** approach differs from the **Cost-Benefit** approach in environmental ethics. Which approach is CleanTech's management using?

<details>
<summary><b>► View Answer / Hints for Case Study 1</b></summary>

**(a) Ethical Lenses Analysis:**
*   **Utilitarianism:** Focuses on the greatest overall benefit. Management is arguing that the economic benefits (saving $4M, opening on time, providing jobs/shareholder value) outweigh the long-term, uncertain risks to the crops. However, a strict utilitarian must also account for the potential economic devastation of the farming town in 10 years.
*   **Rights Ethics:** Focuses on fundamental human rights. The farming town has a right to their property, livelihood, and informed consent. CleanTech's decision violates these rights by imposing a hidden, involuntary risk on the town without their knowledge, making the decision unethical under this lens regardless of the financial savings.

**(b) Whistle-Blowing Evaluation:**
To be permissible, all four conditions must be met:
1.  **Need (Clear harm):** Yes, there is a risk of severe agricultural harm.
2.  **Proximity:** Yes, Sarah is the engineer conducting the tests; she has firsthand knowledge.
3.  **Capability:** Yes, going to the press would likely force regulatory action.
4.  **Last Resort:** *Unmet.* The case does not state that Sarah escalated this to the CEO or Board of Directors, only to her immediate manager.
*   **Conclusion:** It is currently **unjustified**. She must exhaust internal channels first. Even if she does, because the harm is a decade away (not *imminent* or a direct threat to life), it would only be *permissible*, not *obligatory*.

**(c) Environmental Approaches:**
*   **Cost-Oblivious:** Seeks to make the environment as clean as possible regardless of the financial cost (driven by Rights/Duty ethics).
*   **Cost-Benefit:** Balances pollution reduction against economic costs. CleanTech is using a heavily skewed Cost-Benefit approach (prioritizing the $4M cost savings over downstream safety) and relying entirely on the "legal minimum" rather than ethical engineering practice.
</details>

---

### 🔵 Case Study 2: AutoDrive’s Phantom Braking
**Topics Covered:** Risk Perception, Types of Accidents, Codes of Ethics, Computer Ethics (Chapters 2, 5, 7)

**The Case:**
AutoDrive Inc. is developing an autonomous trucking system. During late-stage software testing, Lead Engineer Marcus discovers a bug: the truck occasionally interprets shadows from overpasses as solid objects, causing "phantom braking" at highway speeds. 

Marcus insists they delay the software release to rewrite the object-detection algorithm and add hardware lidar redundancy. The VP of Engineering denies the request, stating: *"Competitor X is launching next month. We cannot lose our market advantage. Push the software as-is, and we will issue a patch in three months. If it phantom brakes, the human safety driver is supposed to take over anyway. Just put a warning in the manual."*

**Questions:**
**(a) [6 marks]** If a phantom braking incident occurs and causes a fatal rear-end collision on the highway, how would you classify this accident (Procedural, Engineered, or Systemic)? Provide reasoning for your classification.
**(b) [8 marks]** Marcus faces an internal conflict between **NSPE Canon 1** (Paramountcy of public safety) and **NSPE Canon 4** (Faithful agent to employer). According to the structure of professional codes, how must this conflict be resolved, and why is the VP's reliance on the "warning manual" ethically insufficient?
**(c) [6 marks]** Analyze the VP's perception of risk using two specific **Risk Perception Factors** from the syllabus. Why does the VP perceive the risk as acceptable, while the public driving behind the truck would perceive it as highly dangerous?

<details>
<summary><b>► View Answer / Hints for Case Study 2</b></summary>

**(a) Accident Classification:**
*   This is an **Engineered** failure (combined with Procedural). The primary cause is a known flaw in the software design (the algorithm failing under real-world lighting conditions) and a lack of hardware redundancy (missing lidar). While one could argue "Procedural" if the human driver fails to take over, the root cause is the engineered software bug forcing the emergency.

**(b) Resolving the Code Conflict & Warning Labels:**
*   **Resolution:** Professional codes possess a strict hierarchy. **Canon 1 (Public Safety) always overrides Canon 4 (Employer Loyalty).** Marcus must refuse to sign off on the release because the design is actively unsafe.
*   **Warning Insufficiency:** A warning label or manual instruction is *never* a substitute for a safe design. Relying on user perfection (the human driver perfectly taking over in a split second) to mask a known software flaw violates the duty to design for foreseeable misuse.

**(c) Risk Perception Factors:**
*   **Voluntary vs. Involuntary:** The VP is taking a *voluntary* financial/business risk. The drivers on the highway are subjected to an *involuntary* risk they did not consent to. Involuntary risks are perceived as drastically more dangerous.
*   **Expected Probability / Reversibility:** The VP assumes the probability of a crash is low because the human driver will intervene. However, the potential consequence (fatal highway crash) is *irreversible*. 
</details>

---

### 🟠 Case Study 3: The Cloud Migration Contract
**Topics Covered:** Conflict of Interest, Problem Solving (Line Drawing), Contract Bidding (Chapters 4, 6)

**The Case:**
David is a senior systems architect for a government agency, tasked with selecting a vendor for a massive $50 million cloud migration project. David previously worked for "Vendor A," one of the bidding companies, and he still owns $20,000 worth of stock in Vendor A. 

During the bidding process, Vendor A invites David to an all-expenses-paid "technology showcase retreat" at a luxury resort in Hawaii to view a demonstration of their system. Vendor A also claims their system is completely "off-the-shelf and ready to deploy." However, David privately knows from his former colleagues that Vendor A is still building the software and plans to use a competitor's disguised hardware for the demo (strikingly similar to the Paradyne Computers case). 

**Questions:**
**(a) [6 marks]** Identify and explain how an **Actual** and an **Apparent** Conflict of Interest exist in David’s situation. 
**(b) [8 marks]** Use the **Line-Drawing Technique** to evaluate the ethics of David accepting the all-expenses-paid Hawaii trip. Define a Negative Paradigm (NP), a Positive Paradigm (PP), and place the trip on the spectrum with justification.
**(c) [6 marks]** Based on the **Paradyne Computers** case study, identify two specific ethical violations Vendor A is committing with their planned demonstration, and explain why this behavior is harmful to the engineering profession.

<details>
<summary><b>► View Answer / Hints for Case Study 3</b></summary>

**(a) Conflicts of Interest:**
*   **Actual COI:** David owns $20,000 in stock in Vendor A. If he awards them the $50M contract, his personal financial wealth will directly increase. This compromises his objective judgment right now.
*   **Apparent COI:** Even if David were completely objective, traveling to a luxury resort in Hawaii funded by a bidding vendor *looks* like bribery to the public and competing vendors, destroying trust in the government bidding process.

**(b) Line-Drawing the Hawaii Trip:**
*   **Negative Paradigm (NP):** Vendor A secretly hands David an envelope with $10,000 in cash to guarantee they win the contract (Blatant Bribery).
*   **Positive Paradigm (PP):** Vendor A sends David a PDF brochure, and David evaluates it from his government office (Standard business).
*   **Evaluation:** The Hawaii trip lies very close to the **Negative Paradigm**. While it is framed as a "technology showcase," the high financial value of the luxury trip, the timing (during an active bidding process), and the intent to influence his decision make it functionally equivalent to a bribe. David must decline it.

**(c) Paradyne Case Violations:**
*   **Fraud / Misrepresentation:** Claiming a system is "off-the-shelf" when it is still in development is outright lying on a bid.
*   **Deceptive Acts (Falsifying the Demo):** Using a competitor's hardware and passing it off as their own to pass an inspection is deliberate fraud.
*   **Why it's harmful:** It destroys fair competitive bidding (a core professional responsibility), wastes public taxpayer money on failing systems, and replaces engineering competence with deception.
</details>
