# Brokerr - Advanced Crypto Trading Platform

A modern, professional-grade cryptocurrency trading platform built with React, Vite, and Supabase. Features include portfolio management, copy trading, admin dashboard, and comprehensive email notifications.

## Live Demo

**Staging:** [brokerr-staging.vercel.app](https://brokerr-staging.vercel.app)  
**Production:** [brokerr.vercel.app](https://brokerr.vercel.app)

---

## Features

### User Features
- **Dashboard** - Real-time portfolio overview, asset allocations, performance metrics
- **Portfolio Management** - Track holdings, cost basis, current value, P&L analysis
- **Trading** - Buy/sell cryptocurrencies with order management
- **Copy Trading** - Follow and auto-copy trades from professional traders
- **Transaction History** - Detailed logs of all deposits, withdrawals, and trades
- **Settings** - Profile management, security settings, email preferences

### Admin Features
- **Admin Dashboard** - Platform metrics, user analytics, system monitoring
- **Manage Users** - User administration, account status, balance management
- **Manage Cryptos** - Add/edit cryptocurrencies, pricing, trading pairs
- **Manage Traders** - Copy trading trader management and performance tracking
- **Transaction Management** - Approve/deny deposits and withdrawals
- **Platform Settings** - System configuration and business rules

### Core Features
- **Supabase Authentication** - Secure email/password authentication
- **Row-Level Security** - Database-level access control for multi-tenant isolation
- **Email Notifications** - Professional HTML email templates for key events
- **Role-Based Access Control** - Admin and user roles with proper route protection
- **Real-Time Data** - Live portfolio updates and transaction tracking

---

## Tech Stack

| Category | Technology |
|----------|------------|
| **Frontend** | React 18 + Vite + React Router |
| **Styling** | Tailwind CSS + shadcn/ui components |
| **Database** | Supabase (PostgreSQL) |
| **Authentication** | Supabase Auth |
| **Email** | Resend.io (or SendGrid alternative) |
| **Deployment** | Vercel |
| **State Management** | React Context API |

---

## Project Structure

```
brokerr/
├── src/
│   ├── pages/
│   │   ├── Dashboard.jsx           # User dashboard
│   │   ├── Portfolio.jsx           # Portfolio view
│   │   ├── Trade.jsx               # Trading interface
│   │   ├── CopyTrading.jsx         # Copy trading page
│   │   ├── Transactions.jsx        # Transaction history
│   │   ├── Settings.jsx            # User settings
│   │   ├── Landing.jsx             # Landing page
│   │   └── admin/
│   │       ├── AdminDashboard.jsx
│   │       ├── ManageUsers.jsx
│   │       ├── ManageCryptos.jsx
│   │       ├── ManageTraders.jsx
│   │       ├── AdminTransactions.jsx
│   │       └── PlatformSettings.jsx
│   ├── components/
│   │   ├── layout/
│   │   │   ├── AppLayout.jsx       # Main app layout
│   │   │   ├── Sidebar.jsx
│   │   │   ├── TopBar.jsx
│   │   │   └── MobileBottomNav.jsx
│   │   └── ui/                      # shadcn/ui components
│   ├── lib/
│   │   ├── AuthContext.jsx         # Auth state management
│   │   ├── emailTemplates.js       # Email HTML templates
│   │   └── utils.js
│   ├── api/
│   │   ├── supabaseClient.js       # Supabase configuration
│   │   ├── base44Client.js         # Database adapter
│   │   └── emailService.js         # Email sending service
│   ├── App.jsx                     # Main app with routing
│   ├── main.jsx                    # Entry point
│   ├── schema.sql                  # Database schema
│   └── index.css                   # Global styles
├── public/
├── .env.example                    # Environment variables template
├── package.json
├── vite.config.js
├── tailwind.config.js
├── index.html
└── README.md
```

---

## Getting Started

### Prerequisites

- Node.js 16+ and pnpm (or npm)
- Supabase account (free tier available)
- Resend.io account (for email)
- Vercel account (for deployment)

### Local Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/yourusername/brokerr.git
   cd brokerr
   ```

2. **Install dependencies**
   ```bash
   pnpm install
   ```

3. **Set up environment variables** (see Environment Setup section below)
   ```bash
   cp .env.example .env.development.local
   # Edit .env.development.local with your credentials
   ```

4. **Start the development server**
   ```bash
   pnpm dev
   ```

5. **Open in browser**
   ```
   http://localhost:5173
   ```

---

## Environment Setup

### Required Environment Variables

Create a `.env.development.local` file in the project root with the following variables:

```env
# Supabase Configuration
VITE_SUPABASE_URL=https://your-project.supabase.co
VITE_SUPABASE_ANON_KEY=your-anon-key-here

# Email Provider (Resend recommended)
VITE_EMAIL_PROVIDER=resend
VITE_RESEND_API_KEY=re_your-api-key-here
VITE_EMAIL_FROM=Brokerr <noreply@brokerr.io>

# Optional
VITE_ADMIN_EMAIL=admin@brokerr.io
```

### Obtaining Credentials

#### Supabase Setup

1. **Create a Supabase project**
   - Go to [supabase.com](https://supabase.com)
   - Click "New Project"
   - Choose a name and region
   - Wait for provisioning (2-3 minutes)

2. **Get your credentials**
   - In Project Settings > API
   - Copy the **Project URL** → `VITE_SUPABASE_URL`
   - Copy the **Anon Key** (public key) → `VITE_SUPABASE_ANON_KEY`
   - ⚠️ Never use the Service Role Key in frontend code

3. **Initialize the database schema**
   - Go to SQL Editor
   - Click "New Query"
   - Copy contents of `src/schema.sql`
   - Paste and click "Run"
   - Wait for completion

4. **Enable Row-Level Security (RLS)**
   - Go to Authentication > Policies
   - Policies are defined in `src/schema.sql`
   - Verify all tables have RLS enabled

#### Resend Email Setup

1. **Create a Resend account**
   - Go to [resend.com](https://resend.com)
   - Sign up with your email
   - Verify your email address

2. **Create an API key**
   - Click "API Keys" in left sidebar
   - Click "Create API Key"
   - Name it "Brokerr Development"
   - Copy the key → `VITE_RESEND_API_KEY`

3. **Verify your sender email**
   - Go to "Domains"
   - Add your domain or use Resend's default email
   - Verify ownership
   - Set as default sender → `VITE_EMAIL_FROM`

---

## Supabase Configuration

### Database Schema

The database includes the following tables (automatically created by `src/schema.sql`):

| Table | Purpose | Key Fields |
|-------|---------|-----------|
| `users` | User accounts | email, role, created_at |
| `user_balances` | Account balances | user_email, total_usd, available_usd |
| `portfolio` | Asset holdings | user_email, crypto_symbol, quantity, cost_basis |
| `transactions` | Deposits/withdrawals/trades | user_email, type, amount, status |
| `copy_trades` | Copy trading settings | user_email, trader_email, is_active |
| `copy_traders` | Professional trader profiles | email, win_rate, total_return |
| `cryptocurrencies` | Crypto metadata | symbol, name, current_price |
| `email_notifications` | Email logs | recipient_email, template_type, status |
| `platform_settings` | System configuration | key, value, updated_at |

### Row-Level Security (RLS) Policies

All tables have RLS enabled with the following policies:

**Users Table:**
- Admins can read all users
- Users can only read their own record

**User Balances Table:**
- Admins can read/write all
- Users can only read their own balance

**Transactions Table:**
- Admins can read/write all
- Users can only read their own transactions

**Portfolio Table:**
- Admins can read all
- Users can only read their own portfolio

See `src/schema.sql` for complete RLS policy definitions.

### Authentication

Supabase provides native authentication with:
- Email/password sign-up and login
- Password reset via email
- Email verification tokens
- Secure session management (HTTP-only cookies)

The `AuthContext` component (`src/lib/AuthContext.jsx`) handles:
- User session management
- Role detection (admin vs. user)
- Route protection
- Login/logout flows

---

## Email System

### Email Configuration

The email system sends professional HTML emails for key events:

**Email Templates** (`src/lib/emailTemplates.js`):
- Welcome Email
- Deposit Approved / Denied
- Withdrawal Approved / Denied
- Trade Confirmation
- Copy Trade Activated / Deactivated

**Email Service** (`src/api/emailService.js`):
- Handles sending via Resend API
- Logs all emails to `email_notifications` table
- Error handling and retry logic

### Sending an Email

```javascript
import { sendDepositApprovedEmail } from '@/api/emailService';

// Send email
await sendDepositApprovedEmail(
  'John Doe',
  'john@example.com',
  1500,
  'USD',
  '0x1234...deposit_address'
);

// Email logged to database for audit trail
```

All emails are logged with status tracking:
- `pending` → Queued for sending
- `sent` → Successfully delivered
- `failed` → Error occurred
- `bounced` → Email bounced

---

## Deployment

### Deploy to Vercel

1. **Push code to GitHub**
   ```bash
   git add .
   git commit -m "Deploy to Vercel"
   git push origin main
   ```

2. **Connect to Vercel**
   - Go to [vercel.com](https://vercel.com)
   - Click "New Project"
   - Import your GitHub repository
   - Vercel auto-detects Vite configuration

3. **Add Environment Variables**
   - In project settings, click "Environment Variables"
   - Add each variable from `.env.example`
   - Select production environment
   - Click "Save"

4. **Deploy**
   - Click "Deploy"
   - Wait for build completion (2-5 minutes)
   - Your app is live!

### Production Environment Variables

Set these in Vercel Dashboard > Settings > Environment Variables:

```env
VITE_SUPABASE_URL=https://your-project.supabase.co
VITE_SUPABASE_ANON_KEY=your-production-key
VITE_EMAIL_PROVIDER=resend
VITE_RESEND_API_KEY=your-production-resend-key
VITE_EMAIL_FROM=Brokerr <noreply@brokerr.io>
VITE_ADMIN_EMAIL=admin@brokerr.io
```

### CI/CD Pipeline

Vercel automatically:
- Builds on every push to main branch
- Runs linting and format checks
- Deploys to preview on pull requests
- Deploys to production on merge

---

## API Reference

### Authentication API

```javascript
import { useAuth } from '@/lib/AuthContext';

const { user, isAdmin, isAuthenticated, logout } = useAuth();
```

### Database API

```javascript
import { dbAPI } from '@/api/supabaseClient';

// Get user profile
const { data, error } = await dbAPI.getUserProfile(userEmail);

// Get user balance
const { data, error } = await dbAPI.getUserBalance(userEmail);

// Get user portfolio
const { data, error } = await dbAPI.getUserPortfolio(userEmail);

// Get transactions
const { data, error } = await dbAPI.getUserTransactions(userEmail);

// Create deposit request
const { data, error } = await dbAPI.createDepositRequest(
  userEmail,
  1500,
  'USD'
);

// Create withdrawal request
const { data, error } = await dbAPI.createWithdrawalRequest(
  userEmail,
  1000,
  'USD',
  '0x1234...'
);
```

### Base44 Client (Database Adapter)

```javascript
import { base44 } from '@/api/base44Client';

// Get all users
const users = await base44.query('users');

// Get user by email
const user = await base44.query('users', {
  column: 'email',
  value: 'user@example.com'
});

// Insert new record
const result = await base44.insert('users', {
  email: 'new@example.com',
  role: 'user'
});

// Update record
const result = await base44.update('users', {
  email: 'user@example.com',
  last_login: new Date()
});

// Delete record
const result = await base44.delete('users', {
  column: 'email',
  value: 'user@example.com'
});
```

---

## Development

### Available Scripts

```bash
# Start development server
pnpm dev

# Build for production
pnpm build

# Preview production build locally
pnpm preview

# Run linter
pnpm lint

# Format code
pnpm format
```

### Code Style

- **JavaScript**: ES6+
- **React**: Functional components with hooks
- **CSS**: Tailwind CSS with shadcn/ui components
- **Linting**: ESLint with recommended rules

### Component Structure

```javascript
// Example component following best practices
import { useState, useEffect } from 'react';
import { Button } from '@/components/ui/button';

export function MyComponent() {
  const [state, setState] = useState(null);

  useEffect(() => {
    // Side effects here
  }, []);

  return (
    <div className="p-4">
      <Button onClick={() => setState(!state)}>
        Toggle
      </Button>
    </div>
  );
}
```

### Best Practices

- Use React hooks (useState, useEffect, useContext)
- Keep components small and focused
- Use Tailwind CSS for styling
- Access database through `dbAPI` or `base44`
- Handle errors gracefully with try/catch
- Log important events for debugging

---

## Security

### Authentication & Authorization

- **Supabase Auth**: Industry-standard email/password authentication
- **Session Management**: Secure HTTP-only cookies
- **Role-Based Access**: Admin vs. user roles enforced at database level
- **Row-Level Security**: Database-enforced data isolation per user

### Data Protection

- **HTTPS Only**: All connections encrypted in transit
- **SQL Injection Prevention**: Parameterized queries through Supabase SDK
- **XSS Prevention**: React's built-in HTML escaping
- **CSRF Protection**: Supabase handles CSRF tokens

### Best Practices

- Never commit `.env` files with real credentials
- Use environment variables for all secrets
- Verify admin role before admin operations
- Validate user input on frontend and backend
- Keep Supabase RLS policies updated
- Monitor email logs for bounced emails

---

## Troubleshooting

### Common Issues

**Issue: "VITE_SUPABASE_URL is not defined"**
- Solution: Check `.env.development.local` exists with correct variable names
- Restart dev server after adding env variables

**Issue: "User authentication failed"**
- Solution: Verify Supabase credentials are correct
- Check that user exists in Supabase Auth
- Verify email is confirmed

**Issue: "Database query returned no data"**
- Solution: Check table name is correct (use `user_balances` not `user_balance`)
- Verify RLS policies allow the query
- Check user has data in that table

**Issue: "Email not sending"**
- Solution: Verify Resend API key is valid
- Check VITE_EMAIL_FROM is set correctly
- Check email logs in Supabase `email_notifications` table

**Issue: "Admin route shows 404"**
- Solution: Verify you're logged in as admin
- Check user role is set to 'admin' in Supabase
- Check AuthContext properly detects role

### Getting Help

1. **Check documentation**: See files in `/docs` folder
2. **Check logs**: Browser console + Supabase logs
3. **Verify credentials**: Re-check all env variables
4. **Restart dev server**: `pnpm dev`
5. **Clear cache**: Delete `.env.development.local` and recreate

---

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit changes (`git commit -m 'Add amazing feature'`)
4. Push to branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

---

## License

This project is licensed under the MIT License - see LICENSE file for details.

---

## Support

For issues, questions, or feature requests:
- Open a GitHub Issue
- Email: support@brokerr.io
- Website: brokerr.io

---

## Changelog

### Version 1.0.0 (Current)
- Initial release with full feature set
- Admin dashboard and user portal
- Email notification system
- Copy trading functionality
- Production-ready with Supabase + Vercel

---

## Acknowledgments

Built with:
- [React](https://react.dev)
- [Vite](https://vitejs.dev)
- [Supabase](https://supabase.com)
- [Tailwind CSS](https://tailwindcss.com)
- [shadcn/ui](https://ui.shadcn.com)
- [Resend](https://resend.com)
- [Vercel](https://vercel.com)
