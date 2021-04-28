# UE4

## Retargeting

* engine doc[https://www.youtube.com/watch?v=FDbpHamn2eY](https://www.youtube.com/watch?v=FDbpHamn2eY)

## Usefull code

### Get string from binary UTF8 string

```cpp
// An UTF8 string
TArray<uint8> ReturnedPayload;
FUTF8ToTCHAR ToTCharConverter(reinterpret_cast<ANSICHAR*>(ReturnedPayload.GetData()), ReturnedPayload.Num());  
FString OutString(ToTCharConverter.Length(), ToTCharConverter.Get());  
UE_LOG(LogTemp, Warning, TEXT("ToTCharConverter.Length: %i"), ToTCharConverter.Length());  
UE_LOG(LogTemp, Warning, TEXT("ReturnedPayload(from UTF8): %s"), *OutString);  
```

## Reflexion system

keep tracks that:
![](https://media.discordapp.net/attachments/826037456881057795/836931897908133918/unknown.png?width=960&height=464)

`Engine\Source\Editor\UnrealEd\Private\Editor.cpp#CopySinglePropertyRecursive()`

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE4MzYzMjQyNzcsLTkwMTQyMjczNSwxNz
E2MTkwNl19
-->