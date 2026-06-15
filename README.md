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
