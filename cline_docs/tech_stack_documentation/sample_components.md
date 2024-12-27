# Sample Components Documentation

## Overview

The Sample components directory provides base component templates and corresponding design images for the job board platform. Each component starts with a basic Screen wrapper component that provides consistent background styling.

## Base Component Structure

All components currently share this base structure:

```jsx
import React from 'react';

const styles = {
  Screen: {
    backgroundColor: '#fdfdfd',
  },
};

const Screen = (props) => {
  return (
    <div style={styles.Screen}>
      {props.children}
    </div>
  );
};

export default Screen;
```

This structure will be enhanced with our tech stack:
- TypeScript for type safety
- Tailwind CSS for styling
- shadcn/ui for UI components
- Next.js features for routing and data fetching

## Component Overview & Design References

### 1. Landing Page (landing_page.jsx)
- Design Reference: "1. Home.png"
- Purpose: Main entry point of the application
- Key Features:
  - Hero section with job search
  - Featured job listings
  - Category navigation
  - Company highlights

### 2. Jobs Listing (jobs.jsx)
- Design Reference: "2. Explore.png"
- Purpose: Display job listings with filtering
- Key Features:
  - Job cards with key information
  - Filter sidebar
  - Search functionality
  - Pagination

### 3. Job Details (job_details.jsx)
- Design Reference: "3. Job detail.png"
- Purpose: Detailed view of a specific job
- Key Features:
  - Job description
  - Company information
  - Application button
  - Similar jobs
  - Google Jobs schema integration

### 4. Job Application (job_application.jsx)
- Design Reference: "4. Job Application.png"
- Purpose: Job application form
- Key Features:
  - Personal information form
  - Resume upload
  - Cover letter input
  - Application tracking

### 5. Advanced Search (advanced_search.jsx)
- Design Reference: "5. Advanced search.png"
- Purpose: Detailed job search interface
- Key Features:
  - Multiple filter options
  - Salary range selection
  - Location search
  - Job type filters
  - Save search functionality

### 6. Company Profile (company_profile.jsx)
- Design Reference: "6. Company profile.png"
- Purpose: Company information and jobs
- Key Features:
  - Company details
  - Current job openings
  - Company culture
  - Reviews and ratings

### 7. Admin Dashboard (admin_dashboard.jsx)
- Design Reference: "Admin Dashboard.png"
- Purpose: Main admin interface
- Key Features:
  - Analytics overview
  - Recent activities
  - Quick actions
  - System status

### 8. Admin Job Postings (admin_job_postings_dashboard.jsx)
- Design Reference: "Admin Job Postings Management.png"
- Purpose: Job posting management
- Key Features:
  - Job listing table
  - Status management
  - Bulk actions
  - Search and filters

### 9. Authentication (login.jsx)
- Design Reference: "Auth Screen for Job Seekers and Employers.png"
- Purpose: User authentication
- Key Features:
  - Login form
  - Registration options
  - Role selection
  - Social auth

### 10. Employer Dashboard (employer_dashboard.jsx)
- Design Reference: "Employer Dashboard.png"
- Purpose: Employer management interface
- Key Features:
  - Job posting management
  - Applicant tracking
  - Analytics
  - Company profile management

### 11. Pricing Page (pricing_page.jsx)
- Design Reference: "Pricing Page.png"
- Purpose: Subscription plans display
- Key Features:
  - Plan comparison
  - Feature list
  - Pricing details
  - Subscribe buttons

## Implementation Guidelines

### 1. Component Structure
```typescript
// src/components/[ComponentName]/[ComponentName].tsx
import { FC } from 'react'
import { cn } from '@/lib/utils'

interface [ComponentName]Props {
  className?: string
  // Add other props
}

export const [ComponentName]: FC<[ComponentName]Props> = ({
  className,
  // Destructure other props
}) => {
  return (
    <div className={cn('bg-background min-h-screen', className)}>
      {/* Component content */}
    </div>
  )
}
```

Required utility setup:
```typescript
// src/lib/utils.ts
import { type ClassValue, clsx } from 'clsx'
import { twMerge } from 'tailwind-merge'

export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs))
}
```

### 2. Styling Approach
- Use Tailwind CSS utility classes
- Leverage shadcn/ui components
- Follow mobile-first responsive design
- Use CSS variables for theming
- Maintain consistent spacing

### 3. State Management
- Use React Query for server state
- Local state with useState/useReducer
- Context for shared state
- Type-safe API calls
- Optimistic updates

### 4. Performance Considerations
- Implement lazy loading
- Use Next.js Image component
- Optimize API calls
- Implement proper caching
- Add loading states

### 5. Accessibility
- Semantic HTML
- ARIA labels
- Keyboard navigation
- Color contrast
- Screen reader support

### 6. Testing Strategy
- Unit tests for logic
- Component testing
- Integration tests
- E2E testing
- Accessibility testing

## Next Steps

1. Convert components to TypeScript
2. Implement shadcn/ui components
3. Add Tailwind CSS styling
4. Implement responsive design
5. Add proper state management
6. Integrate with backend APIs
7. Add proper error handling
8. Implement loading states
9. Add proper testing
10. Ensure accessibility compliance

## Resources

- [shadcn/ui Components](https://ui.shadcn.com)
- [Tailwind CSS Documentation](https://tailwindcss.com/docs)
- [Next.js Documentation](https://nextjs.org/docs)
- [TypeScript Handbook](https://www.typescriptlang.org/docs)
- [React Query Documentation](https://tanstack.com/query/latest)
