# OOT

**Reduce OpenClaw token usage and API costs by 50-80%**

An OpenClaw skill for smart model routing, lazy context loading, optimized heartbeats, budget tracking, and native OpenClaw 2026.2.15 features (session pruning, bootstrap size limits, cache TTL alignment).

[![ClawHub](https://img.shields.io/badge/ClawHub-oot-blue)](https://clawhub.ai/Cloud-Dark/oot)
[![Version](https://img.shields.io/badge/version-1.4.2-green)](https://github.com/Cloud-Dark/oot/blob/main/CHANGELOG.md)
[![License: Apache 2.0](https://img.shields.io/badge/License-Apache%202.0-yellow.svg)](https://opensource.org/licenses/Apache-2.0)
[![OpenClaw](https://img.shields.io/badge/OpenClaw-Skill-purple)](https://openclaw.ai)

---

## Installation

### Option 1: ClawHub (recommended)
```bash
clawhub install Cloud-Dark/oot
```

Or browse to: [clawhub.ai/Cloud-Dark/oot](https://clawhub.ai/Cloud-Dark/oot)

### Option 2: Manual (GitHub)
```bash
git clone https://github.com/Cloud-Dark/oot.git \
  ~/.openclaw/skills/oot
```

Then add to `openclaw.json`:
```json
{
  "skills": {
    "load": {
      "extraDirs": ["~/.openclaw/skills/oot"]
    }
  }
}
```

### One-line install prompt for your agent
> "Install the OOT skill from https://clawhub.ai/Cloud-Dark/oot or, if ClawHub isn't available, clone https://github.com/Cloud-Dark/oot and add the path to skills.load.extraDirs in openclaw.json"

---

## What's New in v1.4.x (OpenClaw 2026.2.15)

Three native config patches that work today with zero external dependencies:

### Session Pruning
Auto-trim old tool results when the Anthropic cache TTL expires, reducing cache re-write costs.
```json
{ "agents": { "defaults": { "contextPruning": { "mode": "cache-ttl", "ttl": "5m" } } } }
```

### Bootstrap Size Limits
Cap workspace file injection into the system prompt.
```json
{ "agents": { "defaults": { "bootstrapMaxChars": 10000, "bootstrapTotalMaxChars": 15000 } } }
```

### Cache Retention for Opus
Amortize cache write costs on long Opus sessions.
```json
{ "agents": { "defaults": { "models": { "anthropic/claude-opus-4-5": { "params": { "cacheRetention": "long" } } } } } }
```

### Cache TTL Heartbeat Alignment
Keep the Anthropic 1h prompt cache warm.
```bash
python3 scripts/heartbeat_optimizer.py cache-ttl
```

---

## Quick Start

**1. Context optimization**
```bash
python3 scripts/context_optimizer.py recommend "hi, how are you?"
```

**2. Model routing**
```bash
python3 scripts/model_router.py "design a microservices architecture"
python3 scripts/model_router.py "thanks!"
```

**3. Optimized heartbeat**
```bash
cp assets/HEARTBEAT.template.md ~/.openclaw/workspace/HEARTBEAT.md
python3 scripts/heartbeat_optimizer.py plan
```

**4. Token budget check**
```bash
python3 scripts/token_tracker.py check
```

**5. Cache TTL alignment**
```bash
python3 scripts/heartbeat_optimizer.py cache-ttl
```

---

## Native OpenClaw Diagnostics (2026.2.15+)

```text
/context list
/context detail
/usage tokens
/usage cost
```

---

## Skill Structure

```text
oot/
├── SKILL.md
├── SECURITY.md
├── CHANGELOG.md
├── .clawhubsafe
├── .clawhubignore
├── scripts/
├── assets/
└── references/
```

---

## Security

All scripts are local-only with no network calls, no subprocess spawning, and no system modifications. See [SECURITY.md](SECURITY.md) for the full audit.

Verify integrity:
```bash
cd ~/.openclaw/skills/oot
sha256sum -c .clawhubsafe
```

---

## Changelog

See [CHANGELOG.md](CHANGELOG.md) for full version history.

---

## Links

- **ClawHub:** https://clawhub.ai/Cloud-Dark/oot
- **GitHub:** https://github.com/Cloud-Dark/oot
- **OpenClaw Docs:** https://docs.openclaw.ai
- **License:** Apache 2.0
- **Author:** [Cloud-Dark](https://github.com/Cloud-Dark)
