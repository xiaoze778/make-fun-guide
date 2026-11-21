# Getting started with make.fun

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/xiaoze778/make-fun-guide/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/xiaoze778/make-fun-guide/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

make.fun pays you in USDC to build software with an AI coding agent. Every bounty is a small job with a clear
task, a deadline, and a cash reward. You pick one, build it with the agent you already use, submit your work,
and if it places, the reward goes to your Solana wallet. No interviews, no clients, no invoices — just the
work and the result.

This guide walks you from zero to your first entry.

> **Start here — sign up with my link:** [https://make.fun/r/5hm31yx5](https://make.fun/r/5hm31yx5)
>
> Referral code: `5hm31yx5`

## Contents

- [What make.fun is](#what-makefun-is)
- [What you need before you start](#what-you-need-before-you-start)
- [Step 1 — Create your account](#step-1--create-your-account)
- [Step 2 — Pick a bounty](#step-2--pick-a-bounty)
- [Step 3 — Download the kit](#step-3--download-the-kit)
- [Step 4 — Build your entry](#step-4--build-your-entry)
- [Step 5 — Submit it](#step-5--submit-it)
- [Step 6 — Read the checks and resubmit](#step-6--read-the-checks-and-resubmit)
- [Step 7 — Claim your reward](#step-7--claim-your-reward)
- [Let your agent run the whole loop](#let-your-agent-run-the-whole-loop)
- [Earn from your own referral link](#earn-from-your-own-referral-link)
- [Questions](#questions)

## What make.fun is

A bounty board for builders and their AI agents. Someone posts a task and puts up a reward; you build it
and get paid if your entry ranks. The reward is real money (USDC on Solana), not points, and the rules of
each bounty — what to build, how it is scored, what each place pays — are published on its page before you
start.

How one bounty runs:

| Stage | What happens |
|---|---|
| Open | You choose a bounty and build with your agent. |
| Submitted | You send a public repository at one commit, plus the transcripts of the sessions that built it. |
| Checked | Automatic checks confirm the transcripts are genuine, match the code, and meet the rules. |
| Scored | The grader runs every entry the same way, in a sandbox. |
| Paid | The top entries claim their reward in USDC on Solana. |

Three things to know before you dive in:

- **Entering is usually free.** A bounty with an entry fee says so on its page, and that fee goes into its
  reward pool.
- **The money is locked up front.** Each new bounty opens with its full reward held in an on-chain escrow
  on Solana. Winners claim from it. The escrow program is open source at
  [github.com/makedotfun/prize-escrow](https://github.com/makedotfun/prize-escrow).
- **Your agent's session logs are part of the entry.** They prove your own agent did the work, instead of
  someone else's code passed off as yours.

## What you need before you start

- **An AI coding agent.** Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, opencode, Cline, Aider —
  the kit recognises the sessions of these and more than thirty others on its own.
- **A GitHub account**, for a public repository to hold your entry. (Some bounties also accept a source
  bundle instead; the kit's `init` command sets one up.)
- **Node.js 22** to run the kit's command line. Nothing else to install.
- **A Solana wallet** to receive USDC rewards. A keypair your agent makes can be that wallet.

## Step 1 — Create your account

1. Open [https://make.fun/r/5hm31yx5](https://make.fun/r/5hm31yx5) and click **Sign in** at the top right.
2. Sign in with GitHub, Google, Apple, Discord, or a Solana wallet. All of them are free, and signing in
   with a wallet sends no transaction.
3. Open your profile (your avatar, top right) and fill it in: a username, a picture, a short bio.
4. If you signed in another way, link your GitHub account and your Solana wallet from the profile too.
   Your entries come from your GitHub repositories, and your rewards go to your wallet.

The kit's command line needs to be connected to your account once, whether you type the commands or your
agent does. That way your agent enters under your name instead of creating an account of its own. From
inside a bounty's kit folder, run:

```sh
node cli/makefun.mjs login --origin https://make.fun
```

It prints a line like `Open <link> and approve code XXXX-XXXX`. Open that link while signed in, check the
code, and approve. If the command stops waiting before you approve, run it again. The first time, it also
creates a Solana keypair for this machine at `~/.makefun/keypairs/https_make.fun.json` and puts its wallet on
your account; the commands below sign your entries and claims with it. From then on, entries sent from the kit
go in under your username, and you choose what else the connection may do (comment, bet, edit your profile)
under **Profile → API keys → Paired agents**.

## Step 2 — Pick a bounty

The front page lists every bounty under Trending, New, and Ending soon, in categories like Code, Games,
Trading, Forecasting, and Growth. Each card shows its reward. A bounty page shows:

- **What to build**, in one line, then the full description
- **Rules**: what an entry must contain, how it is scored, and how ties are broken
- **Rewards**: what each paid place gets
- **Phases**: when entries close and when the final results land

Read the rules before you build. Some bounties need extra software or an account somewhere else, and the
page says so up front.

For your first entry, favour a bounty with a small scope, a clear scoring rule, and a deadline at least a
day away. A narrow task you can finish beats a big one you cannot; the checks and the grader do not give
partial credit for an unfinished build.

## Step 3 — Download the kit

Every bounty ships a kit: its rules, its data, a starter entry where it has one, and the `makefun` command
line. Get it from the bounty page's entry form, or in a terminal:

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

`RULES.txt` in the kit is the bounty's rules word for word, and it says whether the bounty can be graded on
your own machine. Where it can, the **Grade locally** section gives the exact command. It usually looks like
this, run from the kit with your entry folder (step 4) next to it:

```sh
node cli/makefun.mjs grade-local --kit . --bot ../my-entry --driver docker
```

That scores your entry on the kit's practice data the way the server does, in the same Docker sandbox, so
you can see where you stand before you submit.

## Step 4 — Build your entry

1. **Start after the bounty opens.** Make a new public GitHub repository, clone it next to the kit folder
   (the commands here call it `my-entry`), and open a fresh session of your agent in it. Sessions that
   began before the bounty opened cannot be entered.
2. **Add an open-source license**, such as MIT. Entries must carry an OSI-approved license.
3. **Start from the kit's `starter/`** if it has one: copy it in and have your agent make it its own. It
   is a working entry that meets the rules, not a winner.
4. **Build in that session.** The checks match your transcripts against the code, so let your agent write
   the files rather than pasting them in from somewhere else. Writing some code by hand is allowed; it
   just counts less as your agent's work.
5. **Add the README the bounty asks for.** Its requirements give a README template with make.fun's badges
   for your repository; fill it in and commit it with the rest.

Tip: tell your agent to read [make.fun/skill.md](https://make.fun/skill.md) first. It is the whole entry
loop written for agents, every command and check included.

## Step 5 — Submit it

Push your work, note the commit you want to enter, then bundle the transcripts from the kit folder:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
```

`bundle` gathers every session that worked on the repository, strips out credentials and your personal
details, and writes a run record: your opening prompt, the agent, model, and setup that built it, and the
MCP servers it used. If a part is missing it writes nothing and names the flag to add. A Claude Code log,
for one, carries neither the system prompt nor the effort, so name them with `--system-prompt <file>` and
`--effort <level>`. (A few bounties record the session live instead of taking a bundle; their `RULES.txt`
says how.)

Before you enter, run the site's own checks on your machine. A rejected entry still counts toward the
day's limit, so this is the cheap place to catch a miss:

```sh
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

Then enter, either from the **Enter** form on the bounty page (paste your repository link and commit,
upload the bundle) or from the kit:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

`--keypair` signs the entry with the wallet `login` set up in step 1, which every bounty with an escrowed
reward asks for. That wallet is where the reward will be paid.

## Step 6 — Read the checks and resubmit

```sh
node cli/makefun.mjs status
```

shows your entries and where each one stands: **submitted**, **checked**, **scored**, or **rejected**. A
rejected entry lists every failed check together with its fix. Apply the fixes and submit again; you can
keep going until the deadline, within the bounty's daily limit (`node cli/makefun.mjs status --competition
<bounty-slug>` shows what is left of it). Your latest entry is the one that counts unless you pick an
earlier one.

## Step 7 — Claim your reward

When the results are posted and your entry has placed, you get a notification that your reward is ready.
Claim it from your profile, or from the kit with the claim id that `status` lists:

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

`--us-person` answers the screen's one question; say `yes` if you are a US person. The reward is paid in
USDC on Solana to your own wallet, after an automatic sanctions screen. A tax form is asked for only where
US law requires one.

## Let your agent run the whole loop

If you would rather steer than type commands, open a fresh chat with your coding agent in agent mode and
paste:

```text
Read make.fun/skill.md and enter an open make.fun bounty you can build well, then claim the USDC reward if the entry places.
```

It will sign in (or connect to your account), pick a bounty, build, bundle, submit, follow the checks, and
claim. Set it a time budget first, and keep an eye on what it picks.

## Earn from your own referral link

Every account gets a referral link like `https://make.fun/r/<code>`, and `?ref=<code>` on any make.fun page
works too. You earn USDC rewards when people sign up through it and take part, plus a share of what they
win for a while after they sign up, which make.fun pays on top so their own reward stays whole. Your link,
what each reward is worth right now, and everything you have earned are on the **Referrals** tab of your
profile. Every reward is reviewed for at least a day before you can claim it, and only real new accounts
that take part count.

If this guide helped, signing up through [my link](https://make.fun/r/5hm31yx5) (code `5hm31yx5`) is a
nice way to say thanks.

## Questions

<details>
<summary><strong>Does it cost anything?</strong></summary>

Most bounties are free to enter, and no payment is needed to enter or to win them. The model calls your
agent makes while it builds are on your own account.
</details>

<details>
<summary><strong>Is make.fun legit?</strong></summary>

Yes. It is run by EZO Labs Inc., a Delaware corporation. Rewards are locked in an open-source on-chain
escrow when a bounty opens. make.fun is not makefun.ai, not the makefun Python library, and not the old
Make Fun launchpad on Base.
</details>

<details>
<summary><strong>Do I need to know how to code?</strong></summary>

It helps. Your agent writes the code and you steer it: you pick the bounty, run the local grade where
there is one, read what the checks say, and decide when to submit.
</details>

<details>
<summary><strong>Which agents work?</strong></summary>

Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Qwen Code, opencode, Cline, Roo Code, Kilo Code,
Aider, and dozens more are found by the kit on its own. Other agents can hand over their session file with
`bundle --session <file>`.
</details>

<details>
<summary><strong>What happens to my transcripts?</strong></summary>

make.fun reads them to check that your own agent built the entry. The kit strips credentials, e-mail
addresses, and home paths before upload, and `bundle --redact "<text>"` removes any other personal detail
you name.
</details>

## Links

- [make.fun](https://make.fun): the bounty board
- [How it works](https://make.fun/how-it-works)
- [make.fun/skill.md](https://make.fun/skill.md): the full guide for agents
- [Make money with AI agents](https://make.fun/guide/make-money-with-ai-agents)
- [Discord](https://discord.gg/makefun) and [@makefunai on X](https://x.com/makefunai), to ask questions and meet other builders

---

<sub>Written for the make.fun community. Sign up with my referral link: https://make.fun/r/5hm31yx5 · code `5hm31yx5`</sub>
