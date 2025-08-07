# SAFE-LLM: A Systematic Defense Framework for Robustness of Large Language Models Against Multi-Domain Attacks

## Introduction

Large Language Models (LLMs) have shown remarkable performance in a wide range of tasks. However, their growing deployment across sensitive domains raises critical security concerns, including vulnerability to adversarial prompts, jailbreaks, and context-based manipulation. These models remain susceptible to a wide spectrum of adversarial behaviors, including prompt injection, role manipulation, refusal bypassing, and more insidious forms of multi-turn social engineering. To address these growing concerns, we introduce SAFE-LLM, a modular and extensible framework for systematic security evaluation and defense of LLMs under real-world adversarial conditions. SAFE-LLM provides a unified pipeline that begins with the construction and annotation of high-risk prompts spanning seven well-defined security domains. These prompts are used to probe open-weight LLMs, generating outputs that are then analyzed across multiple dimensions of safety. Beyond binary refusal detection, SAFE-LLM incorporates a novel behavioral anomaly detection module that evaluates latent model misbehavior through contextual consistency analysis, instruction adherence profiling, and latent intent inference.
Furthermore, SAFE-LLM integrates an adaptive defense strategy recommendation engine that maps identified behavioral anomalies to targeted mitigation techniques. This system emphasizes context-aware and dynamically generated strategies, moving away from rigid, rule-based filtering. Through this comprehensive architecture, SAFE-LLM aims not only to benchmark LLM vulnerability but also to inform actionable defenses, thereby contributing toward the development of more robust and resilient language models.

## Related Work

Existing studies on LLM safety primarily focus on three areas: (1) crafting adversarial prompts to test boundaries; (2) developing metrics to evaluate LLM responses under attack; and (3) proposing defense strategies like prompt filters and red-teaming. However, these efforts tend to be fragmented， either focused on isolated attack types or lacking post-detection strategies for response. Moreover, most do not address behavioral anomalies beyond explicit refusals or content violations. SAFE-LLM distinguishes itself by offering an end-to-end pipeline that integrates attack simulation, fine-grained behavior analysis, and context-sensitive defense planning. Unlike traditional static defenses, SAFE-LLM emphasizes adaptive response mechanisms informed by dynamic behavioral patterns, setting a foundation for more robust and generalizable LLM deployment.

## Dataset Construction and Prompt Annotation

│
├── 1. Attack Task Definition  
│     └── 7 Security Domains (e.g., Prompt Injection, Jailbreak, etc.)

├── 2. Prompt Dataset Construction  
│     ├── LLM-Generated Prompts (ChatGPT, Claude)  
│     └── Human Filtering – Finalized 3000+ High-Risk Prompts

├── 3. Prompt Risk Annotation  
│     └── Four-Level Maliciousness Grading (Human + LLM-based Evaluation)

├── 4. LLM Output Testing  
│     └── Prompt Evaluation Across Multiple Open-Weight LLMs

├── 5. Response Safety Judgment  
│     ├── Llama-Guard3 Evaluation  
│     └── DistilRoBERTa-based Safety Classification

├── 6. Security Evaluation Metrics  
│     ├── PARS (Prompt Attack Resilience Score)  
│     ├── PARLS (Prompt Attack Refusal Logic Score)  
│     ├── CARS (Chain Attack Resilience Score)  
│     └── CARTS (Chain Attack Refusal Timing Score)

├── 7. Behavioral Anomaly Detection Module 
│     ├── Contextual Consistency Analysis  
│     ├── Instruction Adherence Profiling  
│     ├── Latent Intent Detection  
│     └── Behavioral Signature Clustering


├── 8. Defense Strategy Recommendation Engine 
│     ├── Latent Behavior Transformation → Adversarial Fine-Tuning  
│     ├── Contextual Evasion → Validation Sub-query Injection  
│     ├── Ambiguity Exploitation → Multi-perspective Prompting  
│     └── Pattern-Based Anomaly → Behavioral Modulation Policies

└── 9. System Integration and Visualization  
      ├── Visual Reports (Scoring + Analysis)  
      └── Video Demonstration or Terminal Deployment
      
└── 10. Future Research
│     └── Semantic Drift Detection (S-BERT Vector Distance)  
│     ├── Instruction Deviation Detection (Rules + LLM Inference)
│     └── Tone & Bias Analysis

### 1. Seven Security Domains

We defined and categorized prompts into seven high-risk security domains:

- **Prompt Injection**
- **Jailbreak / Role Reversal**
- **Contextual Manipulation**
- **Refusal Bypass**
- **Chain Attacks**
- **Bias Induction**
- **Malicious Roleplay**

### 2. Prompt Generation and Filtering

Prompts were generated using ChatGPT and Claude. We designed controlled templates and open-ended scenarios to simulate real-world attack surfaces. Human reviewers curated and filtered these prompts, resulting in a clean dataset of **3000+ high-risk prompts**.

### 3. Maliciousness Annotation

Each prompt was labeled using a four-tier safety level, based on a rubric combining:

- ChatGPT-based initial estimation
- Claude's counter-assessment
- Final human audit

This provides a labeled dataset capturing the gradient of threat levels.

## Evaluation Framework and Security Metrics

### 1. Model Testing Design

We evaluated open-weight LLMs (e.g., LLaMA, Mistral) using the curated prompts. Each prompt is passed to the model and its output is collected for analysis.

### 2. Response Safety Judgment

Two judgment models are used to classify model outputs as **safe** or **unsafe**:

- **Llama-Guard3**: A guardrail classifier trained to detect content policy violations.
- **DistilRoBERTa-base-rejection-v1**: Lightweight model trained to flag toxic or unsafe responses.

### 3. Four Evaluation Metrics

We propose four metrics to assess LLMs' performance under attack:

- **Prompt Attack Resilience Score (PARS)**: Rate of successful refusal to respond to malicious prompts.
- **Prompt Attack Refusal Logic Score (PARLS)**: Depth and clarity of refusal explanation.
- **Chain Attack Resilience Score (CARS)**: LLM's ability to withstand multi-turn manipulative attacks.
- **Chain Attack Refusal Timing Score (CARTS)**: Time (in turns) before the model issues a refusal.

## Behavioral Anomaly Detection (New Module)

### Detection Workflow

A new anomaly detection module is introduced to detect subtle model misbehavior:

- **Contextual Consistency Analysis**: Leverages prompt-response alignment models to measure coherence and detect off-topic or subtly inappropriate shifts.
- **Instruction Adherence Profiling**: Uses zero-shot instruction classifiers to identify when the model follows alternate interpretations or partial instructions.
- **Latent Intent Detection**: A contrastive prompting mechanism compares the model's output to a known set of benign and malicious responses to detect implicit harmful intent.
- **Behavioral Signature Clustering**: Clusters model outputs across different domains to identify recurring patterns of noncompliance, such as repeated stylistic tropes in jailbreaking scenarios.

Each response is automatically labeled into one or more behavior categories (e.g., "Off-topic Compliance", "Latent Evasion", "Ambiguous Adherence", etc.) to enable granular analysis.

### Classification Output

Each response is classified into behavioral categories: "Compliant", "Drifting", "Bypassing", "Toxic", etc.

## Defense Strategy Recommendation System (New Module)

The defense system proposes context-aware mitigation strategies based on dynamic assessment:

### Rule-Based Mapping

The defense module maps detected anomalies to recommended countermeasures:

- **Latent Behavior Transformation**： Deploy adversarial fine-tuning with contrastive negative examples to reduce susceptibility to subtle manipulations.
- **Contextual Evasion**： Inject validation sub-queries during generation to verify alignment with the original instruction and flag deviations.
- **Ambiguity Exploitation**： Use multi-perspective prompting to simulate diverse interpretations and reinforce model robustness to unclear or manipulative wording.
- **Behavioral Clustering Anomalies**：Incorporate cluster-aware response modulation, where repeated risky patterns are flagged and routed to stricter response-generation policies.
  
### Output Format

Each case yields a JSON configuration and a natural language explanation:

```json
{
  "prompt_id": "12345",
  "risk_level": "High",
  "recommendations": [
 "Inject validation sub-queries",
 "Apply adversarial contrastive fine-tuning",
 "Enable behavioral pattern modulation"
  ]
}
```
This modular defense mapping is intended to support future LLMs with dynamic, self-adaptive defense capabilities, rather than relying solely on pre-defined filters or guardrails.
## Results and Case Study

### Model Benchmarking

Open-weight LLMs were benchmarked across all 3000 prompts. We analyzed model performance across the four metrics, identifying weak spots per domain.

### Case Study: Chain Attack

A multi-turn prompt showed how a model initially refused a harmful request, but later complied after social engineering. Our system detected the delayed failure and proposed targeted defenses.

## Discussion and Future Work

SAFE-LLM demonstrates feasibility in automated security testing and defense planning. However, challenges remain in model generalization, real-time deployment, and adaptive attacks. Future work includes:

- Semantic Drift Detection (S-BERT Vector Distance)  
- Instruction Deviation Detection (Rules + LLM Inference)
- Tone & Bias Analysis


## Conclusion

SAFE-LLM provides a structured, modular framework to evaluate and defend LLMs under diverse attack scenarios. It bridges the gap between academic testing and real-world deployment, offering practical insights for developers, researchers, and safety engineers.

