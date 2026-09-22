![[Pasted image 20260921224723.png]]

This is the menu where the player can analyze their skills and slotted skill chain effects.

The menu is divided into 4 major widgets.

1. WBP_SkillInfoSheet
	1. Handles displaying clicked skill icon on skill description, SP Cost, Element, and most importantly hit sequence data.
2. WBP_CharacterSelector
	1. Used to alter the display of a horizontal box of 3 [[WBP_SkillModDisplay]](s). To note is that each character selector has their own set of 3 [[WBP_SkillModDisplay]](s).
3. [[WBP_SkillModDisplay]]
	1. The entire thing is encased in a widget switcher with each index having three [[WBP_SkillModDisplay]](s).
	2. Along with it, has capability to receive click inputs to deal with slotting and unslotting skill slot effects from Owned Skill Chain Slots.
	3. It uses **WBP_EditableSkillChainSlot** as the basis for the slot.
4. WBP_OwnedSkillChainSlots
	1. Displays what skill chain slots the actively selected character owns. It composes of multiple **WBP_EditableSkillChainSlot**

## Step 1: Bind Character Select Change

![[Pasted image 20260922022126.png]]
Binding each character selection button press with it returning party member index. This determines what index to reveal for skill list as well as refreshes **WBP_OwnedSkillChainSlots** to display the owned skill chain slots of the actively selected character. It will also flush out any queuing selection from the prior active character.

## Step 2: Display and Update Active Character Skills
![[Pasted image 20260921231732.png]]
It would go through all the skill widgets it has gathered from the master widget and bind to when you the skill icon button is pressed.


## Step 3: Bind Skill Chain Slot Selection for Equipping
When pressed, would show call the populate skill info function to populate the **WBP_SkillInfoSheet** with the clicked skill.
![[Pasted image 20260921235706.png]]
Finally, on each **[[WBP_SkillModDisplay]]** on the master widget, bind when the skill slot button is selected attached to the **[[WBP_SkillModDisplay]]** as well as the skill chain slot buttons. This is what will enable skill slot equip and unequipping. This is through calling [[Function - Start Skill Slot Edit |StartSkillSlotEdit()]].

## Step 4: Set Up Edit Enable/Disable based on combat status
![[Pasted image 20260922000008.png]]
Using gameplay messanger router, can use this to enable and disable editing of skill chain slots to avoid changing it mid-combat which can alter skill costs as well as capabilities.
## Step 5: Set Up ToolTip
![[Pasted image 20260922002003.png]]

This sets up the tooltip which is another widget for the skill chain icons. If the **WBP_EditableSkillChainSlot** that is hovered over has a valid skill chain slot info, it will display it as a tooltip.

![[Pasted image 20260922002241.png]]