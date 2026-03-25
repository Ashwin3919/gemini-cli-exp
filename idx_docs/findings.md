# Agent Benchmark Findings

All benchmarks were conducted on a medium-sized repository. Numbers are
per-session averages. Agent 1 is the standard CLI (ReAct loop); Agent 2 is the
experimental G+ReAct fork.

---

## Summary by Task Complexity

| Metric  | Version  | Simple    | Moderate  | Complex     | Tricky      |
| :------ | :------- | :-------- | :-------- | :---------- | :---------- |
| Tokens  | CLI (a1) | 69,997    | 297,610   | 312,525     | 765,210     |
|         | EXP (a2) | 92,984    | 150,158   | 195,794     | 443,550     |
| Reqs    | CLI (a1) | 5         | 15        | 18          | 42          |
|         | EXP (a2) | 6         | 8         | 9           | 24          |
| Wall(s) | CLI (a1) | 99        | 103       | 181         | 354         |
|         | EXP (a2) | 101       | 77        | 98          | 258         |
| Cache%  | CLI (a1) | 32.2%     | 78.3%     | 66.7%       | 73.0%       |
|         | EXP (a2) | 52.0%     | 77.7%     | 57.2%       | 69.1%       |
| Calls   | CLI (a1) | 4         | 14        | 17          | 39          |
|         | EXP (a2) | 4 (G2+O2) | 9 (G7+O2) | 16 (G12+O4) | 27 (G21+O6) |

_EXP calls are listed as graph_calls + other_calls._

---

## Key Numbers

- **Token reduction (moderate+ tasks):** 37–50% fewer tokens. The efficiency
  gain is structural — graph lookups replace iterative grep/read cycles, not
  flat file reads.
- **Simple tasks:** EXP uses ~33% more tokens than CLI on simple tasks. The
  graph index introduces overhead that is not recovered when there is no
  structural navigation to do.
- **Request reduction:** 43–50% fewer model requests on moderate and above tasks
  (e.g. 18 → 9 on complex, 42 → 24 on tricky).
- **Wall time:** Up to 46% faster on complex tasks (181s → 98s). Moderate tasks:
  25% faster (103s → 77s). Simple tasks: near-identical.
- **Tool depth:** EXP used up to 5 distinct tool types vs CLI's 2, while still
  making fewer model requests — graph calls resolve reasoning that would
  otherwise require additional model turns.

---

## Session-Level Observations (4 Rounds)

The rounds below were run on a medium-sized codebase. They informed the summary
table above and provide qualitative context.

| Metric                  | Agent 1 (Vanilla) | Agent 2 (Experimental) |
| :---------------------- | :---------------- | :--------------------- |
| Avg. Total Tokens       | ~291,000          | ~161,000               |
| Avg. Requests           | 16.3              | 8.8                    |
| Avg. Wall Time          | 2m 59s            | 1m 44s                 |
| Avg. Cache Hit Rate     | ~63%              | ~68%                   |
| Tool Variety (max obs.) | 2                 | 5                      |
| Router Cache Hit Rate   | 0%                | 88.5%                  |

### Round 1

| Metric         | Agent 1    | Agent 2            |
| :------------- | :--------- | :----------------- |
| Total Tokens   | 298,626    | 148,018            |
| Requests       | 15         | 8                  |
| Active Time    | 1m 43s     | 1m 17s             |
| Cache Hit Rate | 79%        | 81.2%              |
| Tools Used     | Grep, Read | Graph, Shell, Read |

### Round 2

| Metric         | Agent 1    | Agent 2                  |
| :------------- | :--------- | :----------------------- |
| Total Tokens   | ~300,000   | ~150,000                 |
| Requests       | 16         | 9                        |
| Active Time    | 1m 43s     | 1m 05s                   |
| Cache Hit Rate | 33%        | 56%                      |
| Tools Used     | Grep, Read | Shell, Graph, Grep, Read |

Agent 2 used 36% less active time. Agent 1 made 233K cache reads vs Agent 2's
116K — Agent 1 was re-reading the same files repeatedly.

### Round 3

| Metric       | Agent 1     | Agent 2                   |
| :----------- | :---------- | :------------------------ |
| Total Tokens | 313,341     | 193,175                   |
| Requests     | 19          | 10                        |
| Wall Time    | 3m 52s      | 1m 59s                    |
| Tool Calls   | 17          | 28                        |
| Tools Used   | Shell, Grep | Graph Search, Graph Query |

Agent 2 was approximately 2x faster in wall time despite making more tool calls.
Token savings: ~120,000 tokens per session for the same result. Agent 2's graph
tool calls averaged 30–42ms each at near-zero token cost.

### Round 4

| Metric                | Agent 1  | Agent 2  |
| :-------------------- | :------- | :------- |
| Total Tokens          | ~300,000 | ~154,000 |
| Requests              | 16       | 9        |
| Wall Time             | 2m 08s   | 1m 55s   |
| Avg. Latency          | 6.7s     | 8.4s     |
| Cache Hit Rate        | 79%      | 81.2%    |
| Router Cache Hit Rate | 0%       | 88.5%    |

Agent 1 had lower per-request latency (6.7s) but needed 15 requests to finish vs
Agent 2's 8. Agent 2's 88.5% router cache hit rate means routing decisions are
being reused across sessions, compounding cost savings over time.

---

## Notes

- The clearest win for G+ReAct is on tasks requiring structural understanding:
  dependency navigation, call chain tracing, multi-file impact analysis. On
  these tasks it wins decisively in tokens, requests, and wall time.
- The graph index is a liability on purely simple tasks (few direct reads, no
  navigation). The overhead does not pay off when there are no structural
  lookups to resolve.
- On tasks involving only 4–5 direct file reads with no navigation, Agent 2's
  per-request latency is higher (8.4s vs 6.7s), but it still makes fewer total
  requests.
- All numbers are from a medium-sized repository. Performance characteristics at
  very large scale (tens of thousands of files) have not been validated.
