---
description: Analyze web page performance and identify bottlenecks using Chrome DevTools MCP
---

# Performance Analysis

Perform comprehensive performance analysis on the specified URL and provide actionable optimization recommendations.

## Target

URL: $ARGUMENTS

## Analysis Steps

1. **Open the page**

   - Use `new_page` tool to navigate to the URL
   - Wait for the page to fully load

2. **Start performance tracing**

   - Use `performance_start_trace` tool to begin recording
   - Capture metrics including LCP, FID, CLS, TTFB

3. **Analyze insights**

   - Use `performance_analyze_insight` tool for each available insight:
     - DocumentLatency
     - LCPBreakdown
     - NetworkDependencyTree
     - DOMSize
     - ThirdParties
     - ForcedReflow

4. **Network analysis**

   - Use `list_network_requests` tool to identify slow requests
   - Check for uncompressed resources
   - Identify render-blocking resources

5. **Console errors check**
   - Use `list_console_messages` tool to find errors that might impact performance

## Report Format

Provide a structured report with:

### Performance Metrics

- LCP (target: < 2.5s)
- TTFB (target: < 600ms)
- CLS (target: < 0.1)

### Critical Issues

List issues in order of impact with:

- Issue description
- Estimated time savings
- Specific fix recommendations

### Quick Wins

Identify easy optimizations that provide significant impact

### Next Steps

Prioritized action items with implementation difficulty

## Success Criteria

- All metrics measured and reported
- At least 3 actionable recommendations provided
- Estimated performance impact quantified
