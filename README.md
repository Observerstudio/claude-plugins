# Observer Studio plugins for Claude Code

Add the marketplace once, then install what you need:

```
claude plugin marketplace add Observerstudio/claude-plugins
claude plugin install och@observer
```

| Plugin | What it does | Source |
|---|---|---|
| `och` | OCH — Observer Coordination Hub: context at session start, triggered lessons before risky commands, mail and collision warnings when you type, `/och:handoff` | [`observer-coordination-hub/clients/plugin`](https://github.com/Observerstudio/observer-coordination-hub/tree/main/clients/plugin) |

`och` needs the `och` CLI and a hub token on your machine; see that repository's README. Update with `claude plugin update och@observer`.
