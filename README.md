# RTC Reward Action

A reusable GitHub Action that automatically awards **RTC (RustChain Token)** to contributors when their pull request is merged. Turn any open-source repository into a crypto-bounty platform with a single YAML file.

## Features

- ✅ **Auto-detect contributor wallet** — reads from PR body or `.rtc-wallet` file
- ✅ **Configurable reward amount** — set any RTC value per merge
- ✅ **Dry-run mode** — test the flow without real payments
- ✅ **Confirmation comments** — posts payment details on the merged PR
- ✅ **Best-effort execution** — never blocks CI, always `continue-on-error` friendly
- ✅ **Generic & reusable** — works with any RustChain-compatible project

## Quick Start

Add `.github/workflows/rtc-reward.yml` to your repository:

```yaml
name: RTC Reward on PR Merge

on:
  pull_request:
    types: [closed]

jobs:
  reward:
    if: github.event.pull_request.merged == true
    runs-on: ubuntu-latest
    steps:
      - uses: foreverzyf/rtc-reward-action@v1
        with:
          node-url: https://rustchain.org
          amount: 5
          wallet-from: project-fund
          admin-key: ${{ secrets.RTC_ADMIN_KEY }}
```

## Inputs

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `node-url` | No | `https://rustchain.org` | RustChain node endpoint |
| `amount` | No | `5` | RTC amount to award |
| `wallet-from` | **Yes** | — | Source wallet for payment |
| `admin-key` | **Yes** | — | Admin signing key (store in `secrets`) |
| `dry-run` | No | `false` | Simulate without real transfer |
| `wallet-file` | No | `.rtc-wallet` | Path to wallet mapping file |

## How Contributors Get Paid

### Option 1: Put wallet in PR body
```markdown
## My awesome feature

rtc-wallet: alice-dev
```

### Option 2: Add `.rtc-wallet` to repo root
```
alice-dev
```

### Option 3: Fallback to GitHub username
If no wallet is found, the action uses the PR author's GitHub login as the wallet name.

## Dry-Run Mode

Test your setup before going live:

```yaml
- uses: foreverzyf/rtc-reward-action@v1
  with:
    node-url: https://rustchain.org
    amount: 5
    wallet-from: project-fund
    admin-key: ${{ secrets.RTC_ADMIN_KEY }}
    dry-run: true
```

The action will simulate the payment and post a comment showing what **would** have happened.

## Example Output

> ## 🏆 RTC Reward
> **Contributor:** @alice-dev  
> **Wallet:** `alice-dev`  
> **Amount:** 5 RTC  
> **Status:** success  
> **TX Hash:** `0xabc123...`

## Security Notes

- Store `RTC_ADMIN_KEY` in **GitHub Secrets** (`Settings → Secrets and variables → Actions`)
- The action uses `pull_request_target` context to access secrets from fork PRs
- `continue-on-error: true` is built-in — CI never fails because of a payment issue

## License

MIT — see [LICENSE](LICENSE)

## Why This Matters

This turns **any GitHub repo** into a bounty platform. Maintainers add one YAML file and contributors earn crypto for merged PRs. Zero setup beyond the action.

---

*Built for the RustChain ecosystem. Compatible with any RTC-enabled project.*
