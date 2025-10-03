---
description: Debug console errors and warnings on web pages with detailed analysis
---

# Console Error Debugging

Investigate and analyze all console errors and warnings on the specified page, providing root cause analysis and fixes.

## Target

URL: $ARGUMENTS

## Investigation Steps

1. **Initial page load**

   - Use `new_page` tool to navigate to URL
   - Wait for complete page load

2. **Capture initial console state**

   - Use `list_console_messages` tool to get all messages
   - Categorize by severity: error, warning, info

3. **Interactive element testing**

   - Use `take_snapshot` tool to identify interactive elements
   - Use `click` tool to trigger interactions that might cause errors
   - Use `fill` tool to test form inputs
   - Monitor console for new errors after each interaction

4. **Error analysis**
   For each error found:

   - Extract error message and stack trace
   - Identify the source file and line number
   - Determine if it's a JavaScript, network, or CORS error
   - Check if it impacts functionality

5. **Network-related errors**

   - Use `list_network_requests` tool to correlate with failed requests
   - Check for 404, 500, or CORS errors
   - Verify resource loading issues

6. **Source investigation**
   - Use `evaluate_script` tool to inspect problematic code
   - Check for undefined variables or null references
   - Verify API response handling

## Report Format

### Error Summary

- Total errors: X
- Total warnings: Y
- Critical errors: Z

### Error Details

For each error provide:

#### Error #1: [Error Message]

- **Severity**: Error/Warning
- **Category**: JavaScript/Network/CORS/Security
- **Source**: filename.js:line
- **Impact**: High/Medium/Low
- **Stack Trace**: [abbreviated trace]

**Root Cause**:
[Explanation of why this error occurs]

**Fix**:

```javascript
// Code fix example
```

**Testing**:
Steps to verify the fix

### Warnings Analysis

List warnings by priority with recommendations

### Prevention Recommendations

- Code review checklist items
- Testing scenarios to add
- Monitoring to implement

## Success Criteria

- All errors categorized and analyzed
- Root cause identified for each error
- Specific code fixes provided
- Impact assessment completed
