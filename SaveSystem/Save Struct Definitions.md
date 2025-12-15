### FInventoryToSave
```cpp
USTRUCT(BlueprintType)  
struct FInventoryToSave  
{     
GENERATED_BODY()  
     UPROPERTY(BlueprintReadWrite, SaveGame)  
    TArray<FItemSpec> ItemsToSave;  
    UPROPERTY(BlueprintReadWrite, SaveGame)  
    TMap<int,FArmorData> SavedArmor;  
  
};
```
Which stores the item specs for materials and FArmorData for the individual armor pieces.

### FDungeonToSave
```cpp
USTRUCT(BlueprintType)  
struct FDungeonToSave  
{  
    GENERATED_BODY()  
  
    UPROPERTY(BlueprintReadWrite, SaveGame)  
    TSet<FGuid> SavedChests;  
  
    UPROPERTY(BlueprintReadWrite, SaveGame)  
    TSet<FGuid> SavedEnemySpawner;  
};
```
This stores the dungeon's opened chests and cleared spawners. (Should be renamed from SavedEnemySpawner to ClearedEnemySpawner and SavedChests to OpenedChests)
### FEquipmentToSave
```cpp
USTRUCT(BlueprintType)  
struct FEquipmentToSave  
{  
    GENERATED_BODY()  
  
    UPROPERTY()  
    TMap<FGameplayTag, FArmorData> SavedEquippedArmor;  
  
};
```

This struct handles per character of storing what is equipped in each slot. The reason for making a map is enable iteration.

### FSkillTreeData
```cpp
USTRUCT(BlueprintType)  
struct FSkillTreeSaveData  
{  
    GENERATED_BODY()  
  
    // Use soft object pointers for UDataAsset references  
    UPROPERTY(SaveGame)  
    TArray<TSoftObjectPtr<USkillTreeNodeData>> ActiveSkillTreeNodes;  
  
    // FName serializes directly without issue  
    UPROPERTY(SaveGame,BlueprintReadOnly)  
    TArray<FName> ActiveSkillTreeNodeNames;  
    //Other stuff  
    UPROPERTY(SaveGame,BLueprintReadOnly)  
    float CurrentXP;  
    UPROPERTY(SaveGame,BlueprintReadOnly)  
    float CurrentAP;  
    UPROPERTY(SaveGame,BlueprintReadOnly)  
    int Level;  
};
```

This handles individual skill tree data such as how much XP and AP the character has and what nodes they have unlocked as well as their current level. Each struct made is tied to a character.