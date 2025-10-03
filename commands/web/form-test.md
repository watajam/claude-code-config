---
description: Test form functionality including validation, accessibility, and user experience
---

# Form Testing

Comprehensively test forms on the specified page for functionality, validation, accessibility, and user experience.

## Target

URL: $ARGUMENTS

## Testing Steps

1. **Form identification**

   - Use `new_page` tool to open URL
   - Use `take_snapshot` tool to find all forms
   - Identify form fields, buttons, and validation

2. **Happy path testing**

   - Use `fill_form` tool to submit valid data
   - Verify successful submission
   - Check success message display
   - Verify redirection if applicable

3. **Validation testing**

   For each field, test:

   **Required field validation**:

   - Submit form with empty required field
   - Verify error message appears
   - Check error message clarity

   **Format validation**:

   - Email: Test invalid formats (test@, @test.com, test)
   - Phone: Test incorrect patterns
   - URL: Test malformed URLs
   - Number: Test non-numeric input
   - Date: Test invalid dates

   **Length validation**:

   - Test minimum length requirements
   - Test maximum length limits
   - Test exact length fields (e.g., zip code)

4. **Error handling**

   - Use `list_console_messages` to check for JavaScript errors
   - Verify user-friendly error messages
   - Check error message positioning
   - Test error message accessibility (aria-live, role="alert")

5. **Accessibility testing**

   - Use `take_snapshot` to verify:
     - All inputs have associated labels
     - Error messages linked via aria-describedby
     - Required fields marked with aria-required
     - Invalid fields marked with aria-invalid
   - Check keyboard navigation (Tab order)
   - Verify focus management on errors

6. **User experience**

   - Test autofocus on first field
   - Check placeholder text clarity
   - Verify loading states during submission
   - Test double-submit prevention
   - Check form reset functionality

7. **Network analysis**

   - Use `list_network_requests` to verify:
     - Form submission endpoint
     - Request method (POST/PUT/PATCH)
     - Response status codes
     - Response time

8. **Edge cases**
   - Test with very long input
   - Test with special characters (', ", <, >, &)
   - Test with script tags (XSS prevention)
   - Test with Unicode characters
   - Test copy-paste behavior

## Report Format

### Form Overview

- Form location: [selector/description]
- Total fields: X
- Required fields: Y
- Validation rules: Z

### Functionality Test Results

✅ **Passed Tests**:

- [Test description]

❌ **Failed Tests**:

- [Test description]
- Expected: [expected behavior]
- Actual: [actual behavior]
- Screenshot: [if applicable]

### Validation Issues

| Field  | Issue         | Severity     | Fix        |
| ------ | ------------- | ------------ | ---------- |
| [name] | [description] | High/Med/Low | [solution] |

### Accessibility Issues

**Critical** (WCAG Level A):

- [Issue with code example]

**Important** (WCAG Level AA):

- [Issue with code example]

### UX Recommendations

- [Improvement suggestion]
- Expected impact: [description]

### Code Fixes

For each issue, provide specific code:

```html
<!-- Before -->
<input type="email" name="email" />

<!-- After -->
<label for="email">Email Address</label>
<input
  type="email"
  id="email"
  name="email"
  required
  aria-required="true"
  aria-describedby="email-error"
/>
<span id="email-error" role="alert"></span>
```

### Security Considerations

- XSS vulnerability check results
- CSRF token presence
- Input sanitization verification

## Success Criteria

- All form fields tested with valid and invalid data
- All validation rules verified
- No console errors during form interaction
- All accessibility requirements met (labels, ARIA)
- Error messages are clear and actionable
- Form submits successfully with valid data
