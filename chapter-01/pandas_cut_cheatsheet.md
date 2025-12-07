# 🐼 Pandas `pd.cut()`: The Master Guide

## Part 1: 🔬 Case Study (Taxi Ride Segmentation)

### The Code
```python
pd.cut(s, 
       bins=[0, 2, 10, s.max()], 
       include_lowest=True,
       labels=['short', 'medium', 'long']).value_counts()
```

### 1. The "Plain English" Translation
We are taking the raw list of taxi ride distances, **bucketing** them into three specific categories ("short", "medium", "long"), and then **counting** how many rides fall into each bucket.

### 2. The Visual Anatomy
*   **Subject:** `s` (Our Series of taxi distances).
*   **Verb:** `pd.cut(...)` (The "Cookie Cutter"). This function slices continuous numbers into specific chunks.
*   **Parameters (The Rules):**
    *   `bins=[0, 2, 10, s.max()]`: The cut points.
        *   **Short:** 0 to 2
        *   **Medium:** 2 to 10
        *   **Long:** 10 to Max
    *   `labels=['short', 'medium', 'long']`: The names we give to those chunks.
*   **Chained Action:** `.value_counts()` (The Tally). After we cut them up, we count the pile sizes.

### 3. Syntax Spotlight: `include_lowest=True`
This is the **Safety Net**.
*   **The Problem:** By default, Python intervals are "Left-Exclusive, Right-Inclusive" `(a, b]`. This means `2 < x <= 10`.
*   **The Edge Case:** If you have a ride that is **exactly** 0 miles, the default rule `0 < x <= 2` would kick it out (NaN).
*   **The Fix:** `include_lowest=True` changes the very first interval to be `[0, 2]`, so exact zeros are included.

---

## Part 2: ✂️ Cheatsheet (General Reference)

**Mission:** Turn continuous variables (numbers) into categorical variables (buckets).
**Analogy:** Sorting mail into different slots based on weight.

### 1. The Anatomy
```python
pd.cut(
    x,                          # 1. The Data (Series or List)
    bins=[0, 10, 50, 100],      # 2. The Boundaries (The "Walls")
    labels=['Low', 'Med', 'Hi'],# 3. The Names (Optional)
    include_lowest=True         # 4. The Safety Net
)
```

### 2. The "Interval" Logic (Crucial!)
By default, Pandas uses math notation: `(a, b]`.
*   `(` = **Exclusive** (Greater than `a`)
*   `]` = **Inclusive** (Less than or equal to `b`)

| Bin Config | Math Notation | Logic |
| :--- | :--- | :--- |
| Default | `(0, 10]` | `0 < x <= 10` (0 is excluded!) |
| `include_lowest=True` | `[0, 10]` | `0 <= x <= 10` (0 is included!) |
| `right=False` | `[0, 10)` | `0 <= x < 10` (10 is excluded!) |

### 3. Common Patterns

**A. Custom Buckets (The "Manual" Way)**
You define exactly where the walls are.
```python
# Good for: Grades, Tax Brackets, defined Business Rules
bins = [0, 60, 70, 80, 90, 100]
labels = ['F', 'D', 'C', 'B', 'A']
pd.cut(scores, bins=bins, labels=labels)
```

**B. Equal Width Buckets (The "Automatic" Way)**
Just tell Pandas *how many* buckets you want. It does the math.
```python
# Good for: Histograms, quick distribution checks
# "Split this data into 4 equal-sized chunks"
pd.cut(prices, bins=4)
```

### 4. Pro-Tips 💡
*   **Handling Outliers:** Always use `np.inf` or `s.max()` for your last bin if you want to catch everything.
*   **The `NaN` Trap:** If a value falls *outside* your bins, it becomes `NaN`. Fix this by checking `min()`/`max()` first.
*   **Chaining:** Often paired with `.value_counts()` to see the distribution immediately.
