This was refactored thanks to Sreerag to be more deterministic in save order and less reliant on default function call order to ensure smooth save order.

Save files are based on a USaveGame object with the class being `USavedGame`.

It contains the following:
```cpp
UPROPERTY(BlueprintReadWrite, SaveGame)  
FInventoryToSave Inventory;  
  
UPROPERTY(BlueprintReadWrite, SaveGame)  
FDungeonToSave DungeonManager;  
  
UPROPERTY(BlueprintReadWrite, SaveGame)  
TArray<FEquipmentToSave> EquipmentItems;  
  
UPROPERTY(BlueprintReadWrite, SaveGame)  
FVector CharacterPosition;  
  
UPROPERTY(BlueprintReadWrite, SaveGame)  
TMap<FName,FSkillTreeSaveData> SkillTreeData;
```

You can access the struct definitions at this page: [[Save Struct Definitions]]

Actors or Components that need to have their data saved and loaded will implement the `ISavableInterface` found in `Interfaces/ISavableInterface.h`.

Specific definition of the interface found here: [[ISavableInterface]], but the primary thing to note is that each implemented actor or component is given a **phase** which determines the order in which things get saved, but most importantly, the order which things get loaded.

## Step 1: Load information when Game Instance is initialized then read it when world is loaded

When custom game instance class `UMyGameInstance` is loaded (Details found in `CustomGameInstance.h`), it will try to load the save slot (input string is irrelevant) using the `USaveSystem` found in file `System/SaveSystem.h`, but **NOT** read it.

It will then bind to when the world is actually ready (loaded into memory) to actually read the save file and start loading in the data to their respective UObjects

```cpp
void UMyGameInstance::Init()  
{  
     Super::Init();  
    SaveSystem->LoadGameFromSlots("SAVEFILE");  
    FCoreUObjectDelegates::PostLoadMapWithWorld.AddUObject(this,&UMyGameInstance::HandleWorldReady);  
}
```

```cpp
void UMyGameInstance::HandleWorldReady(UWorld* LoadedWorld)  
{  
    if (SaveSystem->LoadedSave)  
    {        
	    SaveSystem->LoadGame();  
    }
}
```

## Step 2: Read the save file and load the necessary data into their respective objects

```cpp
void USaveSystem::LoadGame()  
{  
    if (!LoadedFile) return;  
  
    // Sort subsystems same as save  
    Subsystems.Sort([](auto A, auto B)  
        {            return A->GetSavePhase() < B->GetSavePhase();  
        });  
    int32 MaxPhase = 3;  
  
    for (int32 Phase = 0; Phase <= MaxPhase; Phase++)  
    {        for (auto& Sys : Subsystems)  
            if (Sys->GetSavePhase() == Phase)  
                Sys->PreLoad();  
  
        for (auto& Sys : Subsystems)  
            if (Sys->GetSavePhase() == Phase)  
                Sys->LoadData(*LoadedFile);  
  
        for (auto& Sys : Subsystems)  
            if (Sys->GetSavePhase() == Phase)  
                Sys->PostLoad();  
    }}
```

Subsystems is TArray of all actors and components that implement `ISavableInterface`. It will go through each of them and load their respective data based on the current phase.

Subsystems get added by each implemented actor/component with a line of code that goes like this:
```cpp
if (GameInstance && GameInstance->SaveSystem)  
{  
    TScriptInterface<ISavableInterface> Interface(this);  
    GameInstance->SaveSystem->RegisterSubsystem(Interface);  
}
```
