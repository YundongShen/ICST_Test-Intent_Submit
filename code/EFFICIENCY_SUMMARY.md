# Estimated Inference Consumption per Test Case

Of the 210 test case estimates: **Estimated value for each test case = Total number / 210**. These are amortized averages, not measured test case-level latency or the results of a new run of the current comparison script.

| Metric | DeepSeek-LLM-7B-Chat | Qwen 3.5-27B | Gemini 2.5 Flash | Gemini 3.1 Pro Preview |
|---|---:|---:|---:|---:|
| Deployment | Local, 1 A100 80GB | Local, 1 A100 80GB | Vertex AI API | Vertex AI API |
| Estimated mean elapsed time per case (s) | 16.5 | 696.0 (11.6 min) | 36.5 | 18.2 |
| Estimated mean recorded output tokens per case | 206 | 6,907 (including reasoning) | 137 (visible output only) | 104 (visible output only) |
| Most Aligned cases | 54/210 (25.7%) | 178/210 (84.8%) | 170/210 (81.0%) | 190/210 (90.5%) |
| Illustrative cost per case (USD) | ~$0.0232 | ~$0.9802 | ~$0.00060 + unrecorded thinking | ~$0.00294 + unrecorded thinking |
| Illustrative API cost per case with assumed thinking usage (USD) | Not applicable | Not applicable | ~$0.00410–0.01110 | ~$0.0197–0.0533 |

**Timing.** Qwen's slowness is primarily due to two reasons:

It doesn't use an accelerated inference framework; it uses ordinary HF transformers to generate inference tokens one by one, with fallback warnings in the logs.如果用了加速器可以降低到大约XX

The thought process is lengthy, with 12% of calls being truncated at the 9000-token limit.

Therefore, this only represents the speed under "our deployment configuration."

**Local model.** Based on the on-demand price of approximately $5.07/hour for one A100-80GB GPU on Google Cloud.

We don't actually spend any money running it on Berzelius; this is the cost converted to "renting a cloud GPU".

Cheaper cloud providers offer A100-80GB starting at around $0.89/hour, which would put Qwen at around $36 and DeepSeek at around $0.9.
**API Model.** Pricing:

Gemini-2.5-Flash: Input $0.30/million tokens, Output $2.50/million tokens (Thinking tokens are charged based on the output price);

Gemini-3.1-Pro: Input $2/million tokens, Output $12/million tokens.

The table only calculates visible outputs; thinking tokens were not recorded at the time. Estimating an average of 1,000–3,000 thinking tokens per call (298 suites × 3 times = 294 calls):

Flash total approximately $0.9–2.3;

Pro total approximately $4.2–11.2.

Input tokens are counted uniformly using Qwen's tokenizer; actual Gemini pricing may vary slightly.

**Price sources.**  compiled by third parties; it is recommended to verify with Google's official pricing page before formally citing:
Gemini 3.1 Pro Preview on Vertex – getmaxim.ai
Gemini 2.5 Flash on Vertex – getmaxim.ai
Gemini API pricing – ai.google.dev
A100 80GB pricing – beam.cloud
A100 80GB cloud prices – amcompute.com
GCP GPU pricing – cloud.google.com
