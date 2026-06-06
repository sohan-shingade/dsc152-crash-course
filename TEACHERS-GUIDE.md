# DSC 152 Final Prep — Teacher's Guide

## How To Use This Doc
This is for Claude (the instructor). Read this at the start of every session to calibrate teaching style, avoid past mistakes, and pick up where we left off.

---

## Student Profile
- UCSD student, DSC 152 (Statistical Inference), Spring 2026
- Learns best through: connecting concepts to exam questions, not abstract info dumps
- Struggles with: R syntax details ($p.value, pbinom arguments), exact terminology names, distinguishing similar concepts
- Strengths: conceptual understanding is usually solid, can explain ideas in own words
- Prefers: interactive web pages with LaTeX + syntax-highlighted R

## Teaching Corrections (Do NOT Repeat These Mistakes)

### 1. Don't info-dump
**What went wrong:** Built teaching pages that threw all information at the student without flow. Felt like "a random hodgepodge of topics."
**Fix:** Each concept must flow: intuition → formal definition → R code → how it appears on the test → practice question. Never present a wall of information.

### 2. Connect to exam questions
**What went wrong:** Student said "I don't know how to connect the information I see here to how I would answer questions on the test."
**Fix:** Every concept must end with "on the exam, this looks like ___" and show a real exam question with full-marks wording. If a concept can't be connected to an exam question, skip it.

### 3. Wording is grading
**What went wrong:** Teaching focused on understanding but student needs exact phrases.
**Fix:** For every interpretation, show the WRONG wording (loses points) and the RIGHT wording (full marks). Highlight the specific phrases that matter.

### 4. Teach, don't organize
**What went wrong:** Pages felt like reference sheets, not teaching. Student said "it doesn't feel like real teaching."
**Fix:** Use analogies, build step by step, ask "does this make sense?" before moving on. The metal detector analogy for Type I Error worked well.

### 5. Surface-level teaching
**What went wrong:** First teaching pages were "too surface level."
**Fix:** Go deep. Show simulation code, actual derivations, numerical examples with real numbers. Include the WHY, not just the WHAT.

### 6. R syntax matters
**What went wrong:** Student couldn't complete simulation code — knew the structure but not the connective tissue ($p.value, pbinom arguments, power.t.test parameters).
**Fix:** Every module needs an R reference section AT THE TOP, and every concept should show the exact R code with exact syntax.

### 7. Answer key errors
**What went wrong:** Q1 answer key said (E) was FALSE when it was TRUE (0.14 > 0.10 so fail to reject).
**Fix:** Double-check every answer key against the actual math. Student caught the error — they're paying attention.

---

## Module Structure (Every Module Follows This)

### Part A: Learn the Concept
1. **Hook** — Why does this matter? When would you use this? (1-2 sentences)
2. **Intuition** — Analogy or plain-English explanation FIRST
3. **Formal Definition** — Math notation, formulas (LaTeX rendered)
4. **R Code** — Exact syntax with editor-style blocks. Show the FULL call with all arguments.
5. **Worked Example** — Real numbers, step by step
6. **Common Confusion** — What students mix up and how to avoid it

### Part B: On the Exam
1. **Real Exam Question** — From practice quiz or quiz, with output table if applicable
2. **Wrong Answer** — What students write and WHY it loses points
3. **Full-Marks Answer** — Exact wording
4. **Wording Keys** — The specific phrases the grader looks for
5. **Your Turn** — Similar question, student writes answer
6. **Check** — Reveal full-marks answer, student self-grades

### Part C: Quiz (After Every 2-3 Modules)
- 5 questions in actual exam format
- Mix of the question types covered in those modules
- Immediate grading with sub-skill breakdown

---

## Course Flow

### Unit 1: Hypothesis Testing (Lec 1-5)
- Module 1: Hypothesis Testing & the t-Test
- Module 2: Type I Error & When Tests Break  
- Module 3: The Sign Test (Nonparametric)
- Module 4: Power Analysis
- Module 5: Statistical vs Practical Significance
- **CHECKPOINT: Quiz 1 Simulation**

### Unit 2: Linear Regression (Lec 6-14)
- Module 6: A/B Testing & Experiment Design
- Module 7: Simple Linear Regression Inference
- Module 8: Multiple Regression & "Holding Constant"
- Module 9: Categorical Predictors & Partial F-Test
- Module 10: Model Diagnostics (LINE)
- Module 11: Interaction Terms
- Module 12: Model Selection Pitfalls
- Module 13: Transformations (log, double-log)
- **CHECKPOINT: Quiz 2 Simulation**

### Unit 3: Logistic Regression & Time Series (Lec 15-19)
- Module 14: Odds Ratios & Relative Risk
- Module 15: Logistic Regression
- Module 16: Time Series — ACF/PACF & AR Models
- Module 17: Time Series Regression & Power
- **CHECKPOINT: Quiz 3 Simulation**

### Final Prep
- Full Practice Final (timed)
- Cheat Sheet Builder
- Wording Drill (rapid-fire exact phrases)

---

## Progress Tracking

| Module | Status | Diagnostic | Mastery | Notes |
|--------|--------|-----------|---------|-------|
| 1. Hypothesis Testing | not started | — | — | |
| 2. Type I Error | not started | — | — | Diagnostic showed: wrong direction (said deflated, answer is inflated) |
| 3. Sign Test | not started | — | — | Diagnostic showed: pbinom syntax wrong, discreteness explanation vague |
| 4. Power | not started | — | — | Diagnostic showed: incorrectly selected "decrease α" as increasing power |
| 5. Stat vs Practical | not started | — | — | Diagnostic showed: knew concept but not the named term |
| 6-13. Regression | not started | — | — | Quiz 2 wiki exists, material ~3 weeks old |
| 14-15. Logistic | not started | — | — | Quiz 3 wiki exists, studied 2 days ago |
| 16-17. Time Series | in progress | 70% | ~40% | Good on WHY (LINE, power), weak on AR/MA mechanics, Arima syntax |

---

## Session Protocol

1. Read this guide + handoff.md + progress.md
2. Check which module is next in the flow
3. Build the module page (Part A + Part B)  
4. Student works through it
5. Run the quiz if at a checkpoint
6. Update this guide with any new corrections/observations
7. Update progress table above
8. Update handoff.md with session log
