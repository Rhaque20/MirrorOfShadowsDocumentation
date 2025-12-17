For both player and enemies, they use a base class of `GA_GenericAttackAbility` which handles the logic required for an attack ability to function (hit filtration, responding to animation events, consuming resources, reading data to do required logic, etc.).

Whenever an attack is called (combo attacks from player, enemy attack, using skills), it will be called in one of two ways, either by directly by the ability class or through `Ability.Input.Attack` gameplay event. This is determined by the skill data asset as it has a field for ability class pointer. If that is empty, then it will activate the ability through the event. Otherwise it will call it through that pointer. 

![[EmptyAbilityClass.png]]

![[CustomAbilityClass.png]]
*(There is a system for enemy and player that will read through their skills and grant those abilities through their pointer to ensure that the ability can be called)*

### Skill Data Assets
You can find these data assets in **`Content/MyContent/DataAssets/Skills`** which will diverge between player specific skills or enemy skills. They both take from the same parent class `SkillData.h` which inherit a lot of the same fields. The reason for their divergence is due to the nature of skill limiting.

* Player Skills use SP system and require manual charge up to reuse skills.
* Enemy Skills use traditional cooldown (and sub cooldowns) and will require time to be reused.
	* Along with it also provide action intervals after each skill which determines an overall cooldown until they can use any skill again.

These generic attack abilities and what they inherit will rely on the data written in these data assets to function with stuff such as

* Damage modifier
* Stun damage modifier
* Poise damage dealt
* Their respective cooldowns for the gameplay effect (or how much SP to take away from the skill)
* Animations to use for these attacks
* Elemental Damage affinity
* Attack category (for stat purposes like boosting Combo ATK damage by 5% for example)
* Propulsion curves (instead of root motion, it's set velocity based on float curves)
* Resistance interruption mods
	* From scale of 1 to 0 with **0 meaning no poise damage is taken** and **1 meaning full poise damage is received** while using this attack. In between is how much poise damage received is reduced.
*  Skill Icons
* [[Hit Sequence Data]]

There are a few other things, but they are either not fully implemented or not relevant for this page.