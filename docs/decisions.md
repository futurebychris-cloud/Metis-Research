Architectural Decision Log
==========================

Use this file to record significant engineering and research decisions. Do not
record small implementation details.

Entry format
------------

Date:
Decision:
Why it was chosen:
Alternatives considered:
Potential drawbacks:

## 2026-07-19 — Version 1 market and data scope

**Date:** 2026-07-19

**Decision:** Version 1 studies US equities using only `NVDA`. Agents receive
historical OHLCV from fully closed 15-minute candles. They do not receive news,
analyst ratings, social media, fundamentals, human recommendations, prebuilt
technical indicators, other assets, or future information.

**Why it was chosen:** A single asset and neutral market fields create a focused,
auditable first environment with fewer opportunities for feature selection or
future-data leakage.

**Alternatives considered:** Multiple assets; daily or higher-frequency data;
news, fundamentals, or other alternative data; platform-computed indicators.

**Potential drawbacks:** Results may be specific to `NVDA` and its historical
behavior. OHLCV may provide limited context, and 15-minute data requires careful
session, adjustment, and timestamp rules.

## 2026-07-19 — Version 1 agent population and independence

**Date:** 2026-07-19

**Decision:** The final Version 1 population is 30 autonomous agents, with 3–5
agents permitted for early infrastructure tests. All agents use the same model
and have separate portfolios, cash, positions, private memories, and decision
histories. Differences may arise from strategy-neutral reasoning style, memory,
communication, observations, and sampling—not assigned trading strategies.

**Why it was chosen:** Shared model capability makes autonomy, memory, and
communication easier to study while isolated state prevents one agent's private
experience from becoming another's hidden input.

**Alternatives considered:** Different models by agent; strategy-based personas;
shared portfolios, memories, or decision histories; beginning immediately with
all 30 agents.

**Potential drawbacks:** A common model and shared market path may cause highly
correlated behavior. Reasoning-style variation could accidentally encode a
strategy unless it is tightly reviewed and versioned.

## 2026-07-19 — Version 1 execution and portfolio contract

**Date:** 2026-07-19

**Decision:** Agents choose `BUY`, `SELL`, `HOLD`, or `CLOSE`. Version 1 is long
only, without leverage or short selling. Agents decide after observing a fully
closed candle; eligible orders execute at the next candle open with configurable
fees and slippage. Impossible trades are rejected. Each agent starts with
$10,000, may reach 100% exposure, and may place a new order worth at most 25% of
starting capital ($2,500).

**Why it was chosen:** The rules create a simple causal separation between
observation and execution, prevent same-candle leakage, and bound portfolio risk
without prescribing a market strategy.

**Alternatives considered:** Same-candle execution; close-price fills; leverage
or short selling; larger or uncapped orders; fixed rather than configurable
transaction costs.

**Potential drawbacks:** Next-open fills can still be unrealistic without a
careful liquidity model. Long-only and order-size limits constrain the behaviors
agents can discover, and detailed action-sizing semantics remain unresolved.

## 2026-07-19 — Version 1 private memory

**Date:** 2026-07-19

**Decision:** Memory is private to each agent and may contain its observations,
decisions, fills, portfolio history, and self-written notes. Agents cannot
directly access another agent's memory.

**Why it was chosen:** Private memory allows individual behavioral adaptation
while preserving agent independence and enabling controlled memory ablations.

**Alternatives considered:** No memory; shared population memory; direct peer
memory access; centrally generated cross-agent summaries.

**Potential drawbacks:** Memory can reinforce mistakes, create path dependence,
increase context cost, or introduce strategy guidance if retrieval or
summarization is not neutral.

## 2026-07-19 — Version 1 communication treatments

**Date:** 2026-07-19

**Decision:** Version 1A has no communication. Version 1B permits one public
thesis per agent, limited reading, one discussion round, and then a private final
decision. Experiments have no leaderboards.

**Why it was chosen:** Matched communication and no-communication treatments
allow the effect of peer exchange to be studied without exposing private final
decisions or adding competitive performance feedback.

**Alternatives considered:** Unrestricted chat; multiple discussion rounds;
shared memory; consensus decisions; live rankings or leaderboards.

**Potential drawbacks:** Reading selection and message order may influence
results. Even limited communication can amplify errors or cause groupthink, and
the exact reading limit and message schema still require approval.

## 2026-07-19 — Version 1 baselines and useful behavior

**Date:** 2026-07-19

**Decision:** Evaluation includes buy and hold, hold cash, random valid actions,
and one simple predefined strategy clearly labeled as a baseline. Useful behavior
is evaluated using return after costs, drawdown, risk-adjusted performance,
baseline comparison, consistency across historical periods, confidence
calibration, decision diversity, and behavioral convergence—not profit alone.

**Why it was chosen:** Multiple baselines and outcome dimensions reduce the risk
of mistaking nominal profit or one favorable period for useful autonomous
behavior. Baseline strategies remain outside experimental-agent inputs.

**Alternatives considered:** Profit as the sole metric; no baselines; only buy and
hold; supplying baseline behavior to agents as feedback.

**Potential drawbacks:** Multiple metrics complicate inference and can invite
selective reporting unless a primary endpoint is preregistered. The selected
simple strategy and random-action distribution may materially affect comparisons.
