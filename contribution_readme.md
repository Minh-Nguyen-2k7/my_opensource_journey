# Contribution 1: #1006

**Contribution Number:** 1 
**Student:** Ngoc Quang Minh Nguyen  
**Issue:** https://github.com/marketcalls/openalgo/issues/1006  
**Status:** Completed

---

## Why I Chose This Issue

The issue asks for a reusable EmptyState component to replace plain text placeholders across three pages in a React trading dashboard. Right now, when there's no data, users just see bare strings like 'No holdings found' — which is an opportunity for a better UI. I chose this issue because it's well-scoped for a first contribution, touches real UI patterns like conditional rendering and reusable components, and the maintainer provided a clear example to work from. On top of that, I can put my React skills to practice and be better at frontend. I hope that I can learn how frontend/React is used in the real world, and that I can be prepared for future contributions as well.

---

## Understanding the Issue

### Problem Description

There are certain components in the code that is just showing plain text when there is no data, and there is a need to create a component to display more than just a plain text.

### Expected Behavior

There should be a plain text, with an icon displaying the empty state it is having, and an action button for the empty state depending on the need. For instance, if there is no data because filters do not match, the action button can be reset filters.

### Current Behavior

It's just showing plain text, like "there is no data". No icon, though some components still have buttons to solve it. Creating a whole component to unify this UI display, basically.

### Affected Components
There are 3 components affected, as in the issuer's quote:
"""
frontend/src/pages/Holdings.tsx (line 310: "No holdings found")
frontend/src/pages/OrderBook.tsx (line 481: "No orders today")
frontend/src/pages/Positions.tsx (line 852: "No positions match your filters")
"""

---

## Reproduction Process

### Environment Setup

[Notes on setting up your local development environment - challenges you faced, how you solved them]
The installation setup was mentioned in the github, and it was suppose to be simple to recreate the issue. However, "openalgo" requires user account to log in (this issue belongs inside user account), and I can't make a user account due to openalgo requiring some kind of broker. So I had to resolve the issue without actually seeing what the frontend UI looks like. Fortunately Claude helps that by helping me displaying what the component looks like without the empty state, so I can have a better visualization on what needs to be done. There were also some library installation that needs to be put in the terminal, which Claude also helped by giving it the error code.

### Steps to Reproduce

There's not really a chance to reproduce the issue, when I can't see what the frontend looks like. Claude helps me reproduce/showing what the frontend looks like, if that make sense.

### Reproduction Evidence

- **Commit showing reproduction:** 


---

## Solution Approach

### Analysis

The root cause is that the three pages each handle their empty state inline with plain div and text, with no shared abstraction. Each page has slightly different needs (some have filter states, some don't), which makes a reusable component with an optional action prop the right approach.

### Proposed Solution

Create a single reusable EmptyState component accepting icon, title, description, and an optional action prop. Replace the inline plain text in all three pages with this component, passing appropriate icons and descriptions per context.

### Implementation Plan

Using UMPIRE framework (adapted):

**Understand:** Three pages show plain text when data is empty — poor UX with no visual hierarchy or guidance.

**Match:** The project uses shadcn/ui conventions and lucide-react icons throughout, so the component should follow the same patterns.

**Plan:** [Step-by-step implementation plan]
1. Create frontend/src/components/ui/empty-state.tsx with the EmptyState component
2. Replace plain text in Holdings.tsx, OrderBook.tsx, Positions.tsx
3. Handle OrderBook's two sub-states (no data vs filtered) separately

**Implement:**  https://github.com/marketcalls/openalgo/pull/1484

**Review:** Followed shadcn/ui component structure; action is optional so it doesn't break pages that don't need a button

**Evaluate:** Verified visually using Claude to mock up what the component would look like since the app requires a broker login to access these pages

---

## Testing Strategy

### Unit Tests

The project does not have a frontend test suite.

### Integration Tests

The project does not have a frontend test suite.

### Manual Testing

Could not reproduce locally as the app requires broker authentication to access the affected pages. Used Claude to mock up the UI visually based on the actual JSX code to verify the component looked correct. Maintainer verified with npm run build and npm run lint — both green after merge.

---

## Implementation Notes

### Week [X] Progress

[What you built this week, challenges faced, decisions made]

### Week [Y] Progress

[Continue documenting as you work]

### Code Changes

- **Files modified:** frontend/src/components/ui/empty-state.tsx (created), frontend/src/pages/Holdings.tsx, frontend/src/pages/OrderBook.tsx, frontend/src/pages/Positions.tsx
- **Key commits:** https://github.com/marketcalls/openalgo/pull/1484
- **Approach decisions:** OrderBook had two distinct empty states (no data vs no filter results), so I used two separate EmptyState calls with different icons and descriptions rather than a single generic message. Positions uses a single state but conditionally renders the Clear Filters button via the action prop.

---

## Pull Request

**PR Link:** https://github.com/marketcalls/openalgo/pull/1484

**PR Description:** [Draft or final PR description - much of the content above can be adapted]

**Maintainer Feedback:**
- [7/6/2026]: PR merged. Maintainer applied follow-up polish: renamed file to empty-state.tsx (kebab-case convention), switched to import type { LucideIcon }, reformatted to 2-space indentation (Biome), and reverted an unrelated whitespace change. Core implementation kept intact.

**Status:** [Merged]

---

## Learnings & Reflections

### Technical Skills Gained

Learned how to build a reusable React component with optional props using React.ReactNode, how Lucide icons are typed as LucideIcon and used as JSX via destructure renaming (icon: Icon), and how shadcn/ui component conventions work in a real codebase.

### Challenges Overcome

 Could not access the actual UI since the app requires broker authentication. Worked around this by reading the raw JSX and using Claude to mock up what the components looked like, which gave enough context to implement the fix confidently.

### What I'd Do Differently Next Time

Run npm run format before pushing to avoid indentation and import style issues. Also check the existing file naming convention in the folder before naming new files.

---

## Resources Used

https://github.com/marketcalls/openalgo/issues/1006 — original issue
https://lucide.dev/icons — for choosing appropriate icons
https://ui.shadcn.com/docs — shadcn/ui component conventions
