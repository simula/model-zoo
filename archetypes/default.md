---
# Title and creation date for this entry
title: "{{ replace .Name "-" " " | title }}"
date: {{ .Date }}

# Categorise this entry by topic, such as "Image recognition", "GAN", etc. 
# Multiple categories can be added.
category: ["Uncategorised"]

# Tags for framework or language used, type of data, etc 
tags: []

# URL for GitHub or similar
code_repository: ""

# Files for model download
# These are separated in "display name" and "url", where "url" is the download
# link and "display_name" is the title shown instead of the full url.
files:
  # Requirements file, typically "requirements.txt" if using pip
  requirements: 
    display_name: 
    url: 
  # Model save file(s) -- single binary or zip archive
  model: 
    display_name: 
    url:
  # Docker image
  image:
    display_name:
    url:

# One-liner describing the model and its use
summary: ""

# If "true", the entry will not be published
draft: false
---
