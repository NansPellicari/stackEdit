# UE4

## Retargeting

* engine doc[https://www.youtube.com/watch?v=FDbpHamn2eY](https://www.youtube.com/watch?v=FDbpHamn2eY)

## Usefull code

### Get string from binary UTF8 string
```cpp
TArray<uint8> ReturnedPayload;
FUTF8ToTCHAR ToTCharConverter(reinterpret_cast<ANSICHAR*>(ReturnedPayload.GetData()), ReturnedPayload.Num());  
FString OutString(ToTCharConverter.Length(), ToTCharConverter.Get());  
UE_LOG(LogTemp, Warning, TEXT("ToTCharConverter.Length: %i"), ToTCharConverter.Length());  
UE_LOG(LogTemp, Warning, TEXT("ReturnedPayload(from UTF8): %s"), *OutString);  
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTkyOTI1ODQxOCwxNzE2MTkwNl19
-->