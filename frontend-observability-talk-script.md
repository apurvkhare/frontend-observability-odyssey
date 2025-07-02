# Frontend Observability Odyssey - Speaking Script (25 minutes)

## Opening & Introduction (3 minutes)

**[SLIDE 1: Title]**

Good [morning/afternoon], everyone! Welcome to our Frontend Observability Odyssey. 

*[Pause for effect]*

Today, we're going on a journey—not through space, but through the complex world of monitoring, telemetry, and real-world debugging. By the end of this talk, you'll understand how to build truly resilient frontend applications that can tell you exactly what's wrong when things go sideways.

**[SLIDE 2: About Me]**

Before we dive in, let me quickly introduce myself. I'm a software engineer with 9 years of experience building large-scale frontend applications. Currently, I'm a Computer Scientist 2—that's SDE-4—at Adobe. 

*[Click through points]*

Beyond coding, I love teaching and dancing—yes, dancing! I've had the privilege of mentoring over 1,000 developers, helping them level up their skills. My philosophy is simple: I teach what I know and learn what I don't. And today, I'm excited to share what I've learned about frontend observability with you.

## The Hook: Real Incident (4 minutes)

**[SLIDE 3: Incident Alert]**

Now, let me start with a story that happened to us just last month...

*[Click - Alert appears]*

It's 9:47 AM. I'm enjoying my morning coffee when suddenly... *[dramatic pause]* ...my phone buzzes. PagerDuty alert: Production issue detected.

*[Click - Details appear]*

The smoke test for our checkout flow is failing. Error rate: 87%! That's not a typo—87% of our users trying to checkout are hitting errors. Last successful test? Just 5 minutes ago.

My heart drops. The checkout flow is literally how we make money. Every second counts.

**[SLIDE 4: Investigation]**

*[Show code progressively]*

9:47:30 - First thing I do? Check our APM—Application Performance Monitoring. The error trace tells me: "Cannot read property 'id' of undefined" in CheckoutButton.jsx. We've got 1,847 impacted users already.

*[Click to next section]*

9:47:45 - Next, I check our deployment logs. Aha! A new deployment went out at 9:42—"feat: add promo code validation." That's suspiciously close to when errors started.

*[Click to show buggy code]*

And there it is! The new promo code validation function. It's calling `cartItems.reduce()` without checking if cartItems exists. When users have empty carts—boom! Undefined error.

*[Click to final line]*

9:48:00 - Root cause found! Total investigation time: 30 seconds.

**[SLIDE 5: Deploying Fix]**

*[Show the fix]*

9:48:30 - Deploying the hotfix. We use optional chaining to handle empty carts gracefully. Modern JavaScript saves the day!

*[Click for results]*

Fix deployed in 3 minutes 42 seconds. Customers impacted? Effectively zero—we caught it before it hit mainstream traffic. Revenue saved? About $45,000 based on our typical morning traffic.

**[SLIDE 6: What Just Happened?]**

*[Pause for effect]*

Let that sink in. We caught and fixed a critical production bug in under 4 minutes.

*[Click]*

Before customers even noticed.

*[Click]*

This, my friends, is the power of Frontend Observability.

**[SLIDE 7: Smoke Tests]**

How did we catch this so quickly? Smoke tests—our first line of defense.

*[Click through points]*

These are automated tests that run continuously in production, checking critical user flows. They're quick, focused on what matters most—in e-commerce, that's the checkout flow.

*[Click to show code example]*

Here's what our smoke test looks like. Simple but effective. It mimics a real user: browse products, add to cart, checkout, confirm order. When this fails, we know immediately.

*[Click for pro tip]*

Pro tip: Focus on the most critical user paths that generate revenue. You can't test everything, but you must test what matters most.

## Overview & Core Concepts (4 minutes)

**[SLIDE 8: Today's Journey]**

So what are we covering today?

*[Click through each section]*

First, we'll understand what observability really means and why it's different from monitoring. Then we'll dive into the four types of telemetry data. We'll explore how OpenTelemetry creates vendor-neutral standards. And finally, I'll show you a live demo with Grafana and Sentry.

**[SLIDE 9: Understanding Observability Section]**

Let's start with the fundamentals...

**[SLIDE 10: Monitoring vs Observability]**

Here's a question I get a lot: "Isn't observability just a fancy word for monitoring?" No! They're fundamentally different.

*[Click through monitoring points]*

Monitoring watches known metrics. It alerts when thresholds are crossed. It answers "What happened?" It's like having smoke detectors in your house.

*[Click through observability points]*

Observability lets you explore unknown issues. It provides deep insights into WHY something happened. It's like being a detective with forensic tools.

*[Click for summary]*

Think of it this way: Monitoring tells you when your house is on fire. Observability helps you figure out what started it and how to prevent it next time.

**[SLIDE 11: Why Frontend Observability?]**

*[Read quote dramatically]*

"The user's browser is the most chaotic, unpredictable, and important part of a web application."

Your backend might be complex, but at least you control it. The frontend? That's the Wild West.

*[Click - Infinite Environments]*

Your code runs on thousands of different devices, browsers, and network conditions. A bug that only happens on Safari with a slow 3G connection? Good luck reproducing that in your dev environment!

*[Click - Invisible Errors]*

Many frontend errors never make it to your backend logs. JavaScript errors, failed API calls due to network issues, UI glitches—all invisible without frontend observability.

*[Click - Perceived Performance]*

And here's the kicker: your API might respond in 50ms, but if the user sees a spinner for 5 seconds, guess what? You have a performance problem.

**[SLIDE 12: Real User Monitoring]**

This brings us to RUM—Real User Monitoring.

*[Click through definition]*

RUM collects data from actual user sessions. Not your blazing-fast dev machine, not your high-speed corporate network—real users on real devices in the real world.

*[Click to show tracking categories]*

We track four main categories: Performance metrics like Core Web Vitals, JavaScript errors and exceptions, business metrics like conversion rates, and context—what device, what network, what location.

*[Click for summary]*

Remember: You can only optimize what you measure from real users. Synthetic tests in perfect conditions tell you nothing about the user in rural Texas on 3G with an old iPhone.

## Telemetry Types Deep Dive (6 minutes)

**[SLIDE 13: Telemetry Data Types Section]**

Now let's dive into the four pillars of observability...

**[SLIDE 14: Four Pillars Overview]**

*[Click through each pillar]*

Metrics: Aggregated numerical data—think page load times averaged over time.

Events: Discrete occurrences—user clicked button, feature was used.

Logs: Detailed timestamped records—the breadcrumb trail when things go wrong.

Traces: Request journeys across your entire system—frontend to backend to database and back.

**[SLIDE 15: Metrics]**

Let's start with metrics. These are your app's vital signs.

*[Show code]*

Core Web Vitals—LCP, INP, CLS—these directly impact your search rankings. Business metrics like conversion rates and cart abandonment. Technical metrics like JavaScript heap size and API latency.

These are aggregated, giving you trends over time. Is performance degrading? Are errors increasing? Metrics tell you.

**[SLIDE 16: Performance Video]**

If you want to go deeper on performance metrics, check out my React Nexus talk—link will be at the end.

**[SLIDE 17: Events - Understanding User Journey]**

Events are different from metrics. They capture specific user actions with full context.

*[Click through the three aspects]*

Events help us understand feature adoption in three ways:

Discovery—are users even finding our new features?
Completion rates—of those who start, how many finish?
Drop-off points—where exactly are users getting stuck?

**[SLIDE 18: Chat Feature Flow]**

*[Walk through the flow diagram]*

Here's a real example from our chat feature. 10,000 users visit the page, but only 67% even see the chat button. Of those, only 23% click it. Follow the flow—at each step, we lose users.

*[Point to metrics at bottom]*

The result? Only 14.5% of users successfully use the chat feature end-to-end. Without event tracking, we'd never know that 33% of users can't even find the button!

**[SLIDE 19-20: Event Implementation]**

*[Show code examples]*

Look at this event tracking implementation. We're not just recording "button clicked"—we're capturing when, where, by whom, in what context. This rich data lets us understand user behavior, not just count clicks.

**[SLIDE 21: Logs]**

Now, logs—your debugging lifeline.

*[Show bad vs good example]*

See the difference? Bad logging just says "Error in checkout." Good logging provides full context—what failed, for which user, what was in their cart, and crucially, the breadcrumb trail of actions leading to the error.

*[Click to show log levels]*

Use structured logging with proper levels. DEBUG for development, ERROR for production issues, FATAL for app-breaking problems. And always, always include context.

**[SLIDE 22: Traces]**

*[Show Gantt chart]*

Traces follow a request through your entire system. This timeline shows a real request: Frontend makes an API call at 100ms, auth check happens, database query runs—you see exactly where time is spent.

**[SLIDE 23: Distributed Tracing Concepts]**

*[Click - TraceId]*

Every request gets a TraceId—a unique identifier that follows it everywhere.

*[Click - CCID]*

The Correlation Context ID links spans across service boundaries, propagated through HTTP headers.

*[Click - Span explanation]*

A span is like a function call—it has a start, does work, and ends. Multiple spans form a trace.

**[SLIDE 24: Span Hierarchy]**

*[Show the tree diagram]*

Here's how spans relate. The HTTP request is the root span. It spawns child spans—React rendering, API calls. Each API call spawns its own children—database queries, auth checks. This hierarchy shows you exactly how your system processes requests.

**[SLIDE 25: Implementation Code]**

*[Show code quickly]*

Implementation is straightforward with OpenTelemetry. Start a span, do work, handle errors, end the span. The SDK handles the complex parts—context propagation, timing, correlation.

## Alerts & SLAs (3 minutes)

**[SLIDE 26: Alerts]**

Before we dive into architecture, let's talk about alerts—because what good is all this data if nobody acts on it?

*[Show configuration]*

Here's a real alert configuration. When smoke test success drops below 95% for 1 minute, alert the frontend team. Simple but effective.

*[Click through impact points]*

Good alerts catch issues early, before customers complain. They prevent revenue loss by protecting critical flows. And most importantly, they help you avoid SLA breaches—because that 0.1% difference between 99.9% and 99.8% uptime? That could be a $100K penalty.

**[SLIDE 27: SLI, SLO, and SLA]**

Speaking of SLAs, let's clarify these three terms that everyone confuses:

*[Click - SLI]*

SLI—Service Level Indicator. This is what you actually measure. Real numbers: page loads in 1.2 seconds, API success rate is 99.97%.

*[Click - SLO]*

SLO—Service Level Objective. Your internal targets. You want page loads under 2 seconds for 95% of users.

*[Click - SLA]*

SLA—Service Level Agreement. What you promise customers. Usually looser than your SLO to give you buffer room.

*[Click - Summary]*

Pro tip: Keep your SLOs stricter than your SLAs. This buffer gives you time to fix issues before breaking customer promises.

## Architecture & Standards (3 minutes)

**[SLIDE 28: OpenTelemetry Section]**

Now, let's talk about the game-changer: OpenTelemetry.

**[SLIDE 29: OpenTelemetry Standards]**

OpenTelemetry is huge because it's vendor-neutral. One SDK, one set of standards, works with any backend.

*[Click through benefits]*

No vendor lock-in—switch from Datadog to Grafana without changing your code. Consistent instrumentation across all services. Community-driven development. It's the Linux of observability.

*[Click to show code]*

Look how simple it is. One SDK configures traces, metrics, and logs. Auto-instrumentation means you get telemetry without writing code for every operation.

**[SLIDE 30: Telemetry Architecture]**

*[Show architecture diagram]*

Here's how it all fits together. Your applications send data through OpenTelemetry SDK to collectors. Collectors process and route data to specialized storage—Prometheus for metrics, Jaeger for traces, Loki for logs. 

Everything feeds into your observability platform—dashboards, alerts, business intelligence.

*[Click through each layer]*

The Collection Layer instruments your app and standardizes data format. The Storage Layer uses specialized databases optimized for each data type. Query & Analysis tools let you explore and visualize. And the Alerting layer turns insights into action.

*[Click - Key insight]*

The beauty? Every layer is vendor-agnostic thanks to OpenTelemetry. No lock-in, full flexibility.

## Demo & Closing (3 minutes)

**[SLIDE 31: Live Demo]**

Let me show you this in action...

*[Switch to demo]*

[Demo talking points:
- Show Grafana dashboard with real metrics
- Demonstrate error tracking in Sentry
- Show a distributed trace
- Highlight how quickly you can go from alert to root cause]

**[SLIDE 32: Key Takeaways]**

Let's wrap up with the key takeaways:

*[Click through each point]*

First, alerts save revenue. Those 4 minutes we saved in our opening story? That's real money.

Events reveal user intent. It's not about counting clicks—it's about understanding user journeys and friction points.

The four pillars work together. Metrics show trends, events show behavior, logs show details, traces show flow. You need all four.

And OpenTelemetry future-proofs your observability. Invest once, use anywhere.

**[SLIDE 33: Truly Observable]**

*[Read dramatically]*

Here's my challenge to you: If you can debug an unknown issue experienced by a user running version 2.1.3 of your web app on Safari 17.2 in the us-west region with a slow 3G connection—WITHOUT deploying additional code—then your web application is truly observable.

*[Pause]*

This is the gold standard. This is what we should all aim for.

**[SLIDE 34: Contact]**

Thank you! Find me on GitHub, Twitter, and LinkedIn. The slides and demo code are available at the repo link shown.

Any questions?

---

## Timing Notes:
- **Total time:** 25 minutes
- **Leave 2-3 minutes for questions**
- **Practice transitions between sections**
- **Have backup slides ready for deep-dive questions**

## Speaking Tips:
1. **Energy:** Start high-energy with the incident story
2. **Pace:** Slow down for technical concepts, speed up for familiar territory  
3. **Interaction:** Make eye contact, use hand gestures for emphasis
4. **Demos:** Have backup screenshots if live demo fails
5. **Stories:** Use the opening incident story to anchor concepts throughout

## Key Phrases to Emphasize:
- "Under 4 minutes"
- "Before customers noticed"  
- "Real users, real devices, real networks"
- "You can't fix what you can't see"
- "Vendor-neutral standards"
- "Truly observable" 