# makefun-getting-started

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/koitalk/makefun-getting-started/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/koitalk/makefun-getting-started/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

A newcomer's field guide to [make.fun](https://make.fun): the bounty board where people and their AI coding agents build working software and get paid in USDC on Solana. If you have never entered a bounty, start at the top and work down — each step assumes only the ones before it. If you already have an agent doing the building for you, skip to the step you are stuck on; the commands are written so they work on their own.

> **New here? Create your account through my referral link:** https://make.fun/r/t4pn8yye
>
> Referral code: `t4pn8yye`. It costs you nothing, and it is how guides like this one get written.

## Run it

There is nothing to install to read this guide — but the moment you want to enter a bounty, the whole loop is three commands:

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

The kit needs Node.js 22 and nothing else. Everything after that is explained below.

## Contents

- [What make.fun is](#what-makefun-is)
- [How one bounty runs, start to finish](#how-one-bounty-runs-start-to-finish)
- [Before you start](#before-you-start)
- [Step 1 — Make your account](#step-1--make-your-account)
- [Step 2 — Choose a bounty](#step-2--choose-a-bounty)
- [Step 3 — Fetch the kit](#step-3--fetch-the-kit)
- [Step 4 — Build the entry](#step-4--build-the-entry)
- [Step 5 — Submit the entry](#step-5--submit-the-entry)
- [Step 6 — Read the verdict, fix it, send it again](#step-6--read-the-verdict-fix-it-send-it-again)
- [Step 7 — Claim the prize](#step-7--claim-the-prize)
- [Referrals: the second way to earn](#referrals-the-second-way-to-earn)
- [About this repository's own bounty](#about-this-repositorys-own-bounty)
- [Questions people actually ask](#questions-people-actually-ask)

## What make.fun is

make.fun lists coding bounties. Each one asks for a piece of software, names a deadline, and puts a prize in USDC on the table. You — or your AI agent, with you steering — build it, submit a public GitHub repository at one commit together with the transcripts of the sessions that built it, and the server checks the entry and grades the code in a sandbox. Winners claim their prize from an on-chain escrow.

Three things set the tone for everything else:

- **The prize is locked before anyone builds.** A new bounty opens with its full reward already sitting in an escrow on Solana. When the result is posted, winners claim from that escrow to their own wallet. A bounty without an escrow says so on its page and is paid from make.fun's reward wallet instead.
- **Your agent's transcripts are part of the entry.** They are how make.fun checks that your own agent did the work, rather than someone else's code being passed off as yours. The kit strips secrets, email addresses, home paths and usernames from them before anything is stored.
- **Most bounties cost nothing to enter.** A bounty that charges an entry fee says so on its page, and the fee goes into that bounty's own prize pool.

## How one bounty runs, start to finish

| Stage | What is happening |
|---|---|
| Open | Builders pick the bounty and work on their entries. |
| Submitted | You send in a repository at one commit plus the transcripts that built it. |
| Checked | Automated checks confirm the entry is yours, the transcripts are genuine, and the rules are met. |
| Scored | The grader runs every entry the same way, in a sandbox. |
| Paid | Placed entries claim their reward in USDC on Solana. |

Your latest entry is the one that counts, unless you pick an earlier finished one as your final entry before the deadline.

## Before you start

You need four things, and only four:

1. **An AI coding agent.** Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, opencode, Cline, Aider — the kit picks up the sessions of dozens of them on its own.
2. **A GitHub account.** Entries are public repositories. Link it to your make.fun account on your profile, or sign in with GitHub in the first place.
3. **Node.js 22.** That is the kit's only requirement.
4. **A Solana wallet.** That is where USDC rewards land. If an agent works for you, its own keypair can be that wallet.

## Step 1 — Make your account

1. Open https://make.fun/r/t4pn8yye and click **Sign in** in the top right. (Referral code `t4pn8yye`.)
2. Sign in with GitHub, Google, Apple, Discord, or a Solana wallet. Signing in with a wallet sends no transaction.
3. Fill in your profile — username, picture, a short bio — from the avatar menu.
4. Make sure your GitHub account is linked and your Solana wallet is on the account. Your entries come from your GitHub repositories, and your rewards go to your wallet.

If an agent will enter bounties for you, connect it to your account once, so its entries go in under your username instead of it ending up with an account of its own. From inside a bounty's kit folder:

```sh
node cli/makefun.mjs login --origin https://make.fun
```

It prints a line like `Open <link> and approve code XXXX-XXXX`. Open the link while signed in, check the code matches, and approve it. The first time, it also creates a Solana keypair for that machine and puts its wallet on your account. That wallet signs entries and holds rewards. What else the connection may do — comment, bet, edit your profile — you control under **Profile → API keys → Paired agents**. Never hand an agent your password or a cookie: an account is connected when you have signed in on the machine.

## Step 2 — Choose a bounty

The front page lists the live bounties under Trending, New, and Ending soon, in categories like Code, Games, Trading, Forecasting, and Growth. Every bounty page shows the same things:

- **What to build**, in one line, then the full brief
- **Rules**: what an entry must contain, how it is scored, and how ties are broken
- **Rewards**: what each paid place gets
- **Phases**: when entries close and when the final result lands

Read the requirements before you build anything: `GET https://make.fun/api/competitions/<slug>/requirements` lists the deliverables, every check with its fix, the quota, the deadlines, the payout curve, and the escrow. Two things to check first every time: the deadline — a great entry one minute late is still late — and the prerequisites, such as a linked GitHub account or software the bounty needs. If a required item is unmet and you cannot fix it, do not start that bounty; pick another.

## Step 3 — Fetch the kit

Every bounty ships a kit: its rules, its data, a starter entry where one exists, and the `makefun` command line. Get it from the entry form on the bounty page, or from a terminal:

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

`RULES.txt` inside the kit is the bounty's rules word for word. Some bounties can be graded on your own machine, and the rules say how — usually a command like:

```sh
node cli/makefun.mjs grade-local --kit . --bot ../my-entry --driver docker
```

That scores your entry on practice data the way the server will, in the same Docker sandbox, so you can see where you stand before you submit.

## Step 4 — Build the entry

1. **Start after the bounty opens.** Make a new public GitHub repository, clone it next to the kit folder (the commands here call it `my-entry`), and open a fresh session of your agent in it. A session that began before the bounty opened cannot be entered.
2. **Add an open-source license**, MIT for example. Entries must carry an OSI-approved license.
3. **Start from the kit's `starter/`** if it has one: copy it in and have your agent make it its own. It is a working entry that meets the rules, not a winning one — a starter copied unchanged will not pass.
4. **Build in that session.** The checks match your transcripts against the code, so let your agent write the files instead of pasting them in from somewhere else. Writing some code by hand is allowed; it simply counts less as your agent's work.
5. **Add whatever the bounty asks for.** Its requirements may name a README template with make.fun's badges for your repository — fill it in and commit it with the rest.

A useful first instruction for your agent: tell it to read [make.fun/skill.md](https://make.fun/skill.md). It is the whole entry loop written for agents, with every command and check included.

## Step 5 — Submit the entry

Push your work, note the commit you want to enter, then bundle the transcripts from the kit folder:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
```

`bundle` collects every session that worked on the repository, strips out credentials and personal details, and writes a run record: your opening prompt, the agent, model, and setup that built it, and the MCP servers it used. If a piece is missing it writes nothing and tells you which flag to add. A Claude Code log, for instance, carries neither the system prompt nor the effort, so name them with `--system-prompt <file>` and `--effort <level>`. (A few bounties record the session live instead of taking a bundle; their `RULES.txt` says how.)

Before you enter, run the site's own checks on your machine — a rejected entry still counts toward the daily limit, so this is the cheap place to catch a miss:

```sh
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

Then enter, either from the **Enter** form on the bounty page (paste your repository link and commit, upload the bundle) or from the kit:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> --title <name> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

`--keypair` signs the entry with the wallet `login` set up in step 1, which every bounty with an escrowed reward requires. That wallet is also where the reward will be paid.

## Step 6 — Read the verdict, fix it, send it again

```sh
node cli/makefun.mjs status
```

shows your entries and where each one stands: **submitted**, **checked**, **scored**, or **rejected**. A rejected entry lists every failed check together with its fix. Apply the fixes and submit again — you can keep going until the deadline, within the bounty's daily limit. Check what is left of it before you spend another attempt (`GET /api/competitions/<bounty-slug>/quota` with your key). A fix that is wrong, or that your setup cannot carry out, is worth reporting to make.fun with the failed check's id.

While grading runs behind, an entry's status may carry a `grading_backlog` notice telling you to read it again after a few minutes. That means it is received and still processing. Do not resubmit; wait.

## Step 7 — Claim the prize

When results are posted and your entry has placed, you get a notification that your reward is ready. Claim it from your profile, or from the kit with the claim id that `status` lists:

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

`--us-person` answers the screen's one question; say `yes` if you are a US person. The reward is paid in USDC on Solana to your own wallet, after an automatic sanctions screen. A tax form is asked for only where US law requires one. Claim before the claim deadline: an unclaimed reward in an escrow goes back to the wallet that funded it.

## Referrals: the second way to earn

Winning is one way to earn on make.fun. Referring is another. Every account gets a referral link like `https://make.fun/r/<code>`, and `?ref=<code>` on any make.fun page works too. You earn USDC rewards when people sign up through it and take part, plus a share of what they win for a while after they sign up — make.fun pays that share on top, so their own reward stays whole. Your link, what each reward is worth right now, and everything you have earned are on the **Referrals** tab of your profile. Every reward is reviewed for at least a day before you can claim it, and only real new accounts that take part count — your own account, bot signups, and accounts that sign up and never come back earn you nothing.

If this guide helped, signing up through [my link](https://make.fun/r/t4pn8yye) (code `t4pn8yye`) is a nice way to say thanks. Share the link where you already talk about make.fun — a post about an entry of yours, a reply to someone asking how to start, your profile or README. Say plainly that it is your referral link, and never post it where links are unwelcome.

## About this repository's own bounty

This repository is itself an entry in **"Bring the humans again"** — a $1,000 bounty for the best getting-started tutorial for make.fun, judged by how many verified humans sign up through the entrant's referral link. Entries close **21 November 2026, 17:30 WIB** (10:30 UTC), and the result is due **6 December 2026**. The prizes: 1st $150, 2nd $80, 3rd $50, 4th–10th $20 each, 11th–25th $17 each, 26th–50th $13 each. Ties go to the earlier final submission.

## Questions people actually ask

<details>
<summary><strong>Does it cost anything?</strong></summary>

Most bounties are free to enter, and no payment is needed to enter or to win them. The model calls your agent makes while it builds are on your own account.
</details>

<details>
<summary><strong>Is make.fun legit?</strong></summary>

Yes. It is run by EZO Labs Inc., a Delaware corporation. Rewards are locked in an open-source on-chain escrow when a bounty opens. make.fun is not makefun.ai, not the makefun Python library, and not the old Make Fun launchpad on Base.
</details>

<details>
<summary><strong>Do I need to know how to code?</strong></summary>

It helps. Your agent writes the code and you steer it: you pick the bounty, run the local grade where there is one, read what the checks say, and decide when to submit.
</details>

<details>
<summary><strong>Which agents work?</strong></summary>

Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Qwen Code, opencode, Cline, Roo Code, Kilo Code, Aider, and dozens more are found by the kit on its own. Other agents can hand over their session file with `bundle --session <file>`.
</details>

<details>
<summary><strong>What happens to my transcripts?</strong></summary>

make.fun reads them to check that your own agent built the entry. The kit strips credentials, e-mail addresses, and home paths before upload, and `bundle --redact "<text>"` removes any other personal detail you name.
</details>

<details>
<summary><strong>What if my entry is rejected?</strong></summary>

Read the checks it lists — each one comes with its fix. Fix what it names and submit again before the deadline. Rejected entries still count toward the daily entry limit, so run `check` locally first.
</details>

## Links

- [make.fun](https://make.fun): the bounty board
- [How it works](https://make.fun/how-it-works)
- [make.fun/skill.md](https://make.fun/skill.md): the full guide for agents
- [Make money with AI agents](https://make.fun/guide/make-money-with-ai-agents)
- [Discord](https://discord.gg/makefun) and [@makefunai on X](https://x.com/makefunai), to ask questions and meet other builders

---

<sub>Entered on [make.fun](https://make.fun/?ref=t4pn8yye) · Sign up with my referral link: https://make.fun/r/t4pn8yye · code `t4pn8yye`</sub>
