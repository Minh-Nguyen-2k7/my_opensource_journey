# Contribution [#]: [Issue Title]

**Contribution Number:** 2 
**Student:** Ngoc Quang Minh Nguyen  
**Issue:** https://github.com/Raftersecurity/rafter-cli/pull/189  
**Status:** Awating Review

---

## Why I Chose This Issue
I chose this issue because it gave me the chance to work on a real-world project that connects both Node (TypeScript) and Python. The task was clear and meaningful: adding secret scanning for DigitalOcean Personal Access Tokens with precise regex patterns to avoid false positives.

Thanks to this learning experience, I get to navigate project structures, run dependency managers like Poetry and pnpm, and dual test suites. I also gained hands-on experience solving environment setup challenges, testing logic across different runtimes, and seeing how production-grade security tools are built and maintained.

---

## Understanding the Issue

### Problem Description

The secret scanner was missing a dedicated validation pattern to detect DigitalOcean Personal Access Tokens (PATs). Without this, users could accidentally commit active DigitalOcean credentials to their repositories without triggering a warning from the tool.

### Expected Behavior

The tool should automatically scan files and detect DigitalOcean PATs across both supported ecosystems (Node and Python) using an identical, precise regex pattern. It should identify tokens starting with the distinct prefix and flag them cleanly without causing high false-positive rates.

### Current Behavior

Prior to this fix, strings matching the DigitalOcean token format were completely ignored during scans, as no pattern or corresponding test definitions existed in either codebase.

### Affected Components

- node/src/scanners/secret-patterns.ts (Node regex addition)

- node/tests/secret-patterns.test.ts (Node unit tests)

- python/rafter_cli/scanners/secret_patterns.py (Python regex addition)

- python/tests/test_regex_scanner.py (Python unit tests)

---

## Reproduction Process

### Environment Setup

Local Setup: Cloned the repository and managed dependencies using pnpm for the Node side and poetry for the Python side.

Challenges & Solutions: Encountered a local build failure on Windows under Python 3.14 because pydantic-core lacked a pre-compiled binary wheel and required a local Rust compiler toolchain to build from source. Bypassed this local environment block safely by writing a standalone target verification script (quick_test.py) using Python's core re library to validate the raw regex logic independently before committing. Additionally resolved folder-scoping limits with git add by running the stage commands from the root workspace directory instead of subfolders.

### Steps to Reproduce

1. Feed a sample payload containing a structurally valid mock DigitalOcean token (e.g., dop_v1_ followed by 64 hexadecimal characters) into the scanner engine.

2. Run the internal test suites using pnpm test or pytest.

3. Observed result: The scanner ignores the token entirely, returning zero match alerts because the pattern definition is absent from the configuration arrays.

### Reproduction Evidence

- **Commit showing reproduction:** [Link to commit in your fork]
- **Screenshots/logs:** [If applicable]
- **My findings:** [What you discovered during reproduction]

---

## Solution Approach

### Analysis

[Your analysis of the root cause - what's causing the issue?]

### Proposed Solution

[High-level description of your fix approach]

### Implementation Plan

Using UMPIRE framework (adapted):

**Understand:** [Restate the problem]

**Match:** [What similar patterns/solutions exist in the codebase?]

**Plan:** [Step-by-step implementation plan]
1. [Modify file X to do Y]
2. [Add function Z]
3. [Update tests]

**Implement:** [Link to your branch/commits as you work]

**Review:** [Self-review checklist - does it follow the project's contribution guidelines?]

**Evaluate:** [How will you verify it works?]

---

## Testing Strategy

### Unit Tests

- [ ] Test case 1: [Description]
- [ ] Test case 2: [Description]
- [ ] Test case 3: [Description]

### Integration Tests

- [ ] Integration scenario 1
- [ ] Integration scenario 2

### Manual Testing

[What you tested manually and results]

---

## Implementation Notes

### Week [X] Progress

[What you built this week, challenges faced, decisions made]

### Week [Y] Progress

[Continue documenting as you work]

### Code Changes

- **Files modified:** [List]
- **Key commits:** [Links to important commits]
- **Approach decisions:** [Why you chose certain approaches]

---

## Pull Request

**PR Link:** [GitHub PR URL when submitted]

**PR Description:** [Draft or final PR description - much of the content above can be adapted]

**Maintainer Feedback:**
- [Date]: [Summary of feedback received]
- [Date]: [How you addressed it]

**Status:** [Awaiting review / Iterating / Approved / Merged]

---

## Learnings & Reflections

### Technical Skills Gained

[What you learned technically]

### Challenges Overcome

[What was hard and how you solved it]

### What I'd Do Differently Next Time

[Reflection on your process]

---

## Resources Used

- [Link to helpful documentation]
- [Tutorial or Stack Overflow post that helped]
- [GitHub issues or discussions that helped]
