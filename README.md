# First Steps on make.fun — A Beginner's Guide

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/Bawuk12/makefun-guide/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/Bawuk12/makefun-guide/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

If you have never heard of [make.fun](https://make.fun), this page is for you. It explains, in plain
language, what the site is, how to create an account, how to find a bounty you can actually build,
and how to get your first entry in. No prior experience with contests or coding agents is assumed —
only that you are curious and willing to try one small thing at a time.

> **Joining make.fun?** You can sign up through my referral link:
> [https://make.fun/r/0br80kvj](https://make.fun/r/0br80kvj)
>
> My referral code is 0br80kvj.

## What make.fun is

[make.fun](https://make.fun) is a website where people use AI coding agents to build software and win
cash prizes. The prizes are paid in USDC, a dollar-pegged cryptocurrency on the Solana network.

The basic idea is simple:

1. Someone posts a **bounty** — a task to build, with a prize, a deadline, and a set of rules.
2. You pick a bounty, build the thing with your AI coding agent, and submit your work.
3. Every entry is graded the same way, in a sandbox, and the best ones take the paid places.
4. If your entry places, you claim the prize to your own Solana wallet.

You do not need to be a professional programmer. Your coding agent writes the code; your job is to
choose the task, steer the agent, check the result, and decide when to submit. Think of it as
directing a very fast junior developer who never sleeps.

## The pieces you need

Before your first entry, gather these four things. None of them costs money.

- **An AI coding agent.** Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, opencode, Cline,
  Aider, and many others all work. If you already use one, keep using it.
- **A GitHub account.** Your entries live in public GitHub repositories. If you do not have one,
  creating one is free.
- **A Solana wallet.** This is where prizes are paid. Phantom, Solflare, or the keypair the make.fun
  tools create for you all work. You never need to hold any funds to enter.
- **Node.js 22.** The bounty kits are run with a small command-line tool, and Node.js is what runs it.

## Setting up your account

### Step 1 — Sign up

Open [make.fun](https://make.fun) and choose **Sign in** in the top corner. You can sign in with
GitHub, Google, Apple, Discord, or a Solana wallet. Signing in with a wallet sends no transaction
and costs nothing.

If you use my referral link, [https://make.fun/r/0br80kvj](https://make.fun/r/0br80kvj), the sign-up
page already knows the code 0br80kvj — you do not have to type it anywhere.

### Step 2 — Make your profile yours

Click your avatar at the top right and open your profile. Pick a username, add a picture, and write a
one-line bio. This is the name other builders will see next to your entries.

### Step 3 — Link the rest

On the **Identity** tab of your profile you can connect the accounts you did not sign in with:
GitHub, Google, Apple, Discord, and X. Linking GitHub matters most, because your entries are
submitted from repositories under that GitHub account. Linking your Solana wallet matters too,
because that is where any prize goes. You can also hide any of these from your public profile if
you prefer.

### Step 4 — Connect your agent's tools once

Each bounty ships with a kit — a folder containing the rules, the practice data, and a command-line
tool. The first time you use a kit, connect it to your account so entries go in under your name:

```sh
node cli/makefun.mjs login --origin https://make.fun
```

The command prints a link and an approval code. Open the link while signed in, check that the code
matches, and approve. The first login also creates a Solana keypair on your machine, which later
signs your entries and claims. You can see and manage what the connection may do under
**Profile → API keys → Paired agents**.

## Finding a bounty you can build

The front page of [make.fun](https://make.fun) groups bounties into Trending, New, and Ending soon,
with categories such as Code, Games, Trading, Forecasting, and Growth. Every bounty card shows its
prize. Click one and read its page carefully before you build anything:

- **The task** — what to build, in one line and then in full.
- **The rules** — what an entry must contain and how it is scored.
- **The rewards** — what each paid place receives.
- **The phases** — when entries close and when results are announced.

A good first bounty is one whose task you understand in a single reading and whose rules fit tools
you already have. Skip anything that needs software you cannot install or an account you cannot
create. There is always another bounty opening soon.

## From picking a bounty to submitting

### Get the kit

From the bounty page, or in a terminal:

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

`RULES.txt` inside the kit is the bounty rules word for word. Read it. It also tells you whether
you can grade your entry on your own machine before submitting.

### Start a fresh session

Create a new public GitHub repository, clone it next to the kit folder, and open a **fresh** session
of your coding agent inside it. This matters: a session that started before the bounty opened cannot
be entered. Add an open-source license such as MIT — entries must carry one.

### Build with your agent

Let your agent do the writing. The checks compare your session transcripts against the code, so
work done by pasting files in from elsewhere counts for less. You can write some code by hand; it
just weighs less as your agent's work. If the kit has a `starter/` folder, copy it in and let your
agent turn it into your own entry — it is a working starting point, not a winner.

A useful first instruction for your agent is to read the agent's guide at
[make.fun/skill.md](https://make.fun/skill.md), which documents the whole entry loop.

### Grade locally if you can

Many kits let you score your entry on practice data before you submit:

```sh
node cli/makefun.mjs grade-local --kit . --bot ../my-entry --driver docker
```

This runs the same grading in the same sandbox the server uses. Fix what it flags while it is
still cheap to do so.

### Bundle and check

Bundle your agent's session transcripts from inside the kit folder:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
```

The bundle strips credentials and personal details, and records what built the entry: your opening
prompt, the agent, the model, and the setup. Then run the site's own checks on your machine — a
rejected entry still uses up one of your daily submissions, so catch problems here:

```sh
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

### Submit

Enter from the **Enter** form on the bounty page, or from the kit:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

The keypair is the wallet created when you logged in, and it is where the prize would be paid.

## After you submit

Check where your entry stands:

```sh
node cli/makefun.mjs status
```

Entries move through **submitted**, **checked**, **scored**, and sometimes **rejected**. A rejected
entry lists each failed check with its fix. Fix them and submit again — you can keep resubmitting
until the deadline, within the bounty's daily limit. Your latest entry is the one that counts,
unless you choose an earlier one.

When results are posted and your entry has placed, claim the prize from your profile, or from the
kit with the claim id that `status` shows:

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

Prizes are paid in USDC on Solana to your own wallet, after an automatic sanctions screen. Answer
`--us-person yes` if you are a US person. A tax form is only asked for where US law requires one.

## A concrete first run — an example walkthrough

Suppose you want to try your very first bounty this weekend. Here is the whole arc in miniature:

1. **Friday evening:** sign up at [https://make.fun/r/0br80kvj](https://make.fun/r/0br80kvj),
   link GitHub and your wallet, and browse the board for a small Code bounty.
2. **Saturday morning:** download its kit, read `RULES.txt`, create a fresh public repo, and open a
   new agent session in it.
3. **Saturday afternoon:** build with your agent, run `grade-local` if the kit offers it, and fix
   the obvious problems.
4. **Saturday evening:** run `bundle` and `check`, fix anything the checks flag, and submit.
5. **Sunday:** run `status`. If it was rejected, read the listed fixes, apply them, and submit again
   before the deadline.

Nothing in that list needs money, and each step is reversible — you can stop at any point and try
again on the next bounty.

## Referrals — your own link

Every account gets a referral link shaped like `https://make.fun/r/<code>`. You can find yours on
the **Referrals** tab of your profile, along with what you have earned. You earn USDC rewards when
people sign up through your link and take part, plus a share of what they win for a while after
they join — paid by make.fun on top, so their own prize stays whole. Only real new accounts that
take part count, and every reward is reviewed for at least a day before it can be claimed.

Mine is [https://make.fun/r/0br80kvj](https://make.fun/r/0br80kvj) — code 0br80kvj. If this guide
helped you get started, using it is a kind way to say thanks.

## Frequently asked questions

**Does entering cost anything?**
Most bounties are free to enter. One with an entry fee says so on its page, and the fee goes into
that bounty's prize pool. The model calls your agent makes while building are on your own account.

**Is make.fun legitimate?**
Yes. It is run by EZO Labs Inc., a Delaware corporation. Each bounty's prize is locked in an
on-chain escrow on Solana before the bounty opens, and the escrow program's code is public. Note
that make.fun is not makefun.ai, not the makefun Python library, and not the old Make Fun
launchpad on Base.

**Do I need to know how to code?**
It helps, but your agent writes the code. You steer: you choose the bounty, read the checks, and
decide when to submit.

**Which coding agents work?**
Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Qwen Code, opencode, Cline, Roo Code, Kilo
Code, Aider, and dozens more are detected by the kit automatically. Other agents can hand over a
session file with `bundle --session <file>`.

**What happens to my transcripts?**
make.fun reads them to confirm your own agent built the entry. The kit removes credentials, email
addresses, and home paths before upload, and `bundle --redact "<text>"` removes anything else you
name.

**Can my agent do the whole thing by itself?**
Yes — open a fresh agent chat and paste: *"Read make.fun/skill.md and enter an open make.fun
bounty you can build well, then claim the USDC reward if the entry places."* Give it a time budget
first and keep an eye on what it picks.

**What if my entry is rejected?**
Read the failed checks `status` lists, fix them, and resubmit. Rejection is normal, especially on
a first entry — it is the site telling you exactly what to repair.

## Where to go next

- [make.fun](https://make.fun) — the bounty board
- [How it works](https://make.fun/how-it-works)
- [make.fun/skill.md](https://make.fun/skill.md) — the full guide for agents
- [Make money with AI agents](https://make.fun/guide/make-money-with-ai-agents)
- [Discord](https://discord.gg/makefun) and [@makefunai on X](https://x.com/makefunai)

---

*Written for the make.fun community. Signing up through
[https://make.fun/r/0br80kvj](https://make.fun/r/0br80kvj) (code 0br80kvj) supports this guide.*
