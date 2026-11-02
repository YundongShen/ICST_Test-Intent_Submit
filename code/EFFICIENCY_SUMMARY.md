# Estimated Inference Consumption per Test Case

These estimates allocate the existing totals from 98 source suites across 210 test cases: **per-case estimate = aggregate total / 210**. They are amortized averages, not measured case-level latencies or a new run of the current comparison script. Alignment counts follow [manual_review.xlsx](../Data/Comparison_Results/manual_review.xlsx).

| Metric | DeepSeek-LLM-7B-Chat | Qwen 3.5-27B | Gemini 2.5 Flash | Gemini 3.1 Pro Preview |
|---|---:|---:|---:|---:|
| Deployment | Local, 1 A100 80GB | Local, 1 A100 80GB | Vertex AI API | Vertex AI API |
| Estimated mean elapsed time per case (s) | 16.5 | 696.0 (11.6 min) | 36.5 | 18.2 |
| Estimated mean recorded output tokens per case | 206 | 6,907 (including reasoning) | 137 (visible output only) | 104 (visible output only) |
| Most Aligned cases | 54/210 (25.7%) | 178/210 (84.8%) | 170/210 (81.0%) | 190/210 (90.5%) |
| Illustrative cost per case (USD) | ~$0.0232 | ~$0.9802 | ~$0.00060 + unrecorded thinking | ~$0.00294 + unrecorded thinking |
| Illustrative API cost per case with assumed thinking usage (USD) | Not applicable | Not applicable | ~$0.00410–0.01110 | ~$0.0197–0.0533 |

**Timing and tokens.** Elapsed time includes the loading and waiting overhead present in the original run totals. Suite-level medians are not converted into case-level medians. API thinking tokens were not recorded; approximately 177,000 API input tokens were estimated with Qwen's tokenizer rather than provider billing metadata. Output counts therefore have different coverage across models.

**Cost assumptions.** Local runs used allocated GPU resources without a per-run charge. Local dollar amounts are hypothetical GPU rental equivalents at $5.07 per GPU-hour, excluding host and storage costs. API estimates use input/output rates per million tokens of $0.30/$2.50 for Flash and $2/$12 for Pro, with thinking billed as output. The last row assumes 1,000–3,000 thinking tokens for each of the 294 underlying suite-level calls, then allocates the resulting cost over 210 cases; it does not assume 630 new calls. These are estimates, not invoices. Rates: [Google Cloud pricing](https://cloud.google.com/vertex-ai/generative-ai/pricing).

**Scope.** These averages describe the recorded deployment: standard Hugging Face Transformers generation, long Qwen reasoning outputs, and reported DeepSeek tokenization/whitespace problems. They do not establish intrinsic model speed or a general cost advantage for local or API deployment.
