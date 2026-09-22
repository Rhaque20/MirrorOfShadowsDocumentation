
Required gameplay effects

Guard Damage (To inflict guard damage)
Guard Freeze (To prevent regeneration when hit similar to poise)
Guard Break (When running out of guard stamina or getting block broken)
Guard Regen (For restoring parts of the guard gauge)

Inside GEC_AttackScale, call blocked hit with the damage dealt there as an event.

Guard logic located within Player Block:

Upon receiving the event in block, inflict with guard damage, and guard freeze.
Inside player attribute set, call guard break if the guard damage exceeds remaining guard gauge.

Recovery

When receiving blocked damage, blocked damage becomes recovery gained (GE_RecoveryGain). Whenever recovery is gained, briefly pause the drain of recovery. Recovery can accumulate as long as damage taken is blocked. As soon as unblocked damage is received, clear the current recovery.

12% of damage dealt will be used to convert a portion of recovery into health.
Have the health restored be proportional to the negative recovery gain.
