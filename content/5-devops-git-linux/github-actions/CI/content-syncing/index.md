---
title: Content Publishing Pipeline
description: An event-driven selective content publishing pipeline built on top of a static site generator.
draft: false
tags:
  - distributed-systems
  - backend
modified: 2026-04-16
created: 2026-04-16
---
## problem statement : 

You have to build a web application using the Quartz framework to publish a curated subset of your notes.

The source of truth is a private repository. Whenever a new commit is pushed to the `main` branch of this repository, the Quartz-based application should automatically:

1. Pull the latest changes from the repository.
2. Filter the files and directories based on a configuration file (e.g., include/exclude rules), since not all content should be made public.
3. Copy only the filtered content into the Quartz application’s content directory (which acts as the default context directory for serving docs).
4. Trigger a build and deployment process so the updated content is accessible over the internet.

The goal is to create an automated pipeline that selectively publishes content from a private notes repository to a public-facing Quartz documentation site.

