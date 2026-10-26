# Notes

## Summary

<!-- What you did, in a few sentences. What you'd like us to look at first. -->

## Bug fixes

### #1 Deleting the first step of a branch

- Root cause: on deletion, the issue was that the outcome field from ongoing was copying the value to the incoming
- Fix: be sure to add the outcome from incoming to the connexions
- How I verified it: I had to test the whole flow a few times with logs added, I read a bit before the homeworking but this is the first time grasping the flows until this bug, but I find the delete function first in the store, then I saw we use a separate logic file that handles the deletion. I had to debug there with console logs to see whats the issue.

### #2 Switching flows quickly

- Root cause:
- Fix:
- How I verified it:

### #3 Unsaved changes / Discard

- Root cause:
- Fix:
- How I verified it:

### Other things I noticed

<!-- Problems you found that weren't reported. Which ones you fixed, and which ones you left (and why). -->

## Features

### A. Reference picker

<!-- Which outputs you offer, and why. Edge cases you thought about. -->

### B. Safe delete

<!-- How you find references, and why you did it that way. -->

## Decisions and assumptions

<!-- Anything that was unclear and how you decided. -->

## If I had more time

## AI usage

<!--
Which tools you used and for what.
A few examples of prompts or tasks you gave them.
What you kept, what you changed, and what you threw away (and why).
-->
