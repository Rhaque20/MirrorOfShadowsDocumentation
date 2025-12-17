Hit sequence data determines what happens on a particular hit sequence. Useful for multi hit attacks that need different properties to better match the animation or desire.

![[HitSequenceData.png]]

*Do not worry about Hit Scan Shape Data, that is not read yet by the abilities and has no bearing on anything yet*

The struct consists of the following.
1. **Hit Sequence Name:** Used for debugging on enemies and skill display UI when showing breakdown of the skill in the skills menu.
2. **Skill Modifier**: How much does it scale on ATK (Attack)
	1. This will in the future scale on other stats like HP, DEF or even SP Gain.
3. **Poise Damage:** How much poise damage will be dealt if the hit connects. Important for the stagger system.
4. **Knockback Power:** How far back (or forward) does the attack propel hit targets. Negative values will pull the hit targets towards the attack instead of away.
5. **Has Vertical Launch**: If marked true will provide another drop down for the launch power. Used for launchers and attacks that will send attack targets to the ground. (Also important for stagger system).
	1. This also influences if the attack will launch airborne targets(if true) or suspend them in the air (if false).
6. **Additional Hit Effect**: Unused, but will be an additional VFX that is overlayed on top of the generic hit effect when this hit sequence lands.
7. **Stun Damage Modifier:** Only relevant against enemies, determines how much of the skill's damage will go into their stun gauge.
8. **Hit Sound Event:** Determines which WWise sound event to call when the hit connects.
9. **Hit Box Location:** Used to determine where to locate the scene component in local space of the attack to place the hitbox
	1. (Forward vector placement is hard)
10. Hit Scan Shape Data: Don't worry about it.
11. **Hit Type:** Used for identifying hit responses (Parry, Dodge, Block)
	1. Can Trigger Parry: Determines if this attack will trigger a parry (and thus receive stun damage) or not.
		1. If false, will just trigger a deflect where the hit's damage is negated and nothing else.
	2. Will Interrupt On Parry: Only relevant if Can Trigger Parry is true. Will stop the entire attack if this hit sequence got parried.
	3. Hit Type: Comes in the following variations
		1. Normal Hit: Can be freely dodged, blocked, or deflected/parried
		2. UnDodgeable: This hit sequence will ignore the target's dodge status.
		3. UnParryable: This hit sequence will ignore the target's block AND parry status.
		4. Undefendable: This hit sequence will ignore any defensive status (block, dodge or deflect/parry) when this hit connects.
12. **Hit Limit:** By default set to -1. 
	1. -1 indicates no limit and is fine if this hit sequence is called through an animation notify **EVENT**. 
	2. However if the hit is persistent and thus called through an animation notify **STATE**, then a hit limit is in place to avoid overhitting and being dependent on frame rate for amount of hits to land.
13. **Attached to Socket:** Used in tandem with animation notify state, makes the offset of the hitbox from *Hit Box Location* be based on the socket's location instead of the scene component. Useful for attacking hit boxes to weapons or even body parts.