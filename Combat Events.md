

All Tags have a prefix of `Event.Combat`.  This is used for enemy mechanics, equipment skill triggers, or sometimes even status effects.

The table below is what is suffixed. (eg. Hit.Dealt = `Event.Combat.Hit.Dealt`)

| Combat Event Tag Suffix | Trigger Description                                                                                                                   |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| Hit.Dealt               | Triggered on the **ATTACKER LANDING** a hit                                                                                           |
| Hit.Received            | Triggered on the **DEFENDER RECEIVING** a hit                                                                                         |
| Hit.Received.Graze      | Triggered when receiving a grazing hit.                                                                                               |
| Hit.Received.Blocked    | Triggered when receiving a hit and it got successfully blocked                                                                        |
| SkillActivated          | For player when they use an attack that costs SP. For enemies it's whenever they are not using their default attack.                  |
| Receive.Buff            | Triggered when the target has received a buff                                                                                         |
| Receive.Debuff          | Triggered when the target has received a debuff                                                                                       |
| Receive.StatusEffect    | Triggered when the target has been afflicted with a status effect. (This might branch down further to specifics or reuse status tags) |
| Grant.Buff              | Triggered when the caster grants a buff to a target                                                                                   |
| Grant.Debuff            | Triggered when the caster inflicts a debuff to a target                                                                               |
| Grant.StatusEffect      | Triggered when the caster inflicts a status effect to a target.                                                                       |
| Evade                   | Triggered when triggering either a perfect evade or a normal evade.                                                                   |
| Evade.Perfect           | Triggered when triggering a perfect evade.                                                                                            |
| Counter.Deflect         | Used for when triggering a deflect                                                                                                    |
| Counter.Parry           | Used for when triggering a parry                                                                                                      |
| Guard                   | Used for when starting a guard                                                                                                        |
| Battle.Begin            | Used when battle begins                                                                                                               |
| Battle.End              | Used when battle ends                                                                                                                 |
| Fury                    | Used when a boss enters the fury state                                                                                                |
| Exhausted               | Used when a boss enters the exhausted state                                                                                           |
