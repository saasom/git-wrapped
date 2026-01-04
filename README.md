# Git Wrapped

Generate beautiful, Spotify Wrapped-style visualizations of your GitHub activity.

![Terminal Noir Design](https://img.shields.io/badge/design-Terminal%20Noir-f5a623)
![Claude Code Plugin](https://img.shields.io/badge/claude--code-plugin-D97757)

## Features

- **Annual GitHub Stats** - Contributions, repos, languages, streaks, and more
- **Terminal Noir Aesthetic** - CRT scan lines, noise textures, amber & teal accents
- **Personalized Insights** - AI-generated commentary on your coding year
- **Privacy-Friendly** - Alias your private repos with custom names
- **Shareable** - Static HTML that works anywhere

## Preview

Your Git Wrapped includes:
- Total contributions with witty quote
- Repository and streak statistics
- Monthly contribution velocity chart
- Language breakdown with insights
- Top projects by commit count
- Peak performance days
- Weekly rhythm analysis
- Lines of code shipped
- Year-in-review narrative

## Prerequisites

- [Claude Code](https://claude.ai/code) CLI installed
- [GitHub CLI](https://cli.github.com/) (`gh`) installed and authenticated

```bash
# Verify GitHub CLI is authenticated
gh auth status
```

## Installation

### Option 1: Clone and Enable

```bash
# Clone this repository
git clone https://github.com/saasom/git-wrapped.git

# Enable the plugin in Claude Code
claude plugins add ./git-wrapped
```

### Option 2: Direct from GitHub

```bash
claude plugins add https://github.com/saasom/git-wrapped
```

## Usage

```bash
# Generate wrapped for the previous year
claude /git-wrapped

# Generate wrapped for a specific year
claude /git-wrapped 2024

# The command will:
# 1. Confirm your GitHub username and target year
# 2. Fetch all your GitHub stats
# 3. Ask for aliases for any private repos
# 4. Generate a personalized HTML visualization
# 5. Save it to git-wrapped-{year}.html
```

## Output

The generated HTML file is fully self-contained with:
- Embedded fonts (Google Fonts)
- All CSS inline
- No JavaScript dependencies
- Responsive design
- Print-friendly

Open it in any browser or host it anywhere (GitHub Pages, Netlify, Vercel, etc.).

## Customization

During generation, you can:
- Choose custom aliases for private repositories
- Specify the output file location
- Generate for any year you have data

## The Terminal Noir Design

The visual design features:
- **CRT scan lines overlay** - Retro terminal aesthetic
- **Noise texture** - Subtle film grain effect
- **Cormorant Garamond** - Elegant serif for headers
- **JetBrains Mono** - Technical monospace for data
- **Amber (#f5a623)** - Primary accent for highlights
- **Teal (#00d4aa)** - Secondary accent for peaks
- **Terracotta (#D97757)** - Claude's signature color

## How It Works

1. **Data Collection** - Uses GitHub's GraphQL API via `gh` CLI to fetch:
   - Contribution calendar
   - Repository list and languages
   - Commit counts per repo
   - Pull request statistics

2. **Processing** - Calculates:
   - Monthly and weekday breakdowns
   - Longest streak
   - Peak days
   - Total lines of code

3. **Personalization** - Claude generates:
   - Witty hero quote
   - Language insights
   - Year-in-review narrative
   - Personal message

4. **Rendering** - Fills the Terminal Noir HTML template with your data

## Example Output

See a live example: [saasom.github.io/git-wrapped](https://saasom.github.io/git-wrapped)

## Contributing

Contributions welcome! Feel free to:
- Report bugs
- Suggest features
- Submit pull requests
- Share your wrapped!

## License

MIT

---

Built with Claude Code
