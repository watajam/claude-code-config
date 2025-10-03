---
description: Comprehensive page audit covering performance, accessibility, console errors, and responsive design
---

# Full Page Audit

Perform a comprehensive audit of the specified page covering all critical aspects: performance, accessibility, functionality, and responsive design.

## Target

URL: $ARGUMENTS

## Audit Sections

### 1. Performance Analysis

**Steps**:

- Use `performance_start_trace` tool
- Analyze all available insights
- Use `list_network_requests` for resource analysis

**Checks**:

- ✅ LCP < 2.5s
- ✅ FCP < 1.8s
- ✅ CLS < 0.1
- ✅ TTFB < 600ms
- ✅ Page weight < 3MB
- ✅ Render-blocking resources minimized

### 2. Accessibility Audit

**Steps**:

- Use `take_snapshot` for accessibility tree
- Use `evaluate_script` for ARIA validation

**Checks**:

- ✅ All images have alt text
- ✅ Form inputs have labels
- ✅ Proper heading hierarchy
- ✅ Sufficient color contrast (4.5:1)
- ✅ Keyboard navigation works
- ✅ Focus indicators visible
- ✅ ARIA attributes properly used

### 3. Console Health Check

**Steps**:

- Use `list_console_messages` tool
- Test interactive elements with `click` tool

**Checks**:

- ✅ No JavaScript errors
- ✅ No CORS errors
- ✅ No 404 network errors
- ✅ No deprecation warnings
- ✅ No security warnings

### 4. Responsive Design

**Steps**:

- Test 3 key breakpoints with `resize_page` tool
- Use `take_screenshot` at each size

**Checks**:

- ✅ Mobile (375px): No horizontal scroll
- ✅ Tablet (768px): Navigation adapts
- ✅ Desktop (1920px): Content centers properly
- ✅ Touch targets ≥44px on mobile
- ✅ Text readable on all sizes

### 5. SEO Basics

**Steps**:

- Use `evaluate_script` to check meta tags

**Checks**:

- ✅ Title tag present (50-60 chars)
- ✅ Meta description present (150-160 chars)
- ✅ H1 tag present (only one)
- ✅ Images have alt attributes
- ✅ Links are descriptive
- ✅ Canonical URL set

### 6. Security Headers

**Steps**:

- Use `list_network_requests` to inspect headers

**Checks**:

- ✅ Content-Security-Policy present
- ✅ X-Frame-Options set
- ✅ X-Content-Type-Options set
- ✅ HTTPS only (no mixed content)

## Report Format

### Executive Summary

**Overall Score**: X/100

**Critical Issues**: Y  
**Warnings**: Z  
**Passed Checks**: W

**Priority Actions**:

1. [Most critical issue - expected impact]
2. [Second critical issue - expected impact]
3. [Third critical issue - expected impact]

---

### Detailed Results

#### ⚡ Performance (Score: X/100)

**Metrics**:

- LCP: Xs (⚠️ Warning / ✅ Good / ❌ Poor)
- FCP: Xs
- CLS: X
- TTFB: Xms

**Issues Found**:

1. **[Issue Name]** - Severity: High/Med/Low
   - Description: [details]
   - Impact: [quantified impact]
   - Fix: [specific solution]

---

#### ♿ Accessibility (Score: X/100)

**WCAG Compliance**: Level A ✅ | Level AA ⚠️ | Level AAA ❌

**Issues Found**:

1. **[Issue Name]** - WCAG Criterion: [X.X.X]
   - Description: [details]
   - Affected Elements: [count]
   - Fix:
   ```html
   <!-- Code example -->
   ```

---

#### 🐛 Console Health (Score: X/100)

**Error Summary**:

- Errors: X
- Warnings: Y
- Info: Z

**Critical Errors**:

1. **[Error Message]** - Source: [file:line]
   - Root cause: [explanation]
   - Impact: [functional impact]
   - Fix: [solution]

---

#### 📱 Responsive Design (Score: X/100)

**Breakpoint Tests**:

- Mobile (375px): ✅/⚠️/❌
- Tablet (768px): ✅/⚠️/❌
- Desktop (1920px): ✅/⚠️/❌

**Issues Found**:

1. **[Layout Issue]** - Affected: [breakpoints]
   - Description: [details]
   - Screenshot: [reference]
   - Fix:
   ```css
   /* CSS solution */
   ```

---

#### 🔍 SEO (Score: X/100)

**Key Findings**:

- Title: [content] (X characters)
- Description: [content] (X characters)
- H1: [content]

**Issues**: [if any]

---

#### 🔒 Security (Score: X/100)

**Header Check**:
| Header | Status | Value |
|--------|--------|-------|
| CSP | ✅/❌ | [value] |
| X-Frame-Options | ✅/❌ | [value] |
| X-Content-Type | ✅/❌ | [value] |

**Issues**: [if any]

---

### Action Plan

#### Immediate (< 1 day)

1. **[Critical Issue]**
   - Steps to fix
   - Expected improvement
   - Testing checklist

#### Short-term (1-7 days)

1. **[Important Issue]**
   - Steps to fix
   - Expected improvement

#### Long-term (1-4 weeks)

1. **[Enhancement]**
   - Steps to implement
   - Expected benefit

---

### Testing Checklist

After implementing fixes, verify:

- [ ] All performance metrics improved
- [ ] No new console errors introduced
- [ ] Accessibility score improved
- [ ] Responsive design issues resolved
- [ ] SEO elements optimized
- [ ] Security headers properly configured

## Success Criteria

**Minimum Requirements**:

- Overall score ≥ 70/100
- No critical accessibility violations
- No console errors
- Performance metrics meet Core Web Vitals
- Mobile-friendly at 375px viewport

**Recommended Targets**:

- Overall score ≥ 85/100
- WCAG Level AA compliance
- Performance score ≥ 90
- All responsive breakpoints ✅
