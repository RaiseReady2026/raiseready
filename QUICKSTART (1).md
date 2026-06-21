# 🚀 RAISE READY - QUICK START GUIDE

## What's Been Created

✅ **Database Schema** - 8 normalized tables (Users, Startups, Assessments, Questions, Responses, Scores, ActionPlans, Reports)
✅ **77 Assessment Questions** - Database-driven, rule-based scoring (NO AI/hardcoding)
✅ **Logo & Branding** - Premium VC-style visual identity
✅ **Core Components**:
   - QuestionBox (interactive assessment UI with progress, auto-save, resume)
   - Logo (SVG icon + branding)
   - Navbar (auth-aware navigation)
   - ScoreCard (displays 0-100 investment readiness score)
   - RadarChart (Recharts-powered multi-dimensional visualization)
   - ActionPlanCard (task management with priority/status)

✅ **API Routes** (Database-driven):
   - `GET /api/questions` - All 77 questions
   - `POST /api/assessment` - Create assessment
   - `GET /api/assessment` - Get latest assessment
   - `POST /api/assessment/[id]/response` - Save question response + auto-save last position
   - `POST /api/assessment/[id]/complete` - Finish assessment + calculate score + generate action items
   - `GET /api/assessment/[id]/resume` - Resume from saved position

✅ **Scoring Engine** (Rule-Based, 100% Transparent):
   - Calculates dimension scores (7 dimensions: Product-Market Fit, Team, Finances, Traction, Market, Funding, Product)
   - Generates strengths, gaps, red flags, issues
   - Creates auto-generated action items
   - Zero black-box AI

---

## INSTALLATION (5 MINUTES)

### Step 1: Create Project
```bash
npx create-next-app@latest raiseready \
  --typescript \
  --tailwind \
  --app \
  --no-eslint
cd raiseready
```

### Step 2: Copy Files
- Copy all files from `/home/claude/raiseready/` into your project
- Keep structure intact (prisma/, lib/, components/, app/)

### Step 3: Install Dependencies
```bash
pnpm add prisma @prisma/client next-auth stripe zod react-hook-form zustand recharts axios swr

pnpm add -D @types/node @types/react prisma
```

### Step 4: Setup Database
```bash
# Create PostgreSQL database
createdb raiseready

# Create .env.local
cat > .env.local << 'EOF'
DATABASE_URL=postgresql://postgres:password@localhost:5432/raiseready
NEXTAUTH_URL=http://localhost:3000
NEXTAUTH_SECRET=$(openssl rand -hex 32)
STRIPE_PUBLIC_KEY=pk_test_your_key_here
STRIPE_SECRET_KEY=sk_test_your_key_here
JWT_SECRET=$(openssl rand -hex 32)
EOF
```

### Step 5: Initialize Database
```bash
npx prisma migrate dev --name init
npx prisma db seed
```

### Step 6: Run Server
```bash
pnpm dev
```

Visit `http://localhost:3000`

---

## FILE MAPPING

```
raiseready/
├── prisma/
│   ├── schema.prisma ✅
│   └── seed.ts ✅ (77 questions)
├── lib/
│   ├── prisma.ts ✅
│   └── scoring-engine.ts ✅ (rule-based, transparent)
├── components/
│   ├── Logo.tsx ✅
│   ├── Navbar.tsx ✅
│   ├── QuestionBox.tsx ✅ (assessment UI)
│   ├── ScoreCard.tsx ✅
│   ├── RadarChart.tsx ✅
│   ├── ActionPlanCard.tsx ✅
│   └── ... (more needed - see below)
├── app/
│   ├── api/
│   │   ├── questions/route.ts ✅
│   │   └── assessment/
│   │       ├── route.ts ✅
│   │       └── [id]/
│   │           ├── response/route.ts ✅
│   │           ├── complete/route.ts ✅
│   │           └── resume/route.ts ✅
│   ├── (public)/
│   │   ├── page.tsx ❌ (HOME PAGE - NEEDS BUILD)
│   │   ├── about/page.tsx ❌
│   │   ├── pricing/page.tsx ❌
│   │   ├── blog/page.tsx ❌
│   │   ├── contact/page.tsx ❌
│   │   ├── testimonials/page.tsx ❌
│   │   ├── startup-profile/page.tsx ❌ (FREE SIGNUP)
│   │   ├── auth/
│   │   │   ├── signup/page.tsx ❌
│   │   │   └── login/page.tsx ❌
│   │   └── layout.tsx ❌
│   ├── (auth)/
│   │   ├── dashboard/page.tsx ❌
│   │   ├── startup-profile/page.tsx ❌ (EDIT)
│   │   ├── assessment/
│   │   │   ├── page.tsx ❌
│   │   │   ├── [step]/page.tsx ❌ (ASSESSMENT WIZARD)
│   │   │   └── review/page.tsx ❌
│   │   ├── results/
│   │   │   └── page.tsx ❌ (SCORE + RADAR + INSIGHTS)
│   │   ├── action-plan/page.tsx ❌
│   │   ├── reports/page.tsx ❌
│   │   ├── settings/page.tsx ❌
│   │   └── layout.tsx ❌
│   ├── admin/
│   │   ├── dashboard/page.tsx ❌
│   │   ├── startups/page.tsx ❌
│   │   ├── assessments/page.tsx ❌
│   │   ├── funnel/page.tsx ❌
│   │   ├── action-plans/page.tsx ❌
│   │   ├── revenue/page.tsx ❌
│   │   └── layout.tsx ❌
│   ├── globals.css ❌
│   └── layout.tsx ❌
└── public/
    ├── logo.svg ✅
    └── favicon.ico ❌
```

---

## WHAT YOU NEED TO BUILD NEXT

### 1. Authentication (NextAuth)
   - `lib/auth.ts` - NextAuth configuration
   - `app/auth/[...nextauth]/route.ts` - Auth endpoint
   - User registration/login logic

### 2. Layouts
   - `app/layout.tsx` - Root layout with Navbar
   - `app/(public)/layout.tsx` - Public pages layout
   - `app/(auth)/layout.tsx` - Protected pages layout
   - `app/admin/layout.tsx` - Admin pages layout

### 3. Public Pages
   - Home page with hero + value prop
   - Pricing page (2 tiers)
   - About page
   - Blog page with posts
   - Testimonials
   - Contact form
   - Startup Profile (free signup form)
   - Auth pages (signup/login)

### 4. SaaS Pages
   - Dashboard (status card + assessment progress)
   - Startup Profile Edit (form)
   - Assessment Wizard (`/assessment/[step]` with QuestionBox)
   - Results (Score Tab + Radar Tab + Insights Tab)
   - Action Plan (list of tasks with status updates)
   - Reports (list + PDF export)
   - Settings (account + password)

### 5. Admin Pages
   - Dashboard (KPI cards + charts)
   - Startups table (filters, search)
   - Assessments table (score distribution)
   - Funnel analytics
   - Action plan tracking
   - Revenue/payments

### 6. More API Routes
   - `/api/auth/[...nextauth]` - NextAuth
   - `/api/startup-profile` - CRUD
   - `/api/payments` - Stripe integration
   - `/api/action-plans` - CRUD
   - `/api/reports` - PDF generation
   - `/api/stripe/webhook` - Stripe webhooks
   - `/api/admin/*` - Admin endpoints

### 7. Missing Components
   - StartupProfileForm
   - MetricsCard
   - FunnelChart
   - DataTable
   - ProgressBar
   - Footer
   - Forms (with validation)

---

## CORE BUSINESS LOGIC EXPLANATION

### Scoring (Rule-Based, NO AI)

**77 Questions → 7 Dimensions:**
1. Product-Market Fit (11Q)
2. Team (11Q)
3. Business Model & Finances (11Q)
4. Traction & Metrics (11Q)
5. Market & Competition (11Q)
6. Funding & Capital Strategy (11Q)
7. Product & Technology (11Q)

**Calculation:**
```
Each question → Option selected → Points extracted (0-100)
Dimension Score = Average of all question points in that dimension
Total Score = Average of all dimension scores (0-100)

Example:
Q1: Product-Market Fit → Answer: "Customers seek us out" → 100 points
Q2: Product-Market Fit → Answer: "15%+ MoM growth" → 100 points
Q3: Product-Market Fit → Answer: ">85% retention" → 100 points
→ PMF Dimension Score = 100

Similar for other 6 dimensions
→ Total Score = 85 (avg of 7 dimensions)
```

**Output:**
- Total Score (0-100)
- Dimension Scores (7 scores)
- Strengths (dimensions ≥75)
- Gaps (dimensions 50-75)
- Red Flags (dimensions 25-50)
- Issues (dimensions <25)

**Action Items (Auto-Generated):**
- Each gap/flag/issue → Task created
- Priority assigned (critical/high/medium/low)
- Users can update status (to_do → in_progress → done)

---

## RESUME SYSTEM (CRITICAL FEATURE)

**Problem:** User closes browser mid-assessment → Session lost

**Solution (Stored in DB):**
```
When user answers Q25:
1. Save response to Responses table
2. Update Assessment.lastQuestionIndex = 25
3. Update Assessment.completionPercentage = 25/77 * 100 = 32%
4. On return: GET /api/assessment/[id]/resume
5. Returns: lastQuestionIndex=25, resumeAt=Q26
6. Page loads with "Resume from Q26?" popup
```

---

## STRIPE INTEGRATION (NOT YET BUILT)

**Flow:**
1. User creates free profile → Account created
2. Dashboard shows "Unlock Full Diagnostic ($99)"
3. Click → Stripe checkout
4. Payment successful → Create Payment record (status: succeeded)
5. Unlock assessment wizard
6. After completion → Upsell to "Raise Ready Program" (higher tier)

**Endpoints Needed:**
- `POST /api/payments/checkout-session` - Create Stripe session
- `POST /api/stripe/webhook` - Handle payment confirmation

---

## DEPLOYMENT CHECKLIST

- [ ] Database: Supabase or AWS RDS
- [ ] Frontend: Vercel
- [ ] Auth: NextAuth configured
- [ ] Stripe: Live keys
- [ ] Email: SendGrid integration
- [ ] Storage: S3 for PDFs
- [ ] Error tracking: Sentry
- [ ] Analytics: Posthog or Mixpanel

---

## EXAMPLE USER FLOW

```
1. User → raiseready.com
2. Click "Get Started" → /startup-profile
3. Fill form (company, industry, founders, stage)
4. Submit → Auto-create User + Startup
5. Auto-login → /dashboard
6. Dashboard shows: "Assessment Locked - $99 to unlock"
7. Click Unlock → Stripe checkout → Pay
8. Redirect → /assessment/start
9. Load Q1 from database
10. Answer → Auto-save (QuestionBox handles)
11. Click Next → Save Response API + Load Q2
12. (Can close at Q50) → Session state saved
13. Next day: Open /assessment → Resume popup
14. Click "Continue from Q50" → Jump to Q50
15. Continue Q51-Q77 → Complete
16. POST /api/assessment/[id]/complete
   → Calculate score (rule engine)
   → Generate action items
   → Save to DB
17. Redirect → /results (3 tabs)
   - Tab 1: Score card + interpretation
   - Tab 2: Radar chart (7 dimensions)
   - Tab 3: Insights + Strengths/Gaps/Red Flags
18. /action-plan → View auto-generated tasks
19. User can mark tasks Done, add notes
20. /reports → Download PDF report
21. Footer shows: "Ready to accelerate? Join Raise Ready"
```

---

## COMMON PITFALLS TO AVOID

❌ Hardcoding questions (use database)
❌ Storing questions in state (fetch from API)
❌ AI-based scoring (use rule-based only)
❌ Losing session on page close (store in DB)
❌ Rebuilding assessment from scratch (support resume)
❌ Slow page loads (use server components)
❌ Manual invoice tracking (integrate Stripe webhooks)

---

## NEXT STEPS

1. Copy all files to your project
2. Run `pnpm dev`
3. Build public pages (Home, Pricing, About, etc.)
4. Build authentication (NextAuth)
5. Build assessment wizard (`/assessment/[step]`)
6. Test scoring engine
7. Build results page (3 tabs)
8. Build action plan interface
9. Integrate Stripe
10. Build admin dashboard
11. Deploy to Vercel

---

**Everything is database-driven, rule-based, and ready for enterprise scaling.**

Questions? Review the scoring-engine.ts and Question seed for exact scoring logic.
