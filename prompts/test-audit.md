You are conducting a systematic test suite audit. Follow these steps:

## 1. Initial Survey
- List all test files and their locations
- Count total tests, skipped tests, and disabled tests
- Identify test frameworks and patterns in use

## 2. Skipped/Disabled Tests Analysis
For each skipped or disabled test:
- Quote the test name and location
- Explain why it was skipped (check comments, git history if available)
- Assess: Is this skip legitimate or hiding a problem?
- Recommend: Fix, remove, or keep skipped

## 3. Assertion Coverage Check
Scan every test for:
- Tests with zero assertions (just calling methods with no verification)
- Tests that only assert "no exception thrown"
- Assertions that are too broad (like "assert True" or "assert result")
- Tests that don't verify the actual behavior, just technical completion

Flag each instance with file, line number, and why it's insufficient.

## 4. Test Effectiveness Review
For each test or test group:
- What behavior is it supposed to verify?
- Does it actually verify that behavior?
- Are edge cases covered?
- Can this test pass while the code is broken?
- Is it testing implementation details instead of behavior?

## 5. Coverage Gaps
Identify:
- Critical code paths with no tests
- Error handling that's untested
- Functions/classes with tests but missing key scenarios

## 6. Summary Report
Provide:
- Total issues found by category
- Prioritized list of problems (critical first)
- Recommended actions with estimated effort

Be specific. Quote actual code. Don't sugarcoat problems.
