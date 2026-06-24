---
title: "Scaling Autonomous Work"
title_alt: "扩展自主工作"
excerpt: "Long-running agents and parallel collaboration are already appearing. The open question is how to organize them into a scalable general-purpose production system without exhausting human attention."
date: 2026-06-17
lang: en
permalink: /blog/scaling-autonomous-research/
alt_url: /zh/blog/scaling-autonomous-research/
comments: false
share: false
read_time: true
author_profile: false
---

{% include lang-switch.html en_url='/blog/scaling-autonomous-research/' zh_url='/zh/blog/scaling-autonomous-research/' %}

This is not a forecast about a distant future. Long-running agents, small parallel teams, persistent project state, and human approval mechanisms are already appearing. The unresolved question is how to organize these pieces into a general-purpose production system that can grow in scale and duration without consuming human attention at the same rate.

> **Core thesis: the object we need to scale is not the number of agents. It is the system's ability to complete, verify, and integrate reliable work loops while consuming very little human bandwidth.**

## Why this matters

Coding agents are among the most important and widely transferable forms of agents now emerging. A growing share of knowledge work and engineering ultimately runs through software: understanding requirements, reading and writing code, operating data and tools, building evaluations, automating workflows, and iterating from feedback. An agent that can do this reliably does more than increase software output; it can participate in research, product development, operations, and complex engineering, while also helping build other agents, tools, and automation systems.

Even when the final objective is physical, coding agents can expand the reach of automation by developing better perception, planning, control, simulation, and robotics algorithms. Biology and medicine are a concrete example: the underlying problems are physical, but much of the work still runs through computational models and software pipelines. Accelerating the development of systems like AlphaFold by ten- or a hundred-fold could materially advance structural biology, drug discovery, and related fields. They do not remove the need for hardware, laboratories, or contact with reality, but they can already accelerate parts of the design, validation, and iteration cycle. Research agents are a more open-ended and uncertain instance of the same general capability, not the boundary of the idea.

The question is not whether an agent will someday write more code. It is whether capabilities that already exist in partial form can be organized into high-bandwidth, verifiable, and sustained work. If that organizational layer scales reliably, order-of-magnitude gains in useful output per unit time become a possibility worth taking seriously, with effects that need not remain confined to software, AI, or research.

## Why this is worth discussing now

Humans remain limited in number, attention, and working hours. At the same time, effective agent capacity is improving through four interacting forces: stronger foundation models, better agent harnesses and tools, more inference and training compute, and better execution infrastructure. Stronger models can handle harder work; better harnesses make the same model more reliable and persistent; better infrastructure allows more tasks to run in parallel and return results faster.

A positive feedback loop is also beginning to form. Much of agent development is itself coding, evaluation design, failure analysis, and systems optimization. Coding agents can therefore participate directly in improving model training, tools, harnesses, infrastructure, and the next generation of agents. As that loop accelerates, the bottleneck begins to shift from "Is one agent capable enough?" toward "Do we know how to organize a growing supply of capable agents?"

These elements are already real. Fable 5 and other recent systems have substantially extended single-agent runs, with days-long unattended work entering the practical range. Small parallel branches, persistent state, automated verification, and human approval gates are also appearing. The open question is how to combine them in one system that scales in agent count and duration without making duplication, verification, delivery and integration, human review, or state pollution the new bottleneck.

## What should actually scale?

Agent count and total token throughput should not be treated as the final objective. One hundred agents producing code, plans, or reports at the same time do not automatically provide one hundred times the productivity. A more useful unit is a complete work loop that is verified and reusable by future work:

**Understand the objective → Decompose and plan → Execute → Verify → Deliver and integrate**

One useful abstraction is cumulative useful output Q(N, T, H, B), where N is the number of concurrent agents, T is elapsed time, H is human attention, and B is the budget for model calls, compute, tools, and execution. The question is how to make Q continue to grow with N and T while H grows as slowly as possible. The object of scale is not the conversation window, but the number of reliable work loops completed, verified, and absorbed per unit time.

| **Dimension** | **Central question** | **Curve to improve** |
| --- | --- | --- |
| Human boundary | Keep people only where their judgment or authority is genuinely non-delegable | Human effort grows far more slowly than agent count and elapsed time |
| Horizontal scaling | Let more agents advance complementary work at the same time | Speedup remains positive well beyond three or four agents |
| Temporal scaling | Preserve useful output during long, open-ended runs | Productivity decays more slowly across days and longer runs |

## The first question: where is the human–agent boundary?

To keep raising the ceiling of agent systems, humans need to leave more of the common execution path. If every task, merge, delivery, or conclusion requires human confirmation, human throughput remains a hard constraint. A more scalable arrangement is for people to occupy a low-bandwidth control plane for goals and governance, while handling only a small number of genuine exceptions.

There are two different reasons an activity may still require a human. The first is sovereign rather than capability-based: setting work objectives, resolving value conflicts, defining risk budgets, authorizing irreversible real-world actions, changing the definition of success, and deciding whether to deploy, publish, or make consequential commitments. Even when an agent is technically capable, people may still need to retain final responsibility. The second reason is a current capability gap: root-cause diagnosis, designing a decisive validation, deciding whether an anomaly matters, or resolving conflicting evidence. That part of the boundary should be reassessed as agent capabilities change.

The division of labor may therefore be poorly served by a fixed rule such as "humans provide the objective, agents execute, and humans inspect every result." A more useful hypothesis is state-dependent escalation: involve people when the situation is clearly out of distribution, agent disagreement is high, the action is costly or irreversible, or a critical result cannot be verified; otherwise, let the agent system close the loop autonomously. Which states truly require a human should be an empirical question rather than a predefined answer.

> **Design target: when agent count grows tenfold and runtime grows tenfold, human effort should ideally remain nearly unchanged. Humans should be the control plane, not the data plane.**

## The second question: how does productivity scale with agent count?

In many current settings, multi-agent systems show sharply diminishing returns after only a small number of agents. The problem is often not a shortage of nominal workers, but a shortage of independent and valuable work channels. Agents using the same model, context, and objective tend to converge on similar approaches, while task execution, verification, and artifact integration fail to keep pace with proposal generation.

One organizational form worth exploring is a dynamic agent pool rather than a fixed team. What persists is the work state: objectives and task graphs, hypotheses and constraints, evidence, code and artifact lineage, unresolved contradictions, and resource status. Agents can be temporary compute units. They are spawned with a bounded task and the minimum necessary context, work in isolated environments, return structured artifacts, and leave when the assignment is complete.

**Discover parallel work → Spawn agents → Execute independently → Verify and replicate → Integrate and reschedule**

If the pool scales only the agents that propose approaches, it will usually encounter another bottleneck quickly. A production system has at least four stages: planning or proposal, execution, verification, and integration, and throughput is constrained by the slowest stage. If one hundred agents propose five hundred changes per hour while verification can process only ten, the extra scale mainly creates backlog, conflict, and noise. A more capable scheduler would allocate explorers, executors, verifiers, and integrators according to live queues and bottlenecks, while pausing, migrating, or terminating low-value branches.

1. **Discover parallelism.** Complex work does not arrive as one hundred independent tickets. The system must turn an ambiguous objective into a dynamic task DAG and continuously expand the useful parallel frontier.
2. **Create non-redundancy.** Nominal N matters less than the effective number of agents that contribute independent approaches, distinct evidence, and complementary capabilities.
3. **Coordinate sparsely.** One hundred agents cannot read one another's complete logs. Collaboration should happen through task dependencies, typed artifacts, and evidence—not an all-to-all group chat.
4. **Scale verification and integration.** Generating candidates is only half the loop. Testing, replication, contradiction resolution, branch merging, delivery, and downstream updates need comparable bandwidth.
5. **Learn the organization itself.** The system should remember which role mixtures, agent types, and collaboration patterns produced real progress, so that scheduling improves over time.

The relevant curve is useful work per unit time as N grows. "One hundred agents in one day accomplishing roughly forty days of effective single-agent work" is an illustrative target, not a description of current capability. Evaluation must also separate genuine collaboration from best-of-N independent attempts and from simply spending more total compute. Organization creates 1+1>1 value only when coordinated work consistently outperforms independent work under a comparable budget.

## The third question: how does productivity hold up over time?

Fable 5 and other recent systems have substantially extended how long a single agent can keep working. On relatively well-specified tasks, days-long unattended execution is becoming practical. This section therefore does not need to focus on how to keep an agent running or on the underlying technical mechanisms.

The more important question is whether longer work remains valuable. An agent that runs for several days may keep making progress, but it may also repeat attempts or slowly exhaust what remains in one direction. Coding and research are demanding examples, but the same issue applies to product development, operations, and complex engineering: the relevant question is not how long the agent stays active, but how much new and useful output it can still produce over a longer period.

The curve to improve is useful output per unit time as a run grows longer. The goal is not to keep an agent active forever, but to preserve high productivity over longer periods and to make sensible choices about whether to continue, change direction, or stop, with as little continuous human supervision as possible. Fable 5 suggests that this curve is already moving outward; the next question is whether that progress can extend from a single long task to open-ended, continuous complex work.

## Putting the three questions together: a scalable autonomous work system

The three questions are coupled. More agents create coordination and integration debt faster, which can shorten the productive horizon. Longer operation produces more exceptions and contradictions, which can pull humans back into the critical path. The true object is therefore not one isolated curve, but a scalable autonomy region defined jointly by agent count N, duration T, and human attention H.

> **Human goals, values, and risk boundary → Persistent project state and dynamic scheduler → Ephemeral Planners, Explorers, Executors, Verifiers, and Integrators → Verified software, knowledge, decisions, and the next task frontier**

This framework points toward an organizational form that is already beginning to emerge: humans own a low-bandwidth constitution and exception path; the persistent object is the project and work state; agents are elastic compute units that can be created and destroyed; collaboration happens primarily through artifacts and evidence; the workforce mix changes with the current bottleneck; and a result counts as useful output only after it has been verified, delivered, and integrated into downstream work.

Sustained output may also require scaling a portfolio of tasks and projects rather than placing one hundred agents on one problem indefinitely. Low-hanging fruit within a direction will be exhausted. The organization needs to reallocate capacity across projects: shrink saturated directions, reinforce anomalous or newly promising ones, pause work that is waiting on external results, and rapidly form temporary teams around newly discovered needs.

## What this framework points toward

This transition has already begun. The rate of work is increasingly shaped not only by what the strongest model can do, but by what the production system around it can organize: whether scarce human judgment appears only at high-value points, whether large populations of agents can pursue complementary work in parallel, and whether project state can preserve the consistency of goals, evidence, artifacts, and versions over long periods.

If the horizontal curve can move from rapid saturation after a few agents toward meaningful marginal gains across dozens or hundreds; if the temporal curve can extend from a few productive hours to days or longer; and if human attention does not grow at the same rate, knowledge work and complex engineering may achieve order-of-magnitude gains in throughput. Coding agents are the most important underlying lever: software connects research, products, business systems, and robotics development, so this organizational capability can spread from digital work into manufacturing, energy, biology, materials, and other parts of the physical economy.

> **An emerging unit of general-purpose productivity is the autonomous work system: persistent project state, elastic agents, limited human governance, and continuously verified work loops.**

