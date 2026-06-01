---
name: run-multiple-scans
description: "Run all stock scanners by dispatching staggered cron jobs 5 minutes apart. Triggered by Run, Run multiple scans, run scans."
---

# Run Multiple Scans

Dispatch all 41 scanners via **staggered cron jobs** (5 minutes apart, one scanner per turn). This avoids gateway overload from firing all 41 messages simultaneously.

## Trigger

When the user says any of the following (case-insensitive), run this skill immediately without asking questions: "Run", "Run multiple scans", "run scans", "Run scans", "run the scans"

## How It Works

1. Create 41 one-shot cron jobs, each firing 5 minutes apart
2. Each cron job sends an `agentTurn` message to the coordinator's current session with the scanner's sessionKey
3. The coordinator receives the message and dispatches a single `sessions_send` (fire-and-forget, `timeoutSeconds: 0`) to that scanner
4. Each cron job auto-deletes after firing (`deleteAfterRun: true`)
5. Report completion after all 41 cron jobs are created

**CRITICAL: Never fire all 41 `sessions_send` calls in one turn — this overloads the gateway and causes timeouts.**

## Dispatch Template

### Step 1: Create staggered cron jobs

For each scanner, create a cron job 5 minutes apart:

```
cron({
  action: "add",
  job: {
    name: "dispatch-<scanner-name>",
    schedule: { kind: "at", at: "<ISO-8601 timestamp, 5 min apart>" },
    payload: { kind: "agentTurn", message: "DISPATCH: Send scan command to <scanner-name> at sessionKey <sessionKey>", timeoutSeconds: 60 },
    sessionTarget: "current",
    deleteAfterRun: true,
    enabled: true
  }
})
```

### Step 2: When cron fires, dispatch the scanner

When the coordinator receives the DISPATCH message, call:

```
sessions_send({
  sessionKey: "<sessionKey>",
  message: "Read and run your own scanner skill now. Cover all your standard criteria and deliver results to your Discord channel.",
  timeoutSeconds: 0
})
```

Then reply NO_REPLY (no channel noise for each individual dispatch).

## Scanner Table

| # | agentId | sessionKey |
|---|---------|------------|
| 1 | sector-rotation-scanner | agent:sector-rotation-scanner:discord:channel:<discord-channel-id> |
| 2 | technical-pattern-scanner | agent:technical-pattern-scanner:discord:channel:<discord-channel-id> |
| 3 | unusual-options-scanner | agent:unusual-options-scanner:discord:channel:<discord-channel-id> |
| 4 | short-squeeze-scanner | agent:short-squeeze-scanner:discord:channel:<discord-channel-id> |
| 5 | accumulation-scanner | agent:accumulation-scanner:discord:channel:<discord-channel-id> |
| 6 | breaking-news-scanner | agent:breaking-news-scanner:discord:channel:<discord-channel-id> |
| 7 | second-order-scanner | agent:second-order-scanner:discord:channel:<discord-channel-id> |
| 8 | 48-hour-surge-scanner | agent:48-hour-surge-scanner:discord:channel:<discord-channel-id> |
| 9 | 3-day-5pct-scanner | agent:3-day-5pct-scanner:discord:channel:<discord-channel-id> |
| 10 | 7-day-explosive-scanner | agent:7-day-explosive-scanner:discord:channel:<discord-channel-id> |
| 11 | operational-milestone-scanner | agent:operational-milestone-scanner:discord:channel:<discord-channel-id> |
| 12 | ipo-spac-scanner | agent:ipo-spac-scanner:discord:channel:<discord-channel-id> |
| 13 | pre-surge-scanner | agent:pre-surge-scanner:discord:channel:<discord-channel-id> |
| 14 | pre-catalyst-scanner | agent:pre-catalyst-scanner:discord:channel:<discord-channel-id> |
| 15 | pre-move-discovery-scanner | agent:pre-move-discovery-scanner:discord:channel:<discord-channel-id> |
| 16 | pullback-buy-scanner | agent:pullback-buy-scanner:discord:channel:<discord-channel-id> |
| 17 | dip-before-catalyst-scanner | agent:dip-before-catalyst-scanner:discord:channel:<discord-channel-id> |
| 18 | contrarian-sentiment-scanner | agent:contrarian-sentiment-scanner:discord:channel:<discord-channel-id> |
| 19 | oversold-bounce-scanner | agent:oversold-bounce-scanner:discord:channel:<discord-channel-id> |
| 20 | base-breakout-scanner | agent:base-breakout-scanner:discord:channel:<discord-channel-id> |
| 21 | mega-mover-scanner | agent:mega-mover-scanner:discord:channel:<discord-channel-id> |
| 22 | sec-filing-anomaly-scanner | agent:sec-filing-anomaly-scanner:discord:channel:<discord-channel-id> |
| 23 | insider-buying-scanner | agent:insider-buying-scanner:discord:channel:<discord-channel-id> |
| 24 | insider-selling-dilution-scanner | agent:insider-selling-dilution-scanner:discord:channel:<discord-channel-id> |
| 25 | policy-catalyst-scanner | agent:policy-catalyst-scanner:discord:channel:<discord-channel-id> |
| 26 | international-exchanges-scanner | agent:international-exchanges-scanner:discord:channel:<discord-channel-id> |
| 27 | event-driven-scanner | agent:event-driven-scanner:discord:channel:<discord-channel-id> |
| 28 | analyst-upgrade-downgrade-scanner | agent:analyst-upgrade-downgrade-scanner:discord:channel:<discord-channel-id> |
| 29 | earnings-estimate-revision-scanner | agent:earnings-estimate-revision-scanner:discord:channel:<discord-channel-id> |
| 30 | pre-earnings-runup-scanner | agent:pre-earnings-runup-scanner:discord:channel:<discord-channel-id> |
| 31 | earnings-post-event-scanner | agent:earnings-post-event-scanner:discord:channel:<discord-channel-id> |
| 32 | crypto-correlation-scanner | agent:crypto-correlation-scanner:discord:channel:<discord-channel-id> |
| 33 | biotech-catalyst-scanner | agent:biotech-catalyst-scanner:discord:channel:<discord-channel-id> |
| 34 | biotech-conference-scanner | agent:biotech-conference-scanner:discord:channel:<discord-channel-id> |
| 35 | pdufa-scanner | agent:pdufa-scanner:discord:channel:<discord-channel-id> |
| 36 | small-cap-earnings-scanner | agent:small-cap-earnings-scanner:discord:channel:<discord-channel-id> |
| 37 | small-cap-explosive-scanner | agent:small-cap-explosive-scanner:discord:channel:<discord-channel-id> |
| 38 | nano-cap-earnings-scanner | agent:nano-cap-earnings-scanner:discord:channel:<discord-channel-id> |
| 39 | nano-cap-explosive-scanner | agent:nano-cap-explosive-scanner:discord:channel:<discord-channel-id> |
| 40 | gap-and-go-scanner | agent:gap-and-go-scanner:discord:channel:<discord-channel-id> |
| 41 | momentum-scanner | agent:momentum-scanner:discord:channel:<discord-channel-id> |

## Rules

- **Stagger dispatches.** Never send all 41 `sessions_send` calls in one turn. Always use staggered cron jobs 5 minutes apart.
- **Fire-and-forget only.** Use `timeoutSeconds: 0` on all sessions_send calls. Do NOT wait for results.
- **sessionTarget: "current".** Cron jobs must use `sessionTarget: "current"` with `payload.kind: "agentTurn"` (non-default agents cannot use `sessionTarget: "main"`).
- **deleteAfterRun: true.** Each cron job auto-deletes after firing.
- **sessionKey format.** Always use `agent:{agentId}:discord:channel:{channelId}`.
- **Discord only.** Every scanner posts to its own bound Discord channel.
- **No questions.** Start immediately on trigger.
- **No channel noise.** When a cron fires a DISPATCH, send the sessions_send and reply NO_REPLY.