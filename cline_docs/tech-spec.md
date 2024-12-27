# Job Board Technical Specification

## 1. Google Jobs Integration Requirements

### Required Schema Properties

```typescript
interface IJobPosting {
  // Required Properties
  title: string;             // Job title only, no company names/locations
  description: string;       // Full HTML description with responsibilities & requirements
  datePosted: string;       // ISO 8601 format (e.g., "2024-01-24")
  hiringOrganization: {
    "@type": "Organization";
    name: string;           // Company name, not location
    sameAs?: string;        // Company website URL
    logo?: string;          // Company logo URL (ratio between 0.75 and 2.5)
  };
  jobLocation: {            // Physical work location
    "@type": "Place";
    address: {
      "@type": "PostalAddress";
      streetAddress: string;
      addressLocality: string; // City
      addressRegion: string;   // State/Province
      postalCode: string;
      addressCountry: string;  // Country code (required)
    };
  }[];

  // Highly Recommended Properties
  validThrough?: string;    // ISO 8601 format expiration date
  employmentType?: ("FULL_TIME" | "PART_TIME" | "CONTRACTOR" | "TEMPORARY" | 
                    "INTERN" | "VOLUNTEER" | "PER_DIEM" | "OTHER")[];
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

  // Remote Work Properties
  jobLocationType?: "TELECOMMUTE";
  applicantLocationRequirements?: {
    "@type": "Country" | "State";
    name: string;
  }[];
}
```

### Schema Validation Rules

1. Title Requirements:
   - Must be job title only
   - No company names, locations, or dates
   - No excessive special characters
   - Must match visible page content

2. Description Requirements:
   - Must be in HTML format
   - Must include full job details
   - Must include responsibilities and requirements
   - Must match visible page content
   - Supported HTML tags: <p>, <ul>, <li>
   - Must use <br>, <p>, or \n for paragraph breaks

3. Location Requirements:
   - Must include physical work location OR remote work details
   - Country code is required for all addresses
   - For remote jobs:
     - Set jobLocationType: "TELECOMMUTE"
     - Include applicantLocationRequirements
     - Must clearly state remote work in description

4. Date Requirements:
   - datePosted: Must be in ISO 8601 format
   - validThrough: Must be in ISO 8601 format
   - Expired jobs must be removed or marked expired

### Implementation Guidelines

1. Page Structure:

```typescript
interface IJobPage {
  canonicalUrl: string;     // Required for duplicate listings
  structuredData: {         // Placed in <head> as JSON-LD
    "@context": "https://schema.org/";
    "@type": "JobPosting";
    // ... job posting properties
  };
  content: {
    title: string;         // Visible job title
    description: string;   // Visible job description
    // ... other visible content
  };
}
```

2. Content Requirements:
   - One job posting per page
   - No job listings on search/category pages
   - All schema data must match visible content
   - Direct apply option must be available
   - No login walls before viewing full description

3. Technical Requirements:
   - Implement proper canonical URLs
   - Enable efficient Googlebot crawling
   - Use Indexing API for updates
   - Submit comprehensive sitemaps

## 2. Database Schema Updates

```sql
CREATE TABLE job_postings (
  id UUID PRIMARY KEY,
  title VARCHAR(255) NOT NULL,
  description TEXT NOT NULL,
  date_posted TIMESTAMP WITH TIME ZONE NOT NULL,
  valid_through TIMESTAMP WITH TIME ZONE,
  employment_type VARCHAR(50)[] NOT NULL,
  salary_min DECIMAL,
  salary_max DECIMAL,
  salary_currency VARCHAR(3),
  salary_interval VARCHAR(10),
  company_id UUID NOT NULL REFERENCES companies(id),
  is_remote BOOLEAN DEFAULT FALSE,
  remote_country_codes VARCHAR(2)[],
  physical_address JSONB,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  status VARCHAR(20) DEFAULT 'active',
  CONSTRAINT valid_salary_range CHECK (salary_max >= salary_min)
);

CREATE INDEX idx_job_postings_status ON job_postings(status);
CREATE INDEX idx_job_postings_date_posted ON job_postings(date_posted);
CREATE INDEX idx_job_postings_company_id ON job_postings(company_id);
```

## 3. API Updates

1. Job Posting Endpoints:

```typescript
// POST /api/jobs
interface ICreateJobRequest {
  title: string;
  description: string;
  employmentType: string[];
  salary?: {
    min: number;
    max: number;
    currency: string;
    interval: string;
  };
  location: {
    isRemote: boolean;
    remoteCountries?: string[];
    physicalAddress?: {
      street: string;
      city: string;
      state: string;
      postalCode: string;
      country: string;
    };
  };
  validThrough?: string;
}

// PUT /api/jobs/:id
interface IUpdateJobRequest extends Partial<ICreateJobRequest> {
  status?: 'active' | 'expired' | 'filled';
}
```

2. Validation Middleware:

```typescript
const validateJobPosting = (req: Request, res: Response, next: NextFunction) => {
  const schema = z.object({
    title: z.string()
      .min(3)
      .max(255)
      .regex(/^[^!@#$%^&*]*$/), // No special characters
    description: z.string()
      .min(100)  // Ensure meaningful description
      .includes(['responsibilities', 'requirements']), // Required sections
    // ... other validations
  });
  
  const result = schema.safeParse(req.body);
  if (!result.success) {
    return res.status(400).json({ errors: result.error.flatten() });
  }
  next();
};
```

## 4. Integration Testing

1. Schema Validation Tests:

```typescript
describe('Job Posting Schema', () => {
  test('validates required fields', () => {
    // Test all required fields
  });
  
  test('validates remote work requirements', () => {
    // Test remote work specific fields
  });
  
  test('validates salary ranges', () => {
    // Test salary formatting
  });
});
```

2. Content Validation Tests:

```typescript
describe('Job Posting Content', () => {
  test('matches schema data with visible content', () => {
    // Compare structured data with rendered HTML
  });
  
  test('validates HTML formatting', () => {
    // Check supported HTML tags
  });
});
```

## 5. Monitoring and Maintenance

1. Google Search Console Integration:
   - Monitor rich results status
   - Track indexing coverage
   - Monitor manual actions

2. Automated Checks:
   - Daily validation of active listings
   - Expiration date monitoring
   - Schema validation monitoring
   - Content matching verification

3. Error Handling:
   - Invalid schema detection
   - Expired job cleanup
   - Duplicate listing detection
   - Schema validation errors

4. Performance Monitoring:
   - Track Google Jobs clicks
   - Monitor application rates
   - Track listing visibility
   - Monitor crawl efficiency
