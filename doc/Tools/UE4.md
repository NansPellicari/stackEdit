# UE4

## Retargeting

* engine doc[https://www.youtube.com/watch?v=FDbpHamn2eY](https://www.youtube.com/watch?v=FDbpHamn2eY)

## Usefull code

```cpp
TArray<uint8> ReturnedPayload;
FUTF8ToTCHAR ToTCharConverter(reinterpret_cast<ANSICHAR*>(ReturnedPayload.GetData()), ReturnedPayload.Num());  
FString OutString(ToTCharConverter.Length(), ToTCharConverter.Get());  
FString OutStringBase(reinterpret_cast<ANSICHAR*>(OutputBuffer.GetData()));  
UE_LOG(LogTemp, Warning, TEXT("OutputBuffer.Num(): %i"), OutputBuffer.Num());  
UE_LOG(LogTemp, Warning, TEXT("ToTCharConverter.Length: %i"), ToTCharConverter.Length());  
UE_LOG(LogTemp, Warning, TEXT("ReturnedPayload(from UTF8): %s"), *OutString);  
UE_LOG(LogTemp, Warning, TEXT("OutputBuffer: %s"), *OutStringBase);
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEzMjc1MTI3NzcsMTcxNjE5MDZdfQ==
-->