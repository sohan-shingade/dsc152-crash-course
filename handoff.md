# DSC 152 Final Exam Prep — Handoff Document

## Context

- **Course:** DSC 152 — Applied Statistical Data Analysis and Inference, UCSD Spring 2026
- **Instructor:** Peter Chi
- **Final Exam:** Saturday June 6, 2026, 3:00–6:00 PM (3 hours)
- **Format:** In-person, on-paper, handwritten. One 8.5x11 double-sided handwritten note sheet allowed. No calculators, no computers.
- **Coverage:** Cumulative (Lec 1-20), with **emphasis on Lec 11+ (interaction terms onward)**
- **Weight:** 30% of final grade

## What Exists Already

- `dsc152-quiz2-wiki.md` — Comprehensive wiki covering Lec 6-11 (A/B testing through interaction terms intro)
- `dsc152-quiz3-wiki.md` — Comprehensive wiki covering Lec 12-17 (interaction terms continued through time series intro)
- Lecture .Rmd files: Lec06-Lec11 in repo root
- Labs 2-9 as .ipynb in `labs/`
- Homework 1-3 in `hw/`
- Quiz 3 cheatsheet, practice exams, study guide as HTML files

## What's NOT Covered Yet

- **Lectures 1-5** (hypothesis testing foundations) — no wiki exists, but these are lower priority
- **Lectures 18-19** (time series regression, power estimation in time series) — no wiki or local files
- **Cross-topic synthesis** — connecting concepts across all units for cumulative exam

## Study System Design

### Approach: Adaptive Diagnostic-Teach-Quiz Loop

Each topic follows this cycle:
1. **DIAGNOSE** — 5 cold questions (no notes) to gauge baseline knowledge
2. **TEACH** — Targeted instruction on weak areas found in diagnostic, with visualizations and examples
3. **QUIZ** — 5 fresh questions to check mastery (target: 85%+)
4. **INTERLEAVE** — Mixed questions from previously studied topics

### Mastery Thresholds
- **90-100%**: Mastered. Skip to interleaved review only.
- **70-89%**: Partially solid. Targeted review on gaps, then re-quiz.
- **40-69%**: Needs work. Full study block.
- **0-39%**: Major gaps. Full study block + prerequisite check.

### How to Continue a Session

1. Read `progress.md` to see current state per topic
2. Check which day/block we're on in the schedule
3. Pick up from the next uncompleted topic or review session
4. After each quiz, update `progress.md` with scores and notes
5. Always end sessions by updating both `progress.md` and this handoff doc

## 4-Day Schedule (June 3-6)

### Day 1 — Tuesday June 3 (Today)
**Focus: High-priority post-Week-5 topics**
- Block 1: Interaction terms (Lec 11-12) — diagnose, teach gaps, quiz
- Block 2: Model selection pitfalls (Lec 13) — diagnose, teach, quiz
- Block 3: Transformations (Lec 14) — diagnose, teach, quiz
- Block 4: Interleaved review of Day 1 topics

### Day 2 — Wednesday June 4
**Focus: Logistic regression & odds + earlier regression**
- Block 1: Warm-up interleaved review (Day 1 topics, 15 min)
- Block 2: Relative risks & odds ratios (Lec 15) — diagnose, teach, quiz
- Block 3: Logistic regression (Lec 16) — diagnose, teach, quiz
- Block 4: Simple & multiple linear regression (Lec 7-8) — quick diagnose, fill gaps
- Block 5: Interleaved review of all topics so far

### Day 3 — Thursday June 5
**Focus: Time series + remaining topics + full integration**
- Block 1: Warm-up interleaved review (all previous, 15 min)
- Block 2: Time series intro + regression (Lec 17-19) — diagnose, teach, quiz
- Block 3: Earlier topics rapid fire — categorical predictors (Lec 9), diagnostics (Lec 10), A/B testing (Lec 6)
- Block 4: Hypothesis testing foundations (Lec 1-5) — rapid diagnostic, fill gaps only
- Block 5: Full cumulative practice exam simulation

### Day 4 — Friday June 6 (Exam Day)
**Focus: Consolidation only — no new material**
- Block 1: Timed practice exam (if available)
- Block 2: Review only missed items from practice
- Block 3: Cheat sheet finalization
- Stop studying 2-3 hours before 3 PM exam

## Topic Priority Map

### Tier 1 — Highest Emphasis (Lec 11+, most exam weight)
| # | Topic | Lectures | Status |
|---|-------|----------|--------|
| 1 | Interaction terms | 11-12 | pending |
| 2 | Model selection pitfalls | 13 | pending |
| 3 | Transformations (log, double-log) | 14 | pending |
| 4 | Relative risks & odds ratios | 15 | pending |
| 5 | Logistic regression | 16 | pending |
| 6 | Time series (ACF/PACF, AR models) | 17 | pending |
| 7 | Time series regression + power | 18-19 | pending |

### Tier 2 — Solid Foundation Needed
| # | Topic | Lectures | Status |
|---|-------|----------|--------|
| 8 | A/B testing, t-test vs permutation | 6 | pending |
| 9 | Simple linear regression inference | 7 | pending |
| 10 | Multiple linear regression | 8 | pending |
| 11 | Categorical predictors & partial F-test | 9 | pending |
| 12 | Model diagnostics (LINE) | 10 | pending |

### Tier 3 — Foundational (quick review)
| # | Topic | Lectures | Status |
|---|-------|----------|--------|
| 13 | Type I error, t-test basics | 1-2 | pending |
| 14 | Nonparametric tests | 3 | pending |
| 15 | Power analysis | 4 | pending |
| 16 | Statistical significance vs effect size | 5 | pending |

## Key Exam Patterns (from Practice Quizzes)

Based on practice quizzes 1-3, the exam will heavily test:
- **Writing fitted equations for specific subgroups** from interaction models
- **Interpreting coefficients** — in context, with correct wording ("holding X constant...")
- **Algebraic derivations** — especially for log/double-log model interpretations
- **Null hypothesis writing** — especially for partial F-tests
- **R code writing** — for plots, model fitting, ACF/PACF
- **Converting between scales** — log-odds to odds ratio, CI transformations
- **Identifying when individual p-values are insufficient** vs partial F-test needed
- **Listing conditions/assumptions** for valid inference

## Wording Emphasis

This course is VERY particular about wording. Key phrases:
- "For every one unit increase in X, Y changes by b1 **on average**, holding all other variables constant"
- "The p-value is the probability of observing a test statistic as extreme or more extreme than the one observed, **assuming H0 is true**"
- "We reject/fail to reject H0" (NEVER "accept H0")
- "There is/is not sufficient evidence to conclude..."
- Regression: "predicted" not "actual", "estimated" not "true"
- Effect size: "practical significance" vs "statistical significance"

## Current Plan

See **BATTLE-PLAN.md** for hour-by-hour schedule. Key points:
- Wed June 3: OFF (Quiz 3 today)
- Thu June 5: Full grind — Lec 18-19 (new), Lec 1-5 (gap), Lec 6-11 (fading), cheat sheet v1
- Fri June 6: Full grind — Lec 12-17 reinforcement, 2 practice exams, cheat sheet final
- Sat June 7: Morning review only, exam at 3 PM

## Session Log

| Date | Session | Topics Covered | Notes |
|------|---------|----------------|-------|
| June 3 | 1 | Setup only | Built plan, diagnostics, tracking. Quiz 3 today, no study time. |
