# Unreal Engine 4 - Garbage Collection and Memory Management

**Unreal Engine 4** has a system to manage the lifetime and memory of its objects. This system is not so intuitive for the people that just starting working on Unreal or the people used to vanilla memory management on C++11 or above. This system affects all objects that inherit from **UObject** and are decorated with the **UPROPERTY()** macro. All objects that meet these conditions will be managed by unreal using a Reference Counting system.

## Smart Pointer Clases
Before talk about the memory management of the **UObject** class. We will talk about the `std::shared_ptr<T>` and `std::weak_ptr<T>` equivalent classes in **Unreal Engine 4**

### TSharedPtr<T>

This smart pointer is equivalent to the std counterpart. Normally it is used in objects that does not inherit from **UOBJECT**.

#### Example:
```cpp
TSharedPtr<DataObject> dataObject = MakeShared<DataObject>(); // Create a Shared Pointer

TSharedPtr<DataObject> strongReference = dataObject;

dataObject = nullptr; //The data referenced is still in memory as strongReference is still pointing it.

strongReference = nullptr; // Data is released as the reference counting reach zero.

```

### TWeakPtr<T>
This smart porinter is equivalent to the std counterpart. Normally it is used in objects that does not inherit from **UOBJECT**. To get a shared pointer from this weak pointer use the function **Pin()**. this last function is equivalent to **lock()** on the std::weak_ptr class.

```cpp
TSharedPtr<DataObject> dataObject = MakeShared<DataObject>(); // Create a Shared Pointer

TWeakPtr<DataObject> = dataObject; // Reference the weak pointer to the shared pointer

dataObject = nullptr;	// Set shared pointer to null so the memory is released.

auto pointer = dataObject.Pin(); // Try to get the reference to the shared pointer

if(pointer.IsValid()) //this is false.
{
	//Not valid as was released when dataObject was set to nullptr.
}

```

## UObject Pointers

If the developer create a class that inherit from **UObject**, that class is now a candidate to use the Unreal Garbage Collector. The object need to be decorated with the **UPROPERTY()** macro be used as raw pointers. When all references to this objects are lost the object will be deleted in the next garbage collection cycle. 

### Usage
* Use the decorator macro `UPROPERTY()` in the member variable that will the garbage collection system.
	* Usign the macro as `UPROPERTY(Transient)` is the best option as removes the value serialization but maintain the reference counting.

* Like the std couterpart do not use raw pointer, without `UPROPERTY()` decoration to reference an **UObject**.

#### Example

```cpp
// SplineData.h
UCLASS()
class USplineData : public UObject // Inherits UObject
{
	GENERATED_BODY()
private:
	int _value;
public:
	USplineData();
	virtual ~USplineData();
};
```

```cpp
// Director.h
UCLASS()
class ADirector : public AActor
{
	GENERATED_BODY()

private: 

	UPROPERTY(Transient)
	SplineData *_splineData;
    
	UPROPERTY(Transient)
	SplineData *_strongReference;
    
    // Code Omitted
    
    void ExecuteTest();
}
```

```cpp
// Director.cpp
void ADirector::ExecuteTest()
{
	_splineData = NewObject<USplineData>();
	_strongReference = _splineData;
    
	_splineData = nullptr; //The data still exits as _strongReference still have a reference to it.
    
   	_strongReference = nullptr;
    
    // Now as _strongReference lost its reference. The allocated memory is free 
    // to be released by the Garbage Collector in a time in the future.
}
```

## AActor Pointers

In the case of objects of **AActor** type, the garbage collection also apply but with one difference. The AActor are always bound to a UWorld Object and if the used delete all its reference the world will continue to have one.

To delete an Actor use:

```cpp
// Destroy the object in its UWorld.
actor->Destroy()

// This will delete the object from the world and ensure that
// all References usign the garbage collection are set to nullptr.

// All the raw/non unreal managed pointers referencing that actor will become invalid.

```
## Common Errors and Solutions

In this section please add the problems encountered related to this topic and its solution.

## Related Topics and Links

* [Unreal Object Handling](https://docs.unrealengine.com/en-US/Programming/UnrealArchitecture/Objects/Optimizations/index.html)
* [Unreal Smart Pointer Library](https://docs.unrealengine.com/en-US/Programming/UnrealArchitecture/SmartPointerLibrary/index.html)