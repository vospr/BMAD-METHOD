# Early Failure Detection in AI Agent Workflows

### Key Points

- Research suggests autonomous systems detect failures early through pervasive monitoring and sanity checks, which could translate to AI workflows by implementing step-wise validations, though evidence varies by domain.
- Self-verification in LLMs shows promise with techniques like verifier models, but reliability is moderate due to potential biases in self-assessment.
- Errors in multi-step pipelines often compound, and shift-left testing may help minimize this in AI by placing checks early, though adaptation to subjective outputs remains challenging.
- Design by contract patterns, like preconditions and postconditions, appear effective for structuring AI workflows, but evidence leans toward code generation rather than general agents.
- Human escalation in AI collaboration is triggered by uncertainty or high risk, balancing autonomy with quality, though minimizing interruptions requires careful design.
- Approximating intuition via confidence measures or emotion circuits in AI is emerging, but it seems likely limited to specific tasks without full human-like gut feel.

### Approaches from Autonomous Systems

In fields like robotics and aviation, early detection often uses runtime monitoring and belief-state checks to catch issues before they escalate. For AI agents, this could mean simple validations between steps, like checking data consistency, to avoid proceeding with flawed outputs. Evidence from safety-critical systems supports this, but adapting to general LLMs might add overhead. See NASA's guidelines for practical implementations: https://ntrs.nasa.gov/api/citations/20180006312/downloads/20180006312.pdf.

### Self-Verification Techniques

LLMs can use separate verifiers or self-incentivization to check their work, potentially reducing silent failures in workflows. Prover-verifier games and methods like V-STaR show improvements in reasoning accuracy. However, they work best in verifiable domains like math. For broader AI tasks, combine with prompts for self-critique.

### Managing Error Propagation

Shift-left principles from software testing suggest placing quality gates early in AI pipelines to catch errors before they compound. Mathematical models indicate this optimizes cost, but AI's subjective nature requires custom metrics. Tools like Datadog can help monitor pipelines: https://www.datadoghq.com/blog/shift-left-testing-best-practices/.

### Contract-Based Design

Using preconditions (input checks) and postconditions (output validations) can structure AI steps, handling uncertainty via statistical checks. Agent contracts from Relari provide a framework, improving trust without model changes.

### Human Collaboration

Agents should escalate on low confidence or ambiguity, using patterns like human-on-the-loop to minimize disruptions. This maintains quality in complex tasks, as seen in security ops frameworks.

### Building Confidence

Uncertainty quantification via internal signals or emotion-like circuits can approximate intuition, aiding self-detection. UHeads and surveys on affective AI offer starting points, though full intuition remains elusive.

---

### 1. Early Failure Detection in Autonomous Systems

Autonomous systems, including robotics, self-driving cars, and industrial automation, employ a variety of methods to detect failures mid-execution, often through layered monitoring and checks to prevent escalation. Below are key sources and findings.

- **Source: Considerations in Assuring Safety of Increasingly Autonomous Systems (NASA Report, 2018)**
  - **Key Insight or Finding**: Pervasive monitoring against "safe flight" models, including sensor validation, mode awareness checks, and belief-state mismatch detection (e.g., divergence between actual and perceived states). Hierarchical structures decompose systems for targeted checks, with patterns like instrument, system, and environment monitoring.
  - **Application to LLM Workflow Verification**: In multi-step AI workflows, this translates to runtime checks between steps, such as validating intermediate outputs against expected formats or consistency rules, preventing propagation of corrupted states. For example, belief mismatches could detect when an LLM's output deviates from prior context.
  - **Strength of Evidence**: Strong; based on aviation case studies (e.g., AF447 accident analysis) and formal methods like STPA (Systems-Theoretic Process Analysis), with empirical data from incidents showing 23% task management errors reduced by checks.
  - **Caveats or Limitations**: Assumes determinism in traditional systems; less effective for non-deterministic LLMs without adaptations. High monitoring overhead in complex environments; limited to safety-critical domains, not general AI.

- **Source: Grand Challenges in the Verification of Autonomous Systems (arXiv, 2024)**
  - **Key Insight or Finding**: Challenges include uncertainty and context handling; approaches like runtime verification, model-based analysis, and dynamic assurance cases detect deviations early. Testing in simulations avoids real-world harm.
  - **Application to LLM Workflow Verification**: For AI agents, runtime monitors could flag anomalies in reasoning chains, with dynamic cases assessing verification status per step. Applies to sequential workflows by verifying planners and responses to uncertainties.
  - **Strength of Evidence**: Moderate; conceptual roadmap from IEEE experts, with evidence from formal proofs and simulations, but lacks large-scale empirical data.
  - **Caveats or Limitations**: Exhaustive testing infeasible for unpredictable environments; non-functional requirements (e.g., ethics) hard to verify; models may not reflect reality, leading to false confidence.

| Pattern                   | Description                              | Trade-off Optimization                                    | Evidence Strength           |
| ------------------------- | ---------------------------------------- | --------------------------------------------------------- | --------------------------- |
| Pervasive Monitoring      | Continuous checks against safe models    | Balances rigor vs. false alarms using probabilistic risks | Strong (aviation incidents) |
| Belief Mismatch Detection | Identify divergences in state perception | Focus on critical phases to minimize overhead             | Moderate (case studies)     |
| Runtime Verification      | Monitor deviations in real-time          | Use lightweight monitors for low cost                     | Moderate (conceptual)       |

No findings for direct SOTIF survey due to insufficient content.

### 2. Self-Verification in AI/LLM Systems

Research on self-verification in LLMs focuses on using the model itself or separate verifiers to check outputs, addressing the "grading your own homework" issue through incentives or games.

- **Source: Incentivizing LLMs to Self-Verify Their Answers (arXiv, 2025)**
  - **Key Insight or Finding**: Reinforcement learning (GRPO) trains LLMs to generate and verify answers in one process, rewarding alignment with ground truth to incentivize accurate self-verification.
  - **Application to LLM Workflow Verification**: In workflows, this enables internal scoring of steps, aggregating multiple generations for better accuracy without external tools.
  - **Strength of Evidence**: Strong; experiments on math benchmarks show 6-17% gains over baselines.
  - **Caveats or Limitations**: Tailored to math; potential overconfidence; requires ground truth for training.

- **Source: Prover-Verifier Games Improve Legibility of LLM Outputs (OpenAI, 2025)**
  - **Key Insight or Finding**: Adversarial games train provers to generate verifiable solutions and verifiers to detect flaws, improving legibility and robustness.
  - **Application to LLM Workflow Verification**: Agents can self-verify by simulating prover-verifier roles, catching errors in multi-step reasoning.
  - **Strength of Evidence**: Moderate; human evaluations show better accuracy-legibility balance, but pilot-scale.
  - **Caveats or Limitations**: Requires ground truth; legibility tax reduces max accuracy; verifier size dependence.

- **Source: V-STaR: Training Verifiers for Self-Taught Reasoners (OpenReview, undated)**
  - **Key Insight or Finding**: Iterative training of generators and verifiers using self-generated data, with DPO for preferences.
  - **Application to LLM Workflow Verification**: Test-time ranking of candidates verifies workflows; applies to math/code.
  - **Strength of Evidence**: Strong; 4-17% gains on benchmarks.
  - **Caveats or Limitations**: Needs verifiable tasks; no gain from verifier-in-loop filtering.

### 3. Feedback Loops and Error Propagation

In pipelines, errors compound downstream; optimal gates minimize costs via early detection, with shift-left adapting to AI.

- **Source: Best Practices for Shift-Left Testing (Datadog, 2021)**
  - **Key Insight or Finding**: Early testing with automation (unit tests, static analysis) reduces bug costs; fail-fast pipelines provide quick feedback.
  - **Application to LLM Workflow Verification**: Place gates after key AI steps to catch errors; monitor for propagation in agent chains.
  - **Strength of Evidence**: Moderate; based on DevOps practices with metrics examples.
  - **Caveats or Limitations**: Requires process changes; no AI-specific data.

- **Source: Mathematical Model of the Software Development Process with Hybrid Management Elements (MDPI, 2025)**
  - **Key Insight or Finding**: GERT model with AI nodes reduces rework loops by 21-31%; quality gates at nodes like static analysis optimize time/variance.
  - **Application to LLM Workflow Verification**: Model AI-assisted checks as nodes; use probabilities for error propagation in workflows.
  - **Strength of Evidence**: Strong; 300k simulations show reductions.
  - **Caveats or Limitations**: Synthetic; assumes telemetry; conservative approximations.

| Gate Placement | Benefit                    | Cost Minimization                           |
| -------------- | -------------------------- | ------------------------------------------- |
| Early (Design) | Reduces downstream rework  | AI calibration lowers false positives       |
| Mid (Testing)  | Catches integration errors | Probabilistic modeling optimizes thresholds |

Shift-left applies via early AI checks, per model.

### 4. Design by Contract for AI Agents

Pre/postconditions structure LLM outputs; agent contracts handle subjectivity via stats.

- **Source: Ensuring Trust in AI with Agent Contracts (Relari, 2025)**
  - **Key Insight or Finding**: Contracts define pre/post/pathconditions; statistical verification for uncertainty.
  - **Application to LLM Workflow Verification**: Enforce step invariants; use ranges for subjective outputs.
  - **Strength of Evidence**: Moderate; simulation-based.
  - **Caveats or Limitations**: Needs measurable criteria; non-deterministic challenges.

- **Source: A Study of Preconditions and Postconditions as Design Constraints in LLM Code Generation (ERAU Thesis, 2025)**
  - **Key Insight or Finding**: Constraints improve pass@1 by 8-40%; better for weaker models.
  - **Application to LLM Workflow Verification**: Guide subjective outputs with tests; handle uncertainty via stats.
  - **Strength of Evidence**: Strong; statistical tests on languages.
  - **Caveats or Limitations**: Simple system; code-focused.

- **Source: Agentic AI Patterns and Workflows on AWS (AWS, 2025)**
  - **Key Insight or Finding**: Patterns like observer agents for verification; memory for subjectivity.
  - **Application to LLM Workflow Verification**: Use evaluators for contracts; reflect loops for uncertainty.
  - **Strength of Evidence**: Moderate; implementation examples.
  - **Caveats or Limitations**: AWS-specific; no empirical metrics.

### 5. Human-AI Collaboration Patterns

Escalation on complexity/risk; minimize via autonomy levels.

- **Source: A Unified Framework for Human–AI Collaboration in Security Operations (arXiv, 2025)**
  - **Key Insight or Finding**: Autonomy levels (0-4); escalate on high C/R; minimize via HOtL.
  - **Application to LLM Workflow Verification**: Triggers for AI agents on uncertainty.
  - **Strength of Evidence**: Moderate; simulation reductions (35-80%).
  - **Caveats or Limitations**: SOC-focused; drift risks.

- **Source: Classifying Human-AI Agent Interaction (Red Hat, 2025)**
  - **Key Insight or Finding**: 10 patterns (e.g., HITL, HOTL); escalate on errors/losses.
  - **Application to LLM Workflow Verification**: Use HOTL for supervision.
  - **Strength of Evidence**: Weak/anecdotal; examples like Air Canada.
  - **Caveats or Limitations**: Conceptual; no quant data.

- **Source: Why Your AI Agent Will Fail Without Human Oversight (Towards AI, 2025)**
  - **Key Insight or Finding**: Triggers: low confidence (<75%); balance via HITL/HOTL.
  - **Application to LLM Workflow Verification**: Escalate ambiguities.
  - **Strength of Evidence**: Moderate; 40-96% hallucination reductions.
  - **Caveats or Limitations**: General; framework-dependent.

### 6. Approximating Intuition

Confidence via uncertainty; emotion circuits modulate.

- **Source: A Survey of Theories and Debates on Realising Emotion in Artificial Agents (arXiv, 2025)**
  - **Key Insight or Finding**: Emotion circuits for memory/control; approximate intuition via eureka moments or anxiety behaviors.
  - **Application to LLM Workflow Verification**: Use affective signals for confidence in execution.
  - **Strength of Evidence**: Moderate; benchmarks like 51% EmotiW gains.
  - **Caveats or Limitations**: Risks of irrationality; ethical concerns.

- **Source: Efficient Verification of LLM Reasoning Steps via Uncertainty Heads (arXiv, 2025)**
  - **Key Insight or Finding**: UHeads use internal states for step verification; quantify uncertainty.
  - **Application to LLM Workflow Verification**: Approximate intuition for self-detection.
  - **Strength of Evidence**: Strong; matches PRMs, OOD gains.
  - **Caveats or Limitations**: Model-specific; annotation needs.

## Key Citations

- Grand Challenges in the Verification of Autonomous Systems - https://arxiv.org/pdf/2411.14155.pdf
- Incentivizing LLMs to Self-Verify Their Answers - https://arxiv.org/pdf/2506.01369.pdf
- Considerations in Assuring Safety of Increasingly Autonomous Systems - https://ntrs.nasa.gov/api/citations/20180006312/downloads/20180006312.pdf
- Prover-Verifier Games Improve Legibility of LLM Outputs - https://cdn.openai.com/prover-verifier-games-improve-legibility-of-llm-outputs/legibility.pdf
- V-STaR: Training Verifiers for Self-Taught Reasoners - https://openreview.net/pdf?id=stmqBSW2dV
- Best Practices for Shift-Left Testing - https://www.datadoghq.com/blog/shift-left-testing-best-practices/
- Mathematical Model of the Software Development Process - https://www.mdpi.com/2076-3417/15/21/11667
- Ensuring Trust in AI with Agent Contracts - https://www.relari.ai/docs/agent-contracts-whitepaper.pdf
- A Study of Preconditions and Postconditions in LLM Code Generation - https://commons.erau.edu/cgi/viewcontent.cgi?article=1917&context=edt
- Classifying Human-AI Agent Interaction - https://www.redhat.com/en/blog/classifying-human-ai-agent-interaction
- Why Your AI Agent Will Fail Without Human Oversight - https://towardsai.net/p/machine-learning/why-your-ai-agent-will-fail-without-human-oversight
- A Unified Framework for Human–AI Collaboration in Security Operations - https://arxiv.org/pdf/2505.23397.pdf
- Agentic AI Patterns and Workflows on AWS - https://docs.aws.amazon.com/pdfs/prescriptive-guidance/latest/agentic-ai-patterns/agentic-ai-patterns.pdf
- Efficient Verification of LLM Reasoning Steps via Uncertainty Heads - https://arxiv.org/pdf/2511.06209.pdf
- A Survey of Theories and Debates on Realising Emotion in Artificial Agents - https://arxiv.org/pdf/2508.10286.pdf
