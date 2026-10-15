# Instagram Content Engagement Analysis (Jan 2025 – Sep 2026)

A follow-up to my [2022–2025 Instagram & Meta Ads analysis](https://github.com/2uira/instagram-meta-ads-performance-analysis). I helped Business Wisdom Institute (BWI) set up a new post-level tracking sheet, then cleaned and analysed **390 Instagram posts across 21 months**, this time doing the cleaning and analysis in **SQL (DuckDB)**.

**Questions**
1. Which formats, content types and posting days earn the most engagement?
2. How has engagement moved month to month?
3. Which collaborators (speakers, trainers, guests) bring the most engagement?

![Engagement by variables](BWI%20IG%20Engagement%20by%20Variables.png)

## Key findings

- **Video drives over 3× the engagement of carousels.** Across 189 posts each, Reels averaged 63 engagements and 1,699 reach, against 19 and 491 for carousels.
- **Educational content is the strongest category with a meaningful sample** (avg 61 engagements over 65 posts). Promotional posts reach the most people (avg 1,808) but their engagement doesn't keep pace, the same pattern found in 2022–2025.
- **Documentation is overused.** It is the second most-posted category (146 posts) but averages only 27 engagements.
- **The best days for reach are the worst days for engagement.** Tuesday and Friday reach the most people (1,425 avg) yet have the lowest engagement; Sunday (56) and Wednesday (50) lead on engagement.
- **Engagement is event-driven, not cumulative.** It spikes around seminars and campaigns (May 2025: 133 avg; Mar 2026: 89) and drops back the following month. Almost every top post features a named collaborator.
- [Collaborator finding: which guests or trainers brought the most engagement]

![Monthly engagement trend](BWI%20IG%20Monthly%20engagement%20Trend.png)

## Recommendations

- [Recommendation 1, e.g. shift carousel-heavy documentation posts into short video recaps]
- [Recommendation 2, e.g. plan content around the event calendar and feature collaborators]
- [Recommendation 3, e.g. test Sunday/Wednesday scheduling for engagement-focused posts]

## Data cleaning (SQL)

The source sheet was **21 monthly blocks stacked into one file** (498 raw rows), each with its own merged 4-row header.

- **Parsed the stacked blocks** with a row-by-row walker, extracting 390 real posts from labels, headers and spacer rows.
- **Staged cleaning in DuckDB** (`stage1 → stage2 → stage3 → posts`) so each step could be verified on its own; `TRY_CAST` turned text placeholders into NULLs instead of errors.
- **Standardised formats.** Three spellings of "Video", watch times mixed between seconds and `mm:ss`, and five different "no value" labels.
- **Flagged 10 paid-boosted posts** (`is_boosted`), since bought reach would distort organic comparisons.
- **Handled multi-tagged posts properly.** 34 posts carried two content types; a bridge table (`STRING_SPLIT` + `UNNEST`) counts each post in both categories instead of discarding the second tag.

## Limitations

Comparisons use raw engagement totals, so formats with higher reach (video) naturally collect more interactions. Engagement *rate* (engagement ÷ reach) gives a fairer comparison and is the next step. Facebook metrics were never logged, so the analysis is Instagram-only.

## Files

| File | Contents |
| --- | --- |
| [Analysis notebook](BWI_Instagram_Content_Analysis_2025-2026.ipynb) | Python parsing + DuckDB SQL cleaning and analysis |
| [Full report (PDF)](BWI_Instagram_Content_Report_2025-2026.pdf) | Findings and recommendations for the marketing team |
| [Power BI dashboard (.pbix)](BWI%20Instagram%20Performance%20Report%2025-26.pbix) | Interactive dashboard (requires Power BI Desktop) |

## Tools

Python (pandas, re) · DuckDB SQL (CTEs, window functions, regex, UNNEST) · Power BI · Jupyter

*Raw data is not included for confidentiality.*
