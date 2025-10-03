---
description: Test mobile experience with CPU and network throttling to simulate real device conditions
---

# Mobile Emulation Testing

Test the specified URL under realistic mobile conditions with CPU throttling and network throttling to identify performance issues on slower devices.

## Target

URL: $ARGUMENTS

## Testing Scenarios

### Scenario 1: Mid-range smartphone (3G)

- **Device**: Mid-range Android phone
- **Viewport**: 375x667
- **CPU**: 4x slowdown
- **Network**: Slow 3G (400 Kbps, 400ms RTT)

### Scenario 2: Budget smartphone (2G)

- **Device**: Entry-level phone
- **Viewport**: 360x640
- **CPU**: 6x slowdown
- **Network**: 2G (50 Kbps, 500ms RTT)

### Scenario 3: High-end smartphone (4G)

- **Device**: Flagship phone
- **Viewport**: 390x844
- **CPU**: 2x slowdown
- **Network**: Fast 4G (4 Mbps, 40ms RTT)

## Testing Steps

For each scenario:

1. **Setup emulation**

   - Use `new_page` tool to open URL
   - Use `resize_page` tool for viewport size
   - Use `emulate_cpu` tool to set CPU throttling
   - Use `emulate_network` tool to set network conditions

2. **Performance measurement**

   - Use `performance_start_trace` tool to begin recording
   - Let page fully load
   - Use `performance_analyze_insight` tool for analysis
   - Measure key metrics:
     - Time to Interactive (TTI)
     - First Contentful Paint (FCP)
     - Largest Contentful Paint (LCP)
     - Total Blocking Time (TBT)

3. **Interaction testing**

   - Use `click` tool to test navigation
   - Use `fill` tool to test form inputs
   - Measure interaction responsiveness
   - Check for janky scrolling

4. **Resource analysis**

   - Use `list_network_requests` tool
   - Identify large resources blocking load
   - Check image optimization
   - Verify JavaScript bundle sizes

5. **User experience check**

   - Use `take_screenshot` tool at key load stages:
     - Initial load (0s)
     - First paint
     - Interactive state
   - Check for layout shifts
   - Verify loading indicators presence

6. **Console monitoring**
   - Use `list_console_messages` tool
   - Check for timeout errors
   - Verify no memory warnings

## Report Format

### Performance Comparison

| Scenario     | Viewport | TTI | FCP | LCP | TBT | Page Weight |
| ------------ | -------- | --- | --- | --- | --- | ----------- |
| Mid-range 3G | 375x667  | Xs  | Xs  | Xs  | Xs  | X MB        |
| Budget 2G    | 360x640  | Xs  | Xs  | Xs  | Xs  | X MB        |
| High-end 4G  | 390x844  | Xs  | Xs  | Xs  | Xs  | X MB        |

### Critical Issues

**Budget Device (2G)**:

- [ ] Page load exceeds 10 seconds
- [ ] Interactive delay >5 seconds
- [ ] Total blocking time >2 seconds

**Mid-range Device (3G)**:

- [ ] Page load exceeds 5 seconds
- [ ] Interactive delay >3 seconds
- [ ] Total blocking time >1 second

### Device-Specific Problems

For each scenario, list:

1. **Loading Issues**

   - Resources timing out
   - Excessive JavaScript execution time
   - Large image downloads

2. **Interaction Issues**

   - Delayed button responses
   - Janky scrolling
   - Form input lag

3. **Visual Issues**
   - Layout shifts during load
   - Missing loading indicators
   - Flash of unstyled content (FOUC)

### Optimization Recommendations

**High Priority** (Budget device usability):

1. [Recommendation]
   - Expected improvement: [metric]
   - Implementation: [code/config]

**Medium Priority** (Mid-range device enhancement):

1. [Recommendation]

**Low Priority** (High-end device optimization):

1. [Recommendation]

### Progressive Enhancement Strategy

**Core Experience** (2G):

- Minimal viable functionality
- Content-first approach
- Defer non-essential scripts

**Enhanced Experience** (3G):

- Additional interactive features
- Optimized images
- Conditional resource loading

**Full Experience** (4G+):

- All features enabled
- High-quality media
- Advanced interactions

### Implementation Examples

```javascript
// Adaptive loading based on connection
if ("connection" in navigator) {
  const connection = navigator.connection;
  if (connection.effectiveType === "2g") {
    // Load minimal resources
  } else if (connection.effectiveType === "3g") {
    // Load optimized resources
  } else {
    // Load full resources
  }
}
```

## Success Criteria

- TTI < 5s on 3G connection
- FCP < 2s on 3G connection
- LCP < 4s on 3G connection
- Page usable (even if not fully loaded) within 3s on 3G
- No console errors on any device configuration
- Smooth scrolling (60fps) on mid-range device
