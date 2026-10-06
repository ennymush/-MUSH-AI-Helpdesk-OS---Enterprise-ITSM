# MUSH AI Helpdesk OS — Enterprise IT Service Management Platform

A production-grade AI-powered IT Service Management (ITSM) platform that combines AI automation with human IT support. Built for organizations deploying internally for hundreds or thousands of employees.

## Features

### 🔐 Authentication & User Management
- Secure sign up, login, forgot password, reset password
- Email verification with token-based system
- Session management with JWT authentication
- Secure password hashing (bcrypt, 12 rounds)
- Full user profiles: name, employee ID, email, phone, department, job title, office location, role

### 👥 Role-Based Access Control (RBAC)
- **Employee**: AI assistance, ticket creation, view own tickets, reply to comments, rate resolutions
- **IT Specialist**: Manage assigned tickets, accept/reject assignments, respond, internal notes, escalate, close tickets
- **IT Manager**: View all tickets, reassign, department performance, SLA config, workload monitoring, reporting
- **Administrator**: Full control - users, departments, roles, AI settings, audit logs, integrations

### 🤖 AI Workflow Engine
- AI is the first point of contact for every employee
- Natural language understanding with category and priority detection
- Automatic troubleshooting with step-by-step instructions
- Hardware issue detection requiring on-site assistance
- AI confidence scoring and full conversation history
- Auto-ticket creation with AI summary, category, priority, troubleshooting attempted
- **AI Resolved** marking for issues solved automatically

### 🎫 Ticket Management
- Full lifecycle: Open → AI Reviewing → Awaiting User → Assigned → In Progress → Waiting for Parts → Pending Approval → Resolved → Closed → Cancelled
- SLA timers with breach tracking
- Attachments, comments, internal notes, and activity timeline
- Auto-notifications for all events
- Ticket ratings and satisfaction tracking
- Knowledge base integration

### 📊 Analytics & Reporting
- Total/AI/Human resolved tickets
- Average resolution time
- SLA compliance rate
- Category/priority/department trends
- Technician performance tracking
- AI success rate
- User satisfaction ratings
- CSV export

### 📁 Asset Management
- Track laptops, printers, monitors, phones, servers, network devices
- Serial numbers, warranties, ownership, maintenance history
- Asset-ticket association

### 🔒 Security
- Helmet.js security headers
- Rate limiting (100 req/min API, 20/15min auth)
- Input validation with express-validator
- RBAC on every endpoint
- Secure file uploads (20MB limit, type validation)
- Comprehensive audit logging
- Server-side API key protection

## Quick Start

### Prerequisites
- Node.js 18+
- npm

### Installation

```bash
# 1. Install dependencies
npm install

# 2. Initialize the database
node scripts/seed.js

# 3. Start the server
npm start
```

Or simply run `Launch_ITSM_Platform.bat` on Windows.

### Access

- **Application**: http://localhost:5000
- **Login Page**: http://localhost:5000/login.html
- **API Health**: http://localhost:5000/api/health

### Default Accounts

| Role | Email | Password |
|------|-------|----------|
| Administrator | it@mushcorp.com | Admin@12345 |

New accounts can be created from the login page. They remain pending until the administrator approves them.

## Project Structure

```
├── server.js              # Main Express server with all API routes
├── db/
│   └── database.js        # SQLite database schema and initialization
├── middleware/
│   └── auth.js            # JWT authentication and RBAC middleware
├── utils/
│   └── audit.js           # Audit logging utility
├── scripts/
│   ├── seed.js            # Database seeding with demo data
│   └── test-api.js        # API endpoint tests
├── mush_ui/
│   ├── index.html         # Main application interface
│   ├── login.html         # Authentication pages
│   ├── app.js             # Frontend application logic
│   ├── proxy-client.js    # Secure proxy client
│   ├── styles.css         # Enterprise styling
│   └── pages.css          # Page-specific styles
└── data/                  # SQLite database (auto-created)
```

## API Overview

### Authentication
- `POST /api/auth/signup` - Create account
- `POST /api/auth/login` - Sign in
- `POST /api/auth/logout` - Sign out
- `POST /api/auth/verify-email` - Verify email
- `POST /api/auth/forgot-password` - Request password reset
- `POST /api/auth/reset-password` - Reset password
- `GET /api/auth/me` - Get current user

### Users & Departments
- `GET/POST /api/users` - Manage users (admin)
- `GET/POST /api/departments` - Manage departments

### Tickets
- `POST /api/tickets` - Create ticket
- `GET /api/tickets` - List tickets (role-filtered)
- `GET /api/tickets/:id` - Get ticket details
- `PUT /api/tickets/:id/status` - Update status
- `PUT /api/tickets/:id/assign` - Assign ticket
- `POST /api/tickets/:id/comments` - Add comment
- `POST /api/tickets/:id/attachments` - Upload files
- `POST /api/tickets/:id/rate` - Rate resolution

### AI Assistant
- `POST /api/ai/chat` - AI chat with automatic ticket creation
- `GET /api/ai/conversations` - List conversations

### Knowledge Base
- `GET/POST /api/kb/articles` - Manage articles
- `POST /api/kb/articles/:id/rate` - Rate helpfulness

### Assets
- `GET/POST /api/assets` - Manage assets

### Analytics
- `GET /api/analytics/overview` - Dashboard metrics

### Administration
- `GET /api/audit-logs` - View audit logs (admin)
- `GET/PUT /api/settings` - System settings (admin)
- `GET/PUT /api/sla` - SLA configuration (manager+)

## AI Configuration

Set either a proxy URL or API key in `.env`:

```env
# Option 1: Via secure proxy server
PROXY_URL=http://localhost:3000/api/proxy/chat
PROXY_SHARED_SECRET=your-secret

# Option 2: Direct NVIDIA NIM API
NV_API_KEY=your-nvidia-api-key

# Option 3: Direct OpenAI API
OPENAI_API_KEY=your-openai-api-key
```

## Useful Scripts

| Command | Purpose |
|---------|---------|
| `npm run seed` | (Re)initialize the database with demo data |
| `npm run cleanup:test-data` | Remove auto-generated test / pending accounts (dry-run without `--yes`) |
| `npm run fix:icons` | Re-apply Font Awesome v6 sidebar icon names |
| `npm test` | Site, API, login and feature smoke tests (server must be running) |

> **Tip:** `scripts/cleanup-test-data.js` runs in dry-run mode by default. Add `--yes` to delete.

## Production Deployment

1. Set a strong `JWT_SECRET` in `.env`
2. Configure `CORS_ORIGIN` for your domain
3. Set up SMTP for email notifications
4. Configure strong passwords for all accounts
5. Run behind a reverse proxy (nginx, Caddy) with HTTPS
6. Regular database backups

## Security Notes

- Never commit `.env` files with real secrets
- API keys are stored server-side only
- All sensitive data is encrypted at rest
- Rate limiting is enabled on all API endpoints
- Audit logging records all administrative actions
- File uploads are validated for type and size

## License

Enterprise internal deployment - © 2026 MUSH Corporation
