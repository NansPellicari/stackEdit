## About ndisplay cluster and function replication:
https://p4-swarm.epicgames.net/reviews/16197795

## Entry point to see how serialization is made on properties:
```cpp
Engine\Source\Runtime\Serialization\Private\StructSerializer.cpp
FStructSerializer::SerializeElement()
```
which call:
```cpp
Engine\Source\Runtime\Serialization\Private\StructSerializer.cpp
StructSerializer::Serialize()
```
which call:
```cpp
Engine\Source\Runtime\Serialization\Public\IStructSerializerBackend.h
or 
Engine\Source\Runtime\Serialization\Public\IStructSerializerBackend.h
FJsonStructSerializerBackend::WriteProperty()
```

## try to get and set property dynamically

```cpp
include "IStructSerializerBackend.h"  
#include "Backends/JsonStructDeserializerBackend.h"  
#include "Backends/JsonStructSerializerBackend.h"  
#include "ISevenSenseRemoteControlModule.h"  
  
/**  
 * This struct is dedicated to set a property value to an actor object. */  
template <class PropertyType, class ValueType>  
struct FActorPropertyWriter  
{  
   AActor* Actor;  
   const FString& PropertyPath;  
   const ValueType& PropertyValue;  
  
   FActorPropertyWriter(AActor* InActor, const FString& InPropertyPath, const ValueType& InPropertyValue)  
      : Actor(InActor),  
        PropertyPath(InPropertyPath),  
        PropertyValue(InPropertyValue) {}  
  
   bool Set() const  
  {  
      FRCObjectReference ObjectRef;  
      ERCAccess Access = ERCAccess::WRITE_TRANSACTION_ACCESS;  
      bool bSuccess = IRemoteControlModule::Get().ResolveObjectProperty(  
         Access,  
         Actor,  
         PropertyPath,  
         ObjectRef  
  );  
  
      if (bSuccess && ObjectRef.Property.IsValid())  
      {  
         FName ResolvedPropertyName = ObjectRef.Property->GetFName();  
         TArray<uint8> NewPayload;  
         FMemoryWriter Writer{NewPayload};  
         TSharedPtr<TJsonWriter<UCS2CHAR>> JsonWriter = TJsonWriter<UCS2CHAR>::Create(&Writer);  
         FJsonStructSerializerBackend WriterBackend{Writer, EStructSerializerBackendFlags::Default};  
  
         // Write the json  
  JsonWriter->WriteObjectStart();  
         JsonWriter->WriteIdentifierPrefix(ResolvedPropertyName.ToString());  
         JsonWriter->WriteValue<ValueType>(PropertyValue);  
         JsonWriter->WriteValue(TEXT("GenerateTransaction"), true);  
         JsonWriter->WriteObjectEnd();  
  
         // Then deserialize the payload onto all the bound objects.  
  FMemoryReader NewPayloadReader(NewPayload);  
         FJsonStructDeserializerBackend Backend(NewPayloadReader);  
  
         NewPayloadReader.Seek(0);  
         // Set a ERCPayloadType and NewPayload in order to follow the replication path  
  bSuccess &= IRemoteControlModule::Get().SetObjectProperties(  
            ObjectRef,  
            Backend,  
            ERCPayloadType::Json,  
            NewPayload  
  );  
  
         if (bSuccess == false)  
         {  
            UE_LOG(  
               LogSevenSenseRC,  
               Warning,  
               TEXT("Field %s can't be set in %s"),  
               *PropertyPath,  
               *Actor->GetName()  
            );  
         }  
      }  
      else  
  {  
         UE_LOG(LogSevenSenseRC, Warning, TEXT("Field %s can't be find in %s"), *PropertyPath, *Actor->GetName());  
      }  
  
      return bSuccess;  
   }  
};  
  
/**  
* This struct is dedicated to get a property value from an actor object. */  
template <class PropertyType, class ValueType>  
struct FActorPropertyReader  
{  
   AActor* Actor;  
   const FString& PropertyPath;  
   ValueType& PropertyValue;  
  
   FActorPropertyReader(AActor* InActor, const FString& InPropertyPath, ValueType& InPropertyValue)  
      : Actor(InActor),  
        PropertyPath(InPropertyPath),  
        PropertyValue(InPropertyValue) {}  
  
  
   bool Get() const  
  {  
      TArray<uint8> WorkingBuffer;  
      FMemoryWriter Writer(WorkingBuffer);  
      TSharedPtr<TJsonWriter<UCS2CHAR>> JsonWriter = TJsonWriter<UCS2CHAR>::Create(&Writer);  
  
      FRCObjectReference ObjectRef;  
  
      bool bSuccess = true;  
  
      bSuccess &= IRemoteControlModule::Get().ResolveObjectProperty(  
         ERCAccess::READ_ACCESS,  
         Actor,  
         PropertyPath,  
         ObjectRef  
  );  
  
      if (ObjectRef.Property.IsValid())  
      {  
         void* ValuePtr = ObjectRef.Property->ContainerPtrToValuePtr<void>(Actor);  
         if (PropertyType* TypedProperty = CastField<PropertyType>(ObjectRef.Property.Get()))  
         {  
            PropertyValue = TypedProperty->GetPropertyValue(ValuePtr);  
            return true;  
         }  
         UE_LOG(  
            LogSevenSenseRC,  
            Warning,  
            TEXT("Wrong PropertyType %s for field %s in %s"),  
            *ObjectRef.Property->GetCPPType(),  
            *PropertyPath,  
            *Actor->GetName()  
         );  
         return false;  
      }  
  
      UE_LOG(LogSevenSenseRC, Warning, TEXT("Field %s can't be find in %s"), *PropertyPath, *Actor->GetName());  
  
      return false;  
   }  
};

bool FActorPropertyHandler::GetValue(float& OutValue)  
{  
   return (FActorPropertyReader<FFloatProperty, float>(Actor, PropertyPath, OutValue)).Get();  
}

bool FActorPropertyHandler::SetValue(float InValue)  
{  
   return (FActorPropertyWriter<FFloatProperty, float>(Actor, PropertyPath, InValue)).Set();  
}

```

## Create an asset with its package

````cpp
struct FCreatePresetAsset  
{  
  static constexpr auto AssetFolder = TEXT("RC_TempAssetFolder");  
  
  explicit FTempPresetAsset(const FString& InAssetName)  
   : AssetName(InAssetName),  
     PackageFolder(FString::Printf(  
           TEXT("/Game/%s"),  
           AssetFolder)),  
     PackageName(FString::Printf(  
           TEXT("%s/%s"),  
           *PackageFolder,  
           *AssetName ))  
{  
   FPackageName::TryConvertLongPackageNameToFilename(PackageName, PathOnDisk);  
}
  
  const FString AssetName; 
  TStrongObjectPtr<UPackage> AssetPackage = nullptr;  
  TStrongObjectPtr<URemoteControlPreset> CreatedAsset = nullptr;  
  FString PackageFolder;  
  const FString PackageName;  
  FString PathOnDisk;  

	URemoteControlPreset* Create()
	{
	   UClass* AssetClass = URemoteControlPreset::StaticClass();  
	  
	   // In game mode it seems that this package is created in memory  
	  AssetPackage = TStrongObjectPtr<UPackage>(CreatePackage(*PackageName));  
	   if (!ensure(AssetPackage))  
	   {  
	      return nullptr;  
	   }  
	  
	   EObjectFlags Flags = RF_Public | RF_Standalone;  
	  
	#if WITH_EDITOR  
	  Flags |= RF_Transactional;  
	#endif  
	  
	  CreatedAsset = TStrongObjectPtr<URemoteControlPreset>(  
	 NewObject<URemoteControlPreset>(AssetPackage.Get(), AssetClass, FName(*AssetName), Flags)  
	   );  
	  
	   UPackage::SavePackage(  
	      AssetPackage.Get(),  
	      CreatedAsset.Get(),  
	      RF_Public | RF_Standalone,  
	      *(PathOnDisk + FPackageName::GetAssetPackageExtension())  
	   );  
	  
	   return CreatedAsset.Get();
	  }
	  
	void Save()  
	{  
	#if WITH_EDITOR  
	 if (AssetPackage.IsValid() && CreatedAsset.IsValid())  
	   {  
	      bool bSaved = UPackage::SavePackage(  
	         AssetPackage.Get(),  
	         CreatedAsset.Get(),  
	         RF_Public | RF_Standalone,  
	         *(PathOnDisk + FPackageName::GetAssetPackageExtension()),  
	         GError,  
	         nullptr,  
	         true,  
	         true,  
	         SAVE_NoError  
	  );  
	   }  
	#endif  
	}
	void FTempPresetAsset::Delete()  
	{  
	   AssetPackage.Reset();  
	   CreatedAsset.Reset();  
	#if WITH_EDITOR  
	  // Because this is called when the module shutdown, all assets are already unload,  
	 // we can only removed the asset directly from the disk. FString PathToDeleteOnDisk;  
	   if (FPackageName::TryConvertLongPackageNameToFilename(PackageFolder, PathToDeleteOnDisk))  
	   {  
	      IFileManager::Get().DeleteDirectory(*PathToDeleteOnDisk, false, true);  
	   }  
	#endif  
	}
}
````

## Convert Bytes String with terminator, without terminator

````cpp
FString PayloadBaseString(InPayload.Num() / sizeof(TCHAR), reinterpret_cast<const TCHAR*>(InPayload.GetData()));
````

## Debug UStruct

```cpp
// In module dependencies:
// "TraceLog",  
// "TraceAnalysis",  
// "TraceServices",  
// "TraceInsights",  
// "GameplayInsights"
/* in .uplugins or .uproject
{  
  "Name": "GameplayInsights",  
  "Enabled": false  
}
*/

#if WITH_EDITOR  
#include "C:\Users\nansp\Perforce\Denys.Dubinin_MSI_853\Engine\Plugins\Animation\GameplayInsights\Source\GameplayInsights\Private\ObjectPropertyTrace.h"  
#include "C:\Users\nansp\Perforce\Denys.Dubinin_MSI_853\Engine\Plugins\Animation\GameplayInsights\Source\GameplayInsights\Private\ObjectPropertyTrace.cpp"  
#endif

ObjectPropertyTrace::IterateProperties(RCFunction->GetFunction(), FunctionArgs.GetStructMemory(),  
   [](const FString& InType, const FString& InKey, const FString& InValue, int32 InId, int32 InParentId)  
{  
   UE_LOG(LogTemp, Warning, TEXT("DebugDefaultProps -- %s(%s): %s"), *InKey, *InType, *InValue);  
});
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbNDIxNDMzMjc0LC0xMDcxNzAzNDg5LC01Mj
UxNTYwMTUsLTEzMzUyNDIxMTQsLTE0MTM3Nzc3MTQsLTEzOTkz
OTc1MTcsLTkxNjQ2Njg2OCwxNzcwNTk4NTgxLC0xMjM2OTY1MD
csMTAxMDY0MTA0MSwtNDI1MDk0NzA3LC0xMzQ2ODg4MzEwXX0=

-->