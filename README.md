# Refactoring to Patterns Notes
My personal notes from: 'Refactoring to Patterns' by Joshua Kerievsky 

# Introduct Null Objects
- If you have conditional logic that checks for null value, refactor it to use a Null Object
- This benifit removes the need to check whether a field or variable is null by making it possible to always call the field or variable safely

## Strategy:
- 1. Create a null object by applying Extract Subclass
- 2. Look for a null check
- 3. Apply the null object to the class that checks for null types

- Example(before):
```java
if (getState().equals(REQUESTED))  // equality logic with type-unsafe string constant
 setState(PermissionState.CLAIMED);
```
- After(Introducted Null Object):
```java
if (getState().equals(PermissionState.REQUESTED))
 setState(PermissionState.CLAIMED);
```

- Another Example(before): 
```java
public boolean mouseMove(Event event, int x, int y) {
    if (mouseEventHandler != null)
      return mouseEventHandler.mouseMove(graphicsContext, event, x, y );
    return true;
  }
```
- After
-- Created NullMouseEventHandler with appropriate method that addresses null result
```java
public class NullMouseEventHandler extends MouseEventHandler {
  public NullMouseEventHandler(Context context) {
    super(context);
  }
  public boolean mouseMove(MetaGraphicsContext mgc, Event event, int x, int y) {
    return true;
  }
}
```
-- Changed null occurances back in super class:
```java
private MouseEventHandler mouseEventHandler = new NullMouseEventHandler();
  public boolean mouseMove(Event event, int x, int y) {
    ~~if (mouseEventHandler != null)~~
      return mouseEventHandler.mouseMove(graphicsContext, event, x, y );
    ~~return true;~~
  }
```


