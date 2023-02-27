---
title: Going Beyond Behaviour Cloning With Off-Policy Reinforcement Learning

event: Cambridge MLG Reading Group - Offline Reinforcement Learning
event_url: http://talks.cam.ac.uk/talk/index/197326

location: Online

summary: How to adapt off-policy reinforcement learning algorithms to the offline reinforcement leaning setting to get better performance on deployment than behaviour cloning.
abstract: In the first part of the talk we will introduce the common terms used in standard online RL. After that we will define the offline RL setting, describing applications and benchmarks. We will then focus on behavioural cloning (BC), as a simple and stable baseline for learning a policy from offline interaction data. As a particular instance of BC, we will describe the decision transformer, a recently proposed method that leverages the transformer architecture to tackle the offline RL setting. In the second part of the talk, we will explore how off-policy RL algorithms originally designed for the online setting (such as SAC) can be adapted to better handle the necessary distribution shift required for improving on the policy in the offline data, without online feedback. We will find that this reduces to a problem of quantifying and managing uncertainty. In the third and last part of the talk, we will first review the classical offline reinforcement learning methods, including ways to evaluate and improve policies using offline data by importance sampling. The challenges and applicability of these methods will be discussed. Then, we will review modern offline RL methods, including policy constraint methods and model-based offline RL methods. In policy constraint methods, we encourage the new policy to be similar to the policy observed in the offline dataset, while in model-based offline RL methods, we quantify the uncertainty of the model and use the uncertainty to discourage the new policy from visiting those uncertain regions.

# Talk start and end times.
#   End time can optionally be hidden by prefixing the line with `#`.
date: "2023-02-15"
#date_end: "2019-09-19T00:00:00Z"
all_day: true

# Schedule page publish date (NOT talk date).
publishDate: "2017-01-01T00:00:00Z"

authors: []
tags: []

# Is this a featured talk? (true/false)
featured: true

image:
  # caption: 'Cambridge Machine Learning Group'
  # focal_point: Bottom

links:
# - icon: twitter
#   icon_pack: fab
#   name: Follow
#   url: https://twitter.com/georgecushen
url_code: ""
url_pdf: ""
url_slides: "https://docs.google.com/presentation/d/1gd5nzMAMoUzTmWfdpZfrdU6cg4GxUAv6EB-FbuSPgr0/edit#slide=id.g206b5329e0f_0_9"
url_video: ""

# Markdown Slides (optional).
#   Associate this talk with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.
slides: ""

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects:
---

<!-- {{% callout note %}}
Click on the **Slides** button above to view the built-in slides feature.
{{% /callout %}}

Slides can be added in a few ways:

- **Create** slides using Wowchemy's [*Slides*](https://wowchemy.com/docs/managing-content/#create-slides) feature and link using `slides` parameter in the front matter of the talk file
- **Upload** an existing slide deck to `static/` and link using `url_slides` parameter in the front matter of the talk file
- **Embed** your slides (e.g. Google Slides) or presentation video on this page using [shortcodes](https://wowchemy.com/docs/writing-markdown-latex/).

Further event details, including [page elements](https://wowchemy.com/docs/writing-markdown-latex/) such as image galleries, can be added to the body of this page. -->
