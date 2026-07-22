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

### Repository and Issue

Repository: Roll20/roll20-character-sheets

Issue: #14138

Issue link: https://github.com/Roll20/roll20-character-sheets/issues/14138

### Environment Setup

I cloned my fork of the Roll20 character sheets repository and created a working branch for issue #14138.

Commands used:

```bash
git clone https://github.com/Frehmand/roll20-character-sheets.git
cd roll20-character-sheets
git checkout -b fix-issue-14138
git push origin fix-issue-14138
```

My setup approach was:

* Fork the upstream Roll20 character sheets repository
* Clone my fork locally
* Create a focused working branch named for the issue
* Work only inside the Dark Heresy 2nd Edition sheet folder
* Inspect the existing roll formulas and roll template behavior
* Validate changes through Git diff review and GitHub checks

### Environment Setup Challenges

One challenge was that the Roll20 character sheets repository contains many different game systems. I needed to avoid editing unrelated sheets. I resolved this by working only in:

```text
Dark_Heresy_2ed/DarkHeresy2ed.html
```

Another challenge was that Roll20 character sheets cannot be fully tested by opening the HTML file in a normal browser. Roll20 uses custom roll syntax, inline roll references, and roll templates that depend on the Roll20 sheet engine. I handled this by carefully reviewing the formula changes, running Git checks, and documenting that live custom-sheet testing was not available with my Roll20 account access.

### Reproduction Steps

I reproduced the issue by inspecting the Dark Heresy 2nd Edition sheet logic.

Steps another person can follow:

1. Open the Roll20 character sheets repository:

```text
https://github.com/Roll20/roll20-character-sheets
```

2. Open the Dark Heresy 2nd Edition sheet file:

```text
Dark_Heresy_2ed/DarkHeresy2ed.html
```

3. Search for the Strength roll button:

```text
name="roll_S"
```

4. Review the original Degrees of Success calculation in the roll button.

5. Search for the Unnatural Strength attribute:

```text
UnS
```

6. Notice that the original successful Strength roll did not add half of Unnatural Strength, rounded up, to the displayed Degrees of Success.

7. Repeat the same review for the other characteristic roll buttons:

```text
roll_WS
roll_BS
roll_T
roll_Ag
roll_Int
roll_Per
roll_WP
roll_Fel
roll_Inf
```

8. Review the roll template success and failure sections to confirm that successful and failed results are displayed separately.

### Expected Behavior

When a characteristic test succeeds, the displayed Degrees of Success should include the matching Unnatural Characteristic bonus.

The expected calculation is:

```text
normal degrees + ceil(Unnatural Characteristic / 2)
```

For example, a successful Strength test should include:

```text
normal degrees + ceil(UnS / 2)
```

### Actual Behavior

The original sheet displayed only the normal Degrees of Success.

It did not add the matching Unnatural Characteristic bonus to successful characteristic tests. This meant that characters with Unnatural Strength, Unnatural Toughness, or other Unnatural Characteristics did not receive the correct displayed Degrees of Success.

### Specific File Involved

The main file involved was:

```text
Dark_Heresy_2ed/DarkHeresy2ed.html
```

The relevant parts of the file were:

* Characteristic roll buttons
* The `dh2ed` roll template success display
* The `dh2ed` roll template failure display

No unrelated files were needed for the fix.

### Root Cause

The root cause was that the original roll buttons only passed the normal `degrees` value to the roll template.

That original `degrees` value was still needed because it controlled the success and failure logic. However, it did not include the extra Degrees of Success from Unnatural Characteristics. Since there was no separate adjusted success value, the sheet could not display the correct successful Degrees of Success for characters with Unnatural Characteristics.

### UMPIRE Plan

#### Understand

I read issue #14138 and identified the reported bug: Unnatural Characteristic bonuses were not being added to successful Degrees of Success.

I inspected `Dark_Heresy_2ed/DarkHeresy2ed.html` and focused on the characteristic roll buttons and the `dh2ed` roll template.

#### Match

I matched the issue to the characteristic roll button pattern in the file. Each characteristic roll button calculated normal degrees and passed values into the roll template.

A representative example was:

```text
name="roll_S"
```

I also matched the display behavior to the roll template. The template already separated successful and failed results, which meant I could add adjusted success display without changing failure display.

#### Plan

My plan was to keep the original `degrees` value unchanged and introduce a separate `successdegrees` value for successful tests.

The original `degrees` value would still be used for success/failure logic. The new `successdegrees` value would only affect what is displayed when the roll succeeds.

The planned formula was:

```text
successdegrees = normal degrees + ceil(Unnatural Characteristic / 2)
```

I planned to update all ten direct characteristic roll buttons:

* Weapon Skill → `UnWS`
* Ballistic Skill → `UnBS`
* Strength → `UnS`
* Toughness → `UnT`
* Agility → `UnAg`
* Intelligence → `UnInt`
* Perception → `UnPer`
* Willpower → `UnWP`
* Fellowship → `UnFel`
* Influence → `UnInf`

#### Implement

The implementation would update the roll buttons so each one passed a `successdegrees` value into the roll template.

The roll template would then display `successdegrees` on successful tests when it exists, while preserving the original `degrees` value for failed tests.

#### Review

Before committing, I planned to review:

```bash
git diff -- DarkHeresy2ed.html
```

I would confirm:

* Only the intended file changed
* Each characteristic used the correct Unnatural Characteristic attribute
* The roll template success display used `successdegrees`
* The failure display logic was not changed
* No duplicate `</button>` tags were accidentally added
* Each edited button remained on a complete HTML line

#### Evaluate

I planned to validate the change by running:

```bash
git diff --check
```

I also planned to review the branch diff on GitHub and confirm that automated checks passed after opening the pull request.

### Edge Cases Considered

I considered these edge cases:

* Failed tests should not use `successdegrees`.
* Characters without an Unnatural Characteristic should not receive an incorrect extra bonus.
* Each characteristic must reference the correct Unnatural attribute.
* Strength should use `UnS`, not a generic value.
* Weapon Skill and Ballistic Skill should use `UnWS` and `UnBS`.
* Influence should use `UnInf`.
* The roll template should still display normal degrees when `successdegrees` is not provided.
* The fix should not change unrelated skill rolls or non-characteristic rolls.

### Final Phase II Plan

The final Phase III plan was:

1. Edit `Dark_Heresy_2ed/DarkHeresy2ed.html`.
2. Add `successdegrees` to the direct characteristic roll buttons.
3. Update the roll template success display to prefer `successdegrees` when available.
4. Preserve the original failure display logic.
5. Review the full diff.
6. Run `git diff --check`.
7. Commit the changes in small, meaningful commits.
8. Push the branch to my fork.
9. Open a pull request to the upstream Roll20 repository.

---

## Phase III: Build

### Current Status

**COMPLETED**

### Implementation Summary

I implemented the fix for Roll20 issue #14138 in the Dark Heresy 2nd Edition character sheet.

Modified file:

```text
Dark_Heresy_2ed/DarkHeresy2ed.html
```

Working branch:

```text
fix-issue-14138
```

Pull request:

```text
https://github.com/Roll20/roll20-character-sheets/pull/15219
```

Merged commit:

```text
https://github.com/Roll20/roll20-character-sheets/commit/ca3689d0a7f2a66e25a7967d39378b7c3575ce9f
```

### Key Commits

```text
5d68cfe1a8 — Add Unnatural Strength to successful test degrees
```

```text
0cc74a0055 — Add Unnatural bonuses to characteristic test degrees
```

### What Changed

The change adds the appropriate Unnatural Characteristic bonus to the displayed Degrees of Success for successful characteristic tests.

The calculation used is:

```text
normal degrees + ceil(Unnatural Characteristic / 2)
```

The implementation added a separate `successdegrees` value instead of replacing the original `degrees` value.

This was important because the original `degrees` value still controls success and failure behavior. The new `successdegrees` value is used only for displaying successful results.

### Characteristics Covered

The fix covers all ten characteristics:

* Weapon Skill → `UnWS`
* Ballistic Skill → `UnBS`
* Strength → `UnS`
* Toughness → `UnT`
* Agility → `UnAg`
* Intelligence → `UnInt`
* Perception → `UnPer`
* Willpower → `UnWP`
* Fellowship → `UnFel`
* Influence → `UnInf`

### Diff Scope

The final implementation was scoped to one file:

```text
Dark_Heresy_2ed/DarkHeresy2ed.html
```

The diff included:

* Updating characteristic roll buttons
* Adding `successdegrees` values for successful tests
* Updating the roll template success display
* Preserving failure behavior
* Avoiding unrelated formatting changes
* Avoiding changes to other character sheets

No unrelated files, debug code, or commented-out code were included.

### Testing and Validation

Because this change was in a Roll20 character sheet, normal browser testing was not enough. Roll20 roll buttons and roll templates depend on the Roll20 sheet engine.

I did not add a new automated unit test because this repository’s character sheet contribution model does not provide a normal unit-test file for this sheet’s roll-template behavior. The change was validated through static review, Git checks, manual formula review, and GitHub automated checks.

I ran:

```bash
git diff --check
```

This passed with no reported whitespace errors.

I also manually reviewed the diff and confirmed:

* Only `Dark_Heresy_2ed/DarkHeresy2ed.html` was modified.
* Each characteristic used the correct Unnatural Characteristic attribute.
* Strength used `UnS`.
* Weapon Skill used `UnWS`.
* Ballistic Skill used `UnBS`.
* Toughness used `UnT`.
* Agility used `UnAg`.
* Intelligence used `UnInt`.
* Perception used `UnPer`.
* Willpower used `UnWP`.
* Fellowship used `UnFel`.
* Influence used `UnInf`.
* Failed-test display logic remained unchanged.
* Edited button lines remained complete and were not broken by accidental line wrapping.
* Duplicate auto-inserted `</button>` tags were removed when necessary.

### Manual Test Cases Reviewed

I manually reviewed these logical test cases:

1. Successful Strength test with Unnatural Strength:

```text
Expected: displayed success degrees include normal degrees + ceil(UnS / 2)
```

2. Successful Toughness test with Unnatural Toughness:

```text
Expected: displayed success degrees include normal degrees + ceil(UnT / 2)
```

3. Successful Weapon Skill test with Unnatural Weapon Skill:

```text
Expected: displayed success degrees include normal degrees + ceil(UnWS / 2)
```

4. Successful Ballistic Skill test with Unnatural Ballistic Skill:

```text
Expected: displayed success degrees include normal degrees + ceil(UnBS / 2)
```

5. Failed characteristic test:

```text
Expected: failure display still uses the original degrees value
```

6. Characteristic without a meaningful Unnatural bonus:

```text
Expected: no unrelated characteristic bonus is applied
```

### Existing Test Suite / Project Checks

I did not run a full local Roll20 runtime test because custom character sheet testing was not available with my Roll20 account access.

I did rely on:

* `git diff --check`
* Full manual diff review
* GitHub pull request checks
* Maintainer review
* Final upstream merge

The pull request passed repository checks and was accepted/merged by the Roll20 maintainers.

### Challenges Faced

The character sheet uses Roll20-specific roll syntax and cannot be fully tested by opening the HTML file in a normal browser.

The main challenge was preserving the original success-and-failure calculation while adding the appropriate Unnatural Characteristic bonus only to the displayed Degrees of Success for successful tests.

I handled this by keeping the original `degrees` value and introducing a separate `successdegrees` value for the adjusted successful result.

Another challenge was editing long HTML button lines safely. VS Code sometimes tried to auto-close button tags, so I checked each edited section and removed duplicate closing tags when needed.

### Engineering Judgment

I intentionally kept the solution scoped to:

```text
Dark_Heresy_2ed/DarkHeresy2ed.html
```

I also chose to preserve the original `degrees` value because it was already used by the template to determine success and failure. Adding `successdegrees` was safer than rewriting the existing success/failure calculation.

I first implemented the Strength-specific change as a smaller increment, then expanded the same pattern to all remaining characteristics after reviewing the result. This reduced the risk of making a large unreviewed change all at once.

### Current Status

**COMPLETED**

### Pull Request

https://github.com/Roll20/roll20-character-sheets/pull/15219

Final status:

```text
Merged
```


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

### Repository

Repository Name: tldr-pages/tldr

Repository Link: https://github.com/tldr-pages/tldr

My Fork: https://github.com/Frehmand/tldr

### Issue Selected

Issue Number: #23030

Issue Title: Page modification request: grafana-cli

Issue Link: https://github.com/tldr-pages/tldr/issues/23030

### Issue Status at Selection

At the time I selected this issue, it was:

* Open
* Unassigned
* Labeled `help wanted`
* Labeled `page edit`
* Not labeled `wontfix`, `blocked`, or `needs discussion`
* Not connected to an existing open pull request from another contributor
* Part of an actively maintained repository with contribution instructions

### Problem Summary

The existing `grafana-cli` tldr page describes the command as a way to “Administer Grafana,” but the examples only show plugin-related commands. This matters because users who open the page may expect broader Grafana CLI examples, such as help, version, global options, or admin commands. I chose this issue because it is a focused documentation improvement in English and is a good fit for practicing a second open source contribution cycle.

### Why I Chose This Issue

I chose this issue because it matches my current skills and learning goals. The main technologies involved are Git, GitHub, Markdown, command-line documentation, and the open source pull request workflow.

This issue is smaller than a complex code bug because it focuses on improving one documentation page. At the same time, it still requires real investigation because I need to compare the existing tldr page with the official Grafana CLI documentation and follow the tldr-pages style guide.

My learning goal is to practice contributing to a new open source project that has a different workflow from my previous Roll20 contribution. This issue gives me practice reading contribution guidelines, editing Markdown documentation, signing a Contributor License Agreement, passing CI checks, and writing a clear pull request.

### Initial Understanding

My initial understanding is that the current `grafana-cli` page is too narrow for its title and description. The page says it is for administering Grafana, but the examples only cover plugin management.

The likely solution is to edit the existing page and add a few broader, common examples while keeping the page short. Since tldr pages are meant to be quick references, the fix should not copy the full official Grafana documentation.

### Specific File Likely Involved

The main file likely involved is:

```text
pages/common/grafana-cli.md
```

This file is in the `common` directory because the issue lists the platform as Common.

### Related Documentation and Maintainer Context

Official Grafana CLI documentation:

```text
https://grafana.com/docs/grafana/latest/administration/cli/
```

The issue also includes a maintainer comment referencing a broader Grafana issue tracker item:

```text
Broader grafana issue tracker: #22349
```

This helped confirm that the problem is related to the broader scope of Grafana CLI documentation, not only one missing command.

### What “Fixed” Looks Like

I will consider this issue fixed when:

* `pages/common/grafana-cli.md` includes examples beyond only plugin commands.
* The page still keeps useful plugin examples.
* The page remains concise and follows tldr-pages style.
* The page keeps the official documentation link.
* The page stays in the correct `common` platform directory.
* The examples use correct tldr placeholder syntax like `{{value}}`.
* The pull request passes CI, Codespell, PR Labeler, and CLA checks.

### Expected Challenges

The main challenge will be choosing examples that are broad enough to solve the issue but still short enough for a tldr page.

Another challenge will be following the project’s documentation style. tldr pages should use short descriptions and practical examples, not long explanations.

A third challenge may be the CLA requirement, since the repository requires contributors to sign the Contributor License Agreement before the pull request can pass all checks.

### Communication

I commented on the issue to introduce myself and express interest in working on it. I explained that I planned to keep the change small by updating the existing `pages/common/grafana-cli.md` page with concise examples that better match the page description.

### Next Steps

For Phase II, I will:

1. Set up the repository locally.
2. Create a working branch.
3. Inspect `pages/common/grafana-cli.md`.
4. Compare the current page with the official Grafana CLI documentation.
5. Identify a small set of practical examples to add or keep.
6. Write a Phase II plan before making the final edit.

---

## Phase II: Research

### Status

Phase II Complete

### Repository

tldr-pages/tldr

### Issue

Issue #23030 — Page modification request: grafana-cli

Issue Link: https://github.com/tldr-pages/tldr/issues/23030

### Environment Setup

For this contribution, I set up the `tldr-pages/tldr` project locally by forking the upstream repository, cloning my fork, adding the upstream remote, and creating a working branch for the issue.

My fork:

```text
https://github.com/Frehmand/tldr
```

Working branch:

```text
grafana-cli-page-update
```

Local setup commands:

```bash
cd /c/Users/mfreh
git clone https://github.com/Frehmand/tldr.git
cd tldr
git remote add upstream https://github.com/tldr-pages/tldr.git
git checkout -b grafana-cli-page-update
```

I confirmed the remote setup with:

```bash
git remote -v
```

The setup approach I used was:

* Fork the upstream repository
* Clone my fork locally
* Add the original repository as `upstream`
* Create a feature branch for the issue
* Inspect the existing documentation page manually
* Review the project’s contribution expectations and PR checklist
* Validate the final change using Git checks and GitHub CI

### Environment Setup Challenges and Resolutions

One setup challenge was that the `tldr` repository is large because it contains many command documentation pages. Cloning and checking out all files took time. I resolved this by waiting for the clone to complete and confirming that the repository opened correctly before creating my branch.

Another challenge was the Contributor License Agreement check after opening the pull request. The CLA check initially did not pass because my local Git commit author email had a typo:

```text
lfrehmand@uscs.edu
```

I corrected my Git author information to use my verified GitHub email:

```bash
git config user.name "Lal Frehmand"
git config user.email "lfrehmand@csus.edu"
git commit --amend --reset-author --no-edit
git push --force-with-lease origin grafana-cli-page-update
```

After correcting the author email, signing the CLA, and rechecking the PR, all required checks passed.

### Reproduction Steps

I reproduced the documentation issue by comparing the existing `grafana-cli` tldr page with the official Grafana CLI documentation.

Steps another person can follow:

1. Open the upstream repository:

```text
https://github.com/tldr-pages/tldr
```

2. Navigate to the existing Grafana CLI page:

```text
pages/common/grafana-cli.md
```

3. Read the page description:

```text
Administer Grafana.
```

4. Review the examples currently listed on the page.

5. Notice that the existing examples focus only on plugin-related commands, such as installing, updating, removing, and listing plugins.

6. Open the official Grafana CLI documentation:

```text
https://grafana.com/docs/grafana/latest/administration/cli/
```

7. Compare the official documentation with the tldr page.

8. Observe that the official documentation includes broader Grafana CLI functionality, including help/version commands, global options, plugin directory options, and admin commands.

### Expected Behavior

The `grafana-cli` tldr page should provide concise examples that match the page description, “Administer Grafana.”

Because the page is titled `grafana-cli` and describes administering Grafana, users should see a small set of common examples that represent broader CLI usage, not only plugin management.

### Actual Behavior

The existing page only included plugin-related examples. It documented plugin installation, plugin update, plugin removal, and plugin listing, but it did not include broader examples such as displaying help, displaying the version, using a custom plugin directory, or using an admin command.

### Specific Files Involved

The main file involved in the issue is:

```text
pages/common/grafana-cli.md
```

This file is in the `common` platform directory because the issue lists the platform as Common.

No source code files or functions were involved because this issue is a documentation page edit, not a software bug.

### Root Cause

The root cause is that the existing `grafana-cli` tldr page is too narrow compared to its title and description.

The page description says “Administer Grafana,” but the examples only cover plugin management. This creates a mismatch between what the page suggests it covers and what examples it actually provides.

### UMPIRE Plan

#### Understand

I first read issue #23030 and identified the reported problem: the `grafana-cli` page only documented plugin commands, even though Grafana CLI supports broader administrative functionality.

I reviewed the existing tldr page and the official Grafana CLI documentation to understand the difference between the current page and the expected broader coverage.

#### Match

I matched the problem to the existing file:

```text
pages/common/grafana-cli.md
```

I also identified the existing formatting pattern used by tldr pages:

```md
- List all installed plugins:

`grafana cli plugins ls`
```

This pattern showed that each example should have:

* A short description
* A blank line
* A single command example in backticks

#### Plan

My plan was to keep the change small and focused.

I planned to modify only:

```text
pages/common/grafana-cli.md
```

The planned examples were:

* Display help
* Display the version
* Install one or more plugins
* Install a plugin using a custom plugin directory
* List installed plugins
* Reset the admin password

I chose these examples because they are practical, supported by official documentation, and make the page broader without turning it into a full manual.

#### Implement

For Phase III, I planned to replace some plugin-only examples with broader examples while preserving useful plugin-related examples.

I would avoid adding too many examples because tldr pages are meant to be concise.

#### Review

Before committing, I planned to review the exact diff with:

```bash
git diff -- pages/common/grafana-cli.md
```

I would confirm:

* Only one file changed
* The file remained in `pages/common`
* The page still linked to official Grafana documentation
* The Markdown formatting matched the existing tldr style
* The file ended with a newline

#### Evaluate

I planned to validate the change by running:

```bash
git diff --check
```

After opening the pull request, I would also confirm that GitHub checks passed, including CI, Codespell, PR Labeler, and CLA.

### Edge Cases Considered

I considered the following edge cases before editing:

* The page should not become too long because tldr pages are short command examples, not full documentation.
* The page should not remove all plugin examples because plugin management is still a valid and common `grafana-cli` use case.
* The admin password example should use a placeholder instead of a real password.
* The custom plugin directory example should use a placeholder path.
* The change should stay platform-neutral because the page is under the `common` directory.
* A separate admin page should not be created unless maintainers specifically request that approach.

### Investigative Depth

I inspected the current file directly with:

```bash
cat pages/common/grafana-cli.md
```

I confirmed that the original page contained only plugin-related examples. I then compared the page with the official Grafana CLI documentation to identify common examples that would broaden the page while keeping it concise.

I also checked the issue metadata before starting. The issue was open, labeled `help wanted` and `page edit`, had no assignee, and showed no linked branch or pull request at the time I started working.

### Final Phase II Plan

My final plan for Phase III was:

1. Edit `pages/common/grafana-cli.md`.
2. Replace some plugin-only examples with broader CLI examples.
3. Keep the page concise and platform-neutral.
4. Run `git diff --check`.
5. Review the full diff.
6. Commit the change.
7. Push the branch to my fork.
8. Open a pull request to `tldr-pages/tldr`.
9. Sign the CLA and confirm all required GitHub checks pass.

---

## Phase III: Build

### Status

Phase III Complete

### Implementation Summary

I implemented the documentation update for the `grafana-cli` tldr page.

Modified file:

```text
pages/common/grafana-cli.md
```

Branch:

```text
grafana-cli-page-update
```

Commit:

```text
f0723ed1b
```

Pull request:

```text
https://github.com/tldr-pages/tldr/pull/23202
```

### What Changed

The original page only showed plugin-related examples. I updated the page so it better matches the description “Administer Grafana.”

The updated page now includes examples for:

* Displaying help
* Displaying the version
* Installing one or more plugins
* Installing a plugin with a custom plugin directory
* Listing installed plugins
* Resetting the admin password

This keeps the page concise while better representing broader Grafana CLI usage.

### Implementation Evidence

The branch contains a meaningful Phase III commit:

```text
f0723ed1b — Update grafana-cli examples
```

The change was scoped to one file only:

```text
pages/common/grafana-cli.md
```

The commit changed only the documentation examples for `grafana-cli`. No unrelated files, formatting-only changes, debug code, or commented-out code were included.

### Diff Scope

The final diff changed only:

```text
pages/common/grafana-cli.md
```

The diff included:

* Removing some duplicate plugin-only coverage
* Adding broader Grafana CLI examples
* Keeping the official documentation link
* Keeping the page under the `common` platform directory

### Testing and Validation

Because this contribution edits a tldr documentation page, I did not add a new automated unit test. There is no application logic or function behavior to test. The appropriate validation for this issue was Markdown/style validation, Git checks, and repository CI.

I validated the change locally with:

```bash
git diff --check
```

This check passed with no output, which means Git did not detect whitespace errors.

I also manually reviewed the Markdown file and confirmed:

* Each example has a short description
* Each command example is wrapped in backticks
* Placeholders use the `{{placeholder}}` format
* The page still links to the official Grafana CLI documentation
* The file stayed in the correct `pages/common` directory
* The final file ends with a newline

After opening the pull request, the required GitHub checks passed:

* CI
* Codespell
* PR Labeler
* CLA

### Test Cases Checked Manually

I manually checked these documentation cases:

1. Help command example:

```text
grafana cli --help
```

This verifies that the page includes a basic discovery/help command.

2. Version command example:

```text
grafana cli --version
```

This verifies that the page includes a basic version-check command.

3. Plugin installation example:

```text
grafana cli plugins install {{plugin_id1 plugin_id2 ...}}
```

This preserves useful existing plugin functionality.

4. Custom plugin directory example:

```text
grafana cli --pluginsDir {{path/to/plugins_directory}} plugins install {{plugin_id}}
```

This adds a broader global-option example while still using a common plugin workflow.

5. Plugin listing example:

```text
grafana cli plugins ls
```

This keeps the existing useful list example.

6. Admin password reset example:

```text
grafana cli admin reset-admin-password {{new_password}}
```

This adds an admin-related example to better match the page description.

### Existing Test Suite / CI

I did not run a full local project test suite because this was a single Markdown documentation page edit. Instead, I used the project’s GitHub checks after opening the pull request.

The pull request checks passed successfully:

* CI passed
* Codespell passed
* PR Labeler passed
* CLA passed after I corrected my Git author email and signed the CLA

### Challenges Faced and Resolutions

One challenge was deciding how much to expand the page. The issue mentioned that the page could either be expanded with broader examples or scoped more specifically to plugin management. I chose a small expansion because the page title and description already suggested broader Grafana administration.

Another challenge was the CLA check. The CLA was pending because my commit author email had a typo. I fixed the Git configuration, amended the commit author, force-pushed safely with `--force-with-lease`, signed the CLA, and confirmed that all checks passed.

A third challenge was making sure the file ended with a newline. The first diff showed `No newline at end of file`, so I reopened the file, added a final newline, saved it, and rechecked the diff.

### Engineering Judgment

I intentionally kept the page short instead of copying many commands from the official Grafana documentation. This follows the purpose of tldr pages: quick, practical examples rather than complete manuals.

I also chose not to create a separate admin page during this first PR. The issue suggested that as one possible direction, but creating a new page would make the change larger. A smaller edit to the existing page is easier for maintainers to review. If maintainers ask for a separate page later, I can adjust the approach.

### Current Status

The pull request was opened:

```text
https://github.com/tldr-pages/tldr/pull/23202
```

All required GitHub checks passed, including CI, Codespell, PR Labeler, and CLA.

The PR was later closed by a maintainer without being merged. No specific code review comments or requested changes were provided on the PR page.

Current PR status:

```text
Closed without merge
```

---

## Phase IV: Submit & Iterate

### Status

Phase IV Complete

### Pull Request

PR Link: https://github.com/tldr-pages/tldr/pull/23202

PR Status: Closed without merge

### PR Summary

This pull request updated the `grafana-cli` tldr page so it better reflected broader Grafana CLI usage instead of only plugin-related commands.

The PR added examples for displaying help, displaying the version, installing plugins, using a custom plugin directory, listing installed plugins, and resetting the admin password.

### Issue Reference

Issue: #23030 — Page modification request: grafana-cli

Issue Link: https://github.com/tldr-pages/tldr/issues/23030

The PR description used:

```text
Closes #23030
```

### Acceptance Checklist

The project’s pull request checklist was completed in the PR description:

* The page was in the correct platform directory: `common`
* The page description kept its link to official documentation
* The page followed the content guidelines
* The page followed the style guide
* The PR modified fewer than 5 pages
* The PR was authored by me and human-reviewed
* The PR title followed the project’s recommended style

### Validation

Before opening the PR, I ran:

```bash
git diff --check
```

This completed with no output, meaning no whitespace errors were detected.

After opening the PR, the required GitHub checks passed:

* CI passed
* Codespell passed
* PR Labeler passed
* CLA passed after I corrected my Git author email and signed the Contributor License Agreement

### Maintainer Feedback Log

* PR opened: https://github.com/tldr-pages/tldr/pull/23202
* The CLA check initially remained pending because my commit author email had a typo.
* I corrected my Git author email, amended the commit, force-pushed safely with `--force-with-lease`, signed the CLA, and confirmed all checks passed.
* The PR was later closed by maintainer @Managor without being merged.
* Since no detailed review comment was provided, I documented the outcome honestly and followed up politely asking what approach would be preferred.
* No specific code review comments or requested changes were provided on the PR page.

### Current Outcome

The PR was submitted to the upstream repository, passed all required checks, and was reviewed/closed by a maintainer.

Final status:

```text
Closed without merge
```

### Learnings and Reflections

This contribution taught me that a PR can be technically valid and pass all checks but still not be merged. Open source maintainers may close a PR because they prefer a different product direction, want a smaller scope, or do not think the change matches the project’s current needs.

Technically, I learned how to update a tldr documentation page, follow a project-specific PR checklist, fix a CLA issue, amend a commit author, and safely force-push with `--force-with-lease`.

If I did this again, I would ask a clarifying question on the issue before implementing the change. Since the issue suggested two possible directions, I should have asked whether maintainers preferred expanding the existing `grafana-cli` page or creating/scoping a separate page before coding.

This was still a useful open source contribution cycle because I selected an issue, researched it, implemented a focused change, opened a real upstream PR, passed all required checks, handled a CLA problem, and documented the final maintainer outcome honestly.
