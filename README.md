# OOT

**Reduce OpenClaw token usage and API costs by 50-80%**

OOT is an OpenClaw skill for smart model routing, lazy context loading, optimized heartbeats, budget tracking, and native OpenClaw 2026.2.15 features such as session pruning, bootstrap size limits, and cache TTL alignment.

[![ClawHub](https://img.shields.io/badge/ClawHub-oot-blue)](https://clawhub.ai/Cloud-Dark/oot)
[![Version](https://img.shields.io/badge/version-1.4.2-green)](https://github.com/Cloud-Dark/oot/blob/main/CHANGELOG.md)
[![License: Apache 2.0](https://img.shields.io/badge/License-Apache%202.0-yellow.svg)](https://opensource.org/licenses/Apache-2.0)
[![OpenClaw](https://img.shields.io/badge/OpenClaw-Skill-purple)](https://openclaw.ai)

---

## What OOT Reduces

- Context bloat from loading too many workspace files
- Model overspend from using expensive models on simple tasks
- Heartbeat waste from over-frequent routine checks
- Session cost drift through local token budget tracking

OOT focuses on reducing what the model sees and which model gets used.

---

## RTK Support

OOT works well with RTK.

- `OOT` reduces context, routing, heartbeat, and budget waste
- `RTK` reduces verbose shell output before it reaches the model

That split matters:

- Use `OOT` when the problem is too much context or the wrong model tier
- Use `RTK` when the problem is huge shell output from `git`, `rg`, `find`, tests, logs, or build tools

Example combined workflow:

```bash
# 1. Pick an appropriate model tier
python3 scripts/model_router.py "review this diff and summarize the failing tests"

# 2. Keep shell output compact
rtk git diff
rtk cargo test
rtk rg "TODO|FIXME" .

# 3. Check budget impact
python3 scripts/token_tracker.py check
```

Recommended RTK commands for shell-heavy sessions:

```bash
rtk git status
rtk git diff
rtk rg "pattern" .
rtk pytest
rtk cargo test
rtk docker logs my-container
```

See [references/RTK.md](references/RTK.md) for the integration guide.

---

## Installation

### Option 1: ClawHub

```bash
clawhub install Cloud-Dark/oot
```

Or browse to: [clawhub.ai/Cloud-Dark/oot](https://clawhub.ai/Cloud-Dark/oot)

### Option 2: Manual Install

```bash
git clone https://github.com/Cloud-Dark/oot.git \
  ~/.openclaw/skills/oot
```

Then add this to `openclaw.json`:

```json
{
  "skills": {
    "load": {
      "extraDirs": ["~/.openclaw/skills/oot"]
    }
  }
}
```

### One-Line Install Prompt

> "Install the OOT skill from https://clawhub.ai/Cloud-Dark/oot or, if ClawHub isn't available, clone https://github.com/Cloud-Dark/oot and add the path to skills.load.extraDirs in openclaw.json"

---

## Quick Start

### 1. Recommend a smaller context set

```bash
python3 scripts/context_optimizer.py recommend "hi, how are you?"
```

### 2. Route the task to the right model tier

```bash
python3 scripts/model_router.py "design a microservices architecture"
python3 scripts/model_router.py "thanks!"
```

### 3. Install the optimized heartbeat

```bash
cp assets/HEARTBEAT.template.md ~/.openclaw/workspace/HEARTBEAT.md
python3 scripts/heartbeat_optimizer.py plan
```

### 4. Check current token budget

```bash
python3 scripts/token_tracker.py check
```

### 5. Align heartbeat with Anthropic cache TTL

```bash
python3 scripts/heartbeat_optimizer.py cache-ttl
```

---

## Native OpenClaw Features

OOT documents and complements native OpenClaw 2026.2.15 features:

- `contextPruning` for cache-TTL-based session pruning
- `bootstrapMaxChars` and `bootstrapTotalMaxChars` for bootstrap size limits
- `cacheRetention: "long"` for Opus cache retention

Useful built-in diagnostics:

```text
/context list
/context detail
/usage tokens
/usage cost
/status
```

---

## Recommended Strategy

For the best savings:

1. Use `context_optimizer.py` to avoid injecting unnecessary files.
2. Use `model_router.py` to keep simple tasks off expensive models.
3. Use `heartbeat_optimizer.py` to avoid idle cache rewrite waste.
4. Use `token_tracker.py` to enforce cost discipline.
5. Use `RTK` for noisy shell commands so large outputs stay compact.

OOT reduces context and model waste. RTK reduces tool-output waste. Together they cover both major token sinks.

---

## Skill Structure

```text
oot/
|-- SKILL.md
|-- SECURITY.md
|-- CHANGELOG.md
|-- .clawhubsafe
|-- .clawhubignore
|-- scripts/
|-- assets/
`-- references/
```

---

## Security

All executable scripts are local-only with no network calls, no subprocess spawning, and no system modifications. See [SECURITY.md](SECURITY.md) for the full audit.

Verify integrity:

```bash
cd ~/.openclaw/skills/oot
sha256sum -c .clawhubsafe
```

---

## Links

- **ClawHub:** https://clawhub.ai/Cloud-Dark/oot
- **GitHub:** https://github.com/Cloud-Dark/oot
- **OpenClaw Docs:** https://docs.openclaw.ai
- **RTK Guide:** [references/RTK.md](references/RTK.md)
- **License:** Apache 2.0
- **Author:** [Cloud-Dark](https://github.com/Cloud-Dark)
