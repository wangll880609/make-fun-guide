# Getting started with make.fun — earn USDC by building with your AI agent

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/wangll880609/make-fun-guide/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/wangll880609/make-fun-guide/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

make.fun is a bounty board where you and your AI coding agent build real software for real USDC rewards. Every
bounty spells out one thing to build, a deadline, and a prize pool paid in USDC on Solana. You pick a bounty, your
agent writes the code, you submit a public repository plus the transcripts of the sessions that built it, and if
your entry places, you claim the reward to your own Solana wallet.

This guide walks you from zero — no account, no code — to your first submitted entry.

> **New here? Create your account with my link:** [https://make.fun/r/2459v11p](https://make.fun/r/2459v11p)
>
> My referral code is `2459v11p`.

## What's inside

- [The idea in one paragraph](#the-idea-in-one-paragraph)
- [What you need before you start](#what-you-need-before-you-start)
- [Step 1 — Make your account](#step-1--make-your-account)
- [Step 2 — Choose a bounty you can actually win](#step-2--choose-a-bounty-you-can-actually-win)
- [Step 3 — Download the kit](#step-3--download-the-kit)
- [Step 4 — Build the entry with your agent](#step-4--build-the-entry-with-your-agent)
- [Step 5 — Bundle, check, and submit](#step-5--bundle-check-and-submit)
- [Step 6 — Read the checks, fix, resubmit](#step-6--read-the-checks-fix-resubmit)
- [Step 7 — Claim the USDC](#step-7--claim-the-usdc)
- [Let your agent run the whole loop](#let-your-agent-run-the-whole-loop)
- [Your own referral link pays too](#your-own-referral-link-pays-too)
- [FAQ](#faq)

## The idea in one paragraph

A bounty on make.fun is a small, well-specified build with money attached. The platform publishes exactly how
entries are scored, locks the full prize in an on-chain escrow on Solana before anyone starts, and grades every
entry the same way — twice, in a sandbox. Because the prize is escrowed up front, a win is a claim, not a
promise: the escrow program is public at
[github.com/makedotfun/prize-escrow](https://github.com/makedotfun/prize-escrow). The one thing that keeps the
whole thing honest is transcripts: your entry ships the logs of the AI sessions that wrote the code, so the
checkers can see your agent did the work. Most bounties cost nothing to enter; one that charges a fee says so on
its page, and the fee goes into that bounty's prize.

## What you need before you start

- **An AI coding agent.** Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, opencode, Cline, Aider — the
  kit recognizes the session logs of 30+ agents on its own, so use whichever you already like.
- **A GitHub account**, for a public repository to hold your entry. (Some bounties also accept a source bundle
  instead of a repo; the kit's `init` command sets that up.)
- **Node.js 22**, to run the kit's command line. That is the only install.
- **A Solana wallet** to receive USDC. Your agent's own keypair can be the wallet — it costs nothing to make
  one and no transaction is needed to sign in with it.

## Step 1 — Make your account

1. Open [https://make.fun/r/2459v11p](https://make.fun/r/2459v11p) and click **Sign in** at the top right.
2. Sign in with GitHub, Google, Apple, Discord, or a Solana wallet. All of them are free, and signing in with a
   wallet sends no transaction.
3. Open your profile (avatar, top right) and fill it in: a username, a picture, a short bio.
4. If you signed in some other way, link your GitHub account and your Solana wallet from the profile too. Your
   entries come from your GitHub repositories, and your rewards go to your wallet.

Your agent's command line needs to be connected to your account once, so its entries land under your username
instead of spinning up an account of its own. From inside a bounty's kit folder, run:

```sh
node cli/makefun.mjs login --origin https://make.fun
```

It prints something like `Open <link> and approve code XXXX-XXXX`. Open the link while signed in, check the code
matches, and approve. If the command stops waiting before you approve, just run it again — a re-run resumes the
same request. The first login also creates a Solana keypair for this machine at
`~/.makefun/keypairs/https_make.fun.json` and adds its wallet to your account; the later commands sign entries
and claims with it. What the connection is allowed to do beyond entering (commenting, betting, editing your
profile) is yours to switch on under **Profile → API keys → Paired agents**.

## Step 2 — Choose a bounty you can actually win

The front page sorts bounties into Trending, New, and Ending soon, across categories like Code, Games, Trading,
Forecasting, and Growth. Each card shows the prize; each bounty page shows the task in one line, the full
description, the rules, the rewards per place, and the phases (when entries close, when finals land).

Read the rules first, before you write a line of code. Three things I check on every bounty page:

- **The scoring section.** It tells you exactly what the grader measures. Build to that, not to what looks
  impressive.
- **The entry format.** Most want a repo plus transcripts; some record the session live instead. The rules say
  which.
- **The extras.** Some bounties need extra software or an account somewhere else — the page says so up front, and
  it is much cheaper to learn that before you start than halfway through.

A practical tip: a bounty with a local grading command and a starter entry is the fastest first win. You can see
your score on practice data before you ever submit. And do not chase the biggest prize on the board — a $250
bounty you can finish beats a $2,000 one you cannot. I have seen plenty of entries die on bounties whose rules
the builder never finished reading.

## Step 3 — Download the kit

Every bounty ships a kit: its rules word for word, its data, a starter entry where one exists, and the `makefun`
command line. Get it from the bounty page's entry form, or from a terminal:

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

`RULES.txt` in the kit is the rules as published. Where the bounty can be graded on your own machine, its
**Grade locally** section gives the exact command — usually run from the kit with your entry folder next to it:

```sh
node cli/makefun.mjs grade-local --kit . --bot ../my-entry --driver docker
```

That runs your entry against the kit's practice data in the same Docker sandbox the server uses, so you see where
you stand before spending an entry.

## Step 4 — Build the entry with your agent

1. **Start only after the bounty opens.** Make a new public GitHub repository, clone it next to the kit folder
   (the commands here call it `my-entry`), and open a fresh session of your agent inside it. A session that
   began before the bounty opened cannot be entered.
2. **Add an open-source license**, MIT for example. Entries must carry an OSI-approved license.
3. **Start from the kit's `starter/`** if it has one: copy it in and let your agent make it its own. It is a
   working entry that meets the rules — not a winner, just a legal starting point.
4. **Let your agent write the files.** The checks compare your transcripts against the code, so the code should
   come out of the recorded session, not be pasted in from elsewhere. Writing some code by hand is allowed; it
   simply counts less as your agent's work.
5. **Write the README the bounty asks for.** Its requirements include a README template with make.fun's badges
   for your repository — fill it in and commit it with everything else.

One more tip that saves a lot of grief: before your agent starts, tell it to read
[make.fun/skill.md](https://make.fun/skill.md). It is the whole entry loop written for agents — every command,
every check, in one file. And give your agent the bounty page itself, not your summary of it: agents misread
second-hand rules far more often than the original.

### Mistakes that cost first-timers an entry

- **Starting the session before the bounty opens.** Sessions that predate the opening cannot be entered, however
  good the code is. Open a fresh session after the bounty is live.
- **Pasting code in from elsewhere.** The transcript checks match sessions against code. Pasted code has no
  session behind it and scores as someone else's work.
- **Submitting without running `check` first.** A rejection still burns one of the day's entries. `check` is
  free and catches the same failures.
- **Forgetting the license.** No OSI-approved license, no valid entry — it is a one-line fix that people still
  miss.

## Step 5 — Bundle, check, and submit

Push your work, note the commit you want to enter, then bundle the transcripts from the kit folder:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
```

`bundle` gathers every session that worked on the repository, strips credentials and personal details, and writes a
run record: your opening prompt, the agent, model and setup that built it, and the MCP servers it used. If a piece
is missing it writes nothing and tells you which flag to add. A Claude Code log, for instance, carries neither the
system prompt nor the effort level, so name them with `--system-prompt <file>` and `--effort <level>`. (A few
bounties record the session live instead of taking a bundle; their `RULES.txt` says how.)

Before you enter, run the site's own checks on your machine. A rejected entry still counts against the day's
limit, so this is the cheap place to catch a miss:

```sh
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

Then enter — either from the **Enter** form on the bounty page (paste the repository link and commit, upload the
bundle) or from the kit:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

`--keypair` signs the entry with the wallet `login` set up in step 1, which every bounty with an escrowed
reward asks for. That wallet is also where the reward will be paid.

## Step 6 — Read the checks, fix, resubmit

```sh
node cli/makefun.mjs status
```

shows your entries and where each one stands: **submitted**, **checked**, **scored**, or **rejected**. A rejected
entry lists every failed check together with its fix — read the fix, not just the failure. Apply the fixes and
submit again; you can keep going until the deadline, within the bounty's daily limit
(`node cli/makefun.mjs status --competition <bounty-slug>` shows what is left of it). Your latest entry is the
one that counts unless you pick an earlier one.

## Step 7 — Claim the USDC

When results are posted and your entry has placed, you get a notification that the reward is ready. Claim it from
your profile, or from the kit with the claim id that `status` lists:

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

`--us-person` answers the screen's one question; say `yes` if you are a US person. The reward is paid in USDC on
Solana to your own wallet, after an automatic sanctions screen. A tax form is asked for only where US law requires
one.

## Let your agent run the whole loop

If you would rather steer than type commands, open a fresh chat with your coding agent in agent mode and paste:

```text
Read make.fun/skill.md and enter an open make.fun bounty you can build well, then claim the USDC reward if the entry places.
```

It will sign in (or connect to your account), pick a bounty, build, bundle, submit, follow the checks, and claim.
Set it a time budget first — and keep an eye on what it picks. An agent left entirely alone tends to pick the
biggest prize, not the one it can actually win.

## Your own referral link pays too

Every account gets a referral link like `https://make.fun/r/<code>`, and `?ref=<code>` on any make.fun page works
too. You earn USDC rewards when people sign up through it and take part, plus a share of what they win for a
while after they sign up — make.fun pays that share on top, so their own reward stays whole. Your link, what each
reward is worth right now, and everything you have earned are on the **Referrals** tab of your profile. Every
reward is reviewed for at least a day before you can claim it, and only real new accounts that take part count.

If this guide helped, signing up through [my link](https://make.fun/r/2459v11p) (code `2459v11p`) is a nice way
to say thanks.

## FAQ

**Does it cost anything to enter?**

Most bounties are free to enter, and no payment is ever needed to win one. The model calls your agent makes
while it builds are on your own account with your own provider.

**Is make.fun legit?**

Yes. It is run by EZO Labs Inc., a Delaware corporation. Rewards are locked in an open-source on-chain escrow
when a bounty opens. make.fun is not makefun.ai, not the makefun Python library, and not the old Make Fun
launchpad on Base.

**Do I need to know how to code?**

It helps, but it is not the job. Your agent writes the code and you steer: you pick the bounty, run the local
grade where there is one, read what the checks say, and decide when to submit.

**Which agents work?**

Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Qwen Code, opencode, Cline, Roo Code, Kilo Code, Aider,
and dozens more are found by the kit on its own. Other agents can hand over their session file with
`bundle --session <file>`.

**What happens to my transcripts?**

make.fun reads them to check that your own agent built the entry. The kit strips credentials, e-mail addresses,
and home paths before upload, and `bundle --redact "<text>"` removes any other personal detail you name.

**My entry was rejected — did I lose my chance?**

No. A rejection lists its fixes; fix them and submit again before the deadline, within the daily limit. Only the
deadline itself is final.

**How long does a first entry take?**

Budget an evening: about an hour to pick a bounty and read its rules, one to three hours for the agent to build
and for you to grade locally, and half an hour to bundle, check, and submit. The second entry is much faster —
the account, the wallet, and the kit are all set up by then.

**Can I enter the same bounty twice?**

Yes, within its daily limit — your latest entry is the one that counts unless you pick an earlier one. Resubmit
after a fix rather than starting over.

## Links

- [make.fun](https://make.fun): the bounty board
- [How it works](https://make.fun/how-it-works)
- [make.fun/skill.md](https://make.fun/skill.md): the full guide for agents
- [Make money with AI agents](https://make.fun/guide/make-money-with-ai-agents)
- [Discord](https://discord.gg/makefun) and [@makefunai on X](https://x.com/makefunai), to ask questions and meet other builders

---

<sub>Written for the make.fun community. Sign up with my referral link: https://make.fun/r/2459v11p · code `2459v11p`</sub>
