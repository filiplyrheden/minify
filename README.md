# Minify - URL Shortener

A modern URL shortener application built with Next.js, TypeScript, and Zod
validation. All URLs are saved in a Vercel Postgres (Neon) database with a
beautiful UI using shadcn/ui components and Tailwind CSS. Transform lengthy URLs
into clean, shareable links that are perfect for social media, marketing
campaigns, and professional communications. Track engagement with built-in
analytics, set expiration dates for temporary campaigns, and limit access with
view restrictions. Whether you're a content creator, marketer, or developer,
Minify provides the tools to manage and monitor your links effectively while
maintaining a professional appearance across all platforms.

## Features

- Shorten any URL with custom or auto-generated codes
- Expiration dates - Set when links should expire
- View limits - Limit the maximum number of clicks
- User authentication - Create account to save and manage URLs
- Dashboard - Update and track your shortened URLs
- QR codes - Generate QR codes for easy sharing
- Modern UI - Beautiful interface with dark/light mode support
- Analytics - Track clicks and usage statistics

## Tech Stack

- **Framework**: Next.js 14 with App Router
- **Language**: TypeScript
- **Database**: Vercel Postgres (Neon provider)
- **Authentication**: NextAuth.js
- **Styling**: Tailwind CSS + shadcn/ui components
- **Validation**: Zod schemas
- **Forms**: React Hook Form
- **Deployment**: Vercel

## Prerequisites

Before you start, make sure you have:

- Node.js (version 18+) - [Download here](https://nodejs.org/)
- Vercel account - [Sign up here](https://vercel.com/)
- GitHub account - [Sign up here](https://github.com/)

### Data Validation

The application uses Zod schemas for robust data validation:

- **URL validation** in `/api/url/schema.ts`
- **User validation** in `/api/user/url/schema.ts`
- **Input sanitization** for all user inputs
- **Type safety** with TypeScript integration

### Authentication Flow

1. **Public usage**: Anyone can create short URLs without account
2. **Registration**: Users can create accounts to manage URLs
3. **Login**: Authenticated users access personal dashboard
4. **Protected routes**: Dashboard and URL management require login
5. **Session management**: NextAuth handles secure sessions

---

## Installation Guide

### Step 1: Clone and Install

# Clone the repository

git clone https://github.com/arebrinkemil/minify.git

# Navigate to project directory

cd minify

# Install dependencies

npm install

### Step 2: Set Up Vercel Account

1. Go to [vercel.com](https://vercel.com)
2. Sign up with GitHub (recommended)
3. Authorize Vercel to access your GitHub account

### Step 3: Create Vercel Postgres Database

#### 3.1 Access Storage Section

1. In Vercel Dashboard → Click "Storage" (top navigation)
2. Click "Create Database"
3. Select "Postgres"

#### 3.2 Configure Database

1. Choose "Neon" as your provider
2. Database settings:
   - **Name**: `minify-db` (or your preferred name)
   - **Region**: Europe (or closest to your location)
   - **Plan**: Start with Free tier
3. Click "Create" and wait 1-2 minutes

#### 3.3 Get Database Credentials

1. Once created, click "Get started with Neon"
2. Find the ".env.local" tab
3. Copy ALL environment variables shown

### Step 4: Configure Environment Variables

#### 4.1 Create Local Environment File

# In your project root (same level as package.json)

touch .env.local

#### 4.2 Add Database Variables

Paste the variables you copied from Neon, plus add NextAuth configuration to
your `.env.development.local` or `.env.local`:

# Neon Database Variables (paste from Vercel)

POSTGRES_DATABASE="neondb" POSTGRES_HOST="ep-xxx-pooler.eu-west-2.aws.neon.tech"
POSTGRES_PASSWORD="your_password"
POSTGRES_PRISMA_URL="postgresql://neondb_owner:password@host/neondb?connect_timeout=15&sslmode=require"
POSTGRES_URL="postgresql://neondb_owner:password@host/neondb?sslmode=require"
POSTGRES_URL_NON_POOLING="postgresql://neondb_owner:password@host/neondb?sslmode=require"
POSTGRES_URL_NO_SSL="postgresql://neondb_owner:password@host/neondb"
POSTGRES_USER="neondb_owner"

# NextAuth Configuration

NEXTAUTH_URL="http://localhost:3000" NEXTAUTH_SECRET=""

# Application Configuration

NEXT_PUBLIC_BASE_URL="http://localhost:3000"

#### 4.3 Generate NextAuth Secret

# Generate a secure random secret

openssl rand -base64 32

Copy the output and add it to your `.env.development.local` or `.env.local` if
you chose that option:

NEXTAUTH_SECRET="your-generated-secret-here"

### Step 5: Initialize Database

# Run the database seed script

npm run seed

Expected Output:

Created "users" table Created "urls" table Database seeded successfully

### Step 6: Start Development Server

# Start the development server

npm run dev

Expected Output:

▲ Next.js 14.2.3

- Local: http://localhost:3000

✓ Ready in 2.5s

Open [http://localhost:3000](http://localhost:3000) to see your application!

## Database Schema

The application uses two main tables:

### Users Table

```sql
CREATE TABLE users (
  id UUID DEFAULT uuid_generate_v4() PRIMARY KEY,
  name VARCHAR(255) NOT NULL,
  email TEXT NOT NULL UNIQUE,
  password TEXT NOT NULL
);
```

### URLs Table

```sql
CREATE TABLE urls (
  user_id UUID REFERENCES users(id),
  original_url TEXT NOT NULL,
  short_url TEXT NOT NULL PRIMARY KEY,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  expires_at TIMESTAMP,
  views INT DEFAULT 0,
  max_views INT
);
```

**Key Features:**

- UUID-based IDs for better security
- Foreign key relationship between users and URLs
- Automatic timestamps for creation tracking
- Built-in analytics with view counters
- Expiration system for temporary links

---

## Deploy to Production

### Step 1: Push to GitHub

git add . git commit -m "Initial setup with database configuration"

### Step 2: Deploy on Vercel

1. Go to [vercel.com/dashboard](https://vercel.com/dashboard)
2. Click "New Project"
3. Import your repository from GitHub
4. Vercel will auto-detect Next.js configuration
5. Click "Deploy"

### Step 3: Configure Production Environment

1. In your Vercel project → Settings → Environment Variables
2. Add all variables from your `.env.local` or `.env.development.local`
3. Update production URLs: NEXTAUTH_URL=https://your-project.vercel.app
   NEXT_PUBLIC_BASE_URL=https://your-project.vercel.app

4. Redeploy to apply changes

## UI/UX Features

### Design System

- **shadcn/ui components** - High-quality, accessible components
- **Tailwind CSS** - Utility-first styling approach
- **CSS Variables** - Dynamic theming support
- **Dark/Light mode** - Automatic theme switching
- **Responsive design** - Mobile-first approach

### Theme Configuration

The app uses a sophisticated color system with CSS variables:

- **Primary/Secondary** colors for branding
- **Muted/Accent** colors for subtle UI elements
- **Destructive** colors for errors and warnings
- **Card/Popover** specific styling for components

---

## Available Scripts

# Development

npm run dev # Start development server npm run build # Create production build
npm run start # Start production server

# Database

npm run seed # Initialize database with schema

# Code Quality

npm run lint # Run ESLint code quality checks

# Deployment

vercel # Deploy to Vercel (requires Vercel CLI)

---

## Troubleshooting

### Database Connection Issues

**Error: "Database connection failed"**

# Check environment variables

cat .env.local | grep POSTGRES

# Verify database is active in Vercel Dashboard

# Retry seed command

npm run seed

### NextAuth Configuration Issues

**Error: "NEXTAUTH_SECRET missing"**

# Generate new secret

openssl rand -base64 32

# Add to .env.local

echo 'NEXTAUTH_SECRET="your-new-secret"' >> .env.local

**Error: "Invalid callback URL"**

- Check `NEXTAUTH_URL` matches your current domain
- Local: `http://localhost:3000`
- Production: `https://your-app.vercel.app`

### Build and Runtime Issues

**Error: "Module not found"**

# Clear cache and reinstall

rm -rf node_modules package-lock.json .next npm install

**Error: "Port 3000 already in use"**

# Kill processes on port 3000

killall -9 node

# Or use different port

npm run dev -- --port 3001

### Seed Script Issues

**Error: "Cannot connect to database"**

- Ensure database is "Active" in Vercel Dashboard
- Check all `POSTGRES_*` variables in `.env.local`or `.env.development.local`
- Try creating database again if needed

---

## Security Considerations

### Environment Variables

- Never commit `.env.local` to version control
- Use strong, unique `NEXTAUTH_SECRET` in production
- Rotate database credentials periodically
- Keep sensitive tokens secure

### Database Security

- UUID-based IDs prevent enumeration attacks
- Foreign key constraints maintain data integrity
- Connection pooling via Neon for performance
- SSL-required connections in production

---

## Performance Features

### Database Optimizations

- Connection pooling with Neon/Vercel Postgres
- Indexed primary keys for fast lookups
- Efficient foreign key relationships
- Automatic connection management

### Next.js Optimizations

- App Router for better performance
- Server Components for reduced client bundle
- Automatic code splitting per route
- Image optimization built-in

### Vercel Platform Benefits

- Edge deployment for global performance
- Automatic HTTPS and CDN
- Zero-config deployment
- Built-in analytics available

## Contributing

Want to help improve Minify? Here are some ways to contribute:

### Good First Issues

- Improve UI/UX design and animations
- Add more comprehensive error handling
- Enhance mobile responsiveness
- Add internationalization (i18n)
- Implement advanced analytics features
- Add bulk URL operations
- Create API rate limiting

### Development Setup

# Fork the repository on GitHub

# Clone your fork

git clone https://github.com/YOUR-USERNAME/minify.git

# Create feature branch

git checkout -b feature/amazing-feature

# Make changes and test

npm run dev npm run lint

# Commit and push

git commit -m "Add amazing feature" git push origin feature/amazing-feature

# Create Pull Request on GitHub

## Support & Resources

### Documentation Links

- [Next.js Documentation](https://nextjs.org/docs)
- [Vercel Postgres Docs](https://vercel.com/docs/storage/vercel-postgres)
- [NextAuth.js Guide](https://next-auth.js.org/)
- [shadcn/ui Components](https://ui.shadcn.com/)
- [Tailwind CSS Docs](https://tailwindcss.com/docs)

### Getting Help

1. Check this troubleshooting guide first
2. Search existing issues on GitHub
3. Create a new issue with:
   - Clear problem description
   - Steps to reproduce
   - Environment details (OS, Node version, etc.)
   - Error messages and logs

### Community

- [GitHub Issues](https://github.com/arebrinkemil/minify/issues) - Bug reports
  and feature requests
- [GitHub Discussions](https://github.com/arebrinkemil/minify/discussions) -
  General questions and ideas

## Roadmap & Future Features

### Planned Enhancements

- [ ] Custom domains - Use your own domain for short URLs
- [ ] Advanced analytics - Detailed click tracking and geographic data
- [ ] Bulk operations - Import/export multiple URLs
- [ ] API endpoints - RESTful API for integrations
- [ ] Team collaboration - Share URLs across team members
- [ ] Link previews - Generate social media previews
- [ ] Password protection - Secure sensitive links
- [ ] Custom QR codes - Branded QR code generation

### Technical Improvements

- [ ] Redis caching - Faster URL resolution
- [ ] Rate limiting - Prevent abuse
- [ ] Email notifications - Link expiration alerts
- [ ] Database migrations - Version-controlled schema changes
- [ ] End-to-end testing - Comprehensive test coverage

**Ready to start shortening URLs? Your Minify application should now be fully
functional!**

**Happy link shortening!**
