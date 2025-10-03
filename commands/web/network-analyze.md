---
description: Analyze network requests to identify slow resources and optimization opportunities
---

# Network Performance Analysis

Analyze all network requests for the specified URL to identify slow resources, large files, and optimization opportunities.

## Target

URL: $ARGUMENTS

## Analysis Steps

1. **Page load with network monitoring**

   - Use `new_page` tool to navigate to URL
   - Network requests are automatically captured

2. **Request inventory**

   - Use `list_network_requests` tool to get all requests
   - Categorize by resource type:
     - Documents (HTML)
     - Stylesheets (CSS)
     - Scripts (JS)
     - Images
     - Fonts
     - XHR/Fetch (API calls)
     - Other

3. **Performance metrics per request**

   - Use `get_network_request` tool for detailed analysis
   - Extract timing information:
     - Queue time
     - Request sent time
     - Download complete time
     - Processing complete time
   - Calculate durations:
     - Total duration
     - Download duration
     - Main thread processing

4. **Size analysis**

   - Identify large resources (>500KB warning, >1MB critical)
   - Check for compression (gzip/brotli)
   - Calculate total page weight

5. **Priority and blocking analysis**

   - Identify render-blocking resources
   - Check resource priority assignments
   - Analyze dependency chains

6. **Third-party resources**

   - Identify external domains
   - Measure third-party impact on load time
   - Check for redundant third-party scripts

7. **Caching strategy**
   - Check cache headers
   - Identify resources without cache headers
   - Verify appropriate cache durations

## Report Format

### Network Summary

- Total requests: X
- Total transfer size: Y MB
- Total page weight: Z MB
- Average request time: W ms

### Slow Resources (>1s)

| Resource | Type   | Size   | Time   | Issue         |
| -------- | ------ | ------ | ------ | ------------- |
| [URL]    | [type] | [size] | [time] | [description] |

### Large Resources (>500KB)

| Resource | Type   | Actual Size | Compressed? | Recommendation |
| -------- | ------ | ----------- | ----------- | -------------- |
| [URL]    | [type] | [size]      | Yes/No      | [action]       |

### Render-Blocking Resources

List resources blocking initial render with optimization suggestions

### Third-Party Impact

| Domain   | Requests | Total Size | Total Time | Impact       |
| -------- | -------- | ---------- | ---------- | ------------ |
| [domain] | X        | Y KB       | Z ms       | High/Med/Low |

### Optimization Recommendations

**High Priority** (>1s improvement):

1. [Recommendation with expected impact]

**Medium Priority** (500ms-1s improvement):

1. [Recommendation]

**Low Priority** (<500ms improvement):

1. [Recommendation]

### Caching Strategy Issues

- Resources without cache headers
- Short cache durations that should be longer
- Long cache durations for frequently changing resources

### Implementation Guide

For each major recommendation, provide:

```javascript
// Code example or configuration
```

## Success Criteria

- All requests catalogued and analyzed
- Resources >500KB identified
- Render-blocking resources listed
- At least 3 actionable optimizations provided
- Expected performance improvement quantified
