## Step 1: Evaluate if slotting is ready
![[Pasted image 20260922004603.png]]
This function gets called when a skill slot attached to a [[WBP_SkillModDisplay]] gets clicked on. It checks to see if a desired skill slot has been already selected and either waits or enacts.

## Step 2a: Remove Active skill chain Slot

![[Pasted image 20260922021009.png]]

If the [[WBP_SkillModDisplay]] has an active chain slot equipped when the **WBP_SkillEditableSkillChainSlot** is pressed, then it will remove it both from display and on the skill itself based on the chain name.

## Step 2b: Slot in new skill chain effect into skill
![[Pasted image 20260922022245.png]]

If the desired skill chain slot has already been selected, then it will slot in the effect from the character's list of skill chain slots. Internally, if the user tries to slot in a different effect into an already occupied slot, it will eject it out and store it inside the list of skill chain slots. The UI will re-read the list after each change so we don't need to do any immediate edit to the UI.

## Step 3: Update UI with new changes
![[Pasted image 20260922022624.png]]

Regardless of whether we remove or slot in, we will update the display on the UI to show the new equipped effect or unequipped effect, then update the **WBP_OwnedSkillChainSlots** to show what skill chain slots remain and also reflect this on the combat UI.