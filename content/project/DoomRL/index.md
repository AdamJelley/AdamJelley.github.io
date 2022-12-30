---
title: Reinforcement Learning to Play Doom
summary: A project to play various Doom games with reinforcement learning agents.
tags:
date: "2021-08-01"

# Optional external URL for project (replaces project detail page).
external_link: ""

image:
  caption: PPO agent playing Doom
  focal_point: Smart

links:
# - icon: twitter
#   icon_pack: fab
#   name: Follow
#   url: https://twitter.com/georgecushen
url_code: "https://github.com/AdamJelley/DoomRL"
url_pdf: ""
url_slides: ""
url_video: "https://github.com/AdamJelley/DoomRL/tree/main/videos"

# Slides (optional).
#   Associate this project with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.
slides: ""
---

A project to apply reinforcement learning to play Doom. Uses [StableBaselines3](https://stable-baselines3.readthedocs.io/en/master/index.html) implementations of RL algorithms on [ViZDoom](http://vizdoom.cs.put.edu.pl/) using the [ViZDoomGym](https://github.com/shakenes/vizdoomgym) wrapper. Key takeaway: reward shaping is key!