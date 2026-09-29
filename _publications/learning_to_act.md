---
title: "Learning to Act While Thinking"
collection: publications
category: conferences
permalink: /publication/learning_to_act
authors: 'Adam Labiosa, Josiah P. Hanna'
date: 2026-06-24
venue: 'Preprint'
note: 'An earlier version appeared at the Reinforcement Learning in Big Worlds Workshop at RLC 2026.'
paperurl: '/files/learning_to_act_while_thinking.pdf'
---

Operating in real-time, dynamic environments requires agents to respond quickly to sudden changes while also planning over long horizons to solve difficult tasks. In such settings, plans take time and energy to compute and the environment does not pause while an agent thinks. As a result, planning should only be done when necessary and plans may arrive stale, as they are computed from a prior state of the world. Such an agent therefore must decide when to request a plan, how to act while waiting for it, and to what extent to follow it once it arrives. We formalize this setting as a real-time meta-reasoning MDP, in which an agent can request a plan from a slow, asynchronous planner but incurs a cost and delay before the plan is returned. Using this formalism, we show when requesting a plan improves an agent's policy given a cost and delay. We then describe our method Acting While Thinking (AWT) to train reinforcement learning agents to learn to request plans and act while they are computed. We then evaluate AWT in five real-time, dynamic tasks, and show that learned policies achieve a higher return and use fewer plans than fixed meta-reasoning and heuristic baselines. Finally, we analyze the behavior of the resulting agents and show when they learn to query additional computation.
