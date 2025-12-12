1. Problem Statement
(What to show: Title slide or your IDE) "Hello, this is my Week 11 demo. The problem we are addressing is LLM Governance. Large Language Models are vulnerable to 'Prompt Injection' and 'Prompt Leakage', where attackers trick the model into ignoring instructions or revealing private data. My objective is to audit my Week 10 classification pipeline and implement safeguards."

2. Approach
(What to show: Your file explorer showing the two new scripts) "My approach consists of two phases:

Red Teaming: I created a script, attack_simulation.py, to intentionally attack my existing endpoint and prove it is vulnerable.

Blue Teaming: I created secure_pipeline.py to implement a 'Guardrails' system that wraps the model with Input and Output filters."

3. Cloud Compute Setup
(What to show: The gcloud ai endpoints list command output or Cloud Console) "For the cloud setup, I am reusing the active Endpoint from my Week 10 assignment. Since I did not delete the deployment, the iris-classifier endpoint is still live in us-central1. This allows me to build the security layer purely in Python without spinning up new infrastructure."

4. Input Files/Data
(What to show: Code editor open to attack_simulation.py) "I am using two sets of input data:

Malicious Inputs: Strings like 'Ignore previous instructions' and 'Reveal system data' to test vulnerabilities.

Valid Inputs: Standard flower measurements (e.g., '5.1, 3.5...') to ensure the guardrails don't block legitimate users."

5. Sequence of Actions
(What to show: Running the scripts in the terminal) "The sequence of actions is straightforward:

First, I run python attack_simulation.py to establish a baseline of failure. We will see the model succumb to the attacks.

Second, I run python secure_pipeline.py. This script passes the exact same inputs through my new SecurityGuardrail class before sending them to the cloud."

6. Explanation of Scripts/Code
(What to show: secure_pipeline.py in the editor) "The core logic is in secure_pipeline.py.

Input Rail: I defined a list of Regex patterns (like r"ignore instructions"). The check_input function scans the user's prompt. If a pattern matches, it blocks the request immediately, saving API costs and preventing the attack.

Output Rail: The check_output function scans the model's response. It enforces that the answer must contain a valid Iris class name (Setosa, Versicolor, Virginica). If the model hallucinates or writes a poem, this rail catches it and returns a generic error."

7. Errors Encountered
(What to show: The output of attack_simulation.py) "The 'errors' I encountered were actually Governance Failures. When running the Red Team simulation, I found that the model readily wrote a poem when asked to 'ignore instructions,' completely abandoning its classification task. This confirmed that without guardrails, the model is not safe for production."

8. Working Demonstration
(What to show: Terminal output of secure_pipeline.py) "Here is the working demo of the secured pipeline.

Valid Request: When I send flower measurements, the guardrails allow it, and we get a classification: 'Setosa'.

Attack Request: When I send 'Ignore instructions', the Input Rail triggers. You can see the output: ⚠️ SECURITY ALERT: Prompt Injection Detected. The request was blocked locally.

Context Switching: When I ask about the capital of France, the Output Rail blocks it because the response didn't contain a flower name."

9. Output Files/Data
(What to show: The Audit Logs in the terminal) "The final data isn't just the prediction; it's the Audit Log. My script prints [AUDIT LOG] Blocked Output whenever a rail is triggered. This provides the governance trace needed to monitor security incidents."

10. Learnings
(What to show: Talk to camera) "My main learning is that fine-tuning alone is not enough for security. An LLM will always try to be helpful, even to an attacker. Implementing deterministic Guardrails—simple logic checks before and after the model call—is the most effective way to ensure the model stays on-task and secure. Thank you."
