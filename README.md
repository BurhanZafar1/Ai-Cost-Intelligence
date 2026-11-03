# AI Cost Intelligence

Estimate what each LLM query costs across 10 commercial models before you send it, and route it to the model with the best quality per dollar.

## The problem

Companies that build on large language models pay per token, and list prices for comparable requests differ by more than an order of magnitude between a small model and a frontier model. Teams that send every request to one premium model turn inference into a variable cost that scales with usage and erodes gross margin. The economic question is familiar: for each unit of work, what is the cheapest input that meets the quality bar? Answering it needs a per-query view of unit cost.

## What it does

* Classifies a prompt by task type (code, analysis, summarization and others) and assigns a 1 to 5 complexity score.
* Estimates input and output tokens and prices the request against list prices for 10 models from OpenAI, Anthropic and Google.
* Ranks eligible models by a quality per dollar score and flags the cheapest, fastest and highest quality options.
* Reports the estimated saving against always using the most expensive model in the registry.
* Charts cumulative savings, cost by task type and model mix for the session.
* Optional live mode sends the prompt to a real API and prices the actual token counts.

## How it works

All pricing and routing logic is in `src/lib/pricing.js` and is covered by unit tests.

1. **Token estimate.** About 0.75 words per token for input; output is input times a task-specific ratio (for example 0.3 for summaries, 2.0 for creative writing), with a floor of 50 tokens.
2. **Cost.** `input_tokens / 1e6 * input_price + output_tokens / 1e6 * output_price`, using list prices in USD per million tokens.
3. **Eligibility.** Complexity sets a minimum capability tier, so harder prompts exclude the smallest models.
4. **Ranking.** Each model gets a quality proxy by tier (45 to 95) and a value score of quality divided by cost. Models are sorted by value.
5. **Live mode.** `api/chat.js` is a Vercel Edge Function that forwards the prompt to the Anthropic Messages API and returns real token usage. The dashboard prices that usage at the selected model's rates and shows which model served it. Without a key it falls back to a labelled simulated response.

Stack: React 18, Vite, Recharts and one serverless function.

## Quickstart

```bash
git clone https://github.com/BurhanZafar1/ai-cost-intelligence.git
cd ai-cost-intelligence
npm install
npm test        # unit tests for the pricing and routing logic
npm run dev     # simulation mode at http://localhost:5173
```

For live mode locally, copy `.env.example` to `.env`, add a key, and run `npx vercel dev`. To deploy, import the repo in Vercel and set the same variables in the project settings.

## Configuration

| Variable | Required | Purpose |
|---|---|---|
| `ANTHROPIC_API_KEY` | Live mode only | Key used by `api/chat.js` |
| `LLM_MODEL` | No | Model the proxy calls; defaults to `claude-sonnet-4-20250514` |

## Example output

Produced by `routeToModel()` for the prompt "Write a Python function that merges two sorted lists and explain its time complexity":

```
task=code complexity=1 input_tokens=19 output_tokens=50
GPT-4.1 Nano         $0.000022 Q45
GPT-4o Mini          $0.000033 Q60
GPT-4.1 Mini         $0.000088 Q72
Gemini 2.5 Flash     $0.000131 Q60
...
Claude Opus 4.6      $0.001345 Q95
premium baseline $0.001345; saving vs baseline $0.001323 per query
```

## Limitations and next steps

* Quality scores are a tier-based prior, not benchmark results, so the ranking leans toward cheap models. The example above sends a coding task to the smallest model.
* Task classification is keyword based.
* Token counts are a word-based approximation.
* Prices are hardcoded list prices that should be checked against provider pages; they ignore batch discounts, prompt caching and long-context surcharges.
* Live mode calls one provider and does not yet route across providers.
* The query log lives in browser memory and is seeded with seven illustrative sample rows.

Next steps: calibrate quality scores against a labelled evaluation set, estimate the quality and cost frontier empirically, and add a cascade that escalates to a larger model when a cheap answer fails a check.

## License

MIT. See [LICENSE](LICENSE).
