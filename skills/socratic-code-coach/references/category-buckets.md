# Coding Category Buckets, Compound Synthesis & Weakness Tracking

This reference defines the combinatorial puzzle generation engine and weakness-targeting mechanics for `socratic-code-coach`.

---

## 1. The Pure-to-Applied Problem Spectrum

Do not overload learners with story-heavy word problems before they master the underlying mathematical invariant. Scale real-world context gradually:

| Stage | Framing Style | Purpose | Example |
| :--- | :--- | :--- | :--- |
| **Stage 1: Clean-Room** | Raw numbers / arrays / chars | Zero narrative noise. Pure invariant isolation. | *"Find the length of the smallest contiguous subarray whose sum $\ge 7$ in `[2, 3, 1, 2, 4, 3]`."* |
| **Stage 2: Tangibles** | Physical everyday objects | Bridges abstract pointers to spatial human intuition. | *"You have a row of colored cards. Find the shortest strip containing all 3 primary colors."* |
| **Stage 3: Applied Real-World** | Production systems & streams | Teaches filtering noisy requirements into formal constraints. | *"Financial order stream: detect the shortest burst of transactions whose aggregate volume breaches the fraud threshold."* |
| **Stage 4: Compound Synthesis** | Fused multi-bucket problems | Teaches two-phase problem decomposition (Subproblem A $\rightarrow$ Subproblem B). | *"Streaming trade engine: find the rolling median (Bucket 3: Heaps) across an expiring sliding window (Bucket 1: Windowing)."* |

---

## 2. Weakness Heatmap & Procedural Re-Skinning

When a learner struggles with an invariant, the coach registers a high `pressure` score in `.agent/learner-profile.json`.

To re-test the weakness without repeating the problem, the mentor shifts the **data representation** and **objective framing** while keeping the **algorithmic invariant identical**:

### Re-Skinning Matrix: Example (Invariant: Dynamic Window Contraction)

| Exposure | Stage | Data Type | Framing | The Invariant Being Drilled |
| :--- | :--- | :--- | :--- | :--- |
| **Try 1** | Stage 1 | Integer Array | Smallest subarray sum $\ge K$ in `[2, 1, 5, 2, 3]` | Contraction: `while (curr_sum >= K) left++` |
| **Try 2** | Stage 1 | String | Shortest substring containing all vowels in `"axyebiuo"` | Contraction: `while (has_all_vowels) left++` |
| **Try 3** | Stage 2 | Cards / Objects | Minimum hand of cards containing at least 2 Aces | Contraction: `while (ace_count >= 2) left++` |
| **Try 4** | Stage 3 | Applied Stream | Shortest window of log events containing error types A, B, and C | Contraction: `while (has_all_errors) left++` |

The learner feels they are solving completely distinct puzzles, but the underlying neural pathway (when and how to shrink the left boundary) is systematically reinforced.

---

## 3. Compound Pattern Recipes (Multi-Bucket Fusions)

At Level 4–5, mentor trains **Decomposition Fluency** by pairing two complementary buckets:

### Recipe A: Windowing + Monotonic Queue (`bucket-stream-window` + `bucket-state-parser`)
- **Concept**: Finding sliding window maximums/minimums in $O(N)$ amortized time.
- **Decomposition**:
  - Subproblem 1: Maintain window boundaries $[L, R]$.
  - Subproblem 2: Maintain monotonic decreasing deque of indices to achieve $O(1)$ max queries.

### Recipe B: Interval Scheduling + Priority Queue (`bucket-intervals-ordering` + `bucket-lowlevel-memory`)
- **Concept**: Meeting room allocation / thread pool scheduling.
- **Decomposition**:
  - Subproblem 1: Sort intervals by start time.
  - Subproblem 2: Min-heap tracks earliest releasing room/core.

### Recipe C: Graph State + Dynamic Programming (`bucket-graph-traversal` + `bucket-dynamic-recurrence`)
- **Concept**: Shortest path with at most $K$ stops / flight booking with layover budgets.
- **Decomposition**:
  - Subproblem 1: Model state as `(node_id, stops_remaining)`.
  - Subproblem 2: Bellman-Ford or memoized Dijkstra on state graph.
