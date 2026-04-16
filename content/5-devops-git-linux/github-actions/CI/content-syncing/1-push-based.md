---
title: The Push Based Model
description:
draft: false
tags:
  - Ci-CD
  - github-actions
  - shell-scripting
modified: 2026-04-16
created: 2026-04-16
---
**The Push-Based Model** is an event-driven content publishing pipeline built to decouple a private data repository from a public-facing Static Site Generator (SSG).

Instead of the presentation layer actively fetching data from a database, the private "Source of Truth" (SoT) acts as a headless Content Management System. Upon modification, it proactively filters, compiles, and "pushes" only the approved subset of data to a public repository, triggering the SSG's build process.

![[Pasted image 20260416223246.png]]





## Benefits Over the Pull-Based Model

When comparing this to a Pull-Based Model (where the SSG repository runs a script to fetch data from the SoT), the Push pattern offers significant architectural advantages:

- **Security & The Principle of Least Privilege:** This is the paramount benefit. In a Pull model, the public repository requires a token with read access to your _entire_ private vault. If the public repo is compromised, all private data is exposed. In the Push model, the private repo holds a token with _write-only_ access to the public presentation layer. The worst-case scenario is defacement of the public site, while private data remains mathematically inaccessible.
- **Pure Presentation Layer:** The public SSG repository remains completely entirely "dumb." It contains no complex fetching logic, no API keys, and no awareness of where its data originates. It strictly handles rendering markdown into HTML.
- **Reduced Attack Surface:** The filtering logic runs within the isolated, secure CI/CD environment of the private repository before anything touches a public network.

## Architectural Trade-Offs

While highly secure, this pattern does introduce a specific trade-off regarding the **Single Responsibility Principle (SRP)** at the macro level:
- **Coupling in the Source of Truth:** The private repository is no longer _just_ storing notes; it is now aware of external systems. It holds the routing configuration and the deployment pipeline, meaning it is tightly coupled to the folder structure of the remote SSG repository. If the SSG changes how it expects content to be structured, the pipeline in the private repository will break and require updating.


## Implementation : 

To recreate this pipeline, the following distinct components must be established:

### 1. **Define the Rules Engine (Configuration):** sync-config.yml 
Create a human-readable configuration file (e.g., YAML) in the private repository. This dictates exactly which directories are allowed to be published and explicitly lists sensitive files that must be excluded from those directories.


```yml
# Directories to sync to Quartz
includeDirs:
  - 2-system-design
  - 3-database-design-internals
  - 6-interview

# Specific files to keep private
excludeFiles:
  - system-design-notes/drafts.md
  - private-passwords.md # Contains db credentials
```


### 2. Establish Secure Authentication :

Generate a Fine-Grained Personal Access Token scoped _only_ to the remote public repository with `Contents: Read and Write` permissions. Inject this token into the private repository's CI/CD secrets vault.


### 3. Create the Delivery Pipeline

Configure an event listener (e.g., a GitHub Action) in the private repository to trigger whenever changes are merged into the main branch.

```yml
name: Deploy Notes to Remote

on:
  push:
    branches:
      - main

jobs:
  sync-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout The Notes Repository
        uses: actions/checkout@v4
        with:
          path: notes

      - name: Checkout Remote Repository
        uses: actions/checkout@v4
        with:
          repository: surajgoraicse/dev-notes-remote
          token: ${{ secrets.REMOTE_REPO_TOKEN }}
          path: remote

      - name: Make script executable
        run: chmod +x notes/sync.sh

      - name: Run Rsync Script
        run: ./notes/sync.sh
        env:
          SOURCE_DIR: notes
          TARGET_DIR: remote/content

      - name: Commit and Push to remote
        run: |
          cd remote
          # Configure Git to act as a bot
          git config user.name "github-actions[bot]"
          git config user.email "github-actions[bot]@users.noreply.github.com"
          
          # Stage the changes in the content directory
          git add content/
          
          # Commit the changes (or do nothing if there are no changes)
          git commit -m "Auto-sync notes from source of truth" || echo "No changes to commit"
          
          # Push to the remote repository
          git push origin HEAD:main
```


### 4. Execute the Filter and Sync

Within the pipeline, parse the configuration file to generate a list of inclusions and exclusions. Use a robust file-syncing utility (like `rsync`) to recursively copy the allowed files into a temporary clone of the public repository, dropping anything marked as private.

```bash
#!/bin/bash

# Stop immediately on errors, don’t allow undefined variables, and don’t hide failures in pipelines
set -euo pipefail

# Default paths, easily overridden by GitHub Action environment variables
SOURCE_DIR="${SOURCE_DIR:-.}"
TARGET_DIR="${TARGET_DIR:-../remote/content}"
CONFIG_FILE="$SOURCE_DIR/sync-config.yml"

echo "Starting synchronization process..."

# Ensure target directory exists and is clean to prevent stale files
rm -rf "$TARGET_DIR"
mkdir -p "$TARGET_DIR"

# Parse the YAML config using yq (pre-installed on GitHub Actions ubuntu runners)
# The output is iterated over to build the include and exclude rules
INCLUDE_DIRS=$(yq '.includeDirs[]' "$CONFIG_FILE")
EXCLUDE_FILES=$(yq '.excludeFiles[]' "$CONFIG_FILE")

# Create a temporary file to hold the exclude patterns for rsync
EXCLUDE_LIST=$(mktemp)
for excl in $EXCLUDE_FILES; do
  echo "$excl" >> "$EXCLUDE_LIST"
done

# Loop through each allowed directory and sync
for dir in $INCLUDE_DIRS; do
  SRC_PATH="$SOURCE_DIR/$dir"
  DEST_PATH="$TARGET_DIR/$dir"
  
  if [ -d "$SRC_PATH" ]; then
    echo "Syncing directory: $SRC_PATH -> $DEST_PATH"
    mkdir -p "$DEST_PATH"
    
    # Execute rsync with strict filtering
    rsync -av \
      --exclude-from="$EXCLUDE_LIST" \
      --include="*/" \
      --include="*.md" \
      --exclude="*" \
      "$SRC_PATH/" "$DEST_PATH/"
  else
    echo "Warning: Source directory '$SRC_PATH' not found. Skipping."
  fi
done

# Clean up the temporary file
rm -f "$EXCLUDE_LIST"

echo "Synchronization completed successfully!"
```


### 5. Commit and trigger : 

Have the pipeline commit the synced files using a bot profile and push the changes to the remote repository. This push acts as the webhook to trigger the static site generator's native hosting deployment (e.g., Vercel, Cloudflare Pages).


