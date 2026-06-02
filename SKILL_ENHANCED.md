---
name: project-guidance
description: |
  **Comprehensive project execution framework for shipping quality code.**
  
  Use this skill when a user is:
  - Starting a NEW project (greenfield)
  - Adding a feature to EXISTING codebase
  - Migrating between architectures
  - Refactoring a struggling system
  - Recovering from mid-project scope creep
  - Planning across multiple teams
  - Building compliance-critical systems
  
  Triggers: "I want to build...", "Help me plan...", "How do I structure...", "We're adding a feature...", "We need to migrate...", "Design is stuck...", "We're scaling to a team", or when you detect under-scoped/ambiguous requirements.
  
  **Philosophy**: Teach the *why* behind each decision. Users should understand trade-offs, not just follow a checklist. This framework prioritizes deep understanding, architectural thinking, and shipping quality code from day one.
---

# Enhanced Project Guidance Framework
## Teach First, Execute Second

---

## **PART 0: PROJECT CONTEXT DETECTION**
### (BEFORE Framework Selection)

**Don't assume one-size-fits-all.** First, identify the project context:

### **Context Selector**

```
1. PROJECT TYPE
   ☐ Greenfield (new project from scratch)
   ☐ Feature Addition (existing codebase)
   ☐ Refactor/Rewrite (replacing current system)
   ☐ Migration (moving from one architecture to another)
   ☐ Integration (adding external service dependencies)

2. TEAM STRUCTURE
   ☐ Solo developer (1 person)
   ☐ Small team (2–5 people, co-located)
   ☐ Distributed team (async, multiple time zones)
   ☐ Cross-functional (multiple teams, dependencies)

3. TIMELINE
   ☐ Rapid (ship this week)
   ☐ Standard (4-week sprint)
   ☐ Extended (2+ months, iterative)
   ☐ Long-term (quarter+, ongoing)

4. CONSTRAINTS
   ☐ Performance-critical (latency/throughput SLAs)
   ☐ Compliance-bound (HIPAA, GDPR, SOC2)
   ☐ Budget-limited (<$50k/month infra)
   ☐ Reliability-critical (99.9%+ uptime required)
   ☐ Legacy-dependent (must integrate existing systems)

5. UNKNOWNS
   ☐ Clear requirements (stakeholders aligned)
   ☐ Fuzzy requirements (need exploration first)
   ☐ Research-heavy (unproven approach)
   ☐ Learning-focused (teaching is as important as shipping)
```

**Action**: Identify user's context, then customize framework accordingly.

---

## **PART 1: FRAMEWORK VARIANTS**

### **Variant A: Greenfield Projects** (New from scratch)
→ Use FULL framework (Phases 1–7)

### **Variant B: Feature Addition** (Existing codebase)
→ SKIP Phases 1–2, START with **Codebase Assessment**, then Phase 3+

### **Variant C: High-Compliance Projects** (HIPAA, GDPR, SOC2)
→ Add **Compliance Deep-Dive** before Phase 2

### **Variant D: Distributed Teams** (Async, multiple teams)
→ Add **Team Coordination Protocol** to Phase 3

### **Variant E: Rapid Projects** (<1 week)
→ Compress Phases 1–2 into **30-minute Sprint Planning**, skip Steps 1–3

### **Variant F: Performance-Critical** (Sub-100ms latency, high throughput)
→ Add **Performance Architecture Review** before Phase 2

---

## **PHASE 0: Codebase Assessment** (Feature Additions Only)
### **When you're building on existing systems**

### **Step 0a: Archaeology & Code Landscape**

Before designing anything, understand what you're inheriting:

1. **Code Quality Audit**
   - Run linter/static analysis (find tech debt hotspots)
   - Coverage report (test-less code = danger zones)
   - Dependency audit (outdated libs? Security vulnerabilities?)
   - Documentation completeness (what's well-documented? What's missing?)

2. **Architecture & Patterns**
   - Current patterns (what architectural style is it? Layers, microservices, monolith?)
   - Consistency (does it follow its own patterns? Deviations?)
   - Constraints (built-in limits: how it scales, database choices, deployment model)

3. **Key Person Risk**
   - Who understands the [critical system]?
   - If they left, what breaks?
   - Cross-train or document before proceeding

**Teaching Point**: You can't add a feature without understanding the foundation. Spending 2 hours here saves 2 weeks of "why is it doing that?"

**Their output**:
- Codebase health scorecard (test coverage, debt hotspots, dependency status)
- Architecture diagram as it *actually* is (not as docs say)
- List of 3 areas where code is fragile / needs refactoring

---

### **Step 0b: Feature Integration Points**

Where does your feature connect to the existing system?

1. **Data Model Integration**
   - Existing schema: how does your data fit?
   - Can you reuse existing tables, or need new ones?
   - Migration risk: will changing schema break existing features?

2. **API/Module Boundaries**
   - Current API contract: what can you call?
   - Performance SLAs: is latency guaranteed?
   - Rate limits: any existing constraints?

3. **Deployment & Configuration**
   - How is the system deployed?
   - Environment variables: how are they managed?
   - Rollback capability: can you undo a bad deploy?

**Teaching Point**: Wrong integration point = weeks of rework.

**Their output**:
- Integration point checklist (which systems does this touch?)
- Data model mapping (how does new data fit existing schema?)
- Deployment compatibility check (can you deploy without downtime?)

---

### **Step 0c: Risk Assessment for Changes**

What could go wrong when you add this feature?

```
| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|-----------|
| Breaking existing feature | ? | ? | ? |
| Data migration fails | ? | ? | ? |
| Performance regression | ? | ? | ? |
| Security vulnerability | ? | ? | ? |
```

**Their output**: Risk register (5-7 items) with likelihood/impact/mitigation

---

## **PHASE 0b: Success Metric Definition**
### **(Required for all projects, but emphasis varies)**

### **Why This First**

Before you design *anything*, agree on what "done" means.

80% of project failures aren't technical—they're **metric misalignment**:
- Engineer thinks "performant" = <500ms
- Product thinks "performant" = <50ms
- Customer thinks "performant" = works at all

**Ask explicitly**:

### **The Success Metric Conversation**

1. **Business/User Outcome** (Not technical!)
   - What does success look like for the *user*?
   - "Save 1 hour/week"? "Increase revenue 10%"? "Zero errors in critical flow"?
   - How will you know if you succeeded? (What evidence?)

2. **Translate to Measurable Metrics**
   - Technical SLOs (p95 latency <100ms, <0.1% error rate)
   - User metrics (time-to-value, error rate, task success rate)
   - Business metrics (adoption, retention, revenue impact)

3. **Define Thresholds**
   - What counts as success? (p95 <100ms? Or <150ms? Why?)
   - What's the failure threshold? (At what point do we abandon this approach?)
   - What's the "good enough" middle ground?

**Example Conversation:**

```
You: "What does success look like?"
User: "Real-time collaboration, like Google Docs"
You: "Real-time is vague. Let me get specific:
  - How many users editing simultaneously?
  - Acceptable latency for seeing other's edits?
  - What if network drops—offline-first required?
  - Conflicts (last-write-wins OK, or need resolution?)"
User: "Oh, just 5-10 simultaneous, <2s latency, offline not needed, conflicts OK"
You: "Now we have real constraints. Different architecture than full Google Docs."
```

**Their output**: Metric definition doc (2–3 pages):
- Outcome statement (1 paragraph)
- 3–5 metrics with thresholds
- How each metric is measured (tool/method)
- Why each metric matters (user impact)
- Acceptable pass/fail ranges

---

## **PHASE 1: Problem Understanding & Scope Definition**
### (Days 1–3: Before Any Design)

### **Step 1: Unpack the Problem Statement**

Guide the user through **deep problem analysis**.

**Ask together:**

- **The Core Problem**
  - What's the *real* problem? (Often different from stated one)
  - Who suffers? What's their daily context?
  - Why haven't they solved it yet? (What's blocking?)
  
- **Success & Constraints**
  - What does *done* look like? (Define 3–5 metrics)
  - What are hard constraints? (Latency, memory, deployment, timeline, team size)
  - What's off-limits? (Define scope boundaries explicitly)
  
- **Feasibility Reality Check**
  - Have they researched if this is already solved?
  - If yes, why rebuild? (Cost? Customization? Learning?)
  - If no, is it unsolved for a *reason*? (Hard AI problem? Regulatory? Infra cost?)

**Teaching Point**: Scope creep happens when success is fuzzy. Be ruthless about clarity.

**Their output**: 1-page problem statement with:
- Who the user is (1–2 workflows)
- What "done" looks like (3–5 outcomes)
- Top 3 constraints
- Top 3 things they're NOT building

---

### **Step 2: Landscape & Technical Approach Analysis**

Teach them to *evaluate* approaches, not just copy someone else's.

**Do together:**

1. **Research Spike** (If unknowns are high)
   - What's the state-of-the-art?
   - Academic papers? Existing implementations?
   - Where are the unsolved parts?

2. **Competitive/Existing Solutions Audit**
   - Find 3–5 existing solutions
   - For each: What works? Where do they fail?
   - Key insight: What *didn't* existing solutions solve?

3. **Technical Approach Comparison**
   - Sketch 2–3 fundamentally different approaches
   - Build a pros/cons grid:
     ```
     | Approach | Pros | Cons | When Best? | Switching Cost |
     |----------|------|------|-----------|----------------|
     | A        | ?    | ?    | ?         | ?              |
     | B        | ?    | ?    | ?         | ?              |
     | C        | ?    | ?    | ?         | ?              |
     ```
   - For each: assumptions baked in? Where does it break?

4. **Recommendation & Justification**
   - Recommend ONE approach
   - Explain: "We chose A because [constraint], assuming [assumption]. If [X changes], B becomes better."

**Teaching Point**: Simplicity wins. Complex approaches cost 20% more upfront, 80% more on maintenance.

**Their output**: Decision doc (1–2 pages):
- 3 approaches compared (table)
- Recommended approach + 3-line justification
- "When to switch" scenarios
- Key assumptions

---

### **Step 3: Clarification & Scope Negotiation**

**If you detect ambiguity:**

```
🚩 AMBIGUITY DETECTED:
You said "real-time collaboration" but haven't defined:
  - Simultaneous users? (2? 100?)
  - Acceptable latency? (<100ms? <500ms?)
  - Conflict resolution? (last-write-wins or algorithmic?)
  - Offline-first required?

Each answer changes the architecture.
```

Ask targeted questions. Iterate until scope is clear.

**Teaching Point**: Ambiguity now = redesign later. Invest time here.

---

## **PHASE 2: Architecture & Design**
### (Days 3–7: Visual Thinking Before Code)

### **Step 4: System Architecture**

Teach systems-level thinking. **Design on paper first.**

**Build together:**

1. **Data Flow Diagram**
   - What data enters?
   - What transformations happen? Why?
   - Where does it live? (Memory, database, cache, edge?)
   - What leaves?

2. **Component Breakdown**
   - 5–7 major pieces
   - How do they communicate?
   - Which are critical (must not fail) vs. best-effort?
   
3. **Technology Decisions** (With reasoning)
   - Database: SQL or NoSQL? Why? Query patterns?
   - Cache layer? When and why?
   - Message queue? For what?
   - API framework? Why this over that?
   - **Teaching point**: Match tool to problem, not based on popularity

4. **Failure Modes & Resilience**
   - "Database down?" → System response?
   - "Message lost?" → Retry? Give up? Log?
   - "10x load spike?" → Graceful degrade or crash?
   - **Teaching point**: Resilience is designed in, not bolted on

**Their output**:
- ASCII/text architecture diagram
- Data flow diagram (label arrows with data type/format)
- Technology decision table (tool, choice, why, alternatives)
- Failure mode matrix (failure, impact, response, how detected)

---

### **Step 5: Define Acceptance Criteria**

Make "working" concrete.

**Examples:**

| ❌ Vague | ✅ Specific & Measurable |
|---------|--------------------------|
| "Fast" | "p95 latency <100ms, p99 <200ms, 1000 req/sec" |
| "Reliable" | "99.9% uptime, <5 failures/month, graceful degrade if cache fails" |
| "Handles edge cases" | "Null inputs → helpful error, empty arrays don't crash, concurrent writes use optimistic locking" |

**For each criterion:**
- How will you measure it? (Profiler? Load test? Manual?)
- What passes and what fails? (Exact thresholds)
- Why does it matter? (User/business impact)

**Their output**: 10–15 acceptance criteria with:
- Criterion statement
- How to test
- Pass/fail threshold

---

## **PHASE 3: Development Strategy**
### (Before Day 1 of Coding)

### **Step 6: Break Into Milestones**

**Shippable slices, not task lists.**

**❌ Bad (Waterfall):**
- Database schema
- API endpoints
- Auth
- Tests
- Deploy

Problem: Nothing works until everything is done.

**✅ Good (Vertical slices):**

**Milestone 1 (Week 1): MVP**
- Core feature works end-to-end (no edge cases)
- Happy path only
- Metrics: Handles 1 request, <5s latency, code is readable

**Milestone 2 (Week 2): Reliability**
- Error handling on every path
- Retries, timeouts, validation
- Basic logging

**Milestone 3 (Week 3): Scale**
- Optimize hot paths (profile first!)
- Caching, async, batch processing
- Handle 100+ concurrent users

**Milestone 4 (Week 4): Harden**
- 70%+ test coverage, security audit
- Docs complete
- Monitoring in place

**Each milestone is shippable.**

**Teaching Point**: Professionals deliver this way. Feedback every week → learn fastest.

**Their output**: Milestone breakdown with:
- What ships each week
- 3–5 acceptance criteria per milestone
- Dependencies between milestones

---

### **Step 6b: Team Coordination Protocol** (Distributed Teams Only)

If multiple teams or async work:

1. **API Contract Definition**
   - Each team defines their interface upfront
   - Mock interfaces before implementation
   - Contract tests verify compatibility

2. **Async Decision Log**
   - Major decisions documented, linked to context
   - 24h feedback window, then decision locked
   - Prevents "I didn't know that" later

3. **Dependency & Blocking Risk**
   - Explicitly map: Team A blocks Team B if...?
   - Weekly dependency check-in (async: Slack updates)
   - Escalation path if blocked

4. **Integration Test Strategy**
   - How/when do you merge work?
   - Staging environment mirrors production
   - What's tested before merge? (Contract tests? End-to-end?)

**Teaching Point**: Async doesn't mean no-communication. It means less synchronous overhead.

**Their output**:
- API specification doc (contracts)
- Decision log template
- Dependency matrix (Team A → Team B)
- Integration testing plan

---

### **Step 6c: Compliance Deep-Dive** (Regulated Domains Only)

If compliance is required (HIPAA, GDPR, SOC2):

1. **Compliance Requirements Mapping**
   - What *exactly* is required? (Read the standard)
   - Audit trail requirements? Encryption? Data residency?
   - User consent/privacy model?

2. **Architecture Impact**
   - Does compliance require certain database type? (GDPR = delete capability → not append-only)
   - Encryption at rest + in-flight? (Performance impact?)
   - Audit logging detail level?

3. **Testing & Validation**
   - How do you prove compliance? (Audit readiness?)
   - Annual compliance test? Continuous monitoring?
   - What triggers re-assessment?

**Teaching Point**: Compliance isn't a feature. It's foundational. Wrong choice = catastrophic.

**Their output**:
- Compliance requirements checklist
- Architecture/compliance mapping (where each requirement is satisfied)
- Audit readiness plan

---

### **Step 7: Code Quality & Standards**

Define how you'll build:

1. **Code Style** (Linter rules)
   - ESLint / pylint / clang-format
   - Enforce before merge
   
2. **Testing Standards**
   - Unit: 70% coverage minimum
   - Integration: critical paths
   - What gets written before code? (TDD? Not necessarily, but intentional)

3. **Review Checklist**
   ```
   ☐ Code follows style guide
   ☐ Tests exist and pass
   ☐ No dead code or debug logs
   ☐ Error handling: happy + error path
   ☐ Performance: profiled if critical
   ☐ Security: no secrets in code, SQL injection checked
   ☐ Documentation: updated
   ☐ Commit message is clear
   ```

**Their output**:
- Linter config
- PR template
- Code review checklist

---

### **Step 8: Build Monitoring & Logging from Day 1**

**Don't bolt this on. It's architecture.**

Every module logs:
- **Entry/exit**: "Function foo() called with X, returned Y"
- **State changes**: "User auth'd, cache flushed, retry #2"
- **Errors with context**: Not just "failed", but what state led to failure?
- **Metrics**: Latency, memory, throughput

**Log levels** (use correctly):
- **DEBUG**: Detailed for developers (args, iterations)
- **INFO**: High-level events (user action, state change)
- **WARN**: Unexpected but recoverable (retry, fallback)
- **ERROR**: Broken but didn't crash
- **FATAL**: System unusable

**Teaching Point**: Good logging = debug production without code.

**Their output**:
- Logging strategy (what, where, levels, retention)
- Structured logging setup (JSON, correlationId for tracing)
- Sample logs from each milestone

---

## **PHASE 4: Validation & Hardening**

### **Step 9: Testing Strategy**

By end, you need:

1. **Unit Tests (70% minimum)**
   - Test in isolation
   - Mock dependencies
   - Happy path + error paths

2. **Integration Tests (50% of unit coverage)**
   - How pieces talk
   - Real database (or test database)
   - Slower, but catch communication bugs

3. **Stress Tests (Load Testing)**
   - Does it handle 10x load?
   - Where's the breaking point?
   - Memory growth unbounded?

4. **Edge Case Tests**
   - Null/undefined
   - Empty arrays/strings
   - Boundary conditions
   - Concurrent requests

5. **User Acceptance Testing**
   - Real person (not you) tries it
   - Happy path
   - What confused them?

**For each test, document:**
- What it tests
- Why it matters
- How to reproduce if it fails

**Teaching Point**: Tests are insurance. Cost 20% upfront, save 80% on debugging.

**Their output**:
- Test coverage report (% by module, trend)
- Test documentation
- Failed test log (showing you can fix)

---

### **Step 10: Performance & Load Testing**

**Measure it. Don't guess.**

1. **Profiling** (Find the real bottleneck)
   - You're always wrong about where time is spent
   - Use profiler (Python: cProfile, Node: 0x, Go: pprof)

2. **Load Testing** (Does it handle peak?)
   - Watch: latency percentiles (p50, p95, p99)
   - Error rate at different loads
   - Memory growth

3. **Memory Profiling** (Leaks?)
   - Peak usage
   - Growth rate
   - Leak detection

**Their output**:
- Profiling results (bottleneck, before/after)
- Load test results (latency curves, p95/p99, error rates at 1x/5x/10x)
- Memory profile (peak, growth, leaks)

---

## **PHASE 5: Documentation & Deployment**

### **Step 11: Documentation** (Non-negotiable)

Good docs = project that lives. Bad docs = project that dies.

**Write:**

1. **README** (5 minutes to use it)
   - What does it do?
   - Prerequisites
   - Installation (copy-paste)
   - Quick example
   - Where to find more help

2. **Architecture Guide** (How system works)
   - High-level overview
   - Component descriptions
   - Key design decisions
   - Performance characteristics

3. **API/Module Documentation**
   - What does each function do?
   - Parameters, return values
   - Examples
   - Common errors and fixes

4. **Deployment Guide**
   - Step-by-step (dev → staging → prod)
   - Environment variables
   - Database migrations
   - Rollback procedure

5. **Troubleshooting** (Common failures & debug)
   - Memory growing? How to profile
   - Requests slow? Where to look
   - Works locally but not production? Check these

6. **Runbook** (Operations)
   - How to scale/failover
   - Emergency procedures
   - Who to contact for issues
   - SLO definitions and alerts

**Teaching Point**: Writing docs forces you to understand your system.

**Their output**: README + 5 other docs in repo

---

### **Step 12: Deployment & Monitoring**

**Version Control**
- Meaningful commits (describe *why*, not just *what*)
- One feature per branch
- Code review before merge

**CI/CD**
- Tests run on every push
- Linter checks before merge
- Build artifacts automatically
- Shipping = button press

**Monitoring**
- Can you see if it's failing in production?
- Key metrics: latency, error rate, memory, disk
- Alerts when something breaks
- Log aggregation (searchable)

**Their output**:
- GitHub Actions (or equivalent) workflow
- Deployment checklist
- Monitoring dashboard

---

## **PHASE 6: Iteration & Maintenance**

### **Step 13: Post-Launch Review**

After 1 week in production:

1. **Did it work as designed?**
   - What assumptions were wrong?
   - What exceeded expectations?

2. **Top 3 issues users hit**
   - Bugs or UX problems?
   - Quick wins?

3. **Performance in the wild**
   - What's actually slow?
   - CPU/memory breakdown?

4. **What to refactor?**
   - What felt hacky?
   - Technical debt?

**Post-Mortem Template:**

```
**What we expected to happen**: [description]
**What actually happened**: [description]
**Why the difference?**: [root cause]

**Top 3 learnings**:
1. [What surprised us?]
2. [What would we do differently?]
3. [What did we get right?]

**Action items**:
- [Fix this]
- [Improve this]
- [Document this learning]
```

**Teaching Point**: Shipping is not the end. Iteration based on real usage levels you up.

---

## **Quick Checklist: "Is This Industry-Level?"**

Before shipping:

- ✅ **Code is clean**: Linter passes, follows conventions, modular, <10 line functions
- ✅ **Tests exist**: 70%+ coverage, tests document behavior, CI passes
- ✅ **Performance is measured**: Know where time is spent, latency meets spec
- ✅ **Failure modes handled**: No silent failures, graceful degradation, retries
- ✅ **Logging is comprehensive**: Debug production issues from logs
- ✅ **Docs are complete**: README, architecture, API, deployment, troubleshooting, runbook
- ✅ **Deployment is automated**: Tests + linter + build automatic, one-click deploy
- ✅ **Version control is clean**: Readable history, atomic commits, clear branches
- ✅ **Stress-tested**: Profiled under load, know breaking points, documented limits
- ✅ **Others can own it**: Someone else can understand, run, debug, extend

---

## **How We'll Work Together**

1. **Detect context** (greenfield? feature? compliance? team size?)
2. **Ask clarifying questions** (don't dump a plan)
3. **Iterate on scope** until clear
4. **Design together** (you think, I ask, refine)
5. **Teach concepts** before executing
6. **You write code** (I review, highlight learning)
7. **Review** (what you did well, what to improve)

**Goal**: Ship something great **AND** understand *why* it's great.

---

## **Decision Trees for Common Scenarios**

### **When You're Stuck Mid-Project**

```
Issue: Design feels wrong / scope ballooning / performance degrading

→ Go back to Phase 1, Step 1
  Did requirements change? Scope unclear? Start there.

→ Was your assumption wrong? (about load, users, constraints?)
  Update assumptions, re-evaluate architecture (Phase 2, Step 4)

→ Can you unblock quickly with a workaround?
  Document as tech debt. Add to post-mortem.
  Don't let perfect be the enemy of shipping.
```

### **When You Can Skip Planning**

```
❌ NEVER skip if:
  - Multiple teams are involved
  - Compliance is required
  - Performance is critical
  - You have hard constraints

✅ CAN compress to 2 hours if:
  - Shipping is urgent (<1 week)
  - Similar architecture to existing work
  - Team is experienced
  - Risk is low (internal tool, experiments)
```

### **When to Refactor vs. Rebuild**

```
REFACTOR if:
  ✅ Core architecture is sound
  ✅ You're fixing specific weak points
  ✅ Team knows the codebase well
  ✅ <50% of code needs changing

REBUILD if:
  ✅ Fundamental architecture is wrong
  ✅ >50% of code needs rewriting anyway
  ✅ Opportunity to modernize stack
  ✅ New team learning the system
```

---

## **Example Artifacts**

### **Problem Statement (Good Example)**
```
WHO: Product managers need to prioritize features without 2-hour meetings

WHAT DONE LOOKS LIKE:
1. Submit idea in <3 minutes
2. AI summarizes to 1-page brief
3. Team votes async in <1 hour
4. Decision logged, documented

CONSTRAINTS:
- All data stays on-prem (GDPR)
- Must work offline (Slack integration)
- <100 people concurrent

NOT BUILDING:
- Full project management tool
- Real-time collaboration
- Mobile app (web only)
```

### **Architecture Decision Table (Good Example)**
```
| Component | Choice | Why | Tradeoff |
|-----------|--------|-----|----------|
| Backend   | Node.js (Express) | Team knows it, fast to ship | Not ideal for heavy compute |
| Database  | PostgreSQL | ACID + JSONB for schema flexibility | More CPU than NoSQL at scale |
| Cache     | Redis (in-memory) | Sub-ms response for voting | Need memory budget, no persistence |
| Queue     | Bull (Redis-backed) | Simpler than RabbitMQ for 1 team | Limited if we scale to 50+ servers |
```

---

## **When to Escalate**

Stop and get expert input if:

```
❌ You're building compliance-critical system (consult lawyer + compliance officer)
❌ You don't know how to implement core approach (prototype + research first)
❌ You're choosing between $50k/month infra cost differences (financial modeling)
❌ You're integrating 5+ external services (API contract review)
❌ You're planning 3+ team dependencies (architecture review)
❌ You're unsure if market wants this (customer research first)
```

---

## **Skills This Teaches**

- ✅ Systematic thinking (breaking large problems into small pieces)
- ✅ Trade-off analysis (no perfect solutions, understanding costs)
- ✅ Architectural thinking (data flow, failure modes, resilience)
- ✅ Scope management (saying no, clear boundaries)
- ✅ Team communication (async decision-making, clear interfaces)
- ✅ Operations (monitoring, logging, deployment automation)
- ✅ Performance thinking (measure before optimizing)
- ✅ Risk thinking (what could go wrong? How do we mitigate?)

---

**Version**: 2.0 (Enhanced with real-world coverage)
**Last Updated**: 2026
