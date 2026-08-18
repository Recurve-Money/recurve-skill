# recurve-skill

Installable instructions that teach an AI agent to operate a Recurve fund - or to guard
one. Two skills, one repo:

| Skill | Who it's for | What it does |
|---|---|---|
| `skills/recurve/` | Agents running a fund | Deploy a vault, post strategies, execute after the veto window, settle and book the fee |
| `skills/recurve-watcher/` | Agents reviewing proposals | Watch for proposals, fork state, replay the calldata, cast a verdict backed by stake |

A skill here is a markdown file plus the reference material it needs. Whatever harness
runs your agent - Claude Code, Hermes, OpenClaw, or your own loop - reads it as context
and calls the [`@recurve/sdk`](https://github.com/Recurve-Money/recurve-sdk) underneath.

## Install

**Claude Code**

```bash
git clone https://github.com/Recurve-Money/recurve-skill
cp -r recurve-skill/skills/recurve ~/.claude/skills/
```

**Hermes**

```bash
hermes plugins install Recurve-Money/recurve-skill
```

**Anything else**

Point your agent at the raw markdown:

```
https://recurvemoney.xyz/skill.md
https://recurvemoney.xyz/skill-watcher.md
```

## Configure

Both skills expect these in the environment:

```bash
RECURVE_RPC_URL=https://rpc.mainnet.chain.robinhood.com
RECURVE_VAULT=0x…
RECURVE_GOVERNOR=0x…
RECURVE_REGISTRY=0x…
PRIVATE_KEY=0x…
```

The watcher skill additionally wants `RECURVE_FORK_RPC_URL` - simulation needs an
archive node, and pointing it at your main RPC will rate-limit you within the hour.

## A note on verdicts

The watcher skill will not let an agent approve a proposal it failed to simulate. A
verdict is final onchain and a wrong approval burns the stake, so "I could not fork,
approving anyway" has to be unreachable rather than discouraged.

## License

MIT
