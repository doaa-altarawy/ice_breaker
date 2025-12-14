# Branch Synchronization

This repository is a fork of [emarco177/ice_breaker](https://github.com/emarco177/ice_breaker) and includes an automated workflow to keep all branches in sync with the upstream repository.

## Automated Sync

The repository uses a GitHub Actions workflow (`.github/workflows/sync-branches.yml`) that automatically syncs all branches from the upstream repository. The workflow:

- Runs daily at midnight UTC
- Runs when changes are pushed to the `main` branch
- Can be triggered manually from the Actions tab

### How It Works

1. Fetches all branches from the upstream repository (`emarco177/ice_breaker`)
2. For each upstream branch:
   - Creates or updates the corresponding local branch
   - Pushes the branch to this fork's origin
3. Preserves any local-only branches (branches that don't exist upstream)

### Manual Trigger

To manually trigger the sync workflow:

1. Go to the **Actions** tab in this repository
2. Select **Sync All Branches from Upstream** workflow
3. Click **Run workflow**
4. Select the branch (usually `main`)
5. Click **Run workflow** button

## Manual Sync (Alternative)

If you need to manually sync branches locally, you can use these commands:

```bash
# Add upstream remote (one time only)
git remote add upstream https://github.com/emarco177/ice_breaker.git

# Fetch all branches from upstream
git fetch upstream --prune

# Sync a specific branch
git checkout <branch-name>
git merge upstream/<branch-name>
git push origin <branch-name>

# Or sync all branches with a script
for branch in $(git branch -r | grep 'upstream/' | grep -v 'HEAD' | sed 's|upstream/||'); do
  git checkout -b "$branch" "upstream/$branch" 2>/dev/null || git checkout "$branch"
  git merge upstream/"$branch"
  git push origin "$branch"
done
```

## Project Branches

This repository includes multiple project branches that correspond to different lessons in the LangChain course:

- `project/hello-world` - Basic LangChain structure
- `project/search-agent` - Modern search agent implementation
- `project/ReAct-search-agent` - Classic ReAct search agent
- `project/ReAct-Algo` - ReAct algorithm fundamentals
- `project/rag-gist` - RAG implementation basics
- `project/code-interpreter` - Code execution agent
- `project/chat-wth-your-pdf` - PDF chat application

Each branch contains a complete, working example that can be checked out and run independently.

## Troubleshooting

### Merge Conflicts

If the automated sync encounters merge conflicts:

1. The workflow will attempt to resolve them automatically
2. If automatic resolution fails, you may need to manually resolve conflicts:
   ```bash
   git checkout <branch-with-conflict>
   git fetch upstream
   git merge upstream/<branch-with-conflict>
   # Resolve conflicts manually
   git add .
   git commit
   git push origin <branch-with-conflict>
   ```

### Missing Branches

If a branch from upstream is not appearing in your fork:

1. Check the workflow run logs in the Actions tab
2. Manually trigger the sync workflow
3. Verify the branch exists in the upstream repository

## Upstream Repository

This fork syncs from: [https://github.com/emarco177/ice_breaker](https://github.com/emarco177/ice_breaker)

For questions about the course content or project structure, please refer to the upstream repository.
