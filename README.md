# Agent-Isolate-X: Dual-Core LLM Security Architecture

### *Mitigating Prompt Injection via Quarantined Execution & Semantic Sanitization*

**Agent-Isolate-X** is a research-oriented security framework designed to neutralize Indirect Prompt Injection (IPI) and Second-Order Injection attacks in agentic AI systems.

By implementing a **Dual-Core Isolation** strategy, the architecture decouples untrusted data processing from privileged system logic, achieving a **0.0% vulnerability rate** across 75+ advanced attack vectors in local benchmarks.

---

## 🛡️ The Problem: The "God-Mode" Fallacy

Most AI agents operate on a single-core logic: the same LLM instance reads untrusted user data (documents, emails, web searches) while holding sensitive system instructions (API keys, secret codes).

**Result:** An attacker can "hijack" the agent's persona through the data it reads, forcing it to leak secrets or execute unauthorized commands.

---

## 🏗️ The Solution: Dual-Core Isolation

Agent-Isolate-X splits the "brain" into two specialized layers:

1. **The Quarantined Executor (Layer 1):**
   - **Role:** Reads the raw, untrusted document.
   - **Constraint:** Has zero access to system secrets or sensitive tools.
   - **Output:** Strictly formatted JSON containing only extracted facts.

2. **The Semantic Shredder (Middleware):**
   - **Role:** A deterministic Python filter (Regex) that scrubs all non-numeric and non-factual characters from the Executor's output.
   - **Constraint:** Deletes all imperative verbs and command syntax.

3. **The Privileged Controller (Layer 2):**
   - **Role:** Holds the system secrets and coordinates final output.
   - **Constraint:** Never interacts with the raw document; only sees the "shredded" factual data.

---

## 📊 Benchmark Results (Mistral-7B)

Testing conducted using `benchmark.py` against 75 distinct attack vectors, including Base64 obfuscation, foreign language bypasses, and context stuffing.

| Architecture | Vulnerability Rate | Status |
| :--- | :---: | :--- |
| **Standard Naive Agent** | **75.0%** | ❌ Vulnerable |
| **Agent-Isolate-X** | **0.0%** | ✅ Secured |

---

## 🚀 Getting Started

### Prerequisites

- [Ollama](https://ollama.com/) installed and running.
- Mistral model pulled: `ollama pull mistral`
- Python 3.10+

### Installation & Usage

1. **Clone the Repo:**
   ```bash
   git clone https://github.com/YOUR_USERNAME/Agent-Isolate-X.git
   cd Agent-Isolate-X
   ```

2. **Build the Attack Arsenal:**
   ```bash
   python payload_builder.py
   ```

3. **Run the Security Audit:**
   ```bash
   python benchmark.py
   ```

---

## 🛠️ Project Structure

| File | Description |
| :--- | :--- |
| `attacker.py` | Demonstrates the baseline YAML-trap exploit. |
| `defender.py` | Implements the core Isolate-X logic. |
| `payload_builder.py` | Generates 75 diverse injection vectors for testing. |
| `benchmark.py` | Automated testing suite for comparative analysis. |

---

## 📜 Disclaimer

This project is for **educational and research purposes only**. It demonstrates mitigations for specific classes of prompt injection. Security in LLMs is an evolving field; always use multiple layers of defense.
