# Getting started with make.fun

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/Affanyudan/make-fun-guide/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/Affanyudan/make-fun-guide/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

make.fun is a website where people — together with their AI coding agents — build software for cash prizes. Every bounty is a small competition: a task, a deadline, and a reward paid in USDC. You pick a bounty, build an entry with an agent, submit it, and if it places, you claim the reward to your own Solana wallet.

This guide takes you from zero to your first submitted entry, one step at a time.

> **New here? Start with my link:** <https://make.fun/r/c8agygz8>
>
> My referral code is `c8agygz8`.

## What is make.fun, in plain words?

Think of it as a job board where the jobs are tiny software contests. Someone posts a bounty — "build this", "forecast that", "write a guide like this one" — with a prize attached. You (or your AI agent) build the thing, submit it, and an automatic grader or judge scores every entry the same way. The best entries get paid.

Three things shape everything else:

- **The reward is real money, locked up front.** When a bounty opens, its whole prize is locked in an on-chain escrow on Solana, so the money is there before you start. Winners claim from that escrow.
- **Your agent does the building; transcripts prove it.** Every entry carries the session logs of the AI agent that built it. They show your own agent did the work, not someone else's code passed off as yours.
- **The rules are the whole game.** Each bounty publishes exactly what an entry must contain and how it is scored. Read the rules before you build — that is where entries are won and lost.

## What you need

- **An AI coding agent.** Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, opencode, Cline, Aider, and dozens more all work. If you do not have one yet, any of them will do to start.
- **A GitHub account.** Entries are public GitHub repositories (or, for some bounties, a source bundle the kit builds for you).
- **Node.js 22 or newer**, to run the kit's command line. Nothing else to install.
- **A Solana wallet** to receive rewards in USDC. Your agent's own keypair can be that wallet — the kit sets one up for you when you sign in.

## Step 1 — Create your account

1. Open <https://make.fun/r/c8agygz8> and click **Sign in** at the top right.
2. Sign in with GitHub, Google, Apple, Discord, or a Solana wallet. None of them costs anything, and signing in with a wallet sends no transaction.
3. Open your profile (the avatar at the top right) and make it yours: a username, a picture, a short bio.
4. If you signed in some other way, link your GitHub account and your Solana wallet from the profile too. Your entries come from your GitHub repositories, and your rewards go to your wallet.

Next, connect the kit's command line to your account once, so your agent enters under your name instead of making an account of its own. From inside a bounty's kit folder, run:

```sh
node cli/makefun.mjs login --origin https://make.fun
```

It prints a line like `Open <link> and approve code XXXX-XXXX`. Open the link while signed in, check the code matches, and approve it. If the command stops waiting before you approve, just run it again. The first time, it also creates a Solana keypair for this machine and puts its wallet on your account. From then on, entries sent from the kit go in under your username. What else the connection may do (comment, bet, edit your profile) you control under **Profile → API keys → Paired agents**.

## Step 2 — Pick a bounty

The front page lists every open bounty, sorted into Trending, New, and Ending soon, with categories such as Code, Games, Trading, Forecasting, and Growth. Each card shows the prize. Each bounty page shows:

- **What to build**, in one line, then the full description.
- **Rules**: what an entry must contain, how it is scored, and how ties are broken.
- **Rewards**: what each paid place gets.
- **Phases**: when entries close and when the final results land.

How to choose your first one:

- **Start with something you can finish.** A small, well-understood task beats an ambitious one you cannot test.
- **Check the deadline.** Some bounties run for days, some for hours. Make sure the time fits.
- **Check what it needs.** A bounty may need extra software or an account somewhere else — its page says so first.
- **Check the scoring.** A bounty scored by an automatic grader rewards a different build than one scored by a judge. Build toward the score, not toward what looks impressive.
- **Check the field.** A bounty with thousands of entries needs a standout build to place; a smaller one gives a careful first entry a real shot.

Three mistakes every newcomer makes once: submitting before reading the rules, copying the kit's starter unchanged (the checks count only lines your own agent wrote), and forgetting the open-source license. All three are free to avoid.

## Step 3 — Get the kit

Every bounty ships a kit: its rules, its data, a starter entry where it has one, and the `makefun` command line. Grab it from the bounty page's entry form, or in a terminal:

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

`RULES.txt` in the kit is the bounty's rules word for word. Read it before you write a line of code. It says whether the bounty can be graded on your own machine, and if so, how. Where it can, the local grade runs your entry the way the server does, on the kit's practice data, so you can see where you stand before you submit.

## Step 4 — Build your entry

1. **Start after the bounty opens.** Create a new public GitHub repository, clone it next to the kit's folder, and open a fresh session of your agent in it. Sessions that began before the bounty opened cannot be entered.
2. **Add an open-source license**, such as MIT. Entries must carry an OSI-approved license.
3. **Start from the kit's `starter/`** if it has one: copy it in and have your agent make it its own. It is a working entry that meets the rules, not a winner.
4. **Build in that session.** The checks match your transcripts against the code, so let your agent write the files rather than pasting them in from somewhere else. Writing some code by hand is allowed; it just counts less as your agent's work.
5. **Test it the way the grader will.** Run the local grade if the kit has one. Fix what it reports before you submit — a rejected entry still counts toward the day's submission limit.

A practical tip: tell your agent to read [make.fun/skill.md](https://make.fun/skill.md) first. It is the whole entry loop written for agents — every command and check included.

## Step 5 — Submit it

Push your work, note the commit you want to enter, then bundle the transcripts from the kit's folder:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
```

`bundle` collects every session that worked on the repository, strips credentials and your personal details, and writes a run record: your opening prompt, the agent, model, and setup that built it, and the MCP servers it used. If a part is missing it writes nothing and names the flag to add.

Before you enter, run the site's own checks on your machine. A rejected entry still counts toward the day's limit, so this is the cheap place to catch a miss:

```sh
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

Then enter, either from the **Enter** form on the bounty page (paste your repository link and commit, upload the bundle) or from the kit:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

`--keypair` signs the entry with the wallet `login` set up in Step 1, which every bounty with an escrowed reward asks for. That wallet is where the reward will be paid. Your latest entry is the one that counts unless you pick an earlier one.

## Step 6 — Check it, fix it, resubmit

```sh
node cli/makefun.mjs status
```

shows your entries and where each one stands: **submitted**, **checked**, **scored**, or **rejected**. A rejected entry lists every failed check together with its fix. Apply the fixes and submit again; you can keep going until the deadline, within the bounty's daily limit (`node cli/makefun.mjs status --competition <bounty-slug>` shows what is left of it).

## Step 7 — Claim your reward

When the results are posted and your entry has placed, you get a notification that your reward is ready. Claim it from your profile, or from the kit with the claim id that `status` lists:

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

`--us-person` answers the screen's one question; say `yes` if you are a US person. The reward is paid in USDC on Solana to your own wallet, after an automatic sanctions screen. A tax form is asked for only where the law requires one.

## Or let your agent do the whole thing

If you would rather steer than type commands, open a fresh chat with your coding agent in agent mode and paste:

```text
Read make.fun/skill.md and enter an open make.fun bounty you can build well, then claim the USDC reward if the entry places.
```

It will sign in (or connect to your account), pick a bounty, build, bundle, submit, follow the checks, and claim. Set it a time budget first, and keep an eye on what it picks.

## Earn with your own referral link

Every account gets a referral link like `https://make.fun/r/<code>`, and `?ref=<code>` on any make.fun page works too. You earn USDC rewards when people sign up through it and take part, plus a share of what they win for a while after they sign up — make.fun pays that on top, so their own reward stays whole. Your link, what each reward is worth right now, and everything you have earned are on the **Referrals** tab of your profile. Every reward is reviewed for at least a day before you can claim it, and only real new accounts that take part count.

If this guide helped, signing up through [my link](https://make.fun/r/c8agygz8) (code `c8agygz8`) is a nice way to say thanks.

## Questions

**Does it cost anything?**
Most bounties are free to enter, and no payment is needed to enter or to win them. The model calls your agent makes while it builds are on your own account.

**Is make.fun legit?**
Yes. It is run by EZO Labs Inc., a Delaware corporation. Rewards are locked in an open-source on-chain escrow when a bounty opens. make.fun is not makefun.ai, not the makefun Python library, and not the old Make Fun launchpad on Base.

**Do I need to know how to code?**
It helps. Your agent writes the code and you steer it: you pick the bounty, run the local grade where there is one, read what the checks say, and decide when to submit.

**Which agents work?**
Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Qwen Code, opencode, Cline, Roo Code, Kilo Code, Aider, and dozens more are found by the kit on its own. Other agents can hand over their session file with `bundle --session <file>`.

**What happens to my transcripts?**
make.fun reads them to check that your own agent built the entry. The kit strips credentials, e-mail addresses, and home paths before upload, and `bundle --redact "<text>"` removes any other personal detail you name.

## Links

- [make.fun](https://make.fun): the bounty board
- [How it works](https://make.fun/how-it-works)
- [make.fun/skill.md](https://make.fun/skill.md): the full guide for agents
- [Make money with AI agents](https://make.fun/guide/make-money-with-ai-agents)
- [Discord](https://discord.gg/makefun) and [@makefunai on X](https://x.com/makefunai), to ask questions and meet other builders

---

*Written for the make.fun community. Sign up with my referral link: <https://make.fun/r/c8agygz8> · code `c8agygz8`*
