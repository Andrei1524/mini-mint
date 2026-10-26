# Notes

## Summary

<!-- What you did, in a few sentences. What you'd like us to look at first. -->

I started fixing all the bugs, I wrote down all my thoughts to help me processs information faster and have a good mental model before using AI.
Then I went into adding the features one by one.
I did everything in PRs so I would start first with the bugs and then the features.

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
I saw that possible outputs are only the ones before the selected _step_, so having that in mind along design system, I went along to claude chat and told I have this cantnip-list example comp, I want a getter to compute possible outputs from a selected step and now I have a generated code to use on computed and a vue template - lets see where this gets me now. Fixed and adjusted the generated code, and tested to see if grouping and code is taking in considerations the connections as well.
)
<!-- Which outputs you offer, and why. Edge cases you thought about. -->

So my logic building the outputs is to see whats the selected step, run the possible scenarions to that step trough the conexions and use that building an array of groups

### B. Safe delete

(plan of attack:just read the feature and now I am thinking to use a new component, stepDeleteConfirmation or an universal reusable one but I would go to just have something
to work and then figure it out later, having one hour left the most straighforward is to see list of components I can use, use the ui.store to hold modal data and state.
I have the modals ready and toggling, now I need to add a new func to check removal logic from outputs dependants. used claude to generate the function checker for output dependants.
trying to understand the reasioning of matching step dependents of a selected step.
)
<!-- How you find references, and why you did it that way. -->

Im finding the references using the help of parseReferences by flattening the properties of the selected step and only keep the ones that matches the step being deleted

## Decisions and assumptions

<!-- Anything that was unclear and how you decided. -->

I had to read the getStepDependents func multiple times to properly understand it, sometimes claude didnt know catnip documentation, I had to look myself at component events and other stuff.

## If I had more time

I would try to come with a better modal system, actually fix the openFlow action by cancelling unused calls and move the fetch to a services layer or a better structure to organize fetches.
I have a failing test that I need to solve.
Humanize the references insertion in steps.
Add loaders on deletion, disable the button as well while loading.
On confirmation delete modal I would improve readability of the dependents outputs.
Add a discard btn (icon) on the selected step editor.
Maybe implement typescript.
Add a linter.
Extend test suite.

## AI usage

<!--
Which tools you used and for what.
A few examples of prompts or tasks you gave them.
What you kept, what you changed, and what you threw away (and why).
-->

I only used claude web chat using only batches of code and relevant contexnt to generate exactly the code I need to go further.

Examples of some claude chats:

-- begin --

me: I have this obj

export const STEP_TYPES = {
Form: {
label: "Form",
icon: "form-step",
defaults: () => ({
title: "Tell us about yourself",
fields: [
{ key: "email", label: "Email" },
{ key: "phone", label: "Phone" }
]
}),
outputs: (step) => (step.properties?.fields ?? []).map((f) => ({ key: f.key, label: f.label || f.key }))
},
IdVerification: {
label: "ID verification",
icon: "id-card",
defaults: () => ({ provider: "BankID", country: "NO" }),
outputs: () => [
{ key: "fullName", label: "Full name" },
{ key: "dateOfBirth", label: "Date of birth" },
{ key: "nationalId", label: "National ID" },
{ key: "country", label: "Country" }
]
},
me: and I have this feature

- The **Insert reference** link in the step sidebar already opens `src/components/ReferencePicker.vue`. It is currently a stub.
- The picker lists the outputs the selected step can use, grouped by step, with readable names.
- Only list outputs that **will actually be available when the selected step runs**.
- Picking an output inserts the reference into the field.
- _Optional:_ references are hard to read as UUIDs. Improve how they are shown to the user if you have time.
  and I want to use this computed to store the possible outputs

const references = computed(() => {

})
me: and this is my example comp, using catnip design system

<catnip-list role="menu" size="m">
        <catnip-list-item v-for="type in ADDABLE_STEP_TYPES" :key="type" @click="add(type)">
          <catnip-icon slot="left" .name="getStepIcon(type)" size="20"></catnip-icon>
          {{ getStepTypeLabel(type) }}
        </catnip-list-item>
      </catnip-list>

From this chat I kept all the code

some time later I asked this to quickly grasp the logic behind mini-mint: on the references code earlier how you actually know when to use certain outputs in some steps?
And used the chat response to make a better idea on how things should work.
-- end --

-- begin --
me: having this new feature

### Feature B: Safe delete

> _Right now, deleting a step that other steps depend on silently breaks the flow. The customer only finds out when it fails in production._

- When a step's outputs are used by other steps, deleting it asks for confirmation first and shows which steps use it.
- This must work for every kind of step property.
  and this func below that needs to check for the outputs now

/** True when the step can be removed without first emptying its branches. */
export function canRemoveStep(flow, stepId) {
const step = getStep(flow, stepId);
if (!step) return false;
return getBranchOutcomes(step).every((outcome) => !getTarget(flow, stepId, outcome));
}
me: this is the entire graph.js file (so claude have a context)

then he generated me the **getStepDependents** function then I went ahead and used that to actually test and build my blocks of the modal
kept this function as it is and added other building blocks
-- end --

I also asked claude multiple times to show me quick examples in text on how the data is flowing, how references are found in getStepDependents and to show me the flow
of stuff to quickly grasp it in these 4 hours.
