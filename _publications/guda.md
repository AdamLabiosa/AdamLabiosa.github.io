---
title: "Guided data augmentation for offline reinforcement learning and imitation learning"
collection: publications
category: conferences
permalink: /publication/guda
authors: 'Nicholas E Corrado, Yuxiao Qu, John U Balis, Adam Labiosa, Josiah P Hanna'
date: 2024-08-01
venue: 'RLC 2024'
paperurl: 'https://arxiv.org/pdf/2310.18247'
---

In offline reinforcement learning (RL), an RL agent learns to solve a task using only a fixed dataset of previously collected data. While offline RL has been successful in learning real-world robot control policies, it typically requires large amounts of expert-quality data to learn effective policies that generalize to out-of-distribution states. Unfortunately, such data is often difficult and expensive to acquire in real-world tasks. Several recent works have leveraged data augmentation (DA) to inexpensively generate additional data, but most DA works apply augmentations in a random fashion and ultimately produce highly suboptimal augmented experience. In this work, we propose Guided Data Augmentation (GuDA), a human-guided DA framework that generates expert-quality augmented data. The key insight behind GuDA is that while it may be difficult to demonstrate the sequence of actions required to produce expert data, a user can often easily characterize when an augmented trajectory segment represents progress toward task completion. Thus, a user can restrict the space of possible augmentations to automatically reject suboptimal augmented data. To extract a policy from GuDA, we use off-the-shelf offline reinforcement learning and behavior cloning algorithms. We evaluate GuDA on a physical robot soccer task as well as simulated D4RL navigation tasks, a simulated autonomous driving task, and a simulated soccer task. Empirically, GuDA enables learning given a small initial dataset of potentially suboptimal experience and outperforms a random DA strategy as well as a model-based DA strategy.
