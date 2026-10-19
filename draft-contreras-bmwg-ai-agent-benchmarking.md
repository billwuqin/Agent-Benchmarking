---
title: "Benchmarking Methodology for AI Agents in Network Operations"
abbrev: "AI Agents Benchmarking"
category: info

docname: draft-contreras-bmwg-ai-agent-benchmarking-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
area: "Operations and Management"
workgroup: "Benchmarking Methodology"
keyword:
 - network operation
 - benchmarking
 - ai agent
author:
 -
    fullname: Luis M. Contreras
    organization: Telefonica
    email: luismiguel.contrerasmurillo@telefonica.com

normative:

informative:


--- abstract

This document defines a benchmarking methodology for evaluating
Artificial Intelligence (AI) agents performing network operations
tasks such as configuration, troubleshooting, and optimization.  This
document focuses on task-oriented performance metrics, including task
completion success, execution efficiency, and robustness across
multi-step workflows, proposing benchmarking practices towards agent-
based, closed-loop network operation scenarios.

The proposed methodology aims to provide a reproducible and vendor-
independent framework to compare AI agent effectiveness in controlled
network environments.

--- middle

# Introduction

Network operations are increasingly adopting AI-driven automation,
including the use of agentic architectures combining Large Language
Models (LLMs), planning modules, and tool interfaces.

This document introduces a task-oriented benchmarking methodology for
AI agents, enabling reproducible evaluation of their capability to
perform network operations.  The methodology defined in this document
is intended to be applicable to emerging AI-native network operation
paradigms, including autonomous networks, closed-loop control
systems, digital twin-based network management, or intent-based
networking with AI agents.

# Conventions and Definitions

{::boilerplate bcp14-tagged}

*  Agent: AI-based system capable of autonomously executing tasks
   through reasoning, planning, and tool interaction.

*  Task: a well-defined network operation objective with measurable
   success criteria.

*  Episode: a sequence of actions taken by the agent to complete a
   task.

*  Tool Interaction: invocation, by the agent, of external systems
   (e.g., CLI, APIs) so to perform actions or obtain information.

*  Success: completion of a task meeting predefined acceptance
   criteria.

*  Failure: inability to complete a task or incorrect execution
   leading to invalid network state.

# Scope

This document defines methodologies to benchmark AI agents performing
network control and management operations, such as:

*  Network configuration tasks (e.g., routing, policy deployment)

*  Troubleshooting and root cause analysis

*  Network optimization and adaptation actions

The final objective is to propose a benchmarking methodology able to
provide a reproducible and vendor-independent framework to compare AI
agent effectiveness in controlled network environments when
performing representative control and management actions in the
network.

It is expected the progressive introduction of different AI agents
performing network operations tasks through multi-step interaction
with network systems and management interfaces.  Then it is needed to
define a methodology capable to assess the proper performance and
functional behavior of those agents when deployed in real networks.
For instance, avoiding poor performance, inaccurate execution, bias
on the decisions taken, etc.

The methodology is intended to enable:

*  Reproducible comparison across AI agents operating on equivalent
   network tasks

*  Evaluation of task effectiveness, execution efficiency,
   operational robustness, and safety-related behavior

*  Characterization of agent behavior across deterministic and
   perturbed operating conditions

# System Under Test (SUT)

The System Under Test (SUT) consists of different parts.  It is
assumed the following parts being in place:

*  AI agent under test, which could require of additional components
   (e.g., other agents, LLM model, memory functions, etc) to
   accomplish the tasks under evaluation.

*  Tool interfaces (e.g., NETCONF, REST APIs, CLI agents)

*  Reference network to be used as common environment for the
   benchmarking execution.  This reference network can consist of
   control plane modules (e.g., SDN controller) and network elements,
   building a setup sufficiently rich so to allow non-evidemt tasks
   to be formulated.

(Note: the reference network could probably need to be defined
according to the reality of the real network in which the agents are
intended to be used, in terms of test topology, services offered by
the network, vendors deployed on it, etc.  Probably the definition of
such reference network should be out of scope of the document).

The SUT boundary is defined as the set of input (prompts, telemetry
ingestion and processing, etc) and output (commands, configurations,
actions) interfaces.  Thus, external systems (such as SDN
controllers, inventory, network topology, telemetry systems, etc) are
isolated from the SUT and treated as part of the test environment.

The following aspects of the SUT (e.g., the AI agent implementation)
have to be documented:

*  Model name and version

*  Inference configuration parameters

*  Prompting or instruction templates used during benchmark execution

*  Memory mechanisms, if any

*  List of enabled tools and associated permissions

*  Safety guardrails or approval mechanisms

*  Any persistent state retained across benchmark runs

Furthermore, if the agent uses stochastic model settings, such as
temperature or sampling parameters, these settings should be also
reported.

#  Benchmarking Framework

The framework for the proposed bechmarking methodology considers the
aspects described in the following sections.

In order to prevent drifts or excesive randomness in the decisions,
the benchmark has to be executed multiple times in order to account
for non-deterministic behavior of AI systems.

##  Task Definition

The tasks to be accomplished in the benchmarking test should be
defined to be:

*  Deterministic and reproducible

*  Clearly specified with expected outputs

*  Independent or part of multi-step workflows

##  Execution Model

Each task execution has to be modeled as an episode including:

*  Input context

*  Sequence of actions

*  Final outcome

##  Environment

The running of the benchmarking has to be performed in a controlled
environment, but mimicking a realistic production scenario.  It can
be built by means of emulated networks, digital twins or controlled
testbeds.  Ideally any of these environments should be representative
enough of the network where the benchmarked agents are expected to be
deployed, for inscaten in terms of topology, vendors deployed
(including hardware and software versions), protocols in place,
   configured services, etc.

#  Benchmarking Metrics

Next sub-sections propose a number of metrics to be derived from the
benchmarkign process.  These metrics are planned to be used for
comparison among the distinct AI agents being tested.

##  Performance Metrics

*  Task Completion Time: it is the time elapsed between the instant
   at which the complete task input is made available to the SUT and
   the instant at which the benchmark harness determines that the run
   has terminated, whether in success, failure, or timeout.

*  Time to First Action: it is the time elapsed between task
   presentation and the first externally observable action taken by
   the SUT, including a tool invocation, configuration proposal, or
   configuration change.

*  Number of iterative steps: it is the total number of sequential
   reasoning, planning, or execution steps performed by the SUT
   during a task episode before reaching a terminal state.  A step
   may include an internal decision point, a tool invocation, or an
   externally observable action as defined by the benchmark harness.

##  Effectiveness Metrics

*  Task Success Rate: it is the ratio of benchmark executions that
   successfully achieve the predefined task acceptance criteria to
   the total number of task executions.

(Note.  Success criteria have to be defined prior to benchmark
execution and evaluated consistently across SUTs).

*  Pass/Fail ratio: it is the ratio between the number of successful
   task executions and the number of failed task executions during
   the benchmark. Failed executions include incorrect outcomes,
   policy violations, unrecoverable errors, or timeout conditions.

*  Partial completion rate: it is the proportion of benchmark
   executions that achieve a subset of the predefined task objectives
   without satisfying the complete success criteria.

##  Efficiency Metrics

*  Tokens consumed (input/output): it is the total number of input
   and output tokens processed by the SUT during a task execution,
   including tokens exchanged with language models and auxiliary
   reasoning components, when applicable.

*  Number of tool invocations: it is the total number of calls made
   by the SUT to external tools, systems, APIs, controllers, or
   management interfaces during the execution of a benchmark task,
   independently of whether the invocation succeeds or fails.

*  Resource utilization: it is the amount of computational resources
   consumed by the SUT during task execution, including processor
   usage, memory consumption, storage usage, network traffic, or
   other implementation-specific resources.

(Note.  Measurement methods and observation intervals have to be
defined for it.  Usage of network resources could be also of
relevance).

##  Robustness Metrics

*  Success under perturbed inputs: it is the ratio of successful task
   executions when the benchmark inputs contain controlled
   perturbations, such as incomplete information, noisy telemetry,
   conflicting instructions, delayed data, or other predefined
   deviations from nominal conditions.

(Note.  To de defined what would be the method of determining the
number and type of perturbations).

*  Retry success rate: it is the proportion of initially unsuccessful
   task executions that achieve successful completion after one or
   more retries permitted by the benchmark procedure.

(Note.  To be defined the number of retries).

*  Stability across multiple runs: it is the degree of consistency of
   the benchmark results obtained from repeated executions of the
   same task under equivalent conditions.

(Note.  The metric could be reported using statistical indicators
such as variance, standard deviation, or confidence intervals of
selected benchmark outcomes.  For that purpose a minimum number of
tests should be considered for having statistical value).

##  Coverage Metrics

*  Percentage of tasks successfully handled: it is the proportion of
   benchmark task types for which the SUT achieves the predefined
   success criteria at least once during the benchmark campaign.

*  Generalization across scenarios: it is the ability of the SUT to
   maintain acceptable performance when executing functionally
   equivalent tasks across different network topologies, vendor
   environments, protocol configurations, operational conditions, or
   service contexts defined by the benchmark.

(Note.  The metric of generalization across scenarios is generic in
nature.  When the benchmarking is used by a network provider this
metric could be not considered since the scenario of future usage in
production network is specific to that network provider).

#  Reporting Format

The results of the benchmarking have to include sufficient
information as detailed in the benchmarking methodology described in
this document plus some additional information:

*  SUT description

*  Benchmarking framework, including task definition, execution
   model, execution logs and the environment on top of which the
   benchmark process runs

*  Benchmarking metrics and their values

*  Time at which the benchmarking is run and some other conditions
   that could be of interest

(Note.  To be defined the structure of the report, since could be of
interest to generate the reporting using structured formats such as
YANG).

# Security Considerations

TODO Security


# IANA Considerations

This document has no IANA actions.

# Acknowledgments
{:numbered="false"}

TODO acknowledge.

--- back

# Campus Network Operation Agent Test Requirements

## Network Topology Test Requirements

The campus network operation agent test needs to support a typical test network topology
as shown in {{campus}}, including the network device layer, operation and maintenance
management system, AI agent module, and AI agent test system.

~~~~
                      +-------------------------+
                      | Campus Network Operation|
                      |        Agent            |
                      +----------+--------------+
                                 |
+--------------------------------+--------------------------------+
|                            ----+---                             |
|                        ////        \\\\                         |
|                       |    Internet/   |                        |
|                       |      WAN       |                        |
|                        \\\\        ////                         |
|                            ----+---                             |
|                                |GE0/0/1                         |
|                         +------+------+                         |
| Aggregation             | o o o o  oo |S3                       |
|                  +-----++-------------++-------+                |
|                  |      | o o o o  oo |        |                |
|                  |      +-------------+        |                |
|                  |                             |                |
|      +-----------+---------+       +-----------+---------+      |
|      |         GE0/0/1     |       |        GE0/0/1      |      |
|Access|     +-----+-----+   |       |     +-----+-----+   |      |
|      |     |O O O    o |S1 |       |     |O O O    o |S2 |      |
|      |     +-----+-----+   |       |     +-----+-----+   |      |
|      |  GE0/0/2  |  GE0/0/3|       |   GE0/0/2 | GE0/0/3 |      |
|      |   +-------------+   |       |   +-------------+   |      |
|      |+--+--+       +--+--+|       |+--+--+       +--+--+|      |
|      ||AP1  |       |AP2  ||       || AP3 |       |AP4  ||      |
|      |+-----+       +-----+|       |+-----+       +-----+|      |
|      +---------------------+       +---------------------+      |
|                                                                 |
|                                                                 |
|       +------+  +------+           +------+  +------+           |
|Device |Laptop|  |Phone |....       |Laptop|  |Phone |...        |
|       +------+  +------+           +------+  +------+           |
+-----------------------------------------------------------------+
~~~~
{:#campus title="Campus Network Operation Test Agent Network Topology" artwork-align="center"}

| Test Config | Category | Config Capabilities|
|---|---|---|
|SUT|Mobile Phone/Laptop|2.4G Access,5G Access|
|Network Element|WLAN AP|Telemetry Support|
|Network Element|Switch|Telemetry Support|
|Network Operation Agent|O&M Management System|Support configuration distribution, state monitoring, and fault handling through the existing management system|
|Network Operation Large Model|LLM|Identify the model vendor, version, and number of parameters and deployed model locally to ensure data remains within the domain|
|Network Operation Agent Hardware Platform|Hardware Platform|GPU/NPU Model and Number|
|Agetn Test System|Test Sytem|simulate scenarios such as traffic peaks and sudden surges with mixed east-west and north-south traffic using a professional traffic testing instrument
collect agent operation information and produce test results|
{: #tab-test-config title="Test System Configuration Requirements"}

## Interactive Q&A Task Test Requirements

The test set is defined based on the scenario definition for Campus Network Operation Agent. The Test requirements is shown as follows:

| Test Objective | Verify the ability of agents to autonomously perceive, analyze, and handle network anomalies without human intervention|
|---|---|
|Test Case Requirement|Cover scenarios such as fault and optimization. Each scenario should cover different difficulty levels from T1 to T3 (basic-medium-hard).|
|Test Topo|Refer to campus topo to select typical network deployment scenarios such as smart classroom or dormitory|
|Pre-set Condition|a) The network environment is running stably, and baseline data has been collected.b) The O&M system's agent capability is enabled and has the
necessary tools access permissions.c) The test system has been configured.|
|Test Step|a) Triggering Event: Pre-set network anomaly events are required for constructing test cases.|
{: #campus-interact title="Interactive Q&A Test System Requirements"}

## Autonomous Response Task Test Requirements


# Data Center Network Operation Agent Test Method

## Network Topology Test Requirements

~~~~
             +-------------------------------+
             | Data Center Network Operation |
             |           Agent               |
             +-------------+-----------------+
                           |
                           |
 +----------------------------------------------------+
 |                     +---+----+                     |
 |                +----| Core 1 +---+                 |
 |                |    +--------+   |                 |
 |                |                 |                 |
 |            +---+----+       +----+---+             |
 |            | Border |       | Border |             |
 |            | Leaf1  |       | Leaf2  |             |
 |            +--------+       +--------+             |
 |                    -----    ----                   |
 |                       -------                      |
 |                 ------       -----                 |
 |        +---------+             +--------+          |
 |        |  Spine1 |             | Spine2 |          |
 |        +----+----+             +----+---+          |
 |             |                       |              |
 |          +-----------+-----------+--+--------+     |
 |          |  |        |           |           |     |
 |       +--|---------+-|---------+-|---------+ |     |
 |       |  |         | |         | |         | |     |
 |  +----+--+-+  +----+-+--+ +----+-+--+ +----+-+--+  |
 |  |  Server |  |  Server | |  Server | |  Server |  |
 |  | Leaf 1-1|  | Leaf 1-2| | Leaf 2-1| | Leaf 2-2|  |
 |  +----+----+  +----+----+ +-----+---+ +-----+---+  |
 |     +-++-----+-----+--+       +-++-----+----+---+  |
 |   +-|--|-+---|-+-----+|     +-|--|-+---|-+-----+|  |
 | +-+-++++-+-+++-+-++--+++  +-+-++++-+-+++-+-++--+++ |
 | |    ||    ||    ||    |  |    ||    ||    ||    | |
 | | svr|| svr|| svr|| svr|  | svr|| svr|| svr|| svr| |
 | | 1-1|| 1-2|| 1-3|| 1-4|  | 2-1|| 2-2|| 2-3|| 2-4| |
 | |    ||    ||    ||    |  |    ||    ||    ||    | |
 | +----++----++----++----+  +----++----++----++----+ |
 |                                                    |
 +----------------------------------------------------+
~~~~
{:#dctopo title="Data Center Network Operation Test Agent Network Topology" artwork-align="center"}

## Interactive Q&A Task Test Requirements

## Autonomous Response Task Test Requirements
