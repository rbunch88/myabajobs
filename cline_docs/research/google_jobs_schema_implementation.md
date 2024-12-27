# Google Jobs Schema Implementation Research

## Overview

Research into implementing Google Jobs schema for optimal job posting visibility and search integration.

## Schema Requirements

### Required Properties

1. **title** (String)
   - Must be job title only
   - No company names/locations
   - No special characters
   - Must match visible content

2. **description** (HTML String)
   - Full job description
   - Must include responsibilities
   - Must include requirements
   - Must match visible content
   - Supported HTML tags: `<p>`, `<ul>`, `<li>`

3. **datePosted** (ISO 8601)
   - Original posting date
   - Format: "YYYY-MM-DD"
   - Must be accurate

4. **hiringOrganization** (Object)
   - Company name
   - Company website
   - Company logo (optional)
   - Must be actual employer

5. **jobLocation** (Object)
   - Physical work location
   - Complete address structure
   - Country code required
   - Multiple locations supported

### Recommended Properties

1. **validThrough** (ISO 8601)
   - Expiration date
   - Must be future date
   - Format: "YYYY-MM-DDThh:mm:ss"

2. **employmentType** (String[])
   - Valid types:
     - FULL_TIME
     - PART_TIME
     - CONTRACTOR
     - TEMPORARY
     - INTERN
     - VOLUNTEER
     - PER_DIEM
     - OTHER

3. **baseSalary** (Object)
   - Currency
   - Value or range
   - Payment interval
   - Must match visible content

## Implementation Strategy

### 1. Schema Generation

```typescript
interface JobPostingSchema {
  "@context": "https://schema.org/";
  "@type": "JobPosting";
  title: string;
  description: string;
  datePosted: string;
  validThrough?: string;
  employmentType?: string[];
  hiringOrganization: {
    "@type": "Organization";
    name: string;
    sameAs: string;
    logo?: string;
  };
  jobLocation: {
    "@type": "Place";
    address: {
      "@type": "PostalAddress";
      streetAddress: string;
      addressLocality: string;
      addressRegion: string;
      postalCode: string;
      addressCountry: string;
    };
  }[];
  baseSalary?: {
    "@type": "MonetaryAmount";
    currency: string;
    value: {
      "@type": "QuantitativeValue";
      value?: number;
      minValue?: number;
      maxValue?: number;
      unitText: "HOUR" | "DAY" | "WEEK" | "MONTH" | "YEAR";
    };
  };
}
```

### 2. Validation System

```typescript
const validateJobPosting = (data: JobPostingSchema): ValidationResult => {
  // Title validation
  if (!isValidTitle(data.title)) {
    return {
      valid: false,
      errors: ['Invalid title format']
    };
  }

  // Description validation
  if (!isValidDescription(data.description)) {
    return {
      valid: false,
      errors: ['Invalid description format']
    };
  }

  // Date validation
  if (!isValidDate(data.datePosted)) {
    return {
      valid: false,
      errors: ['Invalid date format']
    };
  }

  // Continue with other validations...
  return { valid: true };
};
```

### 3. Content Synchronization

```typescript
const syncJobContent = (
  schema: JobPostingSchema,
  content: VisibleContent
): boolean => {
  return (
    schema.title === content.title &&
    stripHtml(schema.description) === stripHtml(content.description) &&
    // Continue with other comparisons...
    true
  );
};
```

## Implementation Guidelines

### 1. Page Structure

```html
<html>
<head>
  <title>Software Engineer - Company Name</title>
  <script type="application/ld+json">
    {
      "@context": "https://schema.org/",
      "@type": "JobPosting",
      // ... job posting data
    }
  </script>
</head>
<body>
  <!-- Visible job content matching schema -->
</body>
</html>
```

### 2. Content Requirements

1. One job posting per page
2. All schema data visible on page
3. Direct apply option available
4. No login walls
5. Complete job details

### 3. Technical Requirements

1. Proper canonical URLs
2. Efficient crawling setup
3. Indexing API integration
4. Sitemap implementation
5. Regular validation

## Testing Strategy

### 1. Schema Validation

```typescript
describe('Job Posting Schema', () => {
  test('validates required fields', () => {
    const jobPosting = createJobPosting();
    expect(validateJobPosting(jobPosting).valid).toBe(true);
  });

  test('validates title format', () => {
    const jobPosting = createJobPosting({ title: '!!! HIRING NOW !!!' });
    expect(validateJobPosting(jobPosting).valid).toBe(false);
  });

  // Additional test cases...
});
```

### 2. Content Matching

```typescript
describe('Content Synchronization', () => {
  test('matches schema with visible content', () => {
    const schema = getJobSchema();
    const content = getVisibleContent();
    expect(syncJobContent(schema, content)).toBe(true);
  });

  // Additional test cases...
});
```

## Monitoring & Maintenance

### 1. Regular Checks

- Schema validation
- Content synchronization
- Expiration dates
- Search Console monitoring
- Indexing status

### 2. Error Handling

- Schema validation errors
- Content mismatch errors
- Indexing errors
- Expiration handling
- Manual actions

### 3. Performance Tracking

- Search appearance rate
- Click-through rate
- Application rate
- Indexing coverage
- Error rates

## Best Practices

### 1. Schema Implementation

- Use typed interfaces
- Implement validation
- Maintain synchronization
- Regular testing
- Monitor changes

### 2. Content Management

- Clear job titles
- Detailed descriptions
- Accurate locations
- Current dates
- Regular updates

### 3. Technical Setup

- Proper markup
- Clean HTML
- Efficient indexing
- Regular monitoring
- Quick fixes

## Common Issues & Solutions

### 1. Schema Validation Failures

- Check required fields
- Validate formats
- Test content matching
- Review HTML structure
- Monitor changes

### 2. Content Mismatch

- Implement sync checks
- Regular validation
- Content monitoring
- Update procedures
- Error logging

### 3. Indexing Issues

- Check robots.txt
- Verify sitemaps
- Monitor coverage
- Test accessibility
- Review structure

## Resources

### Documentation

- [Google Jobs Schema Documentation](https://developers.google.com/search/docs/appearance/structured-data/job-posting)
- [Schema.org JobPosting](https://schema.org/JobPosting)
- [Google Search Console](https://search.google.com/search-console)
- [Google Indexing API](https://developers.google.com/search/apis/indexing-api)

### Tools

- [Rich Results Test](https://search.google.com/test/rich-results)
- [Schema Markup Validator](https://validator.schema.org/)
- [Google URL Inspection](https://support.google.com/webmasters/answer/9012289)
- [Google Search Console](https://search.google.com/search-console)

## Next Steps

1. Implement schema generation
2. Create validation system
3. Set up monitoring
4. Implement testing
5. Deploy initial version
