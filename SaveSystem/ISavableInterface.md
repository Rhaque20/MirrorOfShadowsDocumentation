
```cpp
class MIRROROFSHADOWS_API ISavableInterface  
{  
    GENERATED_BODY()  
  
public:  
    virtual int32 GetSavePhase() const = 0;  
    virtual void PreSave() {}  
    virtual void SaveData(USavedGame& Root) = 0;  
    virtual void PostSave() {}  
  
    virtual void PreLoad() {}  
    virtual void LoadData(const USavedGame& Root) = 0;  
    virtual void PostLoad() {}  
};
```