---
title: SORT IT - Build a PDF Processor

event: BrightTALK Webinar
event_url: https://www.brighttalk.com/webcast/17108/403788

location: Online

summary: How to build a PDF processor to automatically extract, classify and summarise PDF documents.
abstract: In this talk I describe how to build a PDF processor. The processor takes PDF files as input, uses Optical Character Recognition (OCR) to extract the text for each input PDF, classifies the document into one of twenty article categories and summarises the text to an arbitrary number of sententences (by default 3 using Latent Semantic Analysis (LSA)), before composing the document title, classification, summary and original text into a text file which is emailed to the user.

# Talk start and end times.
#   End time can optionally be hidden by prefixing the line with `#`.
date: "2020-04-28"
#date_end: "2019-09-19T00:00:00Z"
all_day: true

# Schedule page publish date (NOT talk date).
publishDate: "2017-01-01T00:00:00Z"

authors: []
tags: []

# Is this a featured talk? (true/false)
featured: true

image:
  caption: ''
  focal_point: Right

links:
# - icon: twitter
#   icon_pack: fab
#   name: Follow
#   url: https://twitter.com/georgecushen
url_code: "https://github.com/AdamJelley/automated-pdf-processor"
url_pdf: ""
url_slides: ""
url_video: "https://www.brighttalk.com/webcast/17108/403788"

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
-
---

# Usage

The [dashboard](dashboard:moj9XTy) shows a view of the input PDFs and output files. Documents can be uploaded by dragging and dropping onto the left hand side of the dashboard. The [scenario](scenario:ProcessDocument) to process the input PDFs can be triggered from here, and the text file contatining the classification, summary and full text will appear in the folder on the right hand side. Please see below for further information.

# Data
Arbitrary PDF files can be placed into the [input folder](managed_folder:QZb3pfTL) for classfication and summarisation.

The document classification model was trained on the 20 newsgroups dataset ([documentation](http://qwone.com/~jason/20Newsgroups/)) which is publicly available from sci-kit learn as described [here](https://scikit-learn.org/0.19/datasets/twenty_newsgroups.html#).

# Project Components

## OCR for Text Extraction

The PDFs are first converted to image files using pdf2image ([documentation](https://pypi.org/project/pdf2image/)) [here](recipe:compute_HNEvJqgm). The OCR is then performed using Google's open-source Tesseract library ([documentation](https://github.com/tesseract-ocr/tesseract#tesseract-ocr)) [here](recipe:compute_htEULTjD).

## Model for Document Classification

As described above, the model was trained on the 20 newsgroups dataset available from sci-kit learn [here](https://scikit-learn.org/0.19/datasets/twenty_newsgroups.html#). The model uses a multiclass logistic regression classifier built on top of a tf-idf transformer and achieves an AUC of 0.995. Full details of the model are available [here](saved_model:y1EXaY1m).

## Text Summarisation

The text summarisation was performed using Dataiku's Text Summarisation [plugin](recipe:compute_PDF_topics) ([documentation](https://www.dataiku.com/product/plugins/text-summarization/)). This plugin provides an interface for three popular text summarisation algorithms including [TextRank](https://web.eecs.umich.edu/~mihalcea/papers/mihalcea.emnlp04.pdf), [KL-Sum](http://www.aclweb.org/anthology/N09-1041) and [LSA](http://www.kiv.zcu.cz/~jstein/publikace/isim2004.pdf). By default this project uses LSA to select 3 sentences to summarise the document, but this can be changed using the [plugin](recipe:compute_PDF_topics) UI.

# Flow

The top part of the flow converts the [PDFs to images](recipe:compute_HNEvJqgm), [extracts the text](recipe:compute_htEULTjD) using OCR and [summarises](recipe:compute_PDF_topics) the document.

The bottom part of the flow gets the [documents](dataset:twenty_newsgroups) and [labels](dataset:target_names_prepared), [joins](recipe:compute_twenty_newsgroups_joined) them together to create the [training data](dataset:twenty_newsgroups_joined), trains the [model](saved_model:y1EXaY1m) on this training data and uses the model to [classify](dataset:PDF_text_labelled) the document.

The document summary is then [joined](recipe:compute_PDF_topics_category) to the document classification, which are cleaned up and written to [output files](managed_folder:htEULTjD) for each document.

# Running the Flow / Automation

The flow can be run using the scenario [ProcessDocument](scenario:ProcessDocument). This scenario will extract the text from all input PDFs, classify and summarise the document, create the output file and email this to the user. By default, text extraction will not be re-performed if the PDF has been processed previously (so previously processed PDFs will not be cleared even if they are removed from the input), but this behaviour can be modified by changing the project variable 'reprocess_PDFs' to 'True' in the first step of the scenario.

Alternatively, when a new PDF is dropped into the [input folder](managed_folder:QZb3pfTL), DSS will automatically recognise that the input has changed, run the flow and email the user the end result. DSS is currently set to check the input folder every 2 minutes and will run the flow it detects that the input has changed and there have been no further changes made over the following minute.

<!-- {{% callout note %}}
Click on the **Slides** button above to view the built-in slides feature.
{{% /callout %}}

Slides can be added in a few ways:

- **Create** slides using Wowchemy's [*Slides*](https://wowchemy.com/docs/managing-content/#create-slides) feature and link using `slides` parameter in the front matter of the talk file
- **Upload** an existing slide deck to `static/` and link using `url_slides` parameter in the front matter of the talk file
- **Embed** your slides (e.g. Google Slides) or presentation video on this page using [shortcodes](https://wowchemy.com/docs/writing-markdown-latex/).

Further event details, including [page elements](https://wowchemy.com/docs/writing-markdown-latex/) such as image galleries, can be added to the body of this page. -->
