# Contribution [#]: [Issue Title]

**Contribution Number:** 4 
**Student:** Ngoc Quang Minh Nguyen  
**Issue:** https://github.com/Raftersecurity/rafter-cli/issues/37  
**Status:** Merged

---

## Why I Chose This Issue

The issue asks for convenience scripts to be added to the Node package.json to streamline common local development tasks like watch-mode testing, test coverage, and cache clearing. I chose this issue because it is a great "good first issue" that directly improves the developer experience (DX) for everyone contributing to the project. It gave me a practical look at project configuration and standard tool alignment (like configuring Vitest for both single-run CI pipelines and interactive local development). Tackling this also helped build my confidence in maintaining clean codebase configurations and navigating open-source repository structures.

---

## Understanding the Issue

### Problem Description

The project was missing standardized convenience commands in node/package.json for common development workflows. Contributors lacked quick, built-in shortcuts to run unit tests interactively (watch mode), generate coverage reports, or clean up compiler/dependency artifacts. Furthermore, the existing "test" script was mapped to open interactive watch mode by default, which can cause issues in automated continuous integration (CI) environments that expect the test process to execute once and exit.

### Expected Behavior

The node/package.json file should expose standard scripts to make development frictionless:

"test": Executes the entire test suite exactly once and exits (perfect for CI and pre-push checks).

"test:watch": Launches tests in interactive watch mode for instant local feedback.

"test:coverage": Generates an up-to-date code coverage report.

"clean": Safely purges compiled build artifacts (dist/) and local compiler caches to allow for a clean build state.

### Current Behavior

Prior to the fix, the "scripts" block only contained basic commands. Specifically, the "test" script was mapped to run "vitest", which defaults to interactive watch mode rather than a single-run test pass. There were no built-in scripts for generating coverage reports or sweeping away stale build caches.

### Affected Components

node/package.json: The "scripts" block required restructuring to update the existing "test" script and introduce the new "test:watch", "test:coverage", and "clean" definitions.

---

## Reproduction Process

### Environment Setup

Operating System: Windows (utilizing Git Bash / terminal).

Node Environment: Managed via Corepack to ensure the correct package manager version pinned by the project maintainers was utilized locally.

Challenges & Solutions: The primary hurdle during setup was resolving package file tracking and alignment issues across the monorepo structure. Running corepack enable successfully resolved these local environment discrepancies, ensuring that pnpm operated identically to the maintainers' environment and kept lockfile entries stable.

### Steps to Reproduce

1. [Step 1]
2. [Step 2]
3. [Observed result]

### Reproduction Evidence

- **Commit showing reproduction:** https://github.com/Raftersecurity/rafter-cli/pull/202/changes/efb3dfed6655d999297cf41484836d6269603168
- **Screenshots/logs:** [If applicable]
- **My findings:** [What you discovered during reproduction]

---

## Solution Approach

### Analysis

The root cause of the issue was a lack of standard, automated script definitions in node/package.json for routine development tasks.

Additionally, a configuration gap existed between local development expectations and typical continuous integration (CI) environments:

The existing "test" script was mapped to vitest, which defaults to interactive watch mode. If executed in a headless CI pipeline or a pre-commit hook, this command would hang indefinitely rather than exiting.

Contributors had to manually memorize and type flags for coverage reports (vitest run --coverage) or manual file cleanup (rm -rf dist/ ...), harming development velocity and increasing the risk of human error.

### Proposed Solution

The fix involves restructuring the "scripts" block in node/package.json to:

Update the default "test" script to run a single execution pass (vitest run), which is safe for CI pipelines.

Introduce a dedicated "test:watch" script to preserve and standardize the interactive testing feedback loop for local development.

Add a "test:coverage" shortcut to generate local coverage reports easily.

Add a "clean" utility script to quickly sweep away target build products and local cache artifacts.

### Implementation Plan

Using UMPIRE framework (adapted):

**Understand:** 
We need to add four standard development shortcuts to node/package.json while ensuring the base "test" command behaves as a single-run execution pass instead of an interactive watch process.

**Match:** 
Standard Node.js and Vitest monorepos typically isolate single-run commands from watch-mode commands. We can use the native --coverage flag provided by Vitest and standard POSIX commands (rm -rf) for directory cleaning.

**Plan:** [Step-by-step implementation plan]
1. Open node/package.json.

2. Locate the "scripts" object.

3. Replace "test": "vitest" with "test": "vitest run".

4. Append "test:watch": "vitest".

5. Append "test:coverage": "vitest run --coverage".

6. Append "clean": "rm -rf dist/ node_modules/.cache/".

7. Save the file and run each command locally to verify behavior.

**Implement:** https://github.com/Raftersecurity/rafter-cli/pull/202/changes/efb3dfed6655d999297cf41484836d6269603168

**Review:** 
[x] Changes align with the maintainer's issue specifications.

[x] No unrelated files or lockfiles were modified.

[x] No API keys, credentials, or personal secrets are present in the diff.

**Evaluate:** 
To verify success, execute the following commands in the node/ subdirectory and check their outputs:

pnpm test — Confirmed the suite runs exactly once and exits with code 0.

pnpm test:watch — Confirmed Vitest starts in interactive watch mode, listening for file changes.

pnpm test:coverage — Confirmed Vitest runs the tests and outputs a structured coverage report in the terminal.

pnpm clean — Confirmed the dist/ and node_modules/.cache/ directories are completely purged without throwing syntax errors.

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

**PR Link:** https://github.com/Raftersecurity/rafter-cli/pull/202

**PR Description:** 
Summary
This PR adds helpful utility scripts to node/package.json to streamline the local development workflow, resolving the requested issue.

Specifically, this adds:

test:watch: Runs tests interactively in watch mode (using vitest).
test:coverage: Runs unit tests with a coverage report.
clean: Easily wipes out builds and cached files (dist/, node_modules/.cache/).
To align with standard monorepo workflows, the main "test" script has been updated to run a single execution (vitest run), while "test:watch" now handles the continuous interactive testing loop.

Test plan
Verify that the new scripts execute correctly locally using your package manager:

Run pnpm test (Runs tests once and exits).
Run pnpm test:watch (Launches interactive watch mode).
Run pnpm test:coverage (Generates test coverage details).
Run pnpm clean (Wipes the target build and cache directories).
Spec update
N/A — Internal package development scripts only.

Checklist
 Both Node and Python implementations updated -> (N/A, Node scripts only)
 Tests pass in both (pnpm test + pytest)
 Versions match in node/package.json and python/pyproject.toml -> N/A
 shared-docs/CLI_SPEC.md updated (if CLI behavior changed) -> N/A
 No secrets, credentials, or API keys in the diff

**Maintainer Feedback:**
- [7/20/2026]: Shipped in #202 (thanks @Minh-Nguyen-2k7!) with a small post-merge tidy in #203 — test now runs vitest run (no watch-mode hang in CI), with test:watch, test:coverage, and clean added. Closing.

**Status:** Merged

---

## Learnings & Reflections

### Technical Skills Gained

Vitest CLI Configuration: Learned how to configure and separate Vitest executions for different environments, specifically isolating single-run execution (vitest run) for CI pipelines from continuous watch mode (vitest) for active local development.

NPM Script Lifecycle & Monorepos: Gained a better understanding of structuring standard development workflows in a Node.js project, ensuring tasks like test coverage and artifact cleaning are easily accessible to contributors.

Package Manager Management: Gained practical experience utilizing Corepack to lock down and manage specific package manager versions (like pnpm) in active development environments.

### Challenges Overcome

Addressing Codebase Drift: The main challenge was noticing that the actual package.json had deviated from the issue author's description. Rather than blindly adding test:watch as a duplicate of the existing test command, I had to analyze the current setup, catch the redundancy, and safely restructure the script definitions to restore proper behavior.

Environment Synchronization: Resolving package discrepancies and lockfile stability issues was challenging at first. I overcame this by enabling Corepack (corepack enable), which instantly aligned my local package manager configuration with the repository standard and cleaned up the file tracking errors.

### What I'd Do Differently Next Time

Verify the Codebase First: In the future, I will always cross-reference the issue description with the live codebase before drafting any solutions. Catching the script discrepancy early on would have saved me some initial planning time, highlighting how crucial it is to inspect existing source files before writing code.

---

## Resources Used

- Vitest Command Line Interface Docs: Vitest CLI Reference — Used to verify the proper flags for single-run execution and coverage generation.

- Node.js Corepack Documentation: Node.js Corepack Guide — Helped troubleshoot the initial setup and enforce package manager alignment.

- Rafter CLI GitHub Repository: Rafter-CLI Issue Tracker — Referencing issue posts and project discussions to understand target repository conventions.