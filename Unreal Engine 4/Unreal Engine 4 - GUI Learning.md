# Unreal Engine 4 - GUI Learning

In **Unreal** the GUI can be developed using **C++**, **Blueprints** or both. We think that the best way to create GUI in **Unreal** is separating the programming logic to the graphical part, for this reason we always recommend creating a **C++** Class and use it as parent for the **Blueprint** version of that class and all the logic should be implemented in the C++ counterpart.

The blueprint part should only contain the graphical elements references.

## Class and Blueprint Implementation

* Class Definition
* Blueprint definition
* Explain Widget Bindings

#### Example

### Widget Bindings

* How do Widget Bindings
* Limitations

#### Example

## Add/Remove From View

// How to add and remove UserWidget

### Remove all Caveats
We do not recomend use Remove all as multiple widgets can live in the same viewport. Using this function we can end up deleting a UserWidget we were not meant to delete. This happened a lot in **ROA3**

## Ease of Use
Some virtual functions of **UserWidget** are meant to be use to improve the ease of use of the UserWidget. Here two of the most used in **ROA3**

### Variable Update/Sync
Every time an editor variable is changed a member function in the UserWidget is called. This function is used to allow change the state of the UserWidget in editor to allow, for example, update the UserWidget and change some image or color as the user change a variable in the inspector. This is really usefull when you make a UserWidget that is Reusable.

#### Example


### Variable Binding

Some UProperties can be linked one to another.

#### Example

##  Good Practices

The hell can break loose fast if the GUI is created without some considerations. Here some Good Practices to be able to maintain the code better.

### UserWidget Reuse
As in software development is always desired to reuse

#### Example

## Render 3D Objects to UI

## Common Errors and Solutions

In this section please add the problems encountered related to this topic and its solution.

## Related Topics and Links