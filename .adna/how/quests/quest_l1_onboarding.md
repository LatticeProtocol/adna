---
quest_id: quest_l1_onboarding
type: side_quest
title: "L0→L1 Compute Upgrade — Self-Service Onboarding"
difficulty: easy
estimated_token_cost: ~3K
model_requirements: "any (with shell access)"
status: active
created: 2026-03-22
updated: 2026-03-22
last_edited_by: agent_stanley
tags: [side_quest, onboarding, l1, compute, deployment]
---

# Quest: L0→L1 Compute Upgrade

## Objective

Upgrade your aDNA vault (L0) to an L1 compute node with JupyterHub. By the end, you'll have a working L1 node running locally and connected to the Lattice network for fleet operations and collaborative compute.

This quest walks through the user-side steps. An admin handles the provisioning — you just need to prepare your machine and provide SSH access.

## Prerequisites

- Mac with Apple Silicon (arm64) — `uname -m` should return `arm64`
- macOS 14.0+ — `sw_vers --productVersion`
- Python 3.12+ — `python3 --version`
- npm installed — `npm --version`
- ~10GB free disk space — `df -h ~`
- Ports 8000 and 8100 free — `lsof -i :8000 2>/dev/null; lsof -i :8100 2>/dev/null`

## Steps

### Phase 1: Preflight (~2 min)

Run the preflight checks and record results:

```bash
echo "=== L1 Preflight ==="
echo "Architecture: $(uname -m)"
echo "macOS: $(sw_vers --productVersion)"
echo "Python: $(python3 --version 2>&1)"
echo "npm: $(npm --version 2>&1)"
echo "Disk free: $(df -h ~ | tail -1 | awk '{print $4}')"
lsof -i :8000 2>/dev/null && echo "WARN: port 8000 in use" || echo "OK: port 8000 free"
lsof -i :8100 2>/dev/null && echo "WARN: port 8100 in use" || echo "OK: port 8100 free"
```

**If Python < 3.12**: `brew install python@3.12`
**If npm missing**: `brew install node`
**If ports in use**: identify the process with `lsof -i :<port>` and stop it.

All checks must pass before continuing.

### Phase 2: Enable SSH Access (~5 min)

The admin needs SSH access to your machine to push the private deployment code and run setup.

**Step 2a: Enable Remote Login**

System Settings → General → Sharing → Remote Login → ON

Verify:
```bash
ssh localhost echo "SSH works"
```

**Step 2b: Choose an exposure method**

Pick one — Cloudflare is the easiest:

| Method | Command | Notes |
|--------|---------|-------|
| **Cloudflare Quick Tunnel** (recommended) | `brew install cloudflared && cloudflared tunnel --url ssh://localhost:22` | Zero config, temporary URL |
| **ngrok** | `ngrok tcp 22` | Simple, raw TCP forwarding |
| **Tailscale** | `brew install tailscale && tailscale up` | Persistent mesh, requires admin invite |

Run your chosen method and **copy the access URL** (e.g., `ssh://user@random-name.trycloudflare.com`).

> **Important**: Install Tailscale via Homebrew, NOT the App Store. The App Store version's CLI isn't in PATH for SSH sessions.

### Phase 3: Handoff to Admin (~15 min wait)

Share the following with your Lattice admin:

1. **SSH access URL** from Phase 2
2. **Your macOS username** — `whoami`
3. **Confirmation that preflight passed** (Phase 1 output)

The admin will run:
```bash
# Admin executes from their machine:
setup_l1_remote.sh <your_ssh_alias> --push-repos full
```

This pushes the latlab repo to your machine and runs the full L1 setup sequence. No GitHub access needed on your end.

**Wait for admin confirmation** that setup completed successfully.

### Phase 4: Verify Your L1 (~3 min)

Once the admin confirms:

```bash
# Run the health check
bash ~/lattice/latlab/deploy/native/latlab_doctor.sh

# Start JupyterHub
bash ~/lattice/latlab/deploy/native/latlab_start.sh
```

Open `http://127.0.0.1:8000` in your browser. You should see the JupyterHub login page.

**First login**: Use NativeAuth signup to create a local account.

### Phase 5: Submit Your Result

Create a result file and submit via PR:

```bash
# From the aDNA repo
cat > how/quests/results/result_l1_onboarding_$(whoami).md << 'EOF'
---
quest_id: quest_l1_onboarding
type: quest_result
submitted_by: <your_name>
model_used: <model>
timestamp: <ISO timestamp>
status: complete
---

# Result: L1 Onboarding

## Preflight Output
<paste Phase 1 output>

## Doctor Output
<paste Phase 4 doctor output>

## Verification
- [ ] JupyterHub login page loads at http://127.0.0.1:8000
- [ ] NativeAuth signup + login works
- [ ] latlab_doctor.sh shows 0 FAIL

## Notes
<any observations, issues encountered, or suggestions>
EOF
```

## What's Next

After completing this quest, you can continue with advanced phases:

- **Phase 2**: Mesh connectivity via Tailscale (see `how/skills/skill_l1_upgrade.md` Phase 2)
- **Phase 3**: Federation relay for remote admin access (Phase 3)
- **Phase 4**: Full compliance with TLS + HMAC (Phase 4)

## Related

- `how/skills/skill_l1_upgrade.md` — the full 4-phase upgrade guide (this quest covers Phase 1)
- `what/docs/adna_standard.md` — the aDNA knowledge architecture
- `CONTRIBUTING.md` — how to submit PRs to the aDNA repo
