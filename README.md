# CodePath AI301 Contribution README

## Phase I: Issue Selection

### Status

Phase I Complete

### Issue

**Repository:** Roll20/roll20-character-sheets  
**Issue:** #14138  
**Issue Title:** [Dark Heresy 2nd Edition] Unnatural Characteristics not being added on successful tests  
**Issue Link:** https://github.com/Roll20/roll20-character-sheets/issues/14138

### Problem Summary

This issue is about a bug in the Dark Heresy 2nd Edition Roll20 character sheet. When a character has an Unnatural Characteristic value, half of that value, rounded up, should be added to the Degrees of Success when the character succeeds on a test. Currently, the sheet does not add the Unnatural Characteristic value to successful tests, so the roll result is missing part of the expected calculation.

### Why I Chose This Issue

I chose this issue because it is open, unassigned, and marked with a `help wanted` label. It also does not appear to have a linked pull request or branch already solving it. I like that the problem is specific and understandable: a successful test should include the Unnatural Characteristic bonus, but currently it does not. This seems like a good first open-source contribution because it is likely focused on one character sheet and one calculation behavior instead of a large system redesign.

### Issue Selection Checklist

- **I understand the problem:** Yes. The Degrees of Success calculation is missing the Unnatural Characteristic bonus on successful tests.
- **The scope fits 3–4 weeks of work:** Yes. The issue appears focused on one Roll20 character sheet and one calculation rule.
- **It matches my skills or things I can learn quickly:** Yes. The likely work involves HTML/JavaScript character sheet logic, which I can learn and trace with AI support.
- **The issue is active and claimable:** Yes. The issue is open, unassigned, and has no linked pull request or branch.
- **There is helpful context:** Yes. The issue explains the expected rule and gives an example using Strength 25 and Unnatural Strength (1).
- **The project has setup/contribution documentation:** Yes. The repository README includes contribution guidance for improving existing character sheets.

### Likely Files or Area Involved

The issue is likely inside the Dark Heresy 2nd Edition character sheet folder in the Roll20 character sheets repository. I expect the relevant files to be the sheet HTML and any related JavaScript/sheetworker logic used to calculate Degrees of Success.

### Fork

My fork: https://github.com/Frehmand/roll20-character-sheets


---

## Phase II: Reproduce & Plan

### Status

**COMPLETED**

### Branch Link

Working branch: https://github.com/Frehmand/roll20-character-sheets/tree/fix-issue-14138

### Environment Setup

I cloned my fork of the Roll20 character sheets repository and created a working branch for issue #14138.

Commands used:

```bash
git clone https://github.com/Frehmand/roll20-character-sheets.git
cd roll20-character-sheets
git checkout -b fix-issue-14138
git push origin fix-issue-14138


---

## Phase III: Build

### Current Status

**COMPLETED**

### Implementation Notes

I implemented the fix for Roll20 issue #14138 in:

```text
Dark_Heresy_2ed/DarkHeresy2ed.html
```

The change adds the appropriate Unnatural Characteristic bonus to the displayed Degrees of Success for successful characteristic tests.

The calculation used is:

```text
normal degrees + ceil(Unnatural Characteristic / 2)
```

The fix covers all ten characteristics:

* Weapon Skill — `UnWS`
* Ballistic Skill — `UnBS`
* Strength — `UnS`
* Toughness — `UnT`
* Agility — `UnAg`
* Intelligence — `UnInt`
* Perception — `UnPer`
* Willpower — `UnWP`
* Fellowship — `UnFel`
* Influence — `UnInf`

I also updated the Dark Heresy 2nd Edition roll template so it displays the adjusted Degrees of Success for successful tests while preserving the original degree value for failed tests.

### Code Changes

Working branch:

https://github.com/Frehmand/roll20-character-sheets/tree/fix-issue-14138

Implementation commits:

https://github.com/Frehmand/roll20-character-sheets/commit/5d68cfe1a8

https://github.com/Frehmand/roll20-character-sheets/commit/0cc74a0055

Files modified:

```text
Dark_Heresy_2ed/DarkHeresy2ed.html
```

### Testing Strategy

I performed the following validation:

* Ran `git diff --check` with no reported errors
* Reviewed the complete Git diff before committing
* Confirmed that all ten characteristic roll buttons reference the correct Unnatural Characteristic attribute
* Confirmed that the edited roll buttons remain on complete HTML lines
* Confirmed that the changes only modified the relevant Dark Heresy 2nd Edition HTML file
* Confirmed that failed-test display logic continues to use the original degree result
* Confirmed that both implementation commits were pushed successfully to the working branch

Live Roll20 testing could not be completed because custom character sheet access was not available on my current Roll20 account. I documented this limitation rather than claiming the fix was fully tested in the Roll20 runtime.

### Challenges Faced

The character sheet uses Roll20-specific roll syntax and cannot be fully tested by opening the HTML file in a normal browser.

The main challenge was preserving the original success-and-failure calculation while adding the appropriate Unnatural Characteristic bonus only to the displayed Degrees of Success for successful tests.

I handled this by keeping the original `degrees` value and introducing a separate `successdegrees` value for the adjusted successful result.

### Current Status

**COMPLETED**

### Pull Request

https://github.com/Roll20/roll20-character-sheets/pull/15219


## Phase IV: Submit & Iterate

### Pull Request

**PR Link:**
https://github.com/Roll20/roll20-character-sheets/pull/15219

**Merged Commit:**
https://github.com/Roll20/roll20-character-sheets/commit/ca3689d0a7f2a66e25a7967d39378b7c3575ce9f

### PR Description

This pull request fixed Roll20 issue #14138 for the Dark Heresy 2nd Edition character sheet.

The contribution updated successful characteristic tests so the displayed Degrees of Success include half of the appropriate Unnatural Characteristic value, rounded up. The fix covers all ten characteristics: Weapon Skill, Ballistic Skill, Strength, Toughness, Agility, Intelligence, Perception, Willpower, Fellowship, and Influence.

### Maintainer Feedback

The pull request passed all automated GitHub checks and had no merge conflicts.

The PR was accepted and merged by the Roll20 maintainers.

### Status

Merged

### Reflection

This phase helped me complete the full open source contribution cycle. I researched an issue, implemented a fix, validated the changes, opened a pull request, documented my work, and had the contribution accepted by the upstream project.

One important lesson I learned is that open source contribution is not only about writing code. It also requires clear documentation, careful review, professional communication, and patience during the pull request process.



# Contribution 2: tldr-pages

## Phase I: Issue Selection

### Status

Phase I Complete

### Issue

Repository: tldr-pages/tldr

Issue: #23030

Issue Title: Page modification request: grafana-cli

Issue Link: https://github.com/tldr-pages/tldr/issues/23030

### Problem Summary

This issue is about improving the existing `grafana-cli` page in the tldr-pages project.

The current `grafana-cli` page only documents plugin-related commands, even though `grafana-cli` supports broader administrative functionality. Because the page title is `grafana-cli`, users may expect examples for more than only plugin management.

The issue suggests two possible directions: expanding the page with additional common `grafana-cli` commands, or making the current page more clearly focused on plugin management and possibly separating admin-related commands into another page.

### Why I Chose This Issue

I chose this issue because it is open, unassigned, and labeled `help wanted` and `page edit`. That makes it a good fit for a documentation-focused open source contribution.

I also chose it because it is from a different repository than my previous Roll20 contribution. My first contribution involved editing a character sheet, while this issue focuses on command-line documentation. This gives me practice contributing to a different type of open source project.

This issue is also a good fit because it is in English and does not require knowledge of another language. The scope is smaller than a complex code bug because the main task is improving a Markdown documentation page.

### Initial Understanding

My initial understanding is that I will need to locate the existing `grafana-cli` tldr page and compare it with the official Grafana CLI documentation.

The current page appears to focus mostly on plugin commands. I will investigate whether the best solution is to add more common examples from Grafana’s official CLI documentation or to clarify that the current page is specifically focused on plugin management.

I will need to follow the tldr-pages style, including short descriptions, concise examples, and correct command placeholder formatting.

### Expected Challenges

The main challenge will be choosing examples that are useful, accurate, and common enough for a tldr page.

Another challenge will be keeping the page concise. The tldr-pages project is not meant to copy full documentation, so I will need to select only practical examples that help users quickly understand the command.

I will also need to make sure any examples come from the official Grafana documentation and match the common platform listed in the issue.

### Next Steps

For Phase II, I will read the tldr-pages contribution guidelines, find the existing `grafana-cli` page, and review the official Grafana CLI documentation.

Then I will identify whether the page should be expanded with broader examples or narrowed to plugin management before making any changes.


ge should be expanded with broader examples or narrowed to plugin management before making any changes.
