About ndisplay cluster and function replication:
https://p4-swarm.epicgames.net/reviews/16197795

Entry point to see how serialization is made on properties:
```cpp
F:\Perforce\Denys.Dubinin_DESKTOP-E8JFBMS_5153\Engine\Source\Runtime\Serialization\Private\StructSerializer.cpp
FStructSerializer::SerializeElement()
```

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

````


> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbODk3OTEwMjA5LC00MjUwOTQ3MDcsLTEzND
Y4ODgzMTBdfQ==
-->