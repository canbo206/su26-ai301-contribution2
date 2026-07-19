# su26-ai301-contribution2
# Contribution [2]: prefer-use-sync-external-store: Replace useState + useEffect with useSyncExternalStore

**Contribution Number:** 2  
**Student:** Canbo Li
**Issue:** (https://github.com/wso2/product-is/issues/27946)  
**Status:** Phase III Ongoing

---

## Why I Chose This Issue

I chose issue #27946 "[React Doctor] prefer-use-sync-external-store" because it is 
a well scoped refactoring task with only 2 affected files, making it very manageable. 
The issue is labeled "Good First Issue" and I was assigned by the maintainer after commenting.

I'm interested in this because:
1. Only 2 files are affected it has a clear scope
2. It builds on what I learned from my first contribution to the same codebase
3. I'll learn about useSyncExternalStore, a React hook I haven't used before
4. I'm already familiar with the identity-apps repo setup from my previous contribution

The issue involves replacing a useState + useEffect pattern used to sync with an 
external store with React's built in useSyncExternalStore hook, which handles 
tearing during concurrent renders more safely.

Left a comment on the issue and was assigned by the maintainer.

---

## Understanding the Issue

### Problem Description

Two components use a useState + useEffect pattern to sync activeTab with the 
browser's URL hash. React has a built in hook useSyncExternalStore specifically designed for subscribing to external stores safely during concurrent renders.

### Expected Behavior

The activeTab state should be managed with useSyncExternalStore(subscribe, getSnapshot) where subscribe handles the hashchange listener and getSnapshot returns the current active tab from the URL.

### Current Behavior

Both components use:
- useState to store activeTab
- useEffect to add/remove a window hashchange event listener that calls setActiveTab

### Affected Components

- features/admin.console-settings.v1/components/console-settings-tabs.tsx (line 202)
- features/admin.tenants.v1/components/system-settings/system-settings-tabs.tsx (line 116)

---

## Reproduction Process

### Environment Setup

- Already had identity-apps forked and cloned from Contribution 1
- Created new branch fix-issue-27946 from master

### Steps to Reproduce

1. Clone https://github.com/wso2/identity-apps
2. Open features/admin.console-settings.v1/components/console-settings-tabs.tsx
3. Navigate to line 202 — observe useState storing activeTab
4. Navigate to line 207 — observe useEffect subscribing to window hashchange 
   events to call setActiveTab
5. Repeat for features/admin.tenants.v1/components/system-settings  system-settings-tabs.tsx at line 116
6. Both files show the same useState + useEffect external store sync pattern

### Reproduction Evidence

- **Commit showing reproduction:**  https://github.com/canbo206/identity-apps/tree/fix-issue-27946
- **Screenshots/logs:** [If applicable]
- **My findings:** Confirmed identical useState + useEffect hashchange patterns 
  in both affected files exactly as described in the issue

---

## Solution Approach

### Analysis

[Your analysis of the root cause - what's causing the issue?]

### Proposed Solution

[High-level description of your fix approach]

### Implementation Plan

Using UMPIRE framework (adapted):

**Understand:** 
Both components sync activeTab with the browser URL hash using useState + useEffect. 
The window hashchange event is an external store. useSyncExternalStore is the 
correct React hook for this pattern.

**Match:**  
Both files have identical patterns so the same fix applies to both.

**Plan:** [Step-by-step implementation plan]
1. Add useSyncExternalStore to the React import in both files
2. Remove useState and useEffect for activeTab
3. Define a subscribe function that adds/removes the hashchange listener
4. Define a getSnapshot function that returns getActiveTabFromUrl()
5. Replace with: const activeTab = useSyncExternalStore(subscribe, getSnapshot)
6. Verify no TypeScript errors

**Implement:** https://github.com/canbo206/identity-apps/tree/fix-issue-27946

**Review:** 
- [ ] Follows TypeScript conventions from CLAUDE.md
- [ ] No new TypeScript errors
- [ ] Checked CONTRIBUTING.md

**Evaluate:** 
- Run pnpm typecheck to verify no errors
- Manually verify tab switching still works correctly

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

### Week [7] Progress

[What you built this week, challenges faced, decisions made]
I've reproduced the issue and reviewed the contribution guidelines for this issue thoroughly
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
