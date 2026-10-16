# Field-service platform owner feedback (survey)

Owners and operators of trade and home-service businesses react to a
field-service platform brief (`input/context.md`) that separates built features
from missing ones. The 14 questions in `input/questionnaire.yaml` cover:

- **Fit and current tools:** how the respondent relates to service businesses
  and what they use today.
- **Features:** which built feature adds the most revenue and which most
  improves service quality.
- **Switching:** which missing features block a switch, the biggest adoption
  risk and pilot intent.
- **Price:** a fair monthly price.
- **Agreement ratings:** ease, quality impact and revenue impact.
- **Free text:** the one change that would win the switch.

The first question screens respondents. Personas who don't run a service
business answer the rest as advisers to a business of their own company's size,
and analysis should segment on `q_business_fit`.

## Cohorts run on October 16, 2026

| Job | Personas | Selection |
|---|---|---|
| `pg-survey-service-owner-small-n100` | 100 | `persona_strategy.json`: random 100 of the 1,464 Self-employed + Skilled Trades + North America personas, seed 42. 28 answered as owners or solo operators. |
| `pg-survey-service-owner-large-n55` | 55 | Every persona with `company_size` Mid (500-5k) and `region` North America, plus one of two profiles. Profile 1: `seniority` Founder, C-suite, VP or Director, with `domain` Skilled Trades or `ind_construction` / `ind_utilities` / `ind_real_estate` Experienced or Veteran (21). Profile 2: Founder or C-suite with `domain` Business & Management (34). None runs a service company. |

The two profiles of the large cohort don't fit one filter set, so that job's
recipe was assembled from the sampled persona ids and is not reproduced by
`persona_strategy.json`. Both jobs used `persona-json-survey` with
`anthropic/claude-sonnet-4-6` and cost $10.85 together.

## Run

```bash
uv run python application/scripts/generate_application_job.py \
  --task application/tasks/survey_service-owner-platform-feedback \
  --execution-mode auto --dataset persona/datasets/matraix-persona-1m \
  --sample-size 100 --seed 42 --model-name anthropic/claude-sonnet-4-6
uv run matraix run -c <recipe-path-from-script-output> --max-cost-usd 6
```

The rationale summaries in `reporting.json` need an OpenAI reporting model.
Without one, run `report_job.py` with `--no-llm`.
