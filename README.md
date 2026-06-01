# Job Search Automation

Uses the Claude API to find PM job listings daily and append them to a local Excel tracker, color-coded by salary range.

## How it works

The script prompts Claude Sonnet (with web search) to find current PM job postings matching your criteria. New listings are appended to `Nick Application Tracker.xlsx` with rows color-coded by salary:
- Green: $150k+
- Yellow: Salary not listed
- Red: Below $150k

Designed to run on a schedule via macOS LaunchAgent (Mon/Wed/Fri by default).

## Setup

1. Install dependencies:
   ```bash
   pip install anthropic openpyxl
   ```
2. Set your Anthropic API key:
   ```bash
   export ANTHROPIC_API_KEY=sk-ant-...
   ```
3. Update the `BASE_PATH` in `job_search.py` to point to your Applications folder containing the tracker spreadsheet
4. Run manually to verify output:
   ```bash
   python job_search.py
   ```

## Scheduling with LaunchAgent

Use the included `com.nickgardone.jobsearch.plist.template` to run on a schedule:

1. Copy and edit the template:
   ```bash
   cp com.nickgardone.jobsearch.plist.template ~/Library/LaunchAgents/com.nickgardone.jobsearch.plist
   ```
2. Edit the plist to set the correct paths for `ProgramArguments` and `ANTHROPIC_API_KEY`
3. Load the agent:
   ```bash
   launchctl load ~/Library/LaunchAgents/com.nickgardone.jobsearch.plist
   ```

## Files

| File | Purpose |
|---|---|
| `job_search.py` | Main script — queries Claude API, appends to tracker |
| `com.nickgardone.jobsearch.plist.template` | macOS LaunchAgent config template for scheduling |
