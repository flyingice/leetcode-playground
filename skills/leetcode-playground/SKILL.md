---
name: leetcode-playground
description: "Practice LeetCode problems interactively. Use when the user wants to solve, practice, or work on a LeetCode problem by number — e.g. 'leetcode 42', 'solve problem 121', 'practice two sum'. Sets up the problem description, creates a solution file in the user's preferred language, and validates the solution when ready."
argument-hint: <number>
---

# LeetCode Playground

Help the user practice a LeetCode problem interactively: show the problem, let them code, then validate.

## Step 1: Parse the question number

Extract the LeetCode question number from the arguments. If no number is provided, ask the user for one.

## Step 2: Show the problem

Recall the problem from your training knowledge. Display:

- **Title** and **number**
- **Difficulty** (Easy / Medium / Hard)
- **Problem description** — the full statement including constraints and examples exactly as they appear on LeetCode
- **Tags** (Array, DP, Tree, etc.)

Do NOT show hints, approaches, or solution ideas. The user is here to solve it themselves.

## Step 3: Determine the language

Check whether the user has previously stated a language preference in this conversation. If not, ask which language they'd like to use.

| Language   | Extension |
|------------|-----------|
| C++        | .cpp      |
| Java       | .java     |
| Python     | .py       |
| Go         | .go       |
| Rust       | .rs       |
| JavaScript | .js       |
| TypeScript | .ts       |
| C          | .c        |
| C#         | .cs       |
| Kotlin     | .kt       |
| Swift      | .swift    |

## Step 4: Prepare the solution file

1. Ensure `~/leetcode-playground/` exists (create it if not).
2. Check if `lc-<number>.<ext>` already exists in that directory.
   - **New file**: pre-fill with only the function/method skeleton — the same signature you'd see in the LeetCode editor. No includes, imports, headers, or `using` statements.
   - **Existing file**: the user has attempted this problem before. Read the file and check whether the most recent function/method body contains meaningful logic (not a stub like `return 0;`, `pass`, or an empty body). If it does, append a new skeleton with a suffixed name (e.g. `twoSum2`, `twoSum3` — pick the next available number). If it doesn't, leave the file as-is. Do NOT touch existing code in either case.
3. Tell the user the file path, then use `AskUserQuestion` with a pre-filled "Done" option so the user can signal when they've finished writing their solution.

## Step 5: Validate the implementation

Once the user signals they are done, read the file and validate.

### 5a. Compilation check

Verify the code compiles (or parses, for interpreted languages) without errors.

| Language   | Command                            |
|------------|------------------------------------|
| C++        | `g++ -fsyntax-only`                |
| Java       | `javac`                            |
| Python     | `python3 -m py_compile`            |
| Go         | `go build`                         |
| Rust       | `rustc --edition 2021 --crate-type lib` |
| JavaScript | `node --check`                     |
| TypeScript | `npx tsc --noEmit`                 |
| C          | `gcc -fsyntax-only`                |
| C#         | `dotnet build` or `csc`            |
| Kotlin     | `kotlinc`                          |
| Swift      | `swiftc -typecheck`                |

If compilation fails, show the errors clearly and use `AskUserQuestion` with a "Done" option so the user can fix and signal again. Repeat until it compiles.

### 5b. Test

Check if a test harness file (`lc-<number>_test.<ext>`) already exists in `~/leetcode-playground/`. If it does and the content looks valid (imports the solution, contains test cases), reuse it as-is.

Only generate a new harness if none exists or the existing one is clearly stale or broken. Place it in `~/leetcode-playground/` as `lc-<number>_test.<ext>`.

Aim for **at least 30 test cases** spread across the categories below. Generate multiple variations per category, not just one token example.

1. **LeetCode-provided examples** — reproduce every example from the problem statement.

2. **Edge cases** — think carefully about the problem's specific pitfalls:
   - Empty input, single-element input, two-element input
   - Boundary values from the constraints (minimum and maximum of each parameter)
   - Off-by-one errors (+1 / -1 on indices, lengths, counts, thresholds)
   - Out-of-boundary access (first/last element, negative indices if applicable)
   - Duplicate values, all-same input, all-distinct input
   - Sorted / reverse-sorted / already-in-target-state input
   - Zero, negative numbers, overflow-prone values (INT_MAX, INT_MIN, large sums) where relevant
   - No valid answer exists (if the problem allows it)
   - Multiple valid answers (verify any valid one is accepted)

3. **Randomized / stress tests** — programmatically generate random inputs of moderate size (n=100–1000) and validate against invariants or a brute-force reference solution.

4. **Performance** — generate at least 5 large-scale inputs at or near the upper constraint bound to catch algorithmic complexity regressions. Vary the input shape (worst-case vs. average-case). Fail if a reasonable time limit (a few seconds) is exceeded.

Compile and run the test harness.

**If all tests pass**, delete the test harness file(s) to keep the directory clean.

**If any test fails**, report results in a markdown table:

| # | Category | Input (brief) | Expected | Actual | Status |
|---|----------|---------------|----------|--------|--------|

Truncate large inputs with `...`. Use a checkmark for pass and X for fail. Show a summary line (e.g. "28/30 passed").

Do NOT suggest fixes, hint at bugs, or explain what might be wrong — the user wants to debug it themselves. Only provide guidance if explicitly asked. Use `AskUserQuestion` with a "Done" option so the user can signal after fixing. Repeat until all tests pass or the user decides to stop.
