# MarketPulse AI

**Where are Australian businesses growing and disappearing  and can an AI explain it like a consultant would?**

This project analyses business entry and exit data across every Australian industry and region, then layers an AI on top that turns the findings into a consulting-style memo. It's an end-to-end pipeline: from raw open government data, to clear analysis, to a written recommendation a non-technical reader can act on.

---

## The headline findings

- **Most industries are growing.** Across 2024–25, only two of nineteen industries had more businesses leaving than joining  agriculture and retail  and even those only marginally. This isn't a story of decline.
- **The national picture holds up locally.** An industry's national trend and its average performance across local areas correlate at **+0.9**  the national signal is trustworthy, not an average masking a messier regional reality.
- **Tasmania is the state under most pressure**, with roughly **49%** of its local industry segments contracting  far more than anywhere else, and a finding a national headline number would completely hide.

---

## What makes this different

Most analytics projects stop at "here's my model" or "here's a chart." Real consulting work ends with a *recommendation*. So the final step here is an AI layer (Llama 3.3 70B via the Groq API) that reads the analysis output and drafts a structured memo  Situation, Key Insights, Recommendation.

The key design choice: **the model only receives the real figures the analysis produced.** It narrates the data; it doesn't invent it. That guardrail is what separates a credible tool from a chatbot guessing at numbers.

---

## The pipeline

1. **Clean**  the ABS spreadsheets aren't analysis-ready (years stacked as labelled blocks, state-totals mixed into regional rows). Reshaped into tidy panels.
2. **Industry analysis**  entry vs exit rates by industry, a "net churn rate" (exits minus entries), and four-year trends.
3. **Regional analysis**  business-count change year-on-year for each Local Government Area × industry, rolled up to state level.
4. **Cross-check**  testing whether national industry trends actually hold at the regional level.
5. **AI memo**  feeding the verified findings to an LLM to generate a consulting briefing.

---

## Tech stack

- **R** + **tidyverse** for cleaning and analysis
- **ggplot2** for visualisation
- **Quarto** for the reproducible report
- **Groq API** (Llama 3.3 70B) for the AI memo layer
- Entirely free and open-source tooling

---

## Repository structure
marketpulse-ai/

|--  01_cleaning_eda.qmd      # Full analysis + AI memo (technical)

|-- 02_report.qmd            # Reader-facing report (no code)

|--  data/                    # Raw ABS data cubes

│   |--  8165DC01.xlsx        # Industry-level entries/exits

│   |--  8165DC10.xlsx        # Business counts by LGA

|--  outputs/                 # Cleaned CSVs (generated on render)


## How to run it

1. Clone the repo and open `marketpulse-ai.Rproj` in RStudio.
2. Install packages: `install.packages(c("tidyverse", "readxl", "ggrepel", "httr2", "knitr"))`
3. For the AI memo, get a free key at [console.groq.com](https://console.groq.com) and add it to your `.Renviron`:GROQ_API_KEY=your_key_here
,Restart R after saving.
4. Render `01_cleaning_eda.qmd` (the AI memo chunk is set to run manually so it doesn't call the API on every render).

Built by Tushar Dhanorkar  Master of Business Analytics
