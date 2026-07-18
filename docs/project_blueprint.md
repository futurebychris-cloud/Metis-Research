# Metis Research Project Blueprint

## Project overview

Metis Research is a scientific platform for studying whether a population of 30
autonomous AI agents can develop useful behavior in historical financial-market
simulations without predefined trading strategies or substantial outside help.
The agents, not a human operator, observe the permitted information, form
hypotheses, decide whether to trade, and learn from their own prior experience.
All activity is paper trading.

The project is scientifically interesting because it combines sequential
decision-making, partial information, memory, multi-agent communication, and
non-deterministic language models in a setting with measurable outcomes. It is
also unusually easy to invalidate. Future-data leakage, survivorship bias,
unrealistic fills, strategy-laden prompts, or repeated tuning on the same period
could produce convincing but false results. The research protocol and causal
boundaries therefore matter at least as much as the agent implementation.

The platform should be treated as an experimental instrument, not as an AI
trading product. It should make controlled comparisons possible and preserve
enough evidence to explain what each agent was allowed to know and what the
system did at every simulated time.

As a software system, the concept is feasible if it begins with a deterministic
causal replay and accounting core, then adds LLM behavior behind narrow audited
boundaries. As a research system, its strongest contribution would be controlled
evidence about autonomy, memory, communication, and convergence—not a single
profitable backtest. Thirty agents provide a population in which interactions can
be studied, but the number 30 does not by itself establish statistical power and
should be justified against replication needs and API cost.

Thirty agents are the target population within a full-scale simulation run; they
are not automatically 30 statistically independent observations. Agents can
share a model family, market path, and experimental conditions. Claims should
therefore rely on replicated runs, seeds where supported, multiple market
periods, and appropriate uncertainty estimates rather than treating every agent
as an independent sample.

## Primary research question

In a historical NVDA market replay using fully closed 15-minute OHLCV candles,
can 30 minimally guided AI agents using the same model independently develop
out-of-sample market behavior that is useful relative to preregistered non-LLM
baselines?

Useful behavior is not simply profitable behavior. Version 1 evaluates return
after costs, drawdown, risk-adjusted performance, comparison against baselines,
consistency across historical periods, confidence calibration, decision
diversity, and behavioral convergence. The aggregation, primary endpoint, and
success threshold remain to be preregistered. Negative, unstable, economically
immaterial, and inconclusive outcomes remain valid results.

The initial meaning of agent "learning" should be behavioral adaptation through
agent-visible memory and history, not an unverified claim that the underlying
model weights have learned. Any later online training or weight updates would be
a separate experimental intervention.

## Secondary research questions

1. **Communication versus no communication:** Under otherwise matched
   conditions, how does controlled agent-to-agent communication affect
   performance, calibration, idea diversity, error propagation, and systemic
   risk?
2. **Memory versus no memory:** Does access to private, causally valid memory
   improve adaptation and consistency, or does it reinforce mistakes and create
   path dependence compared with agents whose context is reset?
3. **Agent diversity:** Do heterogeneous populations outperform homogeneous
   populations in robustness or discovery without merely benefiting from more
   compute? Which strategy-neutral sources of diversity matter?
4. **Groupthink and behavioral convergence:** When and how do agents converge in
   hypotheses, orders, exposures, or errors? Does communication cause convergence
   beyond that explained by shared data and shared base models?
5. **Performance against non-LLM baselines:** Do experimental agents add value
   over buy-and-hold, hold-cash, random-valid-action, and one labeled simple
   predefined-strategy baseline under matched constraints? Baselines are
   evaluators and are never supplied to experimental agents as advice.

## Approved Version 1 research contract

The following choices are approved for Version 1. Parameters marked as open
later in this document require a separate decision before implementation.

| Area | Version 1 decision |
|---|---|
| Market | US equities |
| Asset universe | NVIDIA (`NVDA`) only |
| Agent-visible market data | Historical OHLCV only |
| Timeframe | Fully closed 15-minute candles |
| Target population | 30 autonomous agents |
| Early infrastructure scale | 3–5 agents |
| Model allocation | The same model for every agent |
| Allowed actions | `BUY`, `SELL`, `HOLD`, `CLOSE` |
| Position rules | Long only; no short selling and no leverage |
| Execution timing | Decide after a candle closes; execute eligible orders at the next candle open |
| Starting capital | $10,000 per agent |
| Maximum exposure | 100% per agent |
| Maximum new order | 25% of starting capital ($2,500) |
| Memory | Private and isolated by agent |
| Communication | Version 1A disabled; Version 1B controlled |
| Trading mode | Paper trading only |

Fees and slippage are configurable experimental parameters. Impossible trades
are rejected. Under the long-only contract, `SELL` and `CLOSE` cannot create a
negative position; their final sizing schema still requires approval.

## The role of the 30 autonomous agents

Each agent is an independent decision-making subject within a simulation. It
receives an agent-specific observation, may create its own calculations and
hypotheses, optionally exchanges messages under the experiment's communication
protocol, and privately submits a final decision. It has its own portfolio,
trade history, memory, context, identifiers, and random stream where randomness
is controllable.

Version 1 targets 30 agents; infrastructure tests may begin with 3–5. Every agent
uses the same model. Differences may emerge from strategy-neutral reasoning
style, private memory, controlled communication, agent-specific observations,
and sampling. The system must not create differences by assigning predefined
trading strategies.

Agents independently choose `BUY`, `SELL`, `HOLD`, or `CLOSE`, subject only to
the declared long-only paper-trading and neutral portfolio rules. The platform
must not choose trades for them, rank candidate trades for them, or ask a human
user to supply market analysis. Human researchers define the experiment,
constraints, and measurements; they do not participate in the simulated trading
loop.

Meaningful independence does not require agents to disagree. Natural convergence
is a result to measure. It does require that convergence not be forced by shared
memory, copied final decisions, sequential access to other agents' private
actions, a platform-generated consensus, or hidden common guidance.

To preserve independence:

- Memory, portfolio state, trade history, model context, and final decisions are
  isolated by agent and simulation run.
- Every agent decides from the same causal market cutoff, even if execution is
  processed serially by software.
- In no-communication treatments, agents cannot access messages or another
  agent's state, reasoning, orders, fills, performance, or memory.
- In communication treatments, only messages allowed by the declared topology,
  timing, audience, and budget are shared. Messages retain their source and are
  never rewritten into a platform-endorsed conclusion.
- Communication closes before a private final-decision step. Final decisions are
  not exposed to peers until the experimental protocol permits later historical
  disclosure, if it permits it at all.
- Agent-specific random streams or independently sampled model calls reduce
  lockstep output. Any unsupported or non-deterministic sampling is documented.
- Reasoning-style differences must be strategy-neutral and versioned. Prompts
  such as "be a momentum trader" or "be contrarian" would violate the research
  premise.
- The system measures convergence in text, actions, holdings, and returns, but
  does not artificially force disagreement. Forced disagreement would itself be
  outside guidance.

## Definition of minimal outside guidance

Minimal outside guidance means that the platform supplies only information and
mechanical rules necessary to conduct a valid, safe, and interpretable
simulation. It does not supply a theory of how to profit.

The system may tell an agent:

- its role as an autonomous paper-trading research subject;
- the current simulated timestamp and decision deadline;
- the approved `NVDA` instrument and causally available OHLCV fields from fully
  closed 15-minute candles;
- its cash, positions, prior fills, and private memory;
- the `BUY`, `SELL`, `HOLD`, and `CLOSE` action schema and the meaning of each
  field;
- the long-only, no-leverage rules; $10,000 starting capital; 100% maximum
  exposure; $2,500 maximum new order; and configured fees and slippage;
- whether communication is enabled and the mechanical communication rules; and
- that it must return a private structured decision, including no action when it
  chooses not to trade.

The system must not tell an agent:

- what market pattern to seek or which calculation to perform;
- that a specific asset, direction, signal, indicator, horizon, or trading style
  is promising;
- how successful agents or baselines are currently behaving;
- how to improve return, beat a benchmark, or imitate a peer;
- how an evaluator will reward a particular market action; or
- a worked trading example whose content implicitly demonstrates a strategy.

Neutrality applies to the entire agent-facing environment, not just the main
prompt. Field names, examples, default values, error messages, memory summaries,
communication summaries, retry prompts, and evaluation feedback can all teach a
strategy. Each must be minimal, versioned, reviewable, and identical across
matched experimental conditions except for the intervention being studied.

Mechanical constraints are permitted when they limit operational risk without
selecting a market view. They must be symmetric where possible, declared in the
experiment configuration, and enforced after the agent decides rather than
described as advice. A constraint that materially favors a style or direction
must be treated as an experimental choice, not assumed to be neutral.

## Permitted and prohibited agent inputs

### Permitted inputs

- Historical `NVDA` open, high, low, close, and volume values from fully closed
  15-minute candles at or before the current simulated cutoff.
- The agent's own cash, positions, orders, fills, realized history, and permitted
  mark-to-market state.
- The agent's own private memory derived only from information previously
  available to that agent in the same simulation run.
- Agent-authored messages delivered through the active communication treatment.
- Mechanical paper-trading rules, neutral portfolio limits, and order validation
  errors.
- The experiment's current logical time and identifiers needed for traceability.

OHLCV is an aggregation rather than literal raw exchange events. Its source,
construction, adjustment policy, and availability time must be documented.
Agents may derive their own indicators, summaries, hypotheses, or methods from
the permitted OHLCV history, but the platform does not supply those derivations.

### Prohibited inputs

- Any event, price, bar completion, revised value, constituent membership, or
  corporate-action knowledge unavailable at the simulated cutoff.
- Analyst ratings, prewritten conclusions, human recommendations, selected
  technical indicators, platform-ranked features, or hints about profitable
  behavior.
- News, social-media content, fundamentals, or data for assets other than `NVDA`.
- Predefined strategies, strategy personas, hidden rules, worked trade examples,
  or prompts that encode entry and exit logic.
- Another agent's private memory, portfolio, reasoning, decision, order, or
  performance unless a specifically approved communication experiment exposes a
  precisely declared subset.
- Live leaderboards, baseline actions, future evaluation results, reward shaping,
  or feedback designed to steer the next trade.
- Data chosen after observing test-period results without declaring the resulting
  experiment exploratory.

Historical outcomes from the agent's own completed trades are permitted because
they are part of its allowed history. Evaluation metrics computed for researchers
must remain outside the decision loop. The agent may calculate its own assessment
from its permitted history.

## Scope and non-goals

### Long-term scope

- Version 1 historical replay of `NVDA` 15-minute OHLCV with explicit causal
  time boundaries.
- Thirty isolated experimental agents and controlled population configurations.
- Private memory, optional communication, paper portfolios, and simulated
  execution.
- Reproducible experiment definitions, immutable audit evidence, offline
  evaluation, and comparison with simple non-LLM baselines.
- Exploratory and confirmatory research across multiple historical conditions.

### Non-goals

- Live trading, brokerage integration, real-money execution, investment advice,
  or a human-operated trading terminal.
- Providing experimental agents with predefined trading strategies, curated
  signals, or human market conclusions.
- Optimizing a production trading system or promising profitability.
- Treating paper performance as evidence of live deployability.
- Claiming causal learning from a single market path or claiming exact LLM
  reproducibility when the provider cannot guarantee it.
- Building future roadmap components before their experimental and causal
  contracts are approved.

This architecture phase produces documentation only. It does not create APIs,
database schemas, agent runtimes, replay code, trading code, LLM integrations,
dashboard code, or placeholder modules.

## Research integrity principles

1. **Agents trade; researchers design experiments.** Human choices set the
   experimental envelope but never supply decisions during a run.
2. **No predefined strategies for experimental agents.** Mechanical non-LLM
   strategies may exist only as clearly separated evaluation baselines.
3. **Minimal intervention.** Every agent-facing instruction and derived field
   needs a necessity and neutrality justification.
4. **Chronological access.** Every observation, message, memory item, and state
   transition carries a logical time and obeys the decision cutoff.
5. **No future-data leakage.** Causal access is enforced by construction and
   tested with adversarial boundary cases, not left to prompt compliance.
6. **Paper trading only.** No component should hold brokerage credentials or
   route orders to a live venue.
7. **Reproducibility.** Preserve configurations, prompt versions, model/provider
   identifiers, sampling settings, seeds where available, dataset versions,
   source-code revision, environment metadata, and run manifests.
8. **Complete audit logs.** Record all externally observable inputs, messages,
   structured hypotheses, decisions, orders, fills, state changes, validation
   events, and model-call metadata. "Complete" does not imply access to hidden
   chain-of-thought or internal model activations; the protocol should request a
   concise, auditable rationale without depending on private chain-of-thought.
9. **Separation of action and evaluation.** Evaluation is offline and cannot
   influence agents within the run unless feedback is itself a declared
   experimental treatment.
10. **Preregistration and honest reporting.** Declare hypotheses, primary
    endpoints, exclusions, baselines, periods, and analysis plans before
    confirmatory runs. Report failed, negative, unstable, and inconclusive results.
11. **No silent protocol drift.** A change to prompts, observation fields,
    models, execution rules, memory, or communication creates a new versioned
    experimental condition.
12. **Appropriate inference.** Account for shared market paths and shared models;
    report uncertainty across replicated runs and periods rather than inflating
    sample size with correlated agents or time steps.

## High-level architecture

The future system should use a small set of components connected by explicit,
typed domain events. A single-process implementation is preferable at first.
Component boundaries describe responsibilities and causal isolation; they do not
require microservices, message brokers, or separate deployments.

```text
Experiment Definition + Versioned Artifacts
                    |
                    v
          Orchestrator / Causal Clock
             |                 |
             v                 v
      Market Data Replay   Agent Registry
             |                 |
             +-------> Observation Builder
                               |
                               v
                       Isolated Agent Runtime <----> Private Memory
                               |
                    optional controlled messages
                               |
                               v
                       Communication Broker
                               |
                    private final decision
                               v
                      Order Gateway / Validator
                               |
                               v
                       Execution Simulator
                               |
                               v
                       Portfolio Ledger

All components ----------> Append-only Audit/Event Log
Audit/Event Log ----------> Offline Evaluation + Reproducibility Artifacts
```

The arrows represent allowed information flow, not deployment boundaries. The
offline evaluator has no return path into an active run.

## Component responsibilities

### Experiment definition and artifact registry

Defines the population, treatment assignments, dataset version, time range,
observation schema, prompts, model settings, memory policy, communication policy,
portfolio limits, execution assumptions, and evaluation plan. It assigns stable
identifiers and content hashes to consequential artifacts. A frozen run manifest
is created before simulation begins.

### Orchestrator and causal clock

Advances logical time, starts decision rounds, coordinates component calls, and
enforces phase ordering. It prevents an agent from observing information created
after its decision cutoff. It should coordinate work, not analyze markets or
modify decisions.

### Market data store and replay

Loads versioned historical `NVDA` 15-minute OHLCV and exposes only fully closed
candles available by the current logical time. It handles timestamps, market
calendars, and the declared adjustment policy. It must not expose other assets,
news, ratings, social media, fundamentals, recommendations, or
strategy-oriented features in Version 1.

### Neutral observation builder

Constructs the agent-visible snapshot from causally available market fields and
that agent's private state. It applies the declared field schema without ranking,
selecting, interpreting, or summarizing assets in a way that suggests a trade.
For Version 1, the market portion contains only fully closed `NVDA` 15-minute
OHLCV candles. The exact rendered observation is logged.

### Agent registry and isolated agent runtime

Maintains agent identities and treatment assignments. For each agent, the runtime
assembles permitted context, invokes the configured model, captures a structured
hypothesis or concise rationale, and requests a private final decision. It does
not repair a market opinion or substitute a trade. Format retries repeat neutral
schema instructions and are logged. Version 1 uses the same model for all agents,
targets 30 agents, and permits 3–5 agents for early infrastructure testing.

### Private memory service

Stores and retrieves memory within the `(experiment, simulation run, agent)`
boundary. It applies a declared retention and summarization policy. Memory content
may contain the agent's observations, decisions, fills, portfolio history, and
self-written notes, all derived from information previously available to that
agent. No agent may read another agent's memory. Memory summarization is an
experimental intervention because a summarizer can alter or steer future
behavior.

### Controlled communication broker

Version 1A disables communication. Version 1B permits each agent to publish one
public thesis, read a limited approved set of theses, participate in one
discussion round, and then make a private final decision. The broker enforces
audience, reading limits, message budgets, timing, and delivery order. It logs
messages verbatim with source and time. It does not synthesize consensus, rank
speakers, endorse content, expose private final decisions, or provide a
leaderboard during an experiment.

### Order gateway and validator

Converts an agent's private structured decision into an order request when the
agent chose to act. It checks syntax, available instruments, quantities, cash,
position rules, and neutral limits. Version 1 accepts only `BUY`, `SELL`, `HOLD`,
and `CLOSE`; rejects trades that would exceed cash, a long position, 100%
exposure, or the $2,500 maximum new-order limit; and never permits leverage or a
negative position. Rejections explain only the mechanical violation. The gateway
never changes side, instrument, size, or timing to make an order more favorable.

### Execution simulator

An agent observes a fully closed candle and decides afterward. An eligible order
executes at the next candle open; same-candle execution is prohibited. The
simulator applies configurable fees and slippage and uses no future price when
forming the decision. Detailed liquidity and partial-fill assumptions remain to
be approved.

### Portfolio ledger

Is the authoritative source for each agent's cash, positions, orders, fills, and
realized accounting. Portfolios are separate even when agents trade the same
instrument. State changes occur only from validated ledger events, not from agent
narrative. Each Version 1 agent starts with $10,000, may use at most 100% exposure,
and may place a new order worth no more than 25% of starting capital ($2,500).

### Append-only event and audit log

Records experiment configuration, causal cutoffs, exact agent-visible inputs,
model-call metadata and externally visible outputs, communications, decisions,
orders, fills, portfolio transitions, memory changes, errors, and component
versions. Events should be immutable and ordered within a run. Sensitive provider
credentials must never enter the log.

### Offline evaluator and baseline runner

Computes preregistered metrics after a run and compares treatments with buy and
hold, hold cash, random valid actions, and one labeled simple predefined-strategy
baseline. It evaluates return after costs, drawdown, risk-adjusted performance,
baseline-relative performance, consistency across historical periods, confidence
calibration, decision diversity, and behavioral convergence. It reads run
artifacts but cannot write to an agent's context, memory, portfolio, or active
experiment.

## Agent autonomy safeguards

### Causal and state isolation

Every access is scoped by experiment ID, run ID, agent ID, and simulated cutoff.
The portfolio ledger and memory service deny cross-agent reads by default.
Observation generation uses a snapshot taken before any agent in the round acts.
Processing order therefore cannot give later-processed agents more market or peer
information.

### Private final decisions

Communication, when enabled, occurs in a bounded phase. Each agent then receives
a private decision call. No vote, majority rule, supervisor agent, human approval,
or platform consensus changes the result. Peers cannot see another final decision
before submitting their own for that round.

### Prompt neutrality and governance

- Keep system instructions limited to role, permitted information, causal time,
  mechanical rules, and output schema.
- Do not include few-shot trading examples or strategy-coded personas.
- Version and hash system prompts, observation templates, retry messages, memory
  prompts, and communication instructions.
- Review agent-facing text for directional language, favored horizons, named
  indicators, implicit scoring rules, and examples that reward a trade.
- Change one experimental factor at a time in matched comparisons where
  practical, and use prompt ablations to test sensitivity.
- Log the exact rendered context rather than assuming a template proves what the
  agent saw.

### Feature neutrality

Begin with a small approved catalog of point-in-time market fields. Derived
features supplied by the platform require a separate justification and treatment
because selection itself embeds a theory of relevance. Agents may calculate any
method from permitted data inside their private process. Data cleaning,
adjustments, missing-value handling, and universe construction are versioned so
they cannot silently encode future knowledge.

### Evaluation isolation

Research metrics, peer rankings, baseline behavior, and aggregate success remain
offline. During a run the system returns only mechanical consequences such as an
order rejection, fill, cash change, or position change. Any richer learning
feedback must be explicitly approved as a new treatment and cannot be described
as minimal guidance.

### Communication neutrality

The broker controls transport, not content. It can enforce length and topology,
but cannot summarize a group into a recommended action. Strategy ideas that one
agent independently communicates to another are legitimate treatment effects;
platform-authored strategy content is not. Communication and no-communication
runs must otherwise use matched data, timing, resources, and agent conditions.

### Detecting convergence without manufacturing diversity

Evaluation should compare similarity of hypotheses, message content, decisions,
instruments, direction, order sizes, holdings, and return paths. It should
distinguish convergence before communication from convergence after messages.
Homogeneous and heterogeneous cohorts should be declared treatments. The system
may preserve independent contexts and sampling but must not force agents to be
different merely to improve ensemble appearance.

## Full future data flow

```text
Versioned historical NVDA 15-minute OHLCV
  -> causal replay cutoff after a fully closed candle at simulated time t
  -> neutral per-agent observation of closed candles and private state
  -> private agent reasoning and hypothesis
  -> no communication (1A) or controlled agent-authored communication (1B)
  -> private final BUY / SELL / HOLD / CLOSE decision
  -> validated long-only order (when the action requires one)
  -> simulated fill at the next candle open with configured fees and slippage
  -> isolated portfolio and trade-history update
  -> private memory update using only causally available experience
  -> append-only audit events
  -> offline evaluation, baseline comparison, and research reporting
```

At each decision round:

1. The orchestrator freezes a causal snapshot after a 15-minute `NVDA` candle is
   fully closed. The next candle's open is not part of the observation.
2. The observation builder combines that snapshot with each agent's own
   portfolio, permitted history, memory, and delivered messages.
3. Each agent privately forms a hypothesis. The platform records a concise
   agent-authored rationale or structured hypothesis but does not prescribe its
   content or require inaccessible hidden chain-of-thought.
4. Version 1A has no communication. In Version 1B, each agent publishes one
   public thesis, reads a limited permitted set, and participates in one
   discussion round. There is no leaderboard.
5. Each agent receives a private final-decision opportunity based on the same
   market cutoff and its permitted state.
6. `BUY`, `SELL`, and `CLOSE` decisions create an order when their approved
   sizing semantics require one. `HOLD` creates no order but remains auditable.
7. The validator enforces cash, position, long-only, exposure, and new-order
   limits without improving the order. Eligible orders execute at the next
   candle open with configured fees and slippage. No same-candle execution or
   negative position is permitted.
8. Each fill updates only the originating agent's ledger. The agent later sees
   the result when the causal clock makes it available.
9. The memory policy stores or summarizes only information already available to
   that agent. It cannot include evaluator conclusions or peer-private state.
10. All visible events and artifact versions are logged. Evaluation begins only
    after the run is sealed.

## Core domain terminology

| Term | Meaning | Important boundary |
|---|---|---|
| **Observation** | The exact point-in-time information presented to one agent for a decision round. In Version 1 this includes fully closed `NVDA` 15-minute OHLCV and permitted private state. | It excludes the next candle open and any platform market conclusion. |
| **Hypothesis** | An agent-authored, testable or explanatory belief about market behavior or uncertainty. | It is reasoning content, not necessarily an instruction to trade. |
| **Agent decision** | The agent's private `BUY`, `SELL`, `HOLD`, or `CLOSE` choice, with any proposed intent and concise rationale. | A decision is not yet an accepted order or fill. |
| **Order** | A validated request to transact a specified instrument, side, quantity, type, and eligibility time. | It may be rejected, remain unfilled, partially fill, or fill. |
| **Fill** | A simulated execution event assigning quantity, price, time, and costs to an eligible order. | Only fills change holdings and cash. |
| **Position** | One agent's current quantity and accounting state for one instrument, derived from fills and adjustments. | A desired exposure or open order is not a position. |
| **Portfolio** | The authoritative collection of an agent's cash, positions, and related accounting state. | Every agent has a separate portfolio; narrative claims do not alter it. |
| **Memory** | Agent-private retained information from causally available prior experience in the same run, under a declared policy. | It excludes future data, evaluator feedback, and unauthorized peer state. |
| **Experiment** | A research specification defining hypotheses, treatments, controls, population, data, artifacts, metrics, and analysis plan. | One experiment may require many replicated simulation runs. |
| **Simulation run** | One execution of a frozen experiment configuration over a particular market path and randomness/model-call realization. | A run is a replicate, not the complete experiment. |

## Phased roadmap

Each phase should produce a small, independently testable capability. Later
phases should begin only after the preceding causal and accounting contracts are
verified.

### Phase 0: research protocol and architecture

- Record the approved Version 1 research contract and its remaining open
  parameters.
- Define the initial research hypothesis and neutrality review.
- Approve the data source, timestamp details, historical periods, and metric
  aggregation before implementation.

Deliverable: approved documentation only.

### Phase 1: chronological observation boundary

- Implement a tiny versioned fixture of `NVDA` 15-minute OHLCV, a causal clock,
  and a neutral observation query.
- Prove with boundary tests that only fully closed candles are visible and that
  the next candle open is unavailable at decision time.
- Represent missing data and instrument availability explicitly.

Deliverable: a deterministic replay slice with leakage-focused tests; no agents
or trading.

### Phase 2: paper ledger and minimal execution semantics

- Implement isolated cash, `BUY`/`SELL`/`HOLD`/`CLOSE`, order, fill, long-only
  position, and portfolio transitions.
- Start each agent with $10,000, enforce 100% maximum exposure and a $2,500
  maximum new order, and reject impossible trades.
- Execute eligible orders at the next candle open with configurable fees and
  slippage and no same-candle fill.
- Test conservation, rejection, insufficient-cash, position, fee, and time
  boundaries using fixed test inputs.

Deliverable: deterministic accounting and execution tests; no LLM or strategy.

### Phase 3: agent decision contract and audit events

- Define the neutral observation-to-decision schema and validation behavior.
- Use non-strategic test doubles that emit fixed fixtures solely to test plumbing.
- Capture exact inputs, decisions, validation failures, and state events.

Deliverable: an auditable single-agent loop without a live model. Test doubles
are infrastructure fixtures, not experimental strategies or baselines.

### Phase 4: one isolated AI agent

- Integrate one approved model behind a provider-neutral boundary.
- Freeze and hash minimal prompts and model settings.
- Verify no future input, neutral retries, cost accounting, and complete
  externally observable logs.

Deliverable: repeatable single-agent paper runs on a tiny dataset.

### Phase 5: isolated multi-agent operation without communication

- Begin with 3–5 agents, then scale to 30 agents using the same model while
  preserving separate state and a common causal snapshot.
- Test that processing order cannot leak decisions or state.
- Measure runtime, failures, API cost, and baseline convergence from shared data.

Deliverable: 30-agent no-communication runs.

### Phase 6: memory treatments

- Add explicit no-memory and private-memory policies.
- Validate causal provenance, retention, summarization behavior, and isolation.
- Compare matched runs without exposing evaluator feedback.

Deliverable: controlled memory ablations.

### Phase 7: communication treatments

- Preserve Version 1A as the no-communication control. Add Version 1B with one
  public thesis, limited reading, one discussion round, and a private final
  decision.
- Test delivery, isolation, provenance, private final decisions, and disabled-mode
  equivalence.
- Compare communication and no-communication conditions.

Deliverable: controlled communication ablations.

### Phase 8: evaluation, baselines, and replicated studies

- Implement the preregistered metrics plus buy and hold, hold cash, random valid
  actions, and one labeled simple predefined-strategy baseline.
- Run multiple periods and replicates; estimate uncertainty and convergence.
- Report economic assumptions, model/API costs, negative results, and sensitivity
  to execution and prompt choices.

Deliverable: reproducible research reports. A dashboard, if ever needed, comes
only after the research artifacts and semantics are stable.

## Recommended first implementation chunk

The first implementation chunk should be **the chronological observation
boundary**, not an agent or trading loop.

It should contain only:

- one tiny, human-inspectable `NVDA` 15-minute OHLCV fixture with explicit candle
  close and availability times;
- a logical simulation clock;
- a neutral query that returns only fully closed OHLCV candles available at a
  cutoff;
- an observation record that preserves source timestamps and provenance; and
- focused tests for equality boundaries, incomplete candles, the hidden next
  candle open, out-of-order source records, missing data, and attempts to query
  beyond the current cutoff.

It must explicitly exclude:

- LLM calls, prompts, agent implementations, hypotheses, memory, communication,
  orders, fills, portfolios, databases, web APIs, background workers, dashboards,
  technical indicators, strategy code, baseline trading logic, and generalized
  plugin or provider abstractions.

This chunk establishes the most important invariant—what could have been known
at a decision time—using a surface small enough to inspect and test completely.
The data source, candle timestamp convention, market-session policy, historical
fixture period, and adjustment policy require approval before coding.

## Major risks and mitigations

| Risk | Why it matters | Initial mitigation |
|---|---|---|
| Future-data leakage | An incomplete candle or leaked next-candle open can invalidate results. | Expose only fully closed candles; execute at the next open; centralize causal queries; add adversarial cutoff tests and leakage audits. |
| Unrealistic execution assumptions | Free liquidity or weak cost assumptions can exaggerate paper performance even with next-open fills. | Apply configurable fees and slippage; approve liquidity and partial-fill rules; run sensitivity analysis. |
| Accidental strategy injection | Prompts, fields, examples, summaries, and defaults can silently teach a method. | Minimize and version all agent-facing artifacts; prohibit strategy examples and selected indicators; conduct neutrality reviews and prompt/feature ablations. |
| Excessive human guidance | Researcher intervention can turn autonomy into disguised discretionary trading. | Freeze protocols before runs; prohibit in-run human analysis or approval; record every allowed intervention and treat changes as new conditions. |
| Uncontrolled prompts | Prompt edits, retries, or provider wrappers can change behavior without attribution. | Hash exact rendered prompts and retry text; log model metadata; use neutral schema-only retries; prevent ad hoc run-time edits. |
| Behavioral convergence | Shared models, data, prompts, or communication can collapse the population into one effective policy. | Isolate state and sampling; keep final decisions private; use controlled topology; compare homogeneous and heterogeneous treatments; measure rather than conceal convergence. |
| Irreproducible LLM outputs | Provider changes and stochastic inference can prevent exact reruns. | Store provider/model/version metadata, parameters, seeds where honored, exact inputs and outputs; pin artifacts; use replicated runs and distinguish exact from statistical reproducibility. |
| Excessive API cost | Thirty agents, memory, and communication can multiply calls and tokens rapidly. | Set preregistered call/token budgets; start with tiny datasets and few agents; record per-condition cost; cache only where scientifically valid; scale after estimates. |
| Historical overfitting | Repeated prompt and protocol tuning can adapt the experiment to familiar periods. | Separate exploratory, validation, and held-out periods; preregister confirmatory choices; limit repeated access; report all tested variants. |
| Single-asset selection bias | Choosing `NVDA` can make findings specific to one unusually successful asset. | State the selection explicitly, test multiple historical periods, avoid market-wide claims, and add assets only in a later approved version. |
| Data revisions and timestamp ambiguity | Adjusted OHLCV, candle labels, and inconsistent time zones can leak later knowledge. | Preserve source and availability timestamps, adjustment versions, market sessions, and one canonical time standard. |
| Pseudoreplication | Thirty correlated agents can create false statistical confidence. | Treat runs and independent market periods as replication units where appropriate; model shared dependencies; report uncertainty transparently. |
| Memory contamination | A summary can add facts, mix agents, or embed evaluator advice. | Enforce run/agent namespaces and provenance; version memory policies; test causal retrieval; treat summarization as an intervention. |
| Communication contamination | A broker-generated summary can become an undeclared adviser. | Deliver attributed agent-authored messages verbatim; prohibit platform consensus and rankings; bound topology, rounds, and budgets. |
| Model or provider drift | An unchanged model name may produce different behavior over time. | Record dates and provider metadata, preserve outputs, use fixed evaluation windows, and rerun calibration checks before comparing cohorts. |
| Operational failure bias | Dropped calls or selective retries can favor some agents or periods. | Predeclare retry, timeout, exclusion, and missing-decision rules; log failures; report attrition by condition. |

## Minimal repository structure for the current stage

The current documentation-only stage needs no scaffold beyond:

```text
Metis-Research/
├── AGENTS.md
├── README.md
└── docs/
    ├── decisions.md
    └── project_blueprint.md
```

No source package, test directory, dependency manifest, environment file,
database migration, API directory, or placeholder module should be added until a
specific implementation chunk requires it.

## Architectural decisions to record

`docs/decisions.md` records the approved Version 1 market/data scope, agent
population and isolation, execution and portfolio contract, memory and
communication treatments, and evaluation contract. Future entries should cover:

- the OHLCV source, adjustment policy, timestamp convention, market session, and
  historical periods;
- exact fee, slippage, liquidity, partial-fill, and action-sizing semantics;
- model identity, sampling settings, reasoning-style protocol, and compute budget;
- prompt contract and prompt-neutrality review process;
- memory retention, retrieval, and summarization policy;
- Version 1B reading limits, message schema, and delivery rules;
- the identity of the labeled simple predefined-strategy baseline and the random
  valid-action distribution;
- primary evaluation endpoint, success threshold, replication plan, and
  statistical method; and
- event-log/storage architecture and reproducibility guarantees.

Each entry should contain the date, decision, reason, alternatives considered,
and potential drawbacks. Choices still awaiting approval should remain open
questions rather than being recorded as settled decisions.

## Assumptions

- A full-scale run contains 30 agents, but early infrastructure tests may use
  3–5 without making population-level research claims.
- Agents trade against an exogenous historical replay. Their simulated orders do
  not alter the historical market path unless a future market-impact experiment
  explicitly changes that assumption.
- Initial agent adaptation occurs through context and private memory; model
  weight training is outside the early roadmap.
- Agent-visible market observations are historical `NVDA` 15-minute OHLCV only.
  Necessary timestamps and identifiers are transport metadata, not additional
  market inputs.
- Paper portfolios are isolated; agents do not pool capital or directly transfer
  assets.
- All agents use the same model. Strategy-neutral reasoning style, memory,
  communication, observations, and sampling may produce different behavior.
- Communication, when enabled, contains agent-authored content only. The platform
  transports and logs it without endorsing or summarizing it.
- Complete auditability covers all system-observable artifacts and state changes,
  not inaccessible model internals or hidden chain-of-thought.
- Exact LLM determinism may be unavailable; the research goal is then statistical
  reproducibility with complete provenance.

## Open questions

The following require approval before their related implementation or
confirmatory experiment:

1. **Primary outcome:** What single primary endpoint, threshold, and uncertainty
   criterion will combine the approved evaluation dimensions, and which measures
   are secondary?
2. **Data contract:** Which vendor, historical periods, adjustment policy,
   timestamp convention, market time zone, and regular/extended-hours policy
   apply to the `NVDA` 15-minute OHLCV?
3. **Action schema:** Do `BUY` and `SELL` use shares, dollars, or portfolio
   percentages, and does `CLOSE` always liquidate the full long position?
4. **Execution costs:** What fee and slippage values apply, and does Version 1
   model liquidity, volume limits, partial fills, latency, or market impact?
5. **Model settings and reasoning diversity:** Which single model and sampling
   settings apply, and how will reasoning-style differences remain neutral rather
   than encode trading strategies?
6. **Prompt contract:** Must agents emit a structured hypothesis and confidence,
   or only a decision and concise rationale? Who approves prompt neutrality?
7. **Memory policy:** How long are approved memory items retained, how are they
   retrieved, and is model-generated summarization permitted?
8. **Version 1B communication:** How many public theses may each agent read, how
   are they selected, what are the message limits, and are authors identified?
9. **Baselines:** What distribution generates random valid actions, which simple
   predefined strategy is the labeled baseline, and how are sizing and costs
   matched?
10. **Replication and statistics:** How many runs, model samples, periods, and
    held-out regimes are required, and what analysis handles correlations among
    agents sharing data and models?
11. **Failure semantics:** What happens on missing candles, rejected actions,
    model timeouts, unavailable outputs, or portfolio insolvency?
12. **Artifact retention and privacy:** Which exact prompts, model outputs, and
    datasets can be retained or published under provider terms, licensing, cost,
    and research transparency requirements?

Until these are approved, the architecture should preserve options without
creating generalized abstractions or placeholder implementations.
