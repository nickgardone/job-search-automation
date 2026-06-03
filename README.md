# Job Search Automation

A Claude API agent that queries job listings daily and appends structured results to a local Excel tracker with salary-range color coding.

## How it works

The script uses Claude Sonnet with web search enabled to find current job postings matching configurable criteria. New listings are appended to a local `.xlsx` tracker, with rows color-coded by salary range:
- Green: $150k+
- Yellow: Salary not listed
- Red: Below $150k

Runs on a schedule via macOS LaunchAgent (Mon/Wed/Fri by default).

## Stack

- **Python** — orchestration layer
- **Anthropic SDK** — Claude Sonnet with web search tool
- **openpyxl** — Excel read/write
- **macOS LaunchAgent** — local scheduling

## Setup

1. Install dependencies:
   ```bash
   pip install anthropic openpyxl
   ```
2. Set your Anthropic API key:
   ```bash
   export ANTHROPIC_API_KEY=sk-ant-...
   ```
3. Update `BASE_PATH` in `job_search.py` to point to your tracker spreadsheet
4. Run manually to verify output:
   ```bash
   python job_search.py
   ```

## Scheduling

Use the included `com.nickgardone.jobsearch.plist.template` to run on a schedule:

1. Copy and edit the template:
   ```bash
   cp com.nickgardone.jobsearch.plist.template ~/Library/LaunchAgents/com.nickgardone.jobsearch.plist
   ```
2. Edit paths and `ANTHROPIC_API_KEY` in the plist
3. Load the agent:
   ```bash
   launchctl load ~/Library/LaunchAgents/com.nickgardone.jobsearch.plist
   ```

## Files

| File | Purpose |
|---|---|
| `job_search.py` | Main script — queries Claude API, appends to tracker |
| `com.nickgardone.jobsearch.plist.template` | LaunchAgent config template |
