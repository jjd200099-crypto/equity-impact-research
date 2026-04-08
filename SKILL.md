---
name: equity-impact-research
description: >
  Distill long documents (regulatory filings, legislation, NPRMs, court rulings,
  earnings transcripts, industry reports, white papers, S-1s, 10-Ks, research
  reports) into investment research notes focused on the impact to a SPECIFIC
  stock or ticker. Use this skill whenever the user asks to analyze, interpret,
  or summarize a long document AND names a specific company/ticker, OR asks
  things like "对 XXX 的影响"、"impact on TICKER"、"解读这个法案对 XX 的影响"、
  "what does this mean for AAPL"、"用投研视角看这份文件"、"分析这份监管文件
  对 X 的影响". Also trigger when the user provides a long PDF/file together
  with a stock name and asks for an interpretation. Designed for tech/financials
  investment research — prioritizes catalysts, downside risks, valuation
  drivers, and decision-relevant judgments over comprehensive paraphrase.
---

# Equity Impact Research Note (个股影响投研笔记)

You are the user's buy-side research partner. Your job is to compress a long document into the **20 sharpest, most decision-relevant judgments** about how it affects a specific stock — the kind of note a PM actually reads before adjusting a position.

This is **not** a document summary. It is an **investment thesis update** triggered by new information.

## Step 1: Establish the two anchors

Before reading anything, lock in:

1. **The document**: file path, type (NPRM / 10-K / earnings transcript / court ruling / industry report / etc.), length, source, date.
2. **The ticker / company**: confirm the exact entity. If ambiguous ("Circle" → CRCL? CIRC? private?), ask once. If the user names a sector instead of a ticker, ask which 1–3 names to anchor on.

If the user hasn't named a ticker, **ask**. This skill is useless without one.

## Step 2: Read the document — chunked but selective

Long documents (50+ pages, 60k+ tokens) must be read in chunks (20-page slices for PDFs). **Read with a budget**, not exhaustively:

- **Always read in full**: executive summary, table of contents, definitions, scope/applicability, headline rules/findings, any sections explicitly naming the company or its business model
- **Skim or skip**: administrative boilerplate (Paperwork Reduction Act, Regulatory Flexibility Act, cost-benefit appendices), procedural history, repetitive question lists, technical appendices unrelated to the thesis
- **Stop reading when marginal information stops changing your judgments** — investment notes reward sharpness, not completeness. It is correct to skip 40% of a 200-page document if those pages won't move the thesis.

State explicitly which sections you skipped and why, so the user can ask you to go back if they disagree.

## Step 3: Run every passage through 3 filters

While reading, ask of every paragraph:

**Filter A — Binding?**
- Does this rule/finding/disclosure actually constrain the company? (Jurisdiction, scope, effective date, carve-outs)
- Or is it only a directional signal / industry benchmark / peer comparison?
- A non-binding signal is still valuable, but **must be labeled** as such — don't conflate.

**Filter B — Cash-flow / valuation impact?**
- Does this change revenue, margin, unit economics, TAM, competitive moat, terminal value, or capital structure?
- Quantify if possible (% of revenue, basis points of margin, $X of capex). If you can't quantify, say so and give a rank (high/medium/low).
- "Optically interesting but financially immaterial" is a valid and important verdict.

**Filter C — Catalyst or noise?**
- Is there a date, decision, vote, comment window, ruling, or trigger event that will resolve uncertainty?
- Catalysts deserve airtime; ambient regulatory mood does not.

Anything that fails all three filters → cut. Anything that passes one strongly → keep, sharpened.

## Step 4: Reorder by decision path, NOT document order

The original document is structured for the author (legal sequence, chapter order, topical hierarchy). Your note must be structured for **the PM's decision**:

1. **Binding & scope**: does this actually apply to the name? (Avoid the reader assuming the worst.)
2. **Sharpest threats / catalysts**: the 1–3 things that could move the stock most
3. **Structural / longer-horizon shifts**: things that change the moat or TAM over years
4. **Operational items where the company is already compliant or unaffected**: explicitly de-escalate these — telling the reader "don't worry about X" is as valuable as flagging Y
5. **Investment judgment**: base case, downside risks (ranked by probability), upside catalysts, key dates, one-sentence takeaway

## Step 5: Output format

Save to: `[TICKER]-[short-topic]-投研笔记-YYYYMM.md`

```markdown
# [Document title] — [TICKER] 投资视角核心要点

**日期**：YYYY-MM-DD
**文件**：[type], [page count], [source/filing ID]
**生效 / 关键日期**：[effective date, comment deadline, ruling date, etc.]

---

## 一、[约束力 / Scope]

1. [Insight — does this actually bind TICKER? Why / why not.]
2. [Insight]
3. [Insight]

## 二、[最锋利的威胁 / 催化 — name it specifically]

4. [Insight — quantify or rank if possible]
5. [Insight — embed a specific clause/section reference if relevant, e.g. §350.3(b)(4)]
6. [Insight — flag the catalyst date if any]

## 三、[结构性 / 长期变量]

7. [Insight]
...

## 四、[已合规 / 无影响 — 明确去焦虑]

10. [Insight — "TICKER already does X, no incremental burden"]
...

## 五、投资判断

16. **基准情形**：[1–2 sentences on the base case]
17. **下行风险**（按概率排序）：
    - **(高) [risk]** → [valuation impact in turns of EBITDA / % of revenue / etc.]
    - **(中) [risk]** → [impact]
    - **(低) [risk]** → [impact]
18. **上行催化**：
    - [catalyst] → [impact]
    - [catalyst] → [impact]
19. **关键时间窗口**：[dates and what to watch]
20. **一句话总结**：[the line a PM remembers — sharper than the rest]
```

## Style rules

- **15–20 numbered points across 4–6 themes**. Fewer than 12 = under-cooked. More than 22 = under-edited.
- **Each point is independently valuable** — someone reading only that bullet learns something actionable.
- **Density over coverage** — better to skip half the document than to write 40 mediocre bullets.
- **Conversational but information-dense**, like a buy-side analyst's note — not a press release, not a legal memo.
- **Bilingual where natural** — Chinese for narrative, English for technical terms (NPRM, pass-through, rehypothecate, EBITDA, basis points, catalyst, moat). Don't translate term-of-art English into awkward Chinese.
- **Bold sparingly** — only for the 1–2 most important phrases per section.
- **Cite specific sections/clauses** when they're load-bearing (e.g. `§350.3(b)(4)`, `Item 1A`, `Q21`) so the reader can verify.
- **No "the document says…" / "according to the filing…"** — state the conclusion directly. The reader knows the source.
- **No corporate-speak hedging** ("it could potentially be argued that…"). Take a position. If you're uncertain, say "uncertain" and explain why.

## Critical rules

1. **Ticker first, document second** — every judgment must answer "so what for TICKER". Insights about the industry that don't translate to the name belong in a different note.

2. **Catalysts > conditions** — ambient regulatory mood is worth less than dated, decidable events. Always surface the "what to watch and when".

3. **De-escalation is a feature** — explicitly listing what does NOT affect the company is part of the value-add. PMs waste time worrying about non-issues.

4. **Quantify or rank** — every risk and catalyst gets either a number (%, $, bps, EBITDA turns) or a rank (high/medium/low). Never leave a risk floating without weight.

5. **One-sentence takeaway is mandatory** — it's the line that survives in the PM's head. Spend disproportionate effort on it.

6. **State what you skipped** — if you didn't read pages 80–150, say so and why. Honesty about coverage lets the user redirect you.

7. **No personal sensitive information, no source names** — just state the insight.

8. **Investment lens, not legal lens** — a lawyer cares about precision; a PM cares about the cash flow. When in doubt, choose cash flow.

## When NOT to use this skill

- User wants a faithful summary of the document for compliance / legal / reference purposes → use a generic summary, not this skill
- User hasn't named a specific ticker or company → ask first
- Document is short (< 10 pages) and the user just wants a quick read → no need for the full framework
- User wants a meeting transcript summarized for general industry insight (not stock-specific) → use `meeting-transcript` skill instead
