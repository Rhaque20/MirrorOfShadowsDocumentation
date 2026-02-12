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
}
```

```cpp
void USaveSystem::LoadGameFromSlots(FString SaveName)  
{  
  
    USaveGame* LoadedGame = UGameplayStatics::LoadGameFromSlot(TEXT("SAVEDMAN"), 0);  
    if (LoadedGame)  
    {        LoadedFile = Cast<USavedGame>(LoadedGame);  
        if (LoadedFile)  
        {            LoadedSave = true;  
        }        else  
        {  
            LoadedSave = false;  
        }    }  
}
```

## Step 2: Read the save file and load the necessary data into their respective objects

```cpp
void USaveSystem::RegisterSubsystem(TScriptInterface<ISavableInterface> Subsystem)  
{  
    Subsystems.Add(Subsystem);  
    if (LoadedSave)  
        Subsystem->LoadData(*LoadedFile);  
    else  
    {  
        Subsystem->InitializeData();  
    }}
```

Subsystems is TArray of all actors and components that implement `ISavableInterface`. It will go through each of them and if a save file is loaded, load the respective data. That way the data is loaded for each actor that is ready when they are ready.

Subsystems get added by each implemented actor/component with a line of code that goes like this:
```cpp
if (GameInstance && GameInstance->SaveSystem)  
{  
    TScriptInterface<ISavableInterface> Interface(this);  
    GameInstance->SaveSystem->RegisterSubsystem(Interface);  
}
```
