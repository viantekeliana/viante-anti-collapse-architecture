# viante-anti-collapse-architecture
Research prototype for assumption-aware, human-governed execution in autonomous systems.
README.md
LICENSE
CONTRIBUTING.md
src/
examples/

# Anti-Collapse Architecture (ACA)

**A framework for building resilient autonomous systems that degrade gracefully instead of failing catastrophically.**

Created by Rodney Manyakaidze (Keliana Vianté)

---

## What is this?

Anti-Collapse Architecture is an execution governance framework for autonomous and AI-assisted systems. It addresses a fundamental problem: most automated systems assume perfect information and fail catastrophically when those assumptions break.

ACA provides:

- **Explicit assumption tracking** - Make hidden assumptions visible and measurable
- **Confidence-based execution** - Actions require sufficient confidence in their dependencies
- **Graceful degradation** - Systems shift to safer modes as uncertainty increases
- **Human accountability** - Critical actions require human approval
- **Full auditability** - Every decision is logged and traceable

## The Problem

Consider a cybersecurity system that decides to isolate a network node:

- It assumes network telemetry is accurate
- It assumes the threat model is current
- It assumes the operator understands the full context
- **But it never checks if these assumptions are actually true**

When assumptions silently degrade, systems either:
1. Continue acting on bad information (dangerous)
2. Fail completely (also dangerous)

ACA provides a middle path: graceful degradation with human oversight.

## Quick Example

```javascript
const kernel = new AntiCollapseKernel("Security Operations");

// Register assumptions with confidence levels
kernel.addAssumption(
  "net_telemetry",
  "Network telemetry is reliable",
  0.95,
  AssumptionCategory.CRITICAL
);

kernel.addAssumption(
  "threat_intel",
  "Threat intelligence is current",
  0.70,
  AssumptionCategory.IMPORTANT
);

// Register an action with its dependencies
kernel.registerAction(
  "isolate_node",
  "Isolate compromised node",
  ["net_telemetry", "threat_intel"],
  criticalityLevel: 4
);

// Evaluate whether action is safe
const evaluation = kernel.evaluateAction("isolate_node");

if (evaluation.requiresApproval) {
  // System determined this needs human oversight
  kernel.executeAction("isolate_node", "Operator Rodriguez");
}
```

## Core Concepts

### 1. Assumptions as First-Class Objects

Every system operates on assumptions. ACA makes them explicit:

- **Description**: What are we assuming?
- **Confidence**: How certain are we? (0.0 - 1.0)
- **Category**: How critical is this? (CRITICAL / IMPORTANT / SUPPORTING)
- **History**: How has confidence changed over time?

### 2. Actions Declare Dependencies

Instead of hidden coupling, actions explicitly state what they depend on:

```javascript
Action: "Deploy security patch"
Requires: ["system_backup_verified", "maintenance_window_active"]
Criticality: 3
```

### 3. Confidence-Based Governance

The system evaluates actions based on:
- System state (NORMAL → DEGRADED → CRITICAL → SAFE_MODE)
- Confidence in required assumptions
- Action criticality
- Recent outcome history

### 4. Learning from Outcomes

When actions succeed or fail, assumption confidence adjusts:
- Success → slight confidence boost in dependencies
- Failure → confidence penalty in dependencies

The system learns from experience.

### 5. Temporal Decay

Assumptions naturally lose confidence over time unless validated. A network scan from yesterday is less reliable than one from 5 minutes ago.

## Current Status

This is a **working research prototype**. It demonstrates the core concepts and can be extended for real use cases.

**What works:**
- Assumption tracking and confidence management
- Action-assumption dependency linking
- State-based execution governance
- Human-in-the-loop approval flows
- Outcome-based learning
- Full audit logging

**What needs development:**
- Assumption dependency graphs (assumptions can depend on other assumptions)
- More sophisticated learning algorithms
- Real-time integration with monitoring systems
- Domain-specific extensions
- Performance optimization for production scale

## Use Cases

Potential applications include:

- **Cybersecurity operations** - Incident response with confidence tracking
- **Infrastructure automation** - Safe deployment and rollback
- **AI agent control** - Execution governance for autonomous agents
- **Cost optimization** - Automated cost reduction with safety bounds
- **Clinical decision support** - Medical recommendations with explicit uncertainty

## Installation

```bash
npm install
node examples/demo.js
```

## Project Structure

```
/src
  antiCollapseKernel.js    # Core ACA implementation
  models.js                 # Assumption, Action, State models
/examples
  cybersecurity_demo.js     # Incident response scenario
  infrastructure_demo.js    # Deployment automation example
/tests
  kernel.test.js            # Unit tests
/docs
  ARCHITECTURE.md           # Design principles
  CONTRIBUTING.md           # How to contribute
```

## Contributing

This is open research. Contributions are welcome:

- Try it in your domain and report what works/doesn't
- Extend it with new capabilities
- Improve the documentation
- Share use cases and scenarios
- Submit pull requests

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

## Attribution & Recognition

If you use this work in research, products, or services, please attribute:

```
Anti-Collapse Architecture (ACA)
Created by Rodney Manyakaidze (Keliana Vianté)
https://github.com/[your-username]/anti-collapse-architecture
```

If this work proves valuable to you—commercially or otherwise—I'd appreciate hearing about it. Fair recognition and compensation for genuinely useful work is always welcome.

## License

MIT License - see [LICENSE](LICENSE) for details.

You're free to use this in any way, including commercially. The MIT license is intentionally permissive because I'd rather see this work used and improved than locked away.

If you build something significant with this and want to give back, consider:
- Contributing improvements back to the project
- Sharing case studies of how you used it
- Acknowledging the work in your project
- Reaching out if you'd like to collaborate

## Why I Built This

I developed ACA because I kept seeing the same pattern: intelligent systems failing not because they lacked capability, but because they acted on unstated assumptions that turned out to be false.

The architecture emerged from thinking about how systems should behave when they're uncertain—not optimistically assuming everything is fine, and not pessimistically refusing to act, but **explicitly reasoning about confidence and degrading gracefully**.

If you find value in this approach, use it. If you improve it, share it back. If it helps you build something important, let me know.

## Contact

- **GitHub**: [Rodney Manyakaidze]
- **Email**: [viantekeliana@gmail.com]
- **Discussions**: Use GitHub Issues for technical questions and GitHub Discussions for broader conversations

---

*"The goal is not to eliminate failure, but to ensure that when assumptions break, systems degrade gracefully rather than collapse catastrophically."*
