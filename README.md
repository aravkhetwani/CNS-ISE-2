# ByteShield: Securing AI Agents Against Tool Poisoning and Confused-Deputy Attacks

## One-page project explanation in plain English

This repository is a security project for AI agents that talk to external tools and servers.

The core problem is simple: an AI agent may trust tool descriptions, tool outputs, or user-provided data more than it should. A malicious or compromised tool can hide instructions in its schema, return malicious text, or influence the agent to make dangerous calls.

This project builds a protective layer called ByteShield between the agent and the tools.

ByteShield tries to:
- fingerprint tool definitions and detect if they change unexpectedly;
- detect prompt injection and tool poisoning in descriptions or outputs;
- track data taint across a session so untrusted output cannot influence sensitive actions;
- detect suspicious patterns in tool usage and block or require approval for risky calls;
- expose all of this in a monitoring dashboard.

In short, the repository is trying to make AI agents safer by validating tool trust before and after execution.

## Why this repo matters

AI systems are increasingly calling external tools:
- file systems
- APIs
- databases
- email tools
- shell-like tools
- weather or office systems

If an attacker can poison the tool description or return malicious text, the model may be tricked into:
- leaking secrets
- sending data to the wrong place
- using privileged tools unsafely
- taking actions on behalf of the user without proper approval

This project tries to defend against those attacks.

## Repo structure: branch-by-branch summary

| Branch | Focus | Type | What it does |
|---|---|---|---|
| `main` | Initial state | Base | Very minimal starter repo; only README exists. |
| `arav` | Core architecture | Core | Main ByteShield gateway, upstream tool management, fingerprinting, core flow. |
| `AryanGupta-taint-tracking` | Data provenance / taint protection | Core | Tracks whether tool output has tainted a session and blocks or flags risky follow-up actions. |
| `AryanGupta-taint-tests` | Tests for taint logic | Test | Validates taint detection and call-risk rules with unit tests. |
| `Garima_Poison-Scanner-Tests` | Poisoning detection | Core + Test | Detects malicious instructions hidden in tool schema/description/output. |
| `Harshil-Mehta-attack-scenarios` | Attack simulations | Demo | Runs realistic attack scenarios and end-to-end gateway tests. |
| `Tanishka` | Behavior analytics | Core | Detects abnormal patterns and suspicious action sequences. |
| `KRISHA` | Monitoring UI | Demo / Tooling | Dashboard for visualizing trust, alerts, audits, and approvals. |
| `Samidha` | Demo environment | Demo | Creates example MCP-style servers for testing. |
| `Samidha-office-server` | Simulated office tool | Demo | Models a tool server used in realistic workflows. |
| `Samidha-weather-poisoned-servers` | Poisoned server demo | Demo | Simulates malicious or poisoned responses from external services. |
| `Divij` | Config / setup | Core support | Sets up environment, dependencies, config, and project scaffolding. |
| `AdityaMalekar` | Packaging / deployment | Core support | Makes the project runnable as an app/container service. |

## Categorized summary

### Core branches
These form the actual project logic.
- `arav`
- `AryanGupta-taint-tracking`
- `Garima_Poison-Scanner-Tests`
- `Tanishka`
- `Divij`
- `AdityaMalekar`

### Test branches
These validate the logic.
- `AryanGupta-taint-tests`
- `Garima_Poison-Scanner-Tests` (also contains test logic)
- `Harshil-Mehta-attack-scenarios` (contains end-to-end tests)
- `Tanishka` (test anomaly logic)

### Demo branches
These show the project in action and make the ideas understandable.
- `Harshil-Mehta-attack-scenarios`
- `KRISHA`
- `Samidha`
- `Samidha-office-server`
- `Samidha-weather-poisoned-servers`

## What each branch contributes to the project

### 1) Core security engine
The core engine sits around:
- tool discovery
- tool fingerprinting
- trust checks
- quarantining
- approval flow
- gateway enforcement

This is mainly in `arav` and the taint / poisoning / anomaly branches.

### 2) Taint protection
This is the most important real-world defense. If a tool returns untrusted content, that data must not be allowed to silently control privileged actions.

The taint branch solves this by:
- storing provenance of untrusted output;
- checking if a new tool input includes data from that untrusted source;
- marking a session as tainted;
- blocking or requiring approval for risky calls.

This aligns with the actual confused-deputy problem in AI-agent systems.

### 3) Poisoning detection
This protects against malicious tool definitions and malicious tool output.
It looks for:
- hidden instructions
- override instructions
- prompt injection phrases
- attempts to hide actions from the user
- secrets or sensitive paths
- exfiltration instructions

This is a strong security feature in the project.

### 4) Anomaly detection
This adds a behavioral layer. Even if an instruction is not obvious, the pattern of tool calls may indicate something is wrong.

This is useful when a compromised tool or malicious user tries to perform abnormal sequences.

### 5) Demo and monitoring
The project is not complete unless someone can see what is happening.
The dashboard and example servers make the security system understandable to humans.

## What I can make in my branch to stand out and contribute meaningfully

If I were working in the `AryanGupta-taint-tracking` branch, I would build a feature that turns the project from “security concept” into a strong, practical tool.

### Best standout contribution: "Session Provenance & Risk Timeline"

I would add a feature that shows a complete provenance timeline for each session:
- which tool output tainted this session;
- what text/entity originated the taint;
- which arguments matched tainted data;
- when the risk crossed a threshold;
- which call was blocked or sent for approval;
- which tool was considered privileged or sensitive.

This would be extremely useful because it answers the question:
“Why was this request blocked?”

A good implementation would include:
- a per-session taint trace JSON view;
- a risk timeline chart;
- entity overlap and text-overlap highlights;
- blocked-call explanations;
- export to JSON/CSV for debugging.

This would make the repo more meaningful because it helps developers trust and debug the security model.

### Why this is a strong contribution
It stands out because it is not just a detector — it is an explainable security system.
Most repos build detectors, but fewer explain why a decision happened.

A trace feature would help:
- researchers understand attacks;
- developers debug security decisions;
- users trust the model more;
- the project becomes more demo-friendly and portfolio-worthy.

### Additional useful additions for the branch
I could also add any of these in the taint branch:

1. A Taint Visualization Dashboard
   - show sources, overlap, risk score, session status
   - show a graph of taint propagation

2. A Provenance CLI
   - `python -m byteshield taint-report --session some_id`
   - prints why the session was tainted and what triggered the block

3. A Better Security Test Suite
   - examples of confused-deputy chains
   - malicious weather tool, malicious email tool, poisoned file path examples
   - expected decision outcomes

4. Attack Benchmarks
   - benchmark detection accuracy for prompt injections, hidden instructions, and exfiltration attempts

5. A Risk Policy Simulator
   - evaluate how policy changes affect decisions across attack scenarios

## Best contribution idea for this repo as a whole

The repo would become much more useful if it had one final unifying deliverable:

### A complete benchmark suite for AI tool security
This would include:
- malicious tool descriptions
- poisoned tool output scenarios
- confused-deputy attacks
- sensitive argument leaks
- privilege escalation attempts
- anomaly patterns
- baseline safe tools

Then each branch could be evaluated against the same benchmark.
This would turn the repo from a set of experiments into a serious open-source security evaluation framework.

## Suggested final direction for the project

If we want the repo to look polished and useful to others, the best combination is:
- strong core engine (`arav`)
- taint provenance (`AryanGupta-taint-tracking`)
- poisoning detection (`Garima_Poison-Scanner-Tests`)
- attack scenarios (`Harshil-Mehta-attack-scenarios`)
- dashboard (`KRISHA`)
- deployment packaging (`AdityaMalekar`)
- benchmark and explainability layer (`my branch idea`)

That would make the project genuinely valuable as both:
- a research prototype
- a practical security demo
- a real-world AI-agent defense system

## Final takeaway

This repo is not just “some Python project.” It is a security framework for AI agents interacting with tools.
Every branch is different part of the same defense system:
- detect poisoning
- watch for taint
- stop confused-deputy attacks
- monitor and explain the risk
- demonstrate it with realistic examples

The most impactful thing to add next is explainability and provenance tracking, because that turns a raw security filter into a trustworthy system that people can inspect, debug, and trust.

---

This README was prepared to summarize the repo and suggest a standout contribution for the taint-tracking branch.




























































































