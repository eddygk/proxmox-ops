# Proxmox Ops

Battle-tested Proxmox VE management toolkit — helper scripts, API patterns, and operational knowledge built from running a 46-guest cluster daily.

Works standalone, or as a reference for AI coding agents (Claude Code, Claude Desktop, OpenClaw, etc).

## What's Inside

- **Helper script** (`pve.sh`) with auto node discovery from VMID — no need to know which node a VM lives on
- **Disk resize end-to-end** — API call + in-guest filesystem steps (parted, pvresize, lvextend, resize2fs/xfs_growfs)
- **Guest agent IP discovery** — jq one-liner to pull IPv4 from qemu-guest-agent
- **vmstate snapshot warning** — why `vmstate=1` can freeze your VM and starve I/O on the whole node
- **Operational safety gates** — read-only vs reversible vs destructive, with explicit confirmation guidance
- **Separate provisioning reference** — create, clone, template, delete in its own doc

## Requirements

- `curl`
- `jq`
- Proxmox VE API token ([how to create one](https://pve.proxmox.com/wiki/User_Management#pveum_tokens))

## Setup

```bash
cat > ~/.proxmox-credentials <<'EOF'
PROXMOX_HOST=https://<your-proxmox-ip>:8006
PROXMOX_TOKEN_ID=user@pam!tokenname
PROXMOX_TOKEN_SECRET=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
EOF
chmod 600 ~/.proxmox-credentials
```

## Quick Start

```bash
# Cluster overview
./scripts/pve.sh status

# List all VMs
./scripts/pve.sh vms

# Start/stop by VMID (auto-discovers node)
./scripts/pve.sh start 200
./scripts/pve.sh stop 200

# Snapshots
./scripts/pve.sh snap 200 before-update
./scripts/pve.sh snapshots 200
```

## Files

| File | Purpose |
|------|---------|
| `SKILL.md` | Full reference — API patterns, workflows, safety notes |
| `scripts/pve.sh` | Helper script with auto node discovery |
| `references/provisioning.md` | Create, clone, template, and delete operations |

## Using with AI Agents

### Claude Code / Claude Desktop

Add to your project or reference directory. Claude will pick up `SKILL.md` as context for Proxmox operations:

```bash
# Clone into your project
git clone https://github.com/eddygk/proxmox-ops-skill.git proxmox-ops

# Or reference in CLAUDE.md
echo "For Proxmox operations, see proxmox-ops/SKILL.md" >> CLAUDE.md
```

### OpenClaw / ClawHub

```bash
clawhub install proxmox-ops
```

### Any Agent

The knowledge is in plain markdown and shell scripts — no framework lock-in. Point your agent at `SKILL.md` and it knows how to operate Proxmox safely.

## License

MIT
