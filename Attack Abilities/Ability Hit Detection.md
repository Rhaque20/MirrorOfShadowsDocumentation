![[AnimationEventCall.png]]
*Figure 1: Animation Notify Event being called at a certain animation frame with adjustable parameters for the notify event*

When the animation montage is played, it will go through each frame of the animation and call any events that are placed on those frames. For this article, we'll discuss the hit event. The important thing to note is the hit sequence value as it will be passed into the payload as a float only to get reconverted to an int.

![[HitSequenceEventCall.png]]
*Figure 2: Gameplay event sent via animation notify event using the Hit Sequence value*

![[HitEventFunction.png]]
*Figure 3: Primary logic to follow when hit events are received from animations*

When the hit event is received, it will use the magnitude value from the event payload to determine the sequence. It will take the skill's [[Hit Sequence Data]] and store it as a value to be used for the rest of the logic.

If you remember from the Hit Sequence Data, there is a hit limit field for each sequence. This is where it matters, as when a hit event is called, it checks for the sequence. If the sequence is the **DIFFERENT** as the previously stored one (via index), the hit limit is reset for the current sequence. The TMap of *Struck Targets* is used to enforce the hit limit per target for the hit sequence and will be emptied out when a new hit sequence is received. This gets used in the function *Hit Zone Scan* where it handles both hit scanning and filteration.