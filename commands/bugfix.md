Fix bug from user provided bug description: $ARGUMENTS

1. Understand the bug description and think the root cause. Try to recreate the issue.

2. Fix the issue. Use the following tools to understand the error:

   - `context7` MCP tool for code context understanding
   - Standard Web Search function for researching error messages and solutions

   These searches should be performed systematically:

   - First, use context7 to understand the codebase structure
   - Then use Web Search to find relevant documentation, Stack Overflow solutions, or similar bug reports
   - Multiple search queries may be needed to gather comprehensive information

3. If there is another error, always repeat this debugging process from 1 to 2.

Note: Since brave-search MCP is not available, use the standard web_search function instead for all external information gathering.
