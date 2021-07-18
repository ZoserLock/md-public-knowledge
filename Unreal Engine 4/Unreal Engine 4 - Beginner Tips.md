# Unreal Engine 4 - Beginner Tips

## TFunction<>

### TFunction as a variable

```cpp
struct NumberPrinter
{
private:
    bool HappyBehavior = false;

    TFunction<String(const int)> GetPrintMessage = [=](const int number)
        {
            return FString::Printf(TEXT("The number is: %d"), number);
        };
    TFunction<String(const int)> GetPrintMessageHappy = [=](const int number)
        {
            return FString::Printf(TEXT("Yahoo!! The number is: %d!!!"), number);
        };

public:
    FString PrintIntegerMessage(const int number) const
    {
        return HappyBehavior ? GetPrintMessageHappy(number) : GetPrintMessage(number);
    }
}
```

### TFunction as function parameter

```cpp
struct MyStruct
{
private:
	TMap<int, float> DataMapFloats;
	TMap<int, bool> DataMapBools;

	template <typename PropType>
	PropType GetMapValue(TMap<int, PropType>& DataMap, int ValueKey, TFunction<PropType(const int)> GetDefaultValue)
	{
		PropType* ReturnValue = DataMap.Find(ValueKey);
		if (ReturnValue)
		{
			return *ReturnValue;
		}
		return GetDefaultValue(ValueKey);
	}

public:
    float GetFloatValue(int ValueKey)
    {
        return GetMapValue<float>(DataMapFloats, ValueKey, [](const int) {
            return 0.f;
        });
    }

    float GetFloatValue(int ValueKey)
    {
        return GetMapValue<bool>(DataMapBools, ValueKey, [](const int) {
            return false;
        });
    }
}
```