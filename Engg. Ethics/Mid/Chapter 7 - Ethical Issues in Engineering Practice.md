
## 🌱 1. Environmental Ethics

Environmental protection has become a core political and ethical issue. Professional codes require engineers to hold the safety of people and the environment paramount.

### Concepts and Moral Standing
*   **Sustainable Design:** Engineering that (1) limits environmental harm during use, (2) considers manufacturing and disposal, and (3) aims to maintain environmental quality and our quality of life.
*   **Moral Standing:** How we value the environment dictates our ethical obligations to it.
    *   *Anthropocentric View:* Nature matters only through its value to humans (e.g., saving a rare plant because it might cure a human disease).
    *   *Broader View:* Animals, plants, or the biosphere have intrinsic value in themselves, regardless of human utility.

### Two Approaches to Environmental Problems

| Feature | Cost-Oblivious Approach | Cost-Benefit Approach |
| :--- | :--- | :--- |
| **Goal** | Make the environment as clean as possible. | Balance pollution reduction against economic costs. |
| **Ethical Connection** | Rights and Duty Ethics | Utilitarianism |
| **Main Difficulty** | "As clean as possible" is difficult to define, fund, and enforce in modern society. | Lives, species, and scenic views are difficult to price accurately. |
| **Justice Problem** | Fails if resources are drained unnecessarily. | **Cost-Benefit Analysis must ask "Who pays and who benefits?"** (e.g., putting landfills in poor areas). |

> **⚠️ Limits of Competence:** Professional responsibility includes knowing when you are out of your depth. Engineers may not be qualified to judge every environmental consequence and should seek help from biologists, public-health experts, and physicians.

### 🏛️ Case Study: The Albuquerque–Isleta Pueblo Water Case
*   **The Conflict:** Albuquerque discharged treated wastewater into the Rio Grande meeting standard EPA regulations. Downstream, the Isleta Pueblo tribe (a sovereign nation) used the river water directly for religious ceremonies and sought an arsenic standard **roughly twice as strict** as the EPA requirement.
*   **The Arguments:** Albuquerque argued the $300 million cost was prohibitive and unnecessary. Isleta argued for their right to clean water for cultural/religious use.
*   **The Outcome:** The EPA and the U.S. Supreme Court supported Isleta. 
*   **Ethical Takeaway:** *Should legal compliance settle an engineer's recommendation when a downstream community requires cleaner water?* Meeting minimum legal standards does not always fulfill broader ethical obligations to all stakeholders.

---

## 💻 2. Computer Ethics

Computers do not create entirely new ethical problems; rather, they make familiar wrongdoing easier, larger, or harder to trace. 

### Two Categories of Computer Ethics

| Category | Examples | Central Issue |
| :--- | :--- | :--- |
| **1. Computer used to commit an unethical act** | Theft, hacking, privacy invasion, copyright violation, distributing viruses. | The computer is a tool to mask the criminal/make the crime impersonal and widespread. |
| **2. Computer used improperly as an engineering tool** | Unsuitable design software, unchecked output, unsafe embedded control. | **The engineer remains responsible** for the resulting design or system, regardless of the software used. |

#### Specific Issues:
*   **Privacy:** Centralized computer records make it incredibly easy for unauthorized users to access private data.
*   **Hacking:** Unauthorized access remains unethical even when the hacker is just "seeking a challenge" rather than trying to do harm.
*   **Copyright:** Copying software/media is easy and hard to detect, but it destroys the incentive for creators to produce new work.

### Computers as Design Tools
Using CAD/CAM, finite-element analysis, and structural modeling improves efficiency but comes with risks. Easy-to-use software can create a dangerous **"illusion of competence"** outside an engineer’s actual field of expertise.
*   *Fleddermann’s Requirements for Engineers:*
    1. Know the software’s limits.
    2. Use current versions and patches.
    3. Verify computer-generated results (e.g., using manual estimation).
> **Rule of Thumb:** Software can *never* substitute for good engineering judgment.

### Integration & Autonomous Computers
*   **Embedded Computers:** Remove humans from the control loop. Engineers must ensure software is adequately tested, humans can intervene when necessary, and **hardware redundancy** exists (safety shouldn't rely *solely* on software).
*   **Autonomous Computers:** Systems making decisions without humans (e.g., automated stock trading causing the 1987 crash, or autonomous military weapons). *Where failure can become disastrous, meaningful human control remains necessary.*

### 🏛️ Case Study: Therac-25 Medical Radiation Accidents
*   **The Incident:** Between 1985–1987, at least six patients were severely overdosed (up to $25,000$ rads in a $1\text{ cm}^2$ area) by the Therac-25 radiation machine.
*   **What Went Wrong:**
    *   *Software Bug:* Fast typing allowed the beam to activate before hardware reset.
    *   *Missing Protection:* Earlier models had physical hardware interlocks; Therac-25 relied purely on software.
    *   *Weak Communication:* Displayed "Malfunction 54" and "Underdose"—operators resumed treatment, thinking it failed, giving massive overdoses. 
    *   *Development Process:* Inadequately documented and tested.
*   **Ethical Takeaway:** A software error became physical harm. Engineers must anticipate user errors and build fail-safes (hardware interlocks). 

### 🏛️ Case Study: Avanti Corp. vs. Cadence Design Systems
*   **The Incident:** Cadence alleged that former employees who started Avanti incorporated $\sim 60,000$ lines of Cadence's proprietary code into competing Electronic Design Automation (EDA) products. Avanti executives faced criminal charges.
*   **Ethical Takeaway:** Highlights the difficult boundary between an employee's general skills/experience (which they *can* take with them) vs. trade secrets/source code (which they *cannot* take).

---

## 🔬 3. Ethics and Research

Research demands strict objectivity. Virtue ethics identifies **honesty** as the central research virtue. 
*   **Honesty in Investigation:** Avoid preconceived conclusions, change hypotheses when evidence requires it, maintain objectivity.
*   **Honesty in Reporting:** Report accurately, do not overstate conclusions, give proper credit to collaborators, and clarify results if they are later found to be incorrect.

### Pathological Science (Self-Deception)
Physicist Irving Langmuir coined "pathological science" to describe research where investigators trick themselves through wishful thinking and subjectivity, rather than intentional fraud. 
**Langmuir's 6 Characteristics:**
1. The effect is caused by a barely detectable agent.
2. The effect remains near the limit of detectability (researchers tend to discard data that doesn't fit).
3. Claims of great accuracy.
4. Fantastic theories contrary to experience.
5. Criticisms are answered with *ad hoc* excuses thought up on the spur of the moment.
6. The ratio of supporters rises to $\sim 50\%$ and then gradually falls to oblivion.

### 🏛️ Case Study: The N-Ray Case
*   **The Incident:** René Blondlot claimed to discover "N-Rays" by looking at subtle changes in spark brightness. When physicist R. W. Wood visited, he secretly removed the crucial prism from the machine. Blondlot's team *still* claimed to see the N-Rays perfectly.
*   **Ethical Takeaway:** Expectation can heavily shape observation. Subtle effects require strong controls, criticism, and independent confirmation.

### 🏛️ Case Study: Cold Fusion at Texas A&M
*   **The Incident:** Following the Pons-Fleischmann announcement, a Texas A&M lab (led by Bockris) reported producing tritium ($10^9$ atoms/ml) in tabletop cells.
*   **The Red Flags:** Tritium appeared at "unusually convenient times" (like when funding agencies visited). The lab had poor controls, and intentional spiking/contamination was suspected.
*   **Ethical Takeaway:** A researcher must investigate evidence *against* a desired result as seriously as evidence *supporting* it.

### 🏛️ Case Study: Ghostwriting of Research Articles
*   **The Incident:** Pharmaceutical companies pay "medical education" companies to draft literature reviews and papers, which are then published with prominent university professors listed as the sole authors.
*   **Ethical Takeaway:** Giving authorship to someone who didn't contribute, or hiding the contribution of someone with a heavy financial conflict of interest, is deeply dishonest and introduces severe bias into scientific literature.

---

## 📌 Summary Takeaways

*   **Environmental Decisions:** Require careful attention to sustainability, moral standing, financial cost, and the equitable distribution of burdens (who pays vs. who benefits).
*   **Computer-Assisted Design:** Engineers remain entirely responsible for the output of software tools and the safe integration of software into physical systems.
*   **Autonomous Systems:** Require human control mechanisms when a failure can cause serious or catastrophic harm.
*   **Systems Safety (Therac-25):** Software testing, hardware redundancy, accurate documentation, and usable warnings are matters of life and death.
*   **Research Ethics:** Relies entirely on the virtue of honesty in investigation, reporting, and proper attribution.