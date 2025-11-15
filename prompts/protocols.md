# Protocol Opportunity Analysis Prompt

You are analyzing a Python codebase to identify opportunities for implementing typing.Protocol. Follow these steps exactly and report findings systematically.

## Step 1: Run static analysis
**IMPORTANT**: Always use `uv tool run` and other `uv`commands to execute python tools when working on this codebase.

Execute these commands and save the output:
```bash
uv tool run mypy --strict --show-error-codes . > mypy_output.txt 2>&1
```

If mypy is not configured, run:
```bash
uv tool run mypy --strict --show-error-codes --ignore-missing-imports . > mypy_output.txt 2>&1
```

## Step 2: Scan for duck typing patterns

Run these exact grep commands:
```bash
# Find hasattr checks (potential duck typing)
grep -rn "hasattr.*," . --include="*.py" > hasattr_patterns.txt

# Find functions without return type annotations
grep -rn "^def " . --include="*.py" | grep -v " -> " > untyped_functions.txt

# Find isinstance with multiple types
grep -rn "isinstance.*(" . --include="*.py" | grep "," > isinstance_multiple.txt
```

## Step 3: Analyze each file category

For each pattern found, extract:
1. File path and line number
2. Function name
3. Parameter names that are being duck-typed
4. Methods or attributes accessed on those parameters

Format as:
```
File: path/to/file.py:42
Function: process_data
Parameter: obj
Methods used: .read(), .close()
```

## Step 4: Identify protocol candidates

A valid protocol candidate must meet ALL criteria:
- Parameter is used in 2+ locations in the codebase
- Parameter accesses 2+ methods or attributes
- No existing type hint or type hint is `Any`
- Methods accessed are consistent across usage sites

For each candidate, output:
```
PROTOCOL CANDIDATE #N
Name suggestion: [descriptive name based on behavior]
Required methods:
  - method_name(self, arg: type) -> return_type
  - method_name2(self) -> return_type
  
Used in functions:
  - file.py:line - function_name
  - file2.py:line - function_name2
  
Example implementation:
[show protocol definition code]

Affected call sites: [count]
```

## Step 5: Prioritize by impact

Rank candidates by:
1. Number of call sites (more = higher priority)
2. Number of files affected (more = higher priority)
3. Complexity of interface (more methods = higher priority)

Output as numbered list with impact score.

## Step 6: Check for false positives

For each candidate, verify it's NOT:
- Already using an ABC from typing or collections.abc
- A single-use case (used in only one function)
- Better served by a simple type alias or Union
- Using methods that vary significantly across call sites

Flag any candidates that fail these checks.

## Output format

Provide results in this exact structure:
```
ANALYSIS SUMMARY
================
Total files scanned: [number]
Untyped functions found: [number]
hasattr patterns found: [number]
isinstance multiple types: [number]

HIGH PRIORITY CANDIDATES
========================
[List top 3-5 candidates with full details]

MEDIUM PRIORITY CANDIDATES
==========================
[List remaining candidates]

FALSE POSITIVES EXCLUDED
========================
[List any excluded with reason]

RECOMMENDED NEXT STEPS
======================
1. [Specific file and function to start with]
2. [Second priority]
3. [Third priority]
```

## Critical constraints

- Do NOT suggest protocols for stdlib types that already have protocols (e.g., don't recreate typing.SupportsInt)
- Do NOT suggest protocols for single method interfaces under 3 usages
- Do NOT invent method signatures - extract them from actual code
- Do NOT suggest protocols if inheritance is already in use and working
- ALWAYS verify the methods accessed are actually consistent across usages

## Verification step

Before outputting final recommendations, state:
"I have verified that for each protocol candidate:
- [ ] Methods are consistently used across call sites
- [ ] No existing protocol or ABC covers this
- [ ] Used in 2+ distinct locations
- [ ] Would improve type safety without breaking existing code"
