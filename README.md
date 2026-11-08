# AIDev & GH Archive — Exploratory Data Analysis

## Project Overview

This repository documents my exploratory analysis of AI coding agent contributions to GitHub projects using **AIDev v5, GH Archive, and the GitHub REST API**.

The project focuses on understanding the AIDev dataset, analyzing agentic pull requests (PRs), exploring repository characteristics, and investigating how AIDev can be integrated with GH Archive to study GitHub development activities.

## 1. Git and GitHub Fundamentals

I explored GitHub development workflows through hands-on practice, including:

- Creating repositories and branches
- Making commits and opening pull requests
- Understanding PR states (Open, Closed, Merged)
- Creating and managing issues
- Understanding code reviews, forks, and stars

## 2. AIDev Dataset Exploration

**Dataset:** [AIDev on Hugging Face](https://huggingface.co/datasets/hao-li/AIDev)

I explored the AIDev v5 dataset structure and examined how AI coding agent contributions are identified through GitHub branch names, bot accounts, and PR descriptions.

### Agentic PR Distribution

The dataset contains **7,685,281 agentic PRs** across six AI coding agents.

| AI Agent | Pull Requests | Unique Repositories |
|---|---:|---:|
| OpenAI Codex | 3,844,602 | 354,337 |
| GitHub Copilot | 1,581,924 | 346,224 |
| Claude Code | 1,567,100 | 173,325 |
| Cursor | 317,453 | 81,786 |
| Google Jules | 296,411 | 41,105 |
| Devin | 77,791 | 10,259 |

I also examined repository characteristics, including stars, forks, primary programming languages, and the differences between the full AIDev dataset and the AIDev-pop subset.

## 3. GH Archive Data Exploration

**Data source:** [GH Archive](https://www.gharchive.org/)

I downloaded and analyzed 14 hours of public GitHub event data from March 20, 2026 (10:00–23:59 UTC).

### Key Results

- **2,191,536** GitHub events analyzed
- Examined event distributions, including PushEvent, PullRequestEvent, and IssuesEvent
- Analyzed PR actions such as opened, merged, closed, and reopened
- Created a visualization of hourly GitHub activity

I also used **Google BigQuery** to perform SQL-based filtering and aggregation, validating the results against my local Python analysis.

## 4. AIDev and GH Archive Data Integration

I investigated how AIDev and GH Archive can be joined using common GitHub identifiers.

An important finding is that AIDev's PR-level `id` corresponds to the GitHub Issue ID, while GH Archive's `payload.pull_request.id` corresponds to the GitHub Pull Request ID.

I therefore used a combination of **Repository ID and PR Number** for cross-dataset matching.

### Matching Results

- **7,394 unique PRs** successfully matched
- **9,335 PR event records** matched
- All six AI coding agents represented in the matched data

This demonstrates how GH Archive can supplement AIDev with historical GitHub event information.

## 5. Pull Request Lifecycle Analysis

I analyzed the opened, merged, and closed events of the matched PRs.

Among the matched records, **490 PRs** had both opening and merging events within the observed 14-hour window.

For this subset:

- Median time from PR creation to merge: **3.13 minutes**
- Mean time from PR creation to merge: **21.58 minutes**
- 168 PRs were merged within one minute
- 271 PRs were merged within five minutes

These results are descriptive and limited to PRs whose opening and merging events were both observed within the sampling window. They should not be interpreted as overall agent productivity or merge-time performance.

## 6. GitHub API and Commit Validation

I examined real pull requests to validate the relationship between AIDev, GH Archive, and the GitHub REST API.

### Case Study: netdata/netdata PR #21995

[View Pull Request](https://github.com/netdata/netdata/pull/21995)

- Agent: OpenAI Codex
- Commits: 8
- PR creation and merge timestamps matched between GH Archive and GitHub API
- Successfully matched the GitHub `merge_commit_sha` with a GH Archive `PushEvent.head`

I also examined a Claude Code PR that was merged one second after creation and investigated its GitHub Actions checks and review activity.

## 7. Data Source Comparison

I compared GH Archive and the GitHub REST API in terms of data availability, event structure, historical coverage, and update frequency.

I also checked five consecutive hourly GH Archive files from November 7, 2026. All files were accessible, with Last-Modified timestamps approximately five minutes after the corresponding hour ended.

GH Archive is useful for large-scale historical event analysis, while the GitHub REST API provides additional PR-level and commit-level details.

## 8. Jupyter Notebook

The analysis code, saved results, and visualizations are available here:

[**GHArchive_AIDev_Exploration.ipynb**](./GHArchive_AIDev_Exploration.ipynb)

The notebook contains the exploratory analysis, data processing steps, statistics, matching procedures, and validation examples.

## Data Sources and References

- [AIDev Dataset](https://huggingface.co/datasets/hao-li/AIDev)
- [AIDev Research Paper](https://arxiv.org/abs/2602.09185)
- [GH Archive](https://www.gharchive.org/)
- [GitHub REST API Documentation](https://docs.github.com/en/rest)
- [Google BigQuery](https://cloud.google.com/bigquery)

## Notes

This repository documents exploratory research rather than a finalized empirical study. The analysis is subject to dataset coverage, sampling limitations, missing data, and differences in GitHub event representations.

The Notebook preserves analysis outputs for inspection. Reproducing the full analysis requires access to the source datasets and the necessary Python environment.

