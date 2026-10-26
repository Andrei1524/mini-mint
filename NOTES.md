# Notes

## Summary

<!-- What you did, in a few sentences. What you'd like us to look at first. -->

## Bug fixes

### #1 Deleting the first step of a branch

- Root cause: on deletion, the issue was that the outcome field from ongoing was copying the value to the incoming
- Fix: be sure to add the outcome from incoming to the connexions
- How I verified it: I had to test the whole flow a few times with logs added, I read a bit before the homeworking but this is the first time grasping the flows until this bug, but I find the delete function first in the store, then I saw we use a separate logic file that handles the deletion. I had to debug there with console logs to see whats the issue.

### #2 Switching flows quickly

(plan of attack: #1 identify the flow of list changing, check how slected flows are stored in. possible issues can be that the location of selected flow is not clearing up on click, check for endpoint calls - I would to this before debugging with AI. #2 - found out that the issue its in the editor store - openFlow, there are 2 calls being made at the same time and the quickest wins, I have mutiple ways of fixing this but this what cames in my mind: 1.when a http call is not completed I would cancel that and let the latest http call complete, 2. add an active loading flow and check if we already have one and on switching try to cancel the first one. I will check with AI to be more faster. Ok I pasted the code of the openFlow function, AI did implement a quick guard, its working but what I would to is to further optimize the actuall http call because even I only save correct data, 2 calls are still being made - leave for final or for interview if no time)

- Root cause: this was a racing condition, it appared the quickest call won and thats why the latest click wasnt being saved with right data.
- Fix: test the list by having vuedevstools open, seeing that editor is saving wrong data on multiple cliks, paste the openFlow func to claude and see whats the fix and check it afterwards
- How I verified it: clicking the list, checking with vuedevtools on whats being saved, identify the issue, check with AI the generated code, test if it works.

### #3 Unsaved changes / Discard

(plan of attack: check where is label if its a component or not, check how changes are detected. isDirty getter should detect unsaved changes. Ok, I tried first to see why its not detecting changes, checked with AI the exact getter code, AI pointed that maybe when saving flow and original are pointing to the same obj from the fetch call and it was right, thats the issue, also check for more placing and use structuredClone to actually have a copy of the obj)

- Root cause: when instantiating flow and original in editor store, there was no actually cloning of the flow obj
- Fix: used structuredClone but any other cloning tehniques can be used , such the clone from lodash
- How I verified it: first check the code flow, see what boolean is responsible for showing label and enabling the "discard" btn, check the getter code, research with AI, AI pointed
  that it maybe be a reactivity issue and that was right.

### Other things I noticed

<!-- Problems you found that weren't reported. Which ones you fixed, and which ones you left (and why). -->

## Features

### A. Reference picker

(plan of attack: I would read the feature, understand it, try to check if to use catnip-list component, read the outputs and try to come later with a logic implement.
I saw IdVerification from the editor flow steps is the same as the STEP_TYPEs obj in stepTypes.js so when building the feature get the output from that list of obj by key.
I saw that possible outputs are only the ones before the selected _step_, so having that in mind along signicat design system, I went along to claude chat and told I have this cantnip-list example comp, I want a getter to compute possible outputs from a selected step and now I have a generated code to use on computed and a vue template - lets see where this gets me now. Fixed and adjusted the generated code, and tested to see if grouping and code is taking in considerations the connections as well.
)
<!-- Which outputs you offer, and why. Edge cases you thought about. -->

So my logic building the outputs is to see whats the selected step, run the possible scenarions to that step trough the conexions and use that building an array of groups

### B. Safe delete

(plan of attack:just read the feature and now I am thinking to use a new component, stepDeleteConfirmation or an universal reusable one but I would go to just have something
to work and then figure it out later, having one hour left the most straighforward is to see signicat list of components I can use, use the ui.store to hold modal data and state.)
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
