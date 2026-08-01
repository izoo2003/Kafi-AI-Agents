# KAFI AI Agent

Enterprise AI agents for finance and operations, built for **Kafi Commodities** by **Sheikh Shumyle** , **NeuroGrid Labs** and **Izaan Bin Mujeeb**.

The platform starts at a central agent hub and routes into specialized modules. The **AI Agent Finance** suite is the primary active product — bank reconciliation, credit card verification, expense analysis, fund estimation, and more — powered by Claude and integrated with Google Sheets for shared data.

## Live Demo

**Production:** [https://kafi-ai-agent.vercel.app](https://kafi-ai-agent.vercel.app)

**Repository:** [https://github.com/KAFI-AI-Agent/kafi-ai-agent](https://github.com/KAFI-AI-Agent/kafi-ai-agent)

## Features

### Agent Hub (`/`)

Landing page listing all planned enterprise agents. **AI Agent Finance** is live; Cost/Budgeting, Supply Chain, CRM, HR, Project Manager, and Personal Assistant are marked TBA.

### AI Agent Finance (`/login` → `/dashboard`)

After login, the dashboard provides access to these modules:

| Module | Route | Description |
|--------|-------|-------------|
| Multi-Bank Adjustments | `/multi-bank` | Reconcile ABL, HMB, Faysal, Soneri and other banks — auto-detect format, match by date & amount |
| Credit Card Verification | `/credit-card` | Group transactions by merchant, tick-verify against receipts, SCB format export |
| International Recon | `/international` | Any bank worldwide — AI auto-detects format, multi-currency matching |
| Statement Digitizer | `/statement-digitizer` | 3-step AI pipeline — validate, extract, reconcile any bank statement |
| Ledger vs Ledger | `/ledger-vs-ledger` | Company vs vendor ledger — match by amount & date, flag discrepancies |
| Quotation Comparison | `/quotations` | Multi-vendor AI extraction — side-by-side with cheapest price highlight |
| Expense Analyzer | `/expense-analyzer` | Wise multi-currency statements — filter, chat, PKR conversion |
| Reminders | `/reminders` | Global team reminders — one-time, weekly, or monthly, shown across modules |
| Petty Cash Flow | `/petty-cash` | Daily petty cash register — month-wise view, running balance, cash in/out |
| Fund Estimation Workspace | `/fund-estimator` | Live collaborative ledger — multi-bank balances, PDC tracking, real-time workspace |

Additional pages (legacy / direct access):

| Page | Route |
|------|-------|
| AI Reconciliation Chat | `/recon` |
| Bank vs Ledger Compare | `/compare` |
| Adjustments | `/adjustments` |
| Statement Converter | `/statement-converter` |

### Supported file formats

PDF, Excel (`.xlsx` / `.xls`), CSV, plain text, and scanned images (PNG/JPG). File parsing lives in `src/lib/parse-files.ts`; AI modules use Claude for extraction and analysis.

## Tech Stack

- **Framework:** [Next.js 16](https://nextjs.org) (App Router) + React 19 + TypeScript
- **Styling:** Tailwind CSS 4
- **AI:** Anthropic Claude (`@anthropic-ai/sdk`)
- **Data:** Google Sheets & Google Drive (`googleapis`)
- **Documents:** `pdf-parse`, `xlsx`, `jspdf`, `docx`, `tesseract.js`, `canvas`

## Project Structure

```
kafi-ai-agent/
├── src/
│   ├── app/
│   │   ├── page.tsx                 # Agent hub (landing)
│   │   ├── login/                   # Access gate (user code / testing)
│   │   ├── dashboard/               # Finance module launcher
│   │   ├── multi-bank/              # Multi-bank reconciliation
│   │   ├── credit-card/             # Credit card verification
│   │   ├── international/           # International reconciliation
│   │   ├── statement-digitizer/     # 3-step statement pipeline
│   │   ├── ledger-vs-ledger/        # Ledger cross-check
│   │   ├── quotations/              # Quotation comparison
│   │   ├── expense-analyzer/        # Expense analysis
│   │   ├── reminders/               # Global reminders
│   │   ├── petty-cash/              # Petty cash register
│   │   ├── fund-estimator/          # Fund estimation workspace
│   │   ├── recon/                   # AI chat reconciliation (legacy)
│   │   ├── compare/                 # Bank vs ledger compare (legacy)
│   │   ├── adjustments/             # Adjustments (legacy)
│   │   ├── statement-converter/     # Statement converter (legacy)
│   │   └── api/                     # Server-side API routes
│   ├── components/                  # Shared UI (e.g. ReminderBell)
│   └── lib/
│       ├── parse-files.ts           # PDF / Excel / CSV extraction
│       ├── google-sheets.ts         # Google Sheets backend
│       ├── google-drive.ts          # Google Drive exports
│       └── usage-tracker.ts         # API usage & cost tracking
├── public/                          # Static assets
├── netlify.toml                     # Netlify build config (optional)
├── next.config.ts
└── package.json
```

## Getting Started

### Prerequisites

- Node.js 20+
- npm

### Install & run locally

```bash
npm install
npm run dev
```

Open [http://localhost:4000](http://localhost:4000). The dev server runs on port **4000** (not the Next.js default 3000).

### Build for production

```bash
npm run build
npm start
```

### Lint

```bash
npm run lint
```

## Environment Variables

Create a `.env.local` file in the project root:

| Variable | Required | Description |
|----------|----------|-------------|
| `ANTHROPIC_API_KEY` | Yes (AI modules) | Anthropic API key for Claude |
| `GOOGLE_SERVICE_ACCOUNT_EMAIL` | Yes (Sheets/Drive) | Google service account email |
| `GOOGLE_PRIVATE_KEY` | Yes (Sheets/Drive) | Service account private key (use `\n` for newlines) |
| `GOOGLE_SHEET_ID` | Yes (Sheets modules) | Spreadsheet ID for Fund Estimator, Reminders, Petty Cash, etc. |
| `NEXT_PUBLIC_ADMIN_PIN` | Optional | Super-user PIN for dashboard module visibility controls |
| `NEXT_PUBLIC_FE_PIN_ACCOUNTANT` | Optional | Fund Estimator / Petty Cash role PIN (accountant) |
| `NEXT_PUBLIC_FE_PIN_AA1` | Optional | Fund Estimator / Petty Cash role PIN (AA1) |
| `NEXT_PUBLIC_FE_PIN_AA2` | Optional | Fund Estimator / Petty Cash role PIN (AA2) |

AI features degrade gracefully when `ANTHROPIC_API_KEY` is missing — some modules fall back to local parsers. Google Sheets modules require the Google credentials to function.

## Deployment

The app is deployed on **Vercel**:

- **Live site:** [https://kafi-ai-agent.vercel.app](https://kafi-ai-agent.vercel.app)

A `netlify.toml` is also included for optional Netlify deployment. If deploying to Netlify with Next.js, install `@netlify/plugin-nextjs` and add the plugin to `netlify.toml`.

### Deploy to Vercel

1. Connect the GitHub repository to [Vercel](https://vercel.com).
2. Set all required environment variables in **Project → Settings → Environment Variables**.
3. Deploy — Vercel auto-detects Next.js and runs `npm run build`.

### Deploy to Netlify

1. Connect the repository in the [Netlify dashboard](https://app.netlify.com).
2. Build command: `npm run build` (already in `netlify.toml`).
3. Add environment variables under **Site settings → Environment variables**.
4. Ensure the Next.js Netlify plugin is configured for App Router support.

## API Routes

| Endpoint | Purpose |
|----------|---------|
| `/api/analyze` | Reconciliation analysis |
| `/api/chat` | AI chat assistant |
| `/api/multi-bank` | Multi-bank reconciliation |
| `/api/credit-card` | Credit card parsing & verification |
| `/api/credit-card/export-drive` | Export to Google Drive |
| `/api/international` | International reconciliation |
| `/api/statement-digitizer` | Statement digitization pipeline |
| `/api/statement-converter` | Statement format conversion |
| `/api/ledger-vs-ledger` | Ledger cross-matching |
| `/api/quotations` | Quotation extraction & comparison |
| `/api/expense-analyzer` | Expense categorization & analysis |
| `/api/compare` | Bank vs ledger comparison |
| `/api/adjustments` | Adjustment entries |
| `/api/fund-estimator` | Fund estimation CRUD |
| `/api/fund-estimator/notifications` | Fund estimator notifications |
| `/api/petty-cash` | Petty cash entries |
| `/api/reminders` | Global reminders |
| `/api/dashboard-config` | Dashboard module visibility |
| `/api/usage` | API usage & cost tracking |
| `/api/download-ledger` | Ledger export download |

## Access & Roles

- **User access:** Enter the team access code on `/login`.
- **Testing access:** Time-limited beta access (when enabled).
- **Super user:** Dashboard admin controls for showing/hiding modules (requires username + `NEXT_PUBLIC_ADMIN_PIN`).
- **Fund Estimator / Petty Cash:** Role-based PINs for accountant and AA roles.

## License

Private — © NeuroGrid Labs / Kafi Commodities.
