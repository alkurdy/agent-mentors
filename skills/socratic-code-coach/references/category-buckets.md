# Coding Category Buckets & Procedural Puzzle Generator

This reference powers the infinite puzzle generator in `socratic-code-coach`. It prevents repetitive "LeetCode clone" puzzles by combining an algorithmic archetype with a real-world engineering domain and a mechanical twist.

---

## 1. The 7 Core Category Buckets

### Bucket 1: Linear Scans & Windowing (`bucket-stream-window`)
- **Key Concepts**: Two pointers, sliding window (fixed & dynamic), prefix sums, monotonic queues.
- **Probe (Tier 1)**: Find contiguous subarray of length $K$ with target condition.
- **Escalation (Tier 3-4)**: Unbounded incoming data stream where elements expire after $T$ seconds; find max within rolling window in $O(1)$ amortized time.

### Bucket 2: Associative Maps & Frequency Filtering (`bucket-hash-frequency`)
- **Key Concepts**: Hash maps, hash sets, frequency tables, bucket sort, bitmasks.
- **Probe (Tier 1)**: First unique item in collection; frequency count thresholding.
- **Escalation (Tier 3-4)**: Multi-key indexing with tie-breakers, rolling collision-free deduplication over large streams.

### Bucket 3: Ordering, Intervals & Partitioning (`bucket-intervals-ordering`)
- **Key Concepts**: Interval merging/splitting, binary search on solution space, heap priority queues, Dutch National Flag partitioning.
- **Probe (Tier 1)**: Merge overlapping intervals; binary search on rotated array.
- **Escalation (Tier 3-4)**: Resource allocation with setup/cooldown times; $K$-way stream merge with dynamic priority recalculation.

### Bucket 4: State Machines & Stream Parsers (`bucket-state-parser`)
- **Key Concepts**: Finite State Machines (FSM), monotonic stacks, recursive descent parsing, bracket validation.
- **Probe (Tier 1)**: Simple expression evaluator; validating balanced brackets.
- **Escalation (Tier 3-4)**: Stateful protocol tokenizer supporting escape sequences, comments, and nested dictionary structures without regex.

### Bucket 5: Tree, Graph & State-Space Exploration (`bucket-graph-traversal`)
- **Key Concepts**: BFS/DFS, topological sort (Kahn's), cycle detection, Dijkstra/A*, bipartite checks.
- **Probe (Tier 1)**: Tree traversal; dependency resolution between build tasks.
- **Escalation (Tier 3-4)**: Shortest path with node cooldowns; detecting circular dependencies across dynamically loaded plugins.

### Bucket 6: Recurrence & Dynamic Optimization (`bucket-dynamic-recurrence`)
- **Key Concepts**: Memoization, 1D/2D tabulation, decision trees, unbounded knapsack, state-compression bitmasks.
- **Probe (Tier 1)**: Min-cost path across 2D grid; climbing stairs variation.
- **Escalation (Tier 3-4)**: Stock trading with cooldown and transaction fees; longest common subsequence with wildcards.

### Bucket 7: Systems & Memory Ergonomics (`bucket-lowlevel-memory`)
- **Key Concepts**: Fixed-size ring buffers, LRU/LFU caches, zero-copy byte slicing, bitwise flags, cache alignment.
- **Probe (Tier 1)**: Bounded FIFO queue backed by fixed array.
- **Escalation (Tier 3-4)**: $O(1)$ LRU Cache with TTL expiry; bit-packed permission bitsets for 10 million users.

---

## 2. The 3D Combinatorial Generation Formula

To generate a fresh, never-before-seen puzzle, sample one element from each dimension:

$$\text{Puzzle} = \text{Bucket Archetype} \times \text{Domain Scenario} \times \text{Mechanical Twist}$$

### Dimension A: Real-World Scenarios (Domain Skins)
1. **Financial Exchange**: High-frequency order books, trade settlement, matching engines.
2. **Space & Telemetry**: Satellite sensor bursts, packet loss recovery, orbit interval calculations.
3. **Bioinformatics**: DNA sequence alignment, protein folding repeats, genomic mutation scanning.
4. **Operating Systems**: Thread scheduling, page replacement, disk block allocation, memory compaction.
5. **Logistics & Warehousing**: Autonomous drone routes, warehouse parcel packing, truck dispatching.
6. **Game Engine**: Physics collision sweeps, player leaderboard rankings, visibility field-of-view.
7. **Streaming Media**: Audio buffer sync, subtitle alignment with variable framerate, bit-rate switching.
8. **E-Commerce Flash Sale**: Inventory reservation locks, shopping cart expiration, discount stacking.

### Dimension B: Mechanical Twists (Constraints)
1. **The Memory Squeeze**: Must solve in $O(1)$ auxiliary space (in-place manipulation).
2. **The Streaming Unbound**: Input does not fit in memory; data arrives one item at a time via generator/iterator.
3. **The Poison Pill**: Stream contains intentionally corrupted/malformed packets that must be ignored or quarantined without crashing.
4. **Single-Pass Constraint**: Must compute answer in strictly one pass over the data ($O(N)$ time).
5. **No Built-in Helpers**: Cannot use language sorting (`.sort()`), regex, or collection libraries.
6. **Concurrent Mutations**: State may be updated while reads are active (thread-safe invariant).

---

## 3. Progression & Escalation Workflow

```text
[Select Bucket] ──> [Generate Tier 1 Probe]
                           │
                 Did user solve cleanly?
                 ├── YES (< 5m, clean PCDIT):
                 │     └── Inject Twist OR escalate to Tier 2/3
                 └── NO (Syntax freeze / algorithm breakdown):
                       └── Engage PCDIT Socratic hand-trace
```

### Example Evolution (Bucket 1: Windowing)

1. **Tier 1 (Probe)**:
   *"Given an array of server latency measurements, find the maximum average latency across any consecutive 5-minute window."*
2. **Tier 2 (Progression)**:
   *"Satellite link telemetry: timestamps are out of order by up to 3 seconds. Find the highest error spike in a rolling 10-second window."*
3. **Tier 3 (Twist Injection)**:
   *"Stock exchange ticker: unbounded memory stream. Maintain the median price over the last 1,000 trades in $O(\log K)$ time per trade."*
4. **Tier 4 (Compound)**:
   *"Combine with Bucket 5 (Graph): A network of 500 edge routers. Find the path from Router A to B whose sliding-window packet loss over the last 60 seconds never exceeds 2%."*
