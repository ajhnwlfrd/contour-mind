# Contour Mind: A Thinking Layer for Mental-Model-Driven Spec Generation

## Abstract

Contour Mind is a research project for building a Claude skill that helps software developers move from vague ideas to clear, actionable specifications. The central idea is that developers should not need to know the names of thinking frameworks or mental models in order to benefit from them. Instead, the skill should quietly use those frameworks in the background to ask better questions, reduce cognitive load, clarify intent, expose assumptions, and produce a buildable software specification. This paper focuses only on the thinking layer of Contour Mind. The broader system may eventually include orchestration and implementation layers, but those are treated as future work.

## Purpose

Software development often starts with unclear intent. A developer, product owner, founder, or business user may say, “I want to build this feature,” but the request is often incomplete. The real problem may not be clear, the user may already be jumping to a solution, assumptions may be hidden, edge cases may not have been considered, constraints may be missing, and the implementation prompt given to a coding agent may be too broad or too vague.

Contour Mind exists to solve this early-stage problem. Its purpose is to help users turn rough software ideas into structured, testable, implementation-ready specifications through a guided discovery process. The goal is not to teach users mental models. The goal is to use mental models internally so that the conversation becomes sharper, simpler, and more useful.

## Scope of This Paper

Contour Mind is envisioned as a three-layer system: thinking layer, orchestration layer, and implementation layer. The thinking layer performs discovery, problem decomposition, mental-model-guided questioning, and structured spec generation. The orchestration layer is future work. It may hold the generated spec as a living document, monitor implementation output, detect drift, and refine or re-prompt when needed. The implementation layer is also future work. This is where Claude Code or a similar coding agent would execute discrete, well-scoped development tasks based on the generated spec.

This paper focuses only on the thinking layer. The orchestration and implementation layers are referenced only to show how the thinking layer may eventually fit into a larger end-to-end software development workflow.

## Core Idea

The core idea behind Contour Mind is simple: a good specification is not created by asking more questions. It is created by asking the right question at the right time. Most AI-assisted development tools focus on code generation. They help users build once the user already knows what to build. Contour Mind focuses on the step before coding.

It helps the user clarify what problem is actually being solved, who is affected by the problem, what outcome is expected, what assumptions are being made, what constraints matter, what edge cases could break the solution, what should be built first, what should not be built yet, what would make the implementation successful, and what would make it fail. The thinking layer acts like a senior product thinker, architect, and analyst sitting beside the user before implementation begins.

## Design Principle

The main design principle is: use mental models internally, but keep the user experience conversational. The user should not be forced to choose from frameworks such as first principles, inversion, issue trees, abstraction laddering, or second-order thinking. Instead, the skill should infer which thinking frame is useful from the shape of the conversation.

For example, if the idea is vague, the skill should clarify the level of abstraction. If the problem is large, the skill should decompose it. If assumptions are weak, the skill should test them. If failure matters, the skill should explore what could go wrong. If there are too many possible next steps, the skill should help prioritize. The user experiences this as a natural conversation. The mental model remains mostly invisible.

## The Thinking Layer

The thinking layer is the main subject of this paper. Its responsibility is to guide the user from an unclear starting point to a structured specification. It performs five major functions: understand the initial idea, select the most useful thinking frame, ask targeted discovery questions, build the specification incrementally, and verify that the specification is complete enough for implementation.

The thinking layer should not behave like a static form or questionnaire. It should behave like an adaptive conversation. Each answer from the user may change the direction of the discovery process.

## Router

The router is the core decision mechanism inside the thinking layer. Its job is to decide which mental model or thinking frame should be used next. The router does not apply every model at once. That would increase cognitive load and make the conversation feel mechanical. Instead, the router should choose the smallest useful thinking frame for the current moment.

### Why One Adaptive Router

A single adaptive router keeps the experience coherent. Without a router, the skill may behave like a collection of disconnected frameworks. The user may feel that the conversation is jumping between tools without a clear reason. With one adaptive router, the skill behaves like one intelligent conversation.

The router classifies the current state of the problem, chooses the next useful frame, asks targeted questions, and changes direction when the user’s answer reveals a better path. This matters because discovery work is rarely linear. A user may begin with a feature idea, but the real issue may be a workflow problem. A technical constraint may reveal a business rule. A user story may expose a missing edge case. A failure mode may reveal a hidden dependency. The router allows the thinking layer to move naturally between framing, decomposition, diagnosis, risk analysis, and execution planning.

### Core Decomposition Models

The first version of the thinking layer should focus on a small set of high-value models. These models are useful because they cover the most common problems found in early software specification work.

#### Abstraction Laddering

Abstraction laddering is used when the problem is vague, solution-shaped, or mixed with implementation details. It helps the skill move up or down the level of thinking. For example, a user may say, “I want to build a dashboard.” The skill should not immediately create dashboard requirements. It should first ask, “What decision should this dashboard help someone make?” This moves the conversation up from solution to purpose. The skill may then move back down into concrete details: “What information does the user need in order to make that decision?” Abstraction laddering helps prevent the system from solving the wrong problem well.

#### Issue Trees

Issue trees are used when the problem is large and can be broken into independent branches. For example, a bulk invoice upload feature may be decomposed into CSV upload, file validation, duplicate detection, PDF matching, user correction flow, submission workflow, error handling, audit trail, permissions, and reporting. This helps the skill reason about each part separately. Issue trees are useful because many vague software ideas are actually bundles of smaller problems. The skill should help users see the structure of the work before implementation begins.

#### First Principles

First principles thinking is used when the conversation contains weak assumptions or inherited beliefs. A user may say, “We need to send an email notification every time this happens.” The skill should ask, “What user need does the notification serve?” and then, “Is email the only way to meet that need?” This separates facts from assumptions. First principles thinking helps the skill avoid copying existing workflows blindly. It is especially useful when users are rebuilding old processes, automating manual work, or migrating legacy systems.

#### Inversion

Inversion is used when failure modes matter. Instead of only asking what success looks like, the skill asks, “What would make this feature fail?” For software specifications, this is extremely useful. Inversion helps reveal missing validation rules, security risks, poor user experience, ambiguous business logic, operational failure points, data quality issues, edge cases, and rollback needs. For example, “What would cause a user to upload the wrong file and not realise it?” That question may reveal the need for file previews, validation messages, confirmation steps, or audit logs. Inversion turns vague risk into concrete requirements.

#### Second-Order Thinking

Second-order thinking is used when a decision may create downstream effects. A feature may solve an immediate problem but create future complexity. The skill should ask, “What happens after this is used for a few months?” or “Who else will be affected once this workflow changes?” This model helps reveal maintenance cost, support burden, data migration issues, future scaling problems, reporting needs, training requirements, operational dependencies, and workflow side effects. Second-order thinking helps the specification account for consequences beyond the first release.

### Diagnostic Extensions

Some problems are not merely vague. They are obscured by symptoms, unclear reasoning, or complex relationships. For these situations, the thinking layer may use diagnostic extensions. These models are not always needed. They should be used only when the conversation indicates a specific need.

#### Iceberg Model

The iceberg model is used when the user describes visible symptoms but not underlying causes. For example, “The team keeps making mistakes when processing invoices.” The visible event is the mistake. But the deeper issue may be poor training, unclear workflow, bad user interface, missing validation, confusing business rules, lack of ownership, or no feedback loop. The skill should guide the user from event to pattern to structure. This helps the specification address the real cause rather than only the visible symptom.

#### Ishikawa / Cause-and-Effect Thinking

Ishikawa thinking is useful when recurring issues need to be grouped into cause categories. For software and workflow problems, possible categories may include people, process, system, data, policy, communication, environment, and integration. This helps the skill organize possible causes before jumping to solutions. It is especially useful for operational problems, support issues, and repeated process failures.

#### Ladder of Inference

The ladder of inference is used when the user appears to be drawing conclusions too quickly. For example, “Users do not like this screen.” The skill may ask, “What have users actually said or done?” This separates observation from interpretation. The ladder of inference helps clarify what was observed, what was selected from the observation, what meaning was assigned, what assumption was made, what conclusion was reached, and what action is being proposed. This reduces the risk of building based on unsupported conclusions.

#### Concept Mapping

Concept mapping is used when the system contains many entities and relationships. For example, an invoicing system may include customers, invoice entities, payers, billers, orders, delivery preferences, attachments, approval states, users, and permissions. The skill should help map these relationships before creating requirements. This is useful when the problem is not just a list of tasks, but a system of connected concepts.

### Execution Choice Extensions

Once the problem is understood, the thinking layer may need to help the user decide what to do next. These models support execution planning, but they still belong to the thinking layer because they shape the specification before implementation begins.

#### Decision Matrix

A decision matrix is used when several viable options exist. For example, “Should we build this as a new page, extend an existing page, or create a background process?” The skill can compare options against criteria such as user value, implementation complexity, risk, maintenance cost, time to deliver, scalability, security, and fit with existing architecture. The goal is not to make the decision automatically. The goal is to make the trade-offs visible.

#### Prioritization

Prioritization is used when too many tasks or branches compete for attention. The skill should help identify what must be built first, what can wait, what is risky, what unlocks other work, what is valuable but non-essential, and what should be excluded from the MVP. Prioritization helps keep the final spec buildable. Without prioritization, the output may become too broad for Claude Code or any implementation agent to execute effectively.

## Default Routing Sequence

The router should normally move from ambiguity to action. A default routing sequence may be: if the problem is vague or misframed, use abstraction laddering; if the problem is large and splittable, use issue trees; if assumptions are weak, use first principles; if failure modes matter, use inversion; if long-term effects matter, use second-order thinking; if symptoms obscure causes, use iceberg or Ishikawa; if reasoning leaps are happening, use ladder of inference; if system relationships are unclear, use concept mapping; if options must be compared, use a decision matrix; if too many tasks compete, use prioritization.

This order is intentional. It first clarifies the frame, then breaks down the structure, then tests assumptions, then explores risks, then considers consequences, and finally supports execution choices. The goal is to reduce the chance of producing a polished but incorrect specification.

## Conversational Style

The interaction should feel collaborative, not mechanical. The skill should ask one or two focused questions at a time, avoid overwhelming the user, build on the user’s own words, summarize progress regularly, make assumptions visible, confirm important decisions, avoid unnecessary jargon, mention mental model names only when helpful, keep the user moving toward a concrete output, and treat the spec as something being shaped together. The skill should not sound like a form. It should sound like a thoughtful collaborator.

## Output

The thinking layer produces two primary artifacts: a structured markdown specification and a Claude Code-ready implementation prompt. These two outputs serve different purposes. The specification captures intent. The Claude Code prompt transfers that intent into execution.

## Structured Markdown Specification

The structured specification should include title, problem statement, background, goals, non-goals, users and stakeholders, current workflow, proposed workflow, functional requirements, non-functional requirements, business rules, data requirements, user stories, edge cases, failure modes, constraints, open questions, MVP scope, success criteria, and implementation notes. The spec should be readable by both humans and AI coding tools. It should be specific enough to guide implementation but not so detailed that it prematurely dictates every technical decision.

## Claude Code Prompt Output

The Claude Code prompt should be derived from the completed specification. It should be narrow, actionable, and tied directly to the documented goals and constraints. The prompt should include the specific task to implement, relevant context from the spec, files or areas likely to be affected if known, functional requirements, constraints, edge cases, expected output, testing expectations, what not to change, and how to report uncertainty. The prompt should avoid broad instructions such as “Build the whole feature.” Instead, it should produce scoped implementation tasks such as “Implement CSV validation for the bulk invoice upload flow according to the following rules.” This keeps implementation work discrete, reviewable, and safer.

## Discovery Stage

The discovery stage is where the thinking layer gathers and clarifies information. It should explore problem statement, user or stakeholder, desired outcome, current workflow, pain points, constraints, assumptions, business rules, data requirements, user stories, edge cases, failure modes, non-goals, MVP boundary, and success criteria. The discovery stage should be progressive. The skill should not ask all questions at once. Instead, each question should be selected based on what is currently unclear.

## Verification Stage

The verification stage checks whether the specification is ready for implementation. It should test the spec for completeness, consistency, buildability, ambiguity, missing edge cases, conflicting requirements, hidden assumptions, unclear ownership, weak success criteria, and oversized scope. The purpose of verification is to prevent a polished but ambiguous document from reaching implementation. This is especially important when the spec will be passed to Claude Code or another coding agent. A coding agent can only execute well when the task is clear, bounded, and testable.

## Research Sources

The thinking layer draws inspiration from practical mental model libraries and decision-making frameworks, including Untools, *Super Thinking* by Gabriel Weinberg and Lauren McCann, and *The Great Mental Models* associated with Shane Parrish and fs.blog. These sources are useful because they connect practical decision-making with repeatable thinking patterns. Contour Mind adapts these patterns into a software specification workflow. The purpose is not to reproduce the frameworks academically. The purpose is to operationalize them in a conversational system that helps developers think more clearly before building.

## Research Questions

This project explores three main research questions: Which mental models are most useful for software and product problem decomposition? How can those models be translated into effective discovery questions? What combination of models produces the most complete and buildable specification? These questions shape the design of the router, the conversation flow, and the final spec structure. The success of the thinking layer depends on whether it can choose useful questions without overwhelming the user.

## MVP Scope

The first version of Contour Mind should focus only on the thinking layer. The MVP should support vague idea intake, adaptive questioning, mental-model-guided routing, problem decomposition, assumption discovery, edge case discovery, MVP boundary definition, structured markdown spec generation, Claude Code prompt generation, and spec verification checklist. The MVP should not attempt to support live implementation monitoring, automatic drift detection, code review, multi-agent orchestration, full OODA loop automation, or automatic rewriting of the spec during implementation. Those capabilities belong to future orchestration and implementation layers.

## Future Work

Future versions of Contour Mind may extend beyond the thinking layer. The orchestration layer may hold the spec as a living document and compare implementation output against the original intent. It may follow an OODA-style loop: observe the implementation output, orient against the specification, decide whether drift or ambiguity exists, and act by refining the spec or generating a better prompt. The implementation layer may use Claude Code or a similar coding agent to execute discrete development tasks. Together, these future layers could create a self-correcting workflow where the system does not merely generate a spec once, but continues to protect the intent of the spec during implementation. However, this paper does not define those layers in detail. They remain future work.

## Conclusion

Contour Mind is a thinking layer for turning unclear software ideas into clear, structured, implementation-ready specifications. Its central belief is that better software begins before coding. The quality of implementation depends on the quality of the specification. The quality of the specification depends on the quality of the thinking that produced it.

By using mental models internally, Contour Mind helps users clarify problems, expose assumptions, discover edge cases, define scope, and produce better prompts for coding agents. The user does not need to know the framework names. The user only needs to experience a better conversation. That is the promise of Contour Mind: from messy idea to buildable spec.

## Example
User: I want to build a dashboard.

Contour Mind does not immediately create dashboard requirements.
It first asks: What decision should this dashboard help someone make?

If the answer reveals that the dashboard is really about operational risk, the router may move from abstraction laddering to issue-tree decomposition or inversion.