---
title: "OpenAI's LLM Wins IMO Gold: A Breakthrough in General-Purpose Reasoning"
date: 2025-07-2020
permalink: /posts/2025/07/openai-imo-gold/
tags:
  - OpenAI
  - IMO
  - LLMs
---

## What Just Happened?

OpenAI has reported that its latest experimental reasoning model achieved gold medal-level performance on the 2025 International Math Olympiad (IMO), solving 5 out of 6 problems under the same conditions as human participants (two 4.5‑hour sessions, no external tools).

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">1/N I’m excited to share that our latest <a href="https://twitter.com/OpenAI?ref_src=twsrc%5Etfw">@OpenAI</a> experimental reasoning LLM has achieved a longstanding grand challenge in AI: gold medal-level performance on the world’s most prestigious math competition—the International Math Olympiad (IMO). <a href="https://t.co/SG3k6EknaC">pic.twitter.com/SG3k6EknaC</a></p>&mdash; Alexander Wei (@alexwei_) <a href="https://twitter.com/alexwei_/status/1946477742855532918?ref_src=twsrc%5Etfw">July 19, 2025</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

They achieve this result using new general purpose reinforcement learning and test time compute scaling techniques.

## Why this Matters

### General Purpose Reasoning with Natural Language

DeepSeek's [DeepSeek-Prover-V2](https://arxiv.org/pdf/2504.21801) and DeepMind's [AlphaProof](https://deepmind.google/discover/blog/ai-solves-imo-problems-at-silver-medal-level/) both combine LLMs with [Lean](https://lean-lang.org), a formal programming language used to verify mathematical proofs.
[AlphaGeometry 2](https://arxiv.org/pdf/2502.03544) uses a symbolic engine combined with LLMs to solve geometry problems.
Combining AlphaProof and AlphaGeometry 2 achieved silver medal-level performance in IMO 2024.

AlphaProof is trained by formalizing problems into Lean using a formalizer network, searching for proofs using a solver network (verified using Lean) and using the [AlphaZero](https://deepmind.google/discover/blog/alphazero-shedding-new-light-on-chess-shogi-and-go/?_gl=1*1rrdmid*_up*MQ..*_ga*ODIzMDU1OTMxLjE3NTI5Nzg4MDg.*_ga_LS8HVHCNQ0*czE3NTI5Nzg4MDgkbzEkZzAkdDE3NTI5Nzg4MDgkajYwJGwwJGgw) RL algorithm to solve progressively harder problems.
![Alpha Proof Pipeline](alphaproof.png)

These are all neurosymbolic methods tailored specifically to solve mathematical problems. On the other hand, OpenAI's new experimental reasoning model does not have a symbolic component and uses general purpose reasoning, without being designed specifically to solve IMO problems. It also means a single model can solve all the problems without having to combine task specific models such as AlphaProof and AlphaGeometry 2.

This is exciting as it means the same system can be applied to a wide range of problems which can potentially solve real world problems and is a step towards AI contributing to scientific discoveries.

In my research on multimodal reasoning, I wanted to create a system that can achieve high accuracy in solving multimodal problems. My first thought was using a symbolic component (e.g. Lean) that ensures the final output is correct and easily verifiable. While being easily verifiable is a nice property, it constrains the types of problems you can solve (in this case, you can only solve math proofs).

Instead, I believe major progress in AI will not come from domain specific architectures, but from very simple, general frameworks that can be trained using RL and scaled up arbitrarily with search and learning as in the [Bitter Lesson](https://www.cs.utexas.edu/~eunsol/courses/data/bitter_lesson.pdf).

### New Frontier in Test Time Compute Scaling

The ability of models to reason effectively over very long time horizons (weeks/months) is essential to achieve significant scientific breakthroughs. 

METR posted an [article](https://metr.org/blog/2025-03-19-measuring-ai-ability-to-complete-long-tasks/#:~:text=If%20we%20plot%20this%20on,time%20of%20around%207%20months.)
which showed that the length of tasks AI agents can do is doubling every 7 months
![Length of tasks AI can do is doubling every 7 months](length-of-tasks-log.png).

OpenAI's new experimental reasoning model can reason for multiple hours which shows a continuation of this trend. The longer the models can reason for without breaking down, the harder and more useful real world tasks the model can solve. We introduced [PuzzleWorld](https://arxiv.org/pdf/2506.06211), a puzzlehunt benchmark aimed to test the ability of models to solve open-ended, multimodal problems that humans often take hours or even days to solve. OpenAI's new model shows a promising step in this direction of being able to solve longer time horizon problems.

### A New Paradigm of Multi-Agent Learning

Reasoning models such as OpenAI o3 can reason on the order of minutes.
However, computational cost scales quadratically with the context length and LLM performance tends to degrade on longer context lengths with high quality long context data for training models hard to come by.

A simple question to ask is that: is scaling test time compute on a single model the most compute-efficient, or are there alternatives?

Mixture of Experts (MoE) emerged as a more efficient architecture for LLMs with smaller expert modules being trained jointly with a router module which selects which experts to be activated for a specific task. This allows models with very large amount of parameters to be trained while reducing inference costs as only a fraction of the parameters is activated at inference time.

My speculation is that OpenAI have found a new technique which shows similar test-time compute efficiency gain with multiple LLM agents. 

## Clues on how the system might work

Firstly, this breakthrough was from OpenAI's new multi-agent team (https://x.com/polynoamial/status/1946480714939085301). So it is likely that the system is composed of multiple agents. Also, the team has the right background for building such a multi-agent system, with Noam Brown and Alexander Wei (team lead) both being authors of the [CICERO](https://ai.meta.com/research/cicero/diplomacy/) paper - an AI which achieves human-level performance in the strategy game Diplomacy. CICERO is an AI agent which needs to communicate, negotiate and coordinate with other humans. They also have backgrounds in game theory which underpins many of the ideas in multi-agent learning.

(Fun fact: I attended a talk from Noam Brown at MIT in January 2025 where he was recruiting for the new multi-agent LLM team he was creating. I guess this is the first project that they worked on which they completed over the span of several months.)

Secondly, the team is small, (https://x.com/polynoamial/status/1946478258968531288) and fairly new, suggesting that a new model was not trained from scratch. Instead, it is likely that existing reasoning models were used (likely unreleased reasoning models) and RL post training techniques were applied.





Terence Tao hints how multiple agents may have been used and highlights how it is unfair to compare the performance of human participants with OpenAI's new AI model:
https://mathstodon.xyz/@tao/114881418225852441


He says that while the 6 human IMO participants in each team are not allowed to communicate and the team leader is not allowed to help them, if we change the format such that:
- (Hypothetically) Time passes more slowly for the student (parallel computation, inference time speed ups etc.)
- The leader rewrites the question into a format easier to work with (LLM question rewriting, subproblem decomposition etc.)
- The six students collaborating, communicating with each other on progress and dead ends (multiple LLM agents coordinating)
- Team leader guides the students towards promising approaches and stops them if they are stuck on an approach unlikely to succeed (Coordinator LLM that oversees the other agents)
- Team leader selects best solution, discarding the rest (Coordinator LLM selects best solution out of all the agents)
- If no student obtains a good solution, the team leader withdraws from the competition (Tuning the system to achieve strong performance on IMO 2025 without reporting poor results.)

## How it Works (Maybe)

Disclaimer: The following is speculation on how I think the system may have been built and is not necessarily the case - further research in this area is required to reproduce the results.

Drawing from the prior analysis, the following are my thoughts on how such a system could have been built.

Multiple strong baseline (unreleased) reasoning LLMs are used without limits on computational cost or inference.

A pool of 'Worker' LLMs are used to generate proofs. The workers in this pool can communicate with each other. To prevent the context length from blowing up, workers likely broadcast key pieces of information to a shared channel, accessible by all the LLMs in the pool.

In the context of math problems, the key pieces of information may be when the LLM gets stuck on an approach so decides to give up on it, telling other LLMs not to go down the same path, or if the LLM achieves some progress, showing the other LLMs what they have found.

A 'Coordinator' LLM is then used to manage the pool of workers. The coordinator acts as a general purpose neural verifier/reward model. It monitors the progress of all the LLMs. If an LLM is stuck on an approach, the coordinator may intervene to suggest ways to get unstuck, correct any mistakes, suggest to try a new approach or may kill the LLM altogether. If the LLM is killed, a new worker LLM may be spun up with the key pieces of information from the shared channel to start again. If a worker proposes multiple possible approaches, the coordinator may choose the one that it thinks is most promising in a deep learning guided search process.
