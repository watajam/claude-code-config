---
description: Test responsive design across multiple device sizes and identify layout issues
---

# Responsive Design Check

Test the specified URL across multiple viewport sizes to identify responsive design issues and layout problems.

## Target

URL: $ARGUMENTS

## Device Sizes to Test

1. **Mobile (Portrait)**

   - Size: 375x812 (iPhone X)
   - Common device reference

2. **Mobile (Landscape)**

   - Size: 812x375
   - Horizontal orientation

3. **Tablet (Portrait)**

   - Size: 768x1024 (iPad)
   - Medium screen size

4. **Tablet (Landscape)**

   - Size: 1024x768
   - Horizontal tablet view

5. **Desktop (Standard)**

   - Size: 1920x1080 (Full HD)
   - Most common desktop resolution

6. **Desktop (Wide)**
   - Size: 2560x1440 (2K)
   - Large monitor

## Testing Steps

For each viewport size:

1. **Resize viewport**

   - Use `resize_page` tool to set dimensions
   - Wait for layout reflow

2. **Visual inspection**

   - Use `take_screenshot` tool to capture display
   - Check for layout breaks or overlaps

3. **Element positioning check**

   - Use `take_snapshot` tool to inspect element bounds
   - Verify no content overflow
   - Check navigation menu behavior
   - Verify form field sizing

4. **Touch target validation** (mobile/tablet)

   - Use `evaluate_script` to measure interactive elements
   - Verify minimum 44x44px touch targets
   - Check spacing between clickable elements

5. **Content readability**

   - Use `evaluate_script` to check font sizes
   - Verify line length (45-75 characters optimal)
   - Check text contrast

6. **Console errors**
   - Use `list_console_messages` to check for size-specific errors
   - Verify media query loading

## Report Format

### Overview

- Total issues found: X
- Critical layout breaks: Y
- Minor adjustments needed: Z

### Issues by Viewport

#### Mobile (375x812)

**Critical Issues**:

- [Issue description with screenshot reference]
- Affected elements: [selector or uid]

**Minor Issues**:

- [Issue description]

**Code Fix**:

```css
/* CSS fix example */
```

#### [Repeat for each viewport]

### Cross-Viewport Issues

Issues that appear across multiple sizes:

- Inconsistent navigation behavior
- Flex/Grid layout problems
- Typography scaling issues

### Recommendations

**Immediate Fixes**:
Priority issues requiring immediate attention

**Design Improvements**:
Suggestions for better responsive behavior

**Testing Strategy**:
Breakpoints to add or adjust

## Success Criteria

- No content overflow at any breakpoint
- All interactive elements accessible
- Touch targets meet minimum size on mobile
- Layout maintains visual hierarchy across sizes
- No horizontal scrolling unless intentional
