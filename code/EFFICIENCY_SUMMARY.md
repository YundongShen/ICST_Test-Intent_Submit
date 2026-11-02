# Inference Efficiency Summary

Reported Independent Extraction timings are aggregated over 98 source suites, with three element-extraction calls per suite. Alignment counts come from the 210 cases in [manual_review.xlsx](../Data/Comparison_Results/manual_review.xlsx). The timing unit is a suite, not a case; these figures are not per-case latency measurements of the current comparison runner.

| Metric | DeepSeek-LLM-7B-Chat | Qwen 3.5-27B | Gemini 2.5 Flash | Gemini 3.1 Pro Preview |
|---|---:|---:|---:|---:|
| Deployment | Local, 1 A100 80GB | Local, 1 A100 80GB | Vertex AI API | Vertex AI API |
| Median elapsed time per suite (s) | 26 | 1,476 | 76 | 38 |
| Median excluding reported loading or fixed waiting overhead (s) | 25 | 1,464 | 61 | 23 |
| Sum of suite elapsed times (hours) | 0.96 | 40.6 | 2.13 | 1.06 |
| Median recorded output tokens per suite | 184 | 14,551 (including reasoning) | 264 (visible output only) | 208 (visible output only) |
| Total recorded output tokens | 43,168 | 1,450,441 | 28,872 | 21,904 |
| Most Aligned cases | 54/210 (25.7%) | 178/210 (84.8%) | 170/210 (81.0%) | 190/210 (90.5%) |
| Illustrative total cost (USD, all 98 suites) | ~$4.87 | ~$205.84 | ~$0.13 + unrecorded thinking | ~$0.62 + unrecorded thinking |
| Illustrative cost per suite (USD) | ~$0.050 | ~$2.10 | ~$0.0013 + thinking | ~$0.0063 + thinking |
| API cost scenario with 1,000–3,000 thinking tokens per call (USD, total) | Not applicable | Not applicable | ~$0.86–2.33 | ~$4.14–11.20 |

**Timing and tokens.** Adjusted local times exclude reported model-loading overhead; adjusted API times remove the fixed 15-second wait across three calls. API thinking tokens were not recorded. API input counts use approximately 177,000 tokens estimated with Qwen's tokenizer, not provider billing metadata. Token counts therefore do not have a uniform basis across models.

**Cost assumptions.** Local runs used allocated GPU resources without a per-run charge. The local dollar figures are hypothetical GPU rental equivalents at an assumed $5.07 per GPU-hour, excluding host and storage costs. API estimates use standard input/output rates per million tokens: $0.30/$2.50 for Flash and $2/$12 for Pro, with thinking billed as output. The last row assumes 294 calls (98 × 3); its thinking-token range is an illustration, not a measurement or invoice. Rates: [Google Cloud pricing](https://cloud.google.com/vertex-ai/generative-ai/pricing).

**Interpretation.** In these reported runs, Qwen's aggregate elapsed time was about 38 times Pro's, while Pro had 12 more Most Aligned cases (5.7 percentage points). Local timings reflect standard Hugging Face Transformers generation and Qwen's long reasoning outputs; the DeepSeek run also had reported tokenization/whitespace problems. These observations describe this deployment. They do not establish intrinsic model speed, actual API spending, or a general cost advantage for either deployment method.
