# Smart Expense Splitter Pro

A next-generation expense splitting app combining traditional IOU tracking (like Splitwise) with a prepaid wallet system and intelligent automation.

🔗 **Live Demo**: Deploy to Vercel with one command (see below)

---

## ✨ Features

### Dual Mode System
- **Split Mode** — Track IOUs: who paid, who owes, simplified debt settlement
- **Wallet Mode** — Prepaid wallets per user; expenses deducted instantly with fallback to IOU for insufficient balance

### Expense Management
- Equal, Custom (specified amounts), and Weighted (ratio-based) splits
- Category tagging (food, transport, accommodation, etc.)
- Full expense history with expandable detail view
- Search & filter

### Smart Approval System
- **Auto-approve** if `amount < threshold (default ₹500)` AND `splitType === 'equal'`
- **Require approval** for high-value or non-equal splits
- Approve / Reject with optional reason
- **Full rollback** on rejection (wallet + IOU balances restored)

### Wallet System
- Per-user, per-group wallet balances
- Deposit with quick-amount presets
- Automatic deduction → fallback partial payment
- Low balance warnings
- Transaction history

### Balance & Debt Engine
- Net balance per member (IOU-only; wallet fully auto-settled)
- **Greedy debt simplification** — minimize number of transactions needed
- **Fairness Meter** — visual score showing how equitably costs are distributed

### UI/UX
- Dark mode with glassmorphism cards
- Framer Motion page/tab transitions and animations
- Recharts balance distribution visualization
- Responsive across mobile and desktop
- Notification center with unread badge

---

## 🏗 Architecture

```
split-app/
├── app/                    # Next.js 14 App Router pages
│   ├── page.tsx            # Dashboard
│   ├── groups/
│   │   ├── page.tsx        # Groups list
│   │   └── [id]/page.tsx   # Group detail (tabbed)
│   └── settings/page.tsx   # Settings & user switcher
├── components/
│   ├── ui/                 # Button, Modal, Input, Select, Card, Avatar, Badge
│   ├── layout/             # Navbar with notification bell
│   ├── expenses/           # ExpenseList, ExpenseCard, ExpenseForm
│   ├── wallet/             # WalletView, DepositModal, BalanceChart
│   ├── groups/             # GroupGrid, MemberList, CreateGroupModal
│   └── approvals/          # ApprovalQueue, ApprovalCard
├── store/                  # Zustand stores (persisted to localStorage)
│   ├── useGroupStore.ts    # Groups, members, roles
│   ├── useExpenseStore.ts  # Expenses + approval state machine
│   ├── useWalletStore.ts   # Wallet balances, transactions
│   ├── useNotificationStore.ts
│   └── useAppStore.ts      # Theme, current user, preferences
├── lib/
│   ├── balanceEngine.ts    # Net balances + greedy debt simplification
│   ├── walletEngine.ts     # Deduction, fallback, rollback logic
│   ├── splitCalculator.ts  # Equal / Custom / Weighted split math
│   ├── approvalEngine.ts   # Auto-approve rules
│   └── utils.ts            # Helpers, constants, formatters
└── types/
    └── index.ts            # All TypeScript interfaces
```

### State Management
All state lives in **Zustand** stores with `persist` middleware writing to `localStorage`. No backend required — fully client-side.

### Key Algorithms

**Wallet Engine** (`lib/walletEngine.ts`):
```
For each participant's share:
  if wallet_balance >= share:
    deduct full share from wallet
  else:
    deduct available balance
    remainder → IOU split balance
```

**Debt Simplification** (`lib/balanceEngine.ts`):
```
1. Calculate net balance per member
2. Sort creditors (positive) and debtors (negative)
3. Greedily match largest creditor with largest debtor
4. Repeat until all settled → minimum transaction set
```

---

## 🚀 Setup & Running

### Prerequisites
- Node.js 18+
- npm

### Local Development
```bash
cd split-app
npm install
npm run dev
```
App runs at http://localhost:3000

### Build
```bash
npm run build
npm start
```

### Deploy to Vercel
```bash
npm install -g vercel
vercel --prod
```

---

## 🧪 Demo Data

The app ships with seed data:
- **Goa Trip 2024** (Wallet mode) — 4 members, hotel/dinner/scuba expenses, one pending approval
- **Office Lunch Club** (Split mode) — 3 members, team lunch

Switch between users via **Settings → Current User** to experience different perspectives.

---

## Tech Stack

| Layer | Technology |
|---|---|
| Framework | Next.js 14 (App Router) |
| Language | TypeScript |
| Styling | Tailwind CSS v4 |
| State | Zustand + persist |
| Animations | Framer Motion |
| Charts | Recharts |
| Icons | Lucide React |
| Storage | localStorage (no backend) |
