---
description: Perform accessibility audit on web pages and identify WCAG compliance issues
---

# Accessibility Check

Conduct comprehensive accessibility testing on the specified URL to ensure WCAG 2.1 Level AA compliance.

## Target

URL: $ARGUMENTS

## Audit Steps

1. **Page snapshot**

   - Use `new_page` tool to open the URL
   - Use `take_snapshot` tool to capture accessibility tree

2. **Semantic structure analysis**

   - Use `evaluate_script` tool to check:
     - Proper heading hierarchy (h1-h6)
     - Landmark regions (header, nav, main, footer)
     - List structures (ul, ol)

3. **Interactive elements check**

   - Verify all buttons have accessible names
   - Check form inputs have associated labels
   - Validate links have descriptive text

4. **ARIA attributes validation**

   - Check for proper aria-label/aria-labelledby usage
   - Verify aria-describedby for complex widgets
   - Validate role attributes
   - Check aria-live regions for dynamic content

5. **Keyboard navigation**

   - Use `evaluate_script` to check focus order
   - Verify skip links presence
   - Check focus visibility (focus indicators)

6. **Color contrast**

   - Use `evaluate_script` to check text contrast ratios
   - Verify WCAG AA standards (4.5:1 for normal text)

7. **Alternative text**
   - Check all images have alt attributes
   - Verify decorative images use alt=""

## Report Format

### Critical Issues (WCAG Level A)

Issues that must be fixed:

- Missing alt text
- Keyboard traps
- Form inputs without labels

### Important Issues (WCAG Level AA)

Issues that should be fixed:

- Insufficient color contrast
- Missing ARIA attributes
- Improper heading hierarchy

### Best Practices

Recommendations for improved accessibility:

- Enhanced focus indicators
- Better descriptive text
- ARIA live region improvements

### Code Examples

Provide specific code fixes for each issue found

## Success Criteria

- All interactive elements have accessible names
- No keyboard traps detected
- Color contrast meets WCAG AA standards
- All form inputs properly labeled
