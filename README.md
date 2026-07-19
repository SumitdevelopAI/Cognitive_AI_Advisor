# Research Question
*->* *Can a neuro-symbolic homeostatic computational architecture integrate neural and behavioral signals to maintain a dynamic model of human cognitive state and use that model for interpretable closed-loop adaptation?*

    Can we build an AI system that understands how a person's thinking and mental state change over time by using signals from the brain, body, behavior, and surroundings, and then safely adapts its response to that person in a way we can understand and explain?

# Problem Statemet 
Current computational approaches for human cognitive-state estimation primarily focus on detecting or classifying specific cognitive states from neural or behavioral signals at a given point or within a limited time window. However, human cognitive states are not static or isolated; they evolve continuously and interact over time. A system that only produces individual state predictions may therefore lack a persistent representation of how a person's cognitive condition is changing.

Furthermore, when cognitive-state estimates are used to drive adaptive systems, the reasoning connecting an observed signal, an inferred cognitive state, and a resulting adaptation is often difficult to represent explicitly and interpret.

This research addresses the problem of **maintaining a dynamic and interpretable computational model of human cognitive state from neural and behavioral observations and using that evolving model to support closed-loop adaptation**.

The central challenge is to move beyond isolated cognitive-state prediction toward a system that can continuously update its representation of cognitive state, reason about relevant state changes, and use this information to determine appropriate adaptations while preserving an interpretable relationship between observation, inference, and action.

The research will investigate whether a **neuro-symbolic homeostatic computational architecture** can provide such a mechanism by combining learned representations from human signals with explicit cognitive-state representations and regulatory processes for closed-loop adaptation.

## Specific Research Problem

Existing cognitive workload estimation systems can identify levels of mental workload from EEG and behavioral signals. However, workload is often treated primarily as a classification problem, where a model predicts discrete states such as low, medium, or high workload.

This approach provides limited representation of how an individual's workload evolves during continuous task performance and does not, by itself, provide an explicit mechanism for using changes in estimated workload to regulate an adaptive system.

This research therefore focuses on the problem of **continuously tracking cognitive workload from EEG and behavioral performance and using an explicit, interpretable cognitive-state model to regulate task difficulty in a closed feedback loop.**

## Research Aim

To develop and evaluate a neuro-symbolic homeostatic architecture that dynamically models cognitive workload from EEG and behavioral signals and uses the resulting state representation to adapt task difficulty in real time.

## Primary Research Question

**Can a neuro-symbolic homeostatic architecture improve closed-loop task adaptation by maintaining a dynamic model of cognitive workload derived from EEG and behavioral signals?**

## Research Objectives

1. Develop an EEG and behavioral-signal model for estimating cognitive workload during a controlled cognitive task.

2. Represent estimated workload as a continuously evolving internal cognitive state rather than only as an isolated classification output.

3. Develop a homeostatic controller that maintains workload within a predefined functional operating range by detecting meaningful deviations in the modeled workload state.

4. Implement explicit adaptation rules that increase, maintain, or decrease task difficulty according to workload state, behavioral performance, and model uncertainty.

5. Evaluate the proposed closed-loop system against a non-adaptive system and a conventional workload-based adaptive approach using workload regulation, task performance, adaptation stability, and interpretability as evaluation criteria.

## Initial Experimental Scope

The first study will investigate a controlled working-memory task in which task difficulty can be systematically modified. EEG signals and behavioral measurements such as response accuracy and reaction time will be used to estimate changes in cognitive workload.

The proposed system will maintain an evolving workload state and regulate task difficulty through a closed feedback loop:

EEG + Behavioral Performance → Workload Estimation → Dynamic Workload State → Homeostatic Regulation → Task-Difficulty Adaptation → Human Response → State Update.

The research will focus specifically on cognitive workload regulation. Attention, fatigue, emotion, broader physiological sensing, and general-purpose cognitive modeling are outside the primary scope of the initial study.





