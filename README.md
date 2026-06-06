# DSC 152 Crash Course

Interactive study system for UCSD's DSC 152 (Statistical Inference) final exam.

**Live site:** [dsc152-crash-course.vercel.app](https://dsc152-crash-course.vercel.app)

## What's inside

- **17 modules** covering Lectures 1-19 (hypothesis testing through time series)
- **Quiz 1 checkpoint** with auto-grading and skill breakdown
- **Lab 9 review** (professor confirmed this appears directly on the final)
- **Final exam simulator** — 15 questions, 3-hour timer, auto-graded

Each module follows: **Learn** (intuition, formulas, R code) → **On the Exam** (real quiz questions with full-marks wording) → **Quiz**

## Topics covered

| Unit | Modules | Topics |
|------|---------|--------|
| 1 | 1-5 | Hypothesis testing, Type I Error, sign test, power, stat vs practical significance |
| 2 | 6-13 | A/B testing, simple/multiple regression, categorical predictors, LINE diagnostics, interactions, model selection, transformations |
| 3 | 14-17 | Odds ratios, logistic regression, time series (ACF/PACF, AR models, Arima regression) |
| Final | Lab 9 + Simulator | Model selection vs inference (Lucas 2013), full practice final |

## Tech

Static HTML with KaTeX (math), Prism.js (R syntax highlighting), localStorage (progress tracking). No build step.
