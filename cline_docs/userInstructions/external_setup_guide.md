# External Setup Guide

## Required External Services

### 1. Supabase Setup

#### Creating a Supabase Project

1. Go to [Supabase Dashboard](https://app.supabase.com)
2. Click "New Project"
3. Fill in project details:
   - Name: myabajobs
   - Database Password: (create secure password)
   - Region: (select nearest)
4. Click "Create Project"

#### Getting Credentials

1. Go to Project Settings > API
2. Copy these values:
   - Project URL
   - anon/public key
   - service_role key (keep secure)

### 2. Google Search Console Setup

#### Register Your Site

1. Go to [Google Search Console](https://search.google.com/search-console)
2. Click "Add Property"
3. Choose "URL prefix" or "Domain"
4. Verify ownership using one of the provided methods
5. Complete verification process

#### Enable Indexing API

1. Go to Google Cloud Console
2. Create new project or select existing
3. Enable Indexing API
4. Create service account
5. Download credentials JSON

### 3. Stripe Integration

#### Create Stripe Account

1. Go to [Stripe Dashboard](https://dashboard.stripe.com)
2. Sign up for an account
3. Complete business verification

#### Get API Keys

1. Go to Developers > API Keys
2. Copy these values:
   - Publishable key
   - Secret key
3. Create webhook endpoint
4. Note webhook signing secret

### 4. PayPal Integration

#### Create PayPal Business Account

1. Go to [PayPal Developer](https://developer.paypal.com)
2. Sign up for business account
3. Verify business details

#### Get API Credentials

1. Go to My Apps & Credentials
2. Create new app
3. Copy these values:
   - Client ID
   - Secret key
4. Configure webhook URLs

### 5. Sentry Error Tracking

#### Create Sentry Project

1. Go to [Sentry](https://sentry.io)
2. Create new project
3. Select Next.js platform
4. Copy DSN key

#### Configure Project

1. Set up environment names
2. Configure alert rules
3. Set up team access
4. Enable source maps

### 6. Vercel Deployment

#### Connect Repository

1. Go to [Vercel](https://vercel.com)
2. Click "New Project"
3. Import your repository
4. Configure build settings

#### Configure Environment

1. Add environment variables
2. Set up custom domain
3. Configure deployment regions
4. Set up team access

## Environment Variables Setup

### Local Development

Create `.env.local` file:

```env
# Supabase
NEXT_PUBLIC_SUPABASE_URL=your-project-url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-anon-key
SUPABASE_SERVICE_ROLE_KEY=your-service-key

# Stripe
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY=your-publishable-key
STRIPE_SECRET_KEY=your-secret-key
STRIPE_WEBHOOK_SECRET=your-webhook-secret

# PayPal
NEXT_PUBLIC_PAYPAL_CLIENT_ID=your-client-id
PAYPAL_SECRET_KEY=your-secret-key

# Sentry
NEXT_PUBLIC_SENTRY_DSN=your-dsn-key

# Google
GOOGLE_APPLICATION_CREDENTIALS=path-to-credentials.json
```

### Production Environment

Configure these in Vercel:

1. Go to Project Settings > Environment Variables
2. Add all variables from `.env.local`
3. Ensure proper encryption
4. Set environment-specific values

## Security Considerations

### API Keys

- Never commit API keys to repository
- Use environment variables
- Rotate keys regularly
- Monitor key usage

### Webhook Security

- Validate webhook signatures
- Use HTTPS endpoints
- Implement retry logic
- Log all webhook events

### Database Access

- Use connection pooling
- Implement row-level security
- Regular security audits
- Monitor access patterns

## Monitoring Setup

### Error Tracking

1. Configure Sentry:
   - Set up environments
   - Configure alert rules
   - Enable source maps
   - Set up team notifications

### Performance Monitoring

1. Set up Vercel Analytics
2. Configure Google Analytics
3. Set up database monitoring
4. Configure custom metrics

## Backup Procedures

### Database Backups

1. Configure Supabase backups:
   - Daily automated backups
   - Point-in-time recovery
   - Backup retention policy
   - Backup notifications

### Code Backups

1. Set up repository mirroring
2. Configure branch protection
3. Implement backup automation
4. Regular backup testing

## Troubleshooting Guide

### Common Issues

#### Database Connection

- Check connection string
- Verify network access
- Check firewall rules
- Validate credentials

#### API Integration

- Verify API keys
- Check request format
- Validate endpoints
- Monitor rate limits

#### Authentication

- Check JWT configuration
- Verify provider setup
- Validate user roles
- Monitor auth logs

## Support Resources

### Documentation

- [Supabase Docs](https://supabase.com/docs)
- [Stripe Docs](https://stripe.com/docs)
- [PayPal Docs](https://developer.paypal.com/docs)
- [Vercel Docs](https://vercel.com/docs)
- [Sentry Docs](https://docs.sentry.io)

### Support Channels

- Supabase Discord
- Stripe Support
- PayPal Developer Support
- Vercel Support
- Sentry Support

## Regular Maintenance

### Weekly Tasks

1. Review error logs
2. Check performance metrics
3. Monitor API usage
4. Verify backups
5. Update documentation

### Monthly Tasks

1. Rotate API keys
2. Security audit
3. Performance optimization
4. Update dependencies
5. Review access logs

## Emergency Procedures

### Service Outage

1. Check status pages
2. Review error logs
3. Contact support
4. Implement fallback
5. Notify team

### Security Incident

1. Rotate compromised keys
2. Review access logs
3. Contact support
4. Document incident
5. Implement fixes
