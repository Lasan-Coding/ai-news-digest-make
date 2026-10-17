# AI News Digest (Make)

A daily email with the 5 most interesting AI articles of the last 24 hours, collected from several RSS feeds and each summarized in the same short format. Built in Make.

![Daily digest email](email-digest.PNG)

## Problem

Keeping up with AI news means checking many news articles and blogs, and most posts are long while the relevant point is often one sentence. The goal: one email per day that says what happened, why it matters and why it was picked, with a link to the full article.

## What it does

1. Reads 5 RSS feeds for articles from the last 24 hours (up to 5 per feed): TechCrunch AI, OpenAI, Google DeepMind, Hugging Face and Google Cloud AI.
2. One Claude call (Haiku 5.5) ranks all candidates by title and summary and picks the best 5.
3. For each pick, it fetches the full page and Claude writes a short summary: TL;DR, three takeaways, why it matters, plus an optional line on regulation, competitors or challenges.
4. Each entry gets a source label and a "Why it made the cut" line.
5. The digest is saved as a dated Google Doc and sent by email. If no source published anything, nothing is sent.

![Scenario in Make](scenario-in-make.PNG)

## Design decisions

- **Rank first, then read.** Ranking only uses titles and RSS summaries. Only the 5 winners get the expensive steps (page fetch and summary).
- **Cost control: One ranking call.** Make bills per module run, so comparing all candidates in one call costs the same however many there are.
- **Fallback instead of failure.** If a page can't be fetched, the scenario uses the title and ranking reason and continues.
- **Fixed output format.** Every entry reads the same and stays under 100 words.

## Cost

A full run uses about 40 Make credits. Claude usage is billed separately through the Anthropic API (it costs a few cents).

## Limitations

- The ranking only sees titles and RSS summaries, so a vague title can be ranked too low.
- "Max. 2 articles per source" is a prompt rule, and the model doesn't always follow it.
- Weekends are thinner, because feeds publish less.

## Stack

Make · RSS · HTTP · Claude API (Haiku 5.5) · Google Docs · Gmail
