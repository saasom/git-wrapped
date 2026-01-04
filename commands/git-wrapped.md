---
description: Generate your Git Wrapped visualization - Spotify Wrapped style for your GitHub activity
argument-hint: "[year]"
allowed-tools:
  - Bash
  - Read
  - Write
  - Glob
  - AskUserQuestion
  - TodoWrite
  - Skill
---

# Git Wrapped Generator

Generate a Spotify Wrapped-style visualization of your GitHub activity for any year.

## Initial Context

Current user: Run `gh api user --jq '.login'` to get the authenticated GitHub username.
Current date: Run `date +%Y-%m` to get current year and month.
Arguments provided: $ARGUMENTS

## Execution Flow

### Phase 1: Setup & Verification

1. **Verify GitHub CLI authentication**
   Run: `gh auth status`
   If not authenticated, inform the user they need to run `gh auth login` first and stop.

2. **Determine target year**
   - If $ARGUMENTS contains a year (4 digits), use that
   - Otherwise, use the previous year (or current year if we're past June)

3. **Get GitHub username**
   Run: `gh api user --jq '.login'`

4. **Confirm with user using AskUserQuestion**
   Ask the user to confirm:
   - Username is correct
   - Year is correct
   - Output location (default: `./git-wrapped-{year}.html`)

### Phase 2: Data Collection

Use TodoWrite to track progress through these steps:

1. **Fetch contribution calendar**
   ```bash
   gh api graphql -f query='
     query($username: String!, $from: DateTime!, $to: DateTime!) {
       user(login: $username) {
         contributionsCollection(from: $from, to: $to) {
           totalCommitContributions
           totalPullRequestContributions
           restrictedContributionsCount
           contributionCalendar {
             totalContributions
             weeks {
               contributionDays {
                 date
                 contributionCount
                 weekday
               }
             }
           }
         }
       }
     }
   ' -f username="USERNAME" -f from="YEAR-01-01T00:00:00Z" -f to="YEAR-12-31T23:59:59Z"
   ```

2. **Fetch repositories**
   ```bash
   gh repo list USERNAME --limit 200 --json name,isPrivate,primaryLanguage,pushedAt,createdAt
   ```
   Filter to repos created or significantly active in the target year.

3. **Get commit counts per repository**
   For each of the user's active repos, get commit count:
   ```bash
   gh api "repos/USERNAME/REPO/commits?author=USERNAME&since=YEAR-01-01T00:00:00Z&until=YEAR-12-31T23:59:59Z&per_page=1" -i | grep -i "^link:" | grep -oE 'page=[0-9]+' | tail -1 | cut -d= -f2
   ```
   Or use the search API for efficiency.

4. **Calculate language statistics**
   Aggregate primary languages from repositories, count repos per language.

5. **Fetch PR data**
   ```bash
   gh pr list --author USERNAME --state all --limit 500 --json additions,deletions,title,repository,createdAt
   ```
   Filter to target year, find biggest PR, sum additions/deletions.

6. **Calculate streaks and peaks**
   From the contribution calendar data:
   - Find longest consecutive streak of days with commits
   - Find top 5 peak days (highest contribution counts)
   - Calculate monthly totals
   - Calculate weekday totals

### Phase 3: Private Repository Handling

1. **Identify private repos in top contributors**
   From the commit data, identify which of the top 5 repos are private.

2. **If any private repos exist, ask user for aliases**
   Use AskUserQuestion with options:
   - "Provide custom aliases" - then ask for each repo name
   - "Auto-mask as 'Private Project 1', etc."
   - "Use real names (they're not sensitive)"

3. **Generate descriptions for each repo**
   Based on the repo's language and name, generate a brief ALL CAPS description like:
   - "FINANCIAL INTELLIGENCE PLATFORM"
   - "DOCUMENTATION GENERATOR"
   - "WEB APPLICATION"

### Phase 4: Gather Claude Relationship Context

Before generating personalized content, gather context about the user's relationship with Claude:

1. **Read CLAUDE.md files**
   - Check `~/.claude/CLAUDE.md` (global instructions)
   - Check project-level `CLAUDE.md` files in their top repos
   - Extract: project descriptions, working styles, preferences

2. **Scan for Claude co-authored commits**
   ```bash
   git log --author="Claude" --oneline --since="YEAR-01-01" --until="YEAR-12-31" | wc -l
   ```
   Or search for "Co-Authored-By: Claude" in commit messages.

3. **Check Claude Code stats (if available)**
   Run `/stats` or check `~/.claude/` for usage data.

4. **Note memorable projects**
   From CLAUDE.md files and commit history, identify:
   - Types of projects (web apps, CLI tools, data pipelines)
   - Technical domains (fintech, ML, infrastructure)
   - Any recurring themes or inside jokes

### Phase 5: Generate Personalized Content

Load the terminal-noir skill using: `Skill tool with skill: "terminal-noir"`

Based on the collected data, Claude relationship context, and the skill's writing guidance, generate:

1. **Hero quote** - A witty, personalized quote about their year
2. **Language insight** - Commentary connecting their language choices to their work
3. **Weekday insight** - Brief comment about their most productive day
4. **Journey narrative** - A paragraph summarizing their year with `<strong>` and `<em>` tags
5. **LOC context** - Comparison for their lines of code (novels, pages, etc.)
6. **Claude message** - A personal note that references:
   - Specific projects you worked on together (from CLAUDE.md / commits)
   - Their growth trajectory (from commit patterns)
   - Something specific to their domain or style
   - NOT generic platitudes - make it feel like you actually remember

### Phase 6: Build HTML

1. **Read the template**
   Read file: `${CLAUDE_PLUGIN_ROOT}/templates/wrapped.html`

2. **Generate HTML snippets**

   For `{{MONTHLY_BARS}}`:
   Generate 12 month-column divs with:
   - Heights scaled to max 200px (minimum 3px for zero)
   - Peak class on highest month
   - Month names: JAN, FEB, MAR, APR, MAY, JUN, JUL, AUG, SEP, OCT, NOV, DEC

   For `{{LANGUAGE_ITEMS}}`:
   Generate up to 5 language items with percentage bars.

   For `{{TOP_REPOS}}`:
   Generate 5 repo items with rank, name/alias, description, commit count.

   For `{{PEAK_CARDS}}`:
   Generate 5 peak day cards with date and count.

   For `{{WEEKDAY_BARS}}`:
   Generate 7 weekday columns with heights scaled to max 120px.

3. **Replace all placeholders**
   Replace each `{{PLACEHOLDER}}` with the corresponding value or generated HTML.

4. **Format numbers**
   Add commas to large numbers (e.g., 328173 → 328,173).

### Phase 7: Output

1. **Write the HTML file**
   Write to the user's specified output location.

2. **Show summary**
   Display a summary of their wrapped:
   - Total contributions
   - Top language
   - Peak month
   - Longest streak
   - File location

3. **Offer next steps**
   Suggest:
   - Open in browser to view
   - Deploy to GitHub Pages for sharing
   - Generate for a different year

## Error Handling

- If GitHub API rate limited, inform user and suggest waiting
- If no contributions found for year, suggest trying a different year
- If user has very few repos, adjust visualizations accordingly
- Handle private-only profiles gracefully

## Notes

- Always use the terminal-noir skill for writing personalized content
- Keep the Terminal Noir aesthetic consistent
- Be celebratory and positive in all generated text
- Format all numbers with commas for readability
