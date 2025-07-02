# Frontend Observability - Quick Reference Card

## Talk Flow (25 mins)

### 1. Opening (3 mins)
- [ ] Welcome & title
- [ ] About me - Adobe, 9 years, 1000+ developers
- [ ] Set up the journey metaphor

### 2. The Hook (4 mins) 
- [ ] 9:47 AM - PagerDuty alert!
- [ ] 87% error rate in checkout
- [ ] Show investigation: APM → Deployment logs → Code
- [ ] Root cause in 30 seconds
- [ ] Deploy fix with optional chaining
- [ ] Fixed in 3m 42s, saved $45K
- [ ] What just happened? Power of observability
- [ ] **SMOKE TESTS** - First line of defense

### 3. Overview & Core Concepts (4 mins)
- [ ] Today's journey overview
- [ ] Monitoring vs Observability (fire alarm vs detective)
- [ ] Why Frontend? Chaos of browsers/devices
- [ ] **RUM** - real users, real problems, 4 categories

### 4. Telemetry Types (6 mins)
- [ ] 4 Pillars: Metrics, Events, Logs, Traces
- [ ] Metrics = vital signs (Core Web Vitals)
- [ ] Events = user behavior understanding (3 aspects)
- [ ] Chat feature flow: 14.5% success rate
- [ ] Event implementation with rich context
- [ ] Logs = debugging breadcrumbs with structure
- [ ] Traces = request journey (TraceId, Spans, hierarchy)

### 5. Alerts & SLAs (3 mins)
- [ ] Alert configuration & impact
- [ ] Prevent SLA breaches ($100K difference)
- [ ] **SLI vs SLO vs SLA** - measurements vs targets vs contracts

### 6. Architecture & Standards (3 mins)
- [ ] OpenTelemetry = vendor-neutral
- [ ] Architecture: Apps → Collector → Storage → Analysis
- [ ] Collection & Storage layers

### 7. Demo & Close (3 mins)
- [ ] Live demo (have backup screenshots!)
- [ ] Key takeaways
- [ ] Challenge: Debug Safari on 3G without deploying
- [ ] Contact info & questions

## Key Numbers to Remember
- **3m 42s** - Time to fix production issue
- **87%** - Error rate in opening story
- **$45,000** - Revenue saved
- **14.5%** - Chat feature adoption rate
- **0.1%** - Difference that costs $100K in SLA
- **67%** - Discovery rate (saw chat button)
- **23%** - Engagement rate (clicked chat)

## Memorable Quotes
- "The browser is the most chaotic part of a web app"
- "Monitoring = fire alarm, Observability = detective"
- "You can't fix what you can't see"
- "Real users on real devices in the real world"
- "You can only optimize what you measure from real users"

## Energy Guide
🔥 HIGH: Opening incident story
🎯 MEDIUM: Smoke tests explanation
📊 MEDIUM: Technical concepts  
📈 BUILD: Chat feature adoption funnel
⚡ PEAK: Architecture benefits
🏁 STRONG: Closing challenge

## Key Structural Changes
- **Smoke Tests** moved early (right after incident story)
- **RUM section** added early in core concepts
- **SLI/SLO/SLA** properly positioned after alerts
- **Architecture** streamlined to focus on key layers 