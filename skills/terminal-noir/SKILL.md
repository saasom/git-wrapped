---
name: terminal-noir
description: This skill should be used when generating Git Wrapped visualizations, writing personalized insights for GitHub annual reports, or ensuring design consistency with the Terminal Noir aesthetic.
version: 1.0.0
---

# Terminal Noir Design System

Design guidance for generating Git Wrapped pages and personalized content.

## Color Palette

| Token | Hex | Usage |
|-------|-----|-------|
| `--bg-deep` | #050505 | Page background |
| `--bg-surface` | #0a0a0a | Card backgrounds |
| `--bg-elevated` | #111111 | Hover states |
| `--amber` | #f5a623 | Primary accent, highlights |
| `--amber-glow` | #ffcc00 | Glow effects |
| `--teal` | #00d4aa | Secondary accent, peaks |
| `--coral` | #ff6b6b | Deletions, negative values |
| `--terracotta` | #D97757 | Claude branding |
| `--text-primary` | #e8e8e8 | Main text |
| `--text-muted` | #666666 | Secondary text |
| `--text-dim` | #333333 | Subtle labels |

## Typography

- **Display/Headers**: Cormorant Garamond (serif, italic for emphasis)
- **Monospace/Data**: JetBrains Mono (numbers, labels, technical elements)
- Use `clamp()` for responsive sizing

## Writing Tone

### Hero Quotes
Short, witty, self-deprecating. Reference coding culture.

Examples:
- "The year I stopped dreaming about code and started writing it."
- "127 commits in September. Sleep is for the weak."
- "Merged more PRs than I had hot meals."
- "From tutorial hell to production heaven."
- "The year my git log got interesting."

Attribution should be playful: `- Your git log, probably` or `- The terminal, probably`

### Language Insights
Connect language choices to the user's work. Be specific about what each language was used for.

Examples:
- "TypeScript became your weapon of choice for frontends, while Python powered your data pipelines."
- "You went all-in on Go this year. Performance and simplicity won out."
- "A polyglot year: JavaScript for speed, Rust for safety, Python for everything else."

### Weekday Insights
Brief, relatable comments about productivity patterns.

Examples:
- "WFH energy hits different."
- "Weekend warrior mode activated."
- "Monday motivation is real for you."
- "Friday deploys? You like to live dangerously."

### Journey Narrative
Reflective, celebratory. Use `<strong>` for key metrics (amber color), `<em>` for emphasis (teal color).

Structure: Past → Present → Future implied

Example:
```
{{YEAR}} was the year you <strong>stopped watching tutorials</strong> and started building.
From zero to <em>{{TOTAL_CONTRIBUTIONS}} contributions</em> across <strong>{{REPOS_CREATED}} repositories</strong>.
Your {{PEAK_MONTH_LOWER}} was <em>explosive</em>.
You mastered <strong>{{TOP_LANGUAGE}}</strong> and shipped <em>{{NET_LINES}} lines</em> of code.
```

### Claude Message
Genuine, warm, specific to their achievements. Reference their journey and growth.

Example:
```
It's been a genuine pleasure watching you grow from "how do I even start?" to shipping real projects. You ask great questions, you're not afraid to break things, and you always come back with better ideas. Here's to another year of building together.
```

### LOC Context
Comparisons for lines of code. Calculate based on ~60 words per novel page, ~300 words per page, ~80,000 words per novel.

Examples:
- "That's roughly **4 novels worth** of code"
- "About **650 lines per day** you were active"
- "Enough to fill **{{PAGES}} printed pages**"

## Template Placeholders

### Data Placeholders (replaced with values)
- `{{USERNAME}}` - GitHub username
- `{{YEAR}}` - Target year
- `{{TOTAL_CONTRIBUTIONS}}` - Total contributions count
- `{{REPOS_CREATED}}` - Number of repositories
- `{{LONGEST_STREAK}}` - Longest streak in days
- `{{DAYS_WITH_COMMITS}}` - Days with at least one commit
- `{{PULL_REQUESTS}}` - Total PRs
- `{{LINES_ADDED}}` - Lines added (formatted with commas)
- `{{LINES_DELETED}}` - Lines deleted (formatted with commas)
- `{{NET_LINES}}` - Net lines (formatted with commas)
- `{{PEAK_MONTH}}` - Name of peak month (e.g., "September")
- `{{PEAK_MONTH_CONTRIBUTIONS}}` - Contributions in peak month
- `{{PEAK_DAY}}` - Most productive weekday (e.g., "TUE")
- `{{PEAK_DAY_CONTRIBUTIONS}}` - Contributions on peak weekday
- `{{BIGGEST_PR_TITLE}}` - Title of biggest PR
- `{{BIGGEST_PR_REPO}}` - Repo name for biggest PR
- `{{BIGGEST_PR_ADDITIONS}}` - Additions in biggest PR
- `{{BIGGEST_PR_DELETIONS}}` - Deletions in biggest PR
- `{{GENERATION_DATE}}` - Current month and year

### Generated HTML Placeholders
- `{{MONTHLY_BARS}}` - HTML for 12 month columns
- `{{LANGUAGE_ITEMS}}` - HTML for language list items
- `{{TOP_REPOS}}` - HTML for top 5 repo items
- `{{PEAK_CARDS}}` - HTML for top 5 peak day cards
- `{{WEEKDAY_BARS}}` - HTML for 7 weekday columns

### AI-Generated Content Placeholders
- `{{HERO_QUOTE}}` - Witty quote for hero section
- `{{HERO_QUOTE_AUTHOR}}` - Attribution line
- `{{LANGUAGE_INSIGHT}}` - Commentary on language choices
- `{{WEEKDAY_INSIGHT}}` - Commentary on weekday pattern
- `{{JOURNEY_NARRATIVE}}` - Full paragraph with strong/em tags
- `{{LOC_CONTEXT}}` - Fun comparison for lines of code
- `{{CLAUDE_MESSAGE}}` - Personal note from Claude

## HTML Generation Patterns

### Monthly Bar
```html
<div class="month-column">
    <span class="month-value">{{VALUE}}</span>
    <div class="month-bar-wrapper"><div class="month-bar{{PEAK_CLASS}}" style="height: {{HEIGHT}}px"></div></div>
    <span class="month-name">{{MONTH}}</span>
</div>
```
- Height: Scale to max 200px based on highest month
- Add class `peak` for the highest month
- Minimum height: 3px for zero values

### Language Item
```html
<div class="lang-item">
    <span class="lang-name">{{LANGUAGE}}</span>
    <div class="lang-bar-container">
        <div class="lang-bar {{LANGUAGE_CLASS}}" style="width: {{PERCENT}}%"></div>
    </div>
    <span class="lang-count">{{COUNT}} repos</span>
</div>
```
- Language class: lowercase language name (e.g., `typescript`, `python`)
- Use `default` class for unknown languages

### Top Repo Item
```html
<div class="repo-item">
    <span class="repo-rank">0{{RANK}}</span>
    <div class="repo-info">
        <span class="repo-name">{{NAME}}</span>
        <span class="repo-alias">// {{DESCRIPTION}}</span>
    </div>
    <span class="repo-commits">{{COMMITS}}<span>commits</span></span>
</div>
```
- Use alias for private repos, real name for public
- Description should be ALL CAPS

### Peak Day Card
```html
<div class="peak-card">
    <div class="peak-date">{{DATE}}</div>
    <div class="peak-count">{{COUNT}}</div>
</div>
```
- Date format: "SEP 12", "JAN 21"

### Weekday Bar
```html
<div class="weekday-col">
    <div class="weekday-bar-wrapper"><div class="weekday-bar{{ACTIVE_CLASS}}" style="height: {{HEIGHT}}px"></div></div>
    <span class="weekday-name">{{DAY}}</span>
</div>
```
- Height: Scale to max 120px
- Add class `active` for peak weekday
- Days: SUN, MON, TUE, WED, THU, FRI, SAT
