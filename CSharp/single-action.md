# Single Action
The goal of this pattern is to produce a data structure which holds a single action.

## Lambda as Data Structure
````
public delegate void Operation(int a);

public static class Operations
{    
  public static Operation StandardOperationOnX = a => OperateOnX(a, 0);

  public static void OperateOnX(int a, int x)
  {
    // a function body using a and x
    . . .
  }
}

public class Usage
{
    Operation InstanceOperationOnX = a => Operations.OperateOnX(a, 100);
    
    void Function(int a)
    {
      // using a globally-scoped version of the function.
      Functions.StandardOperationOnX(a);
      
      // using a version of the function scoped to this class.
      InstanceOperationOnX(a);
      
      // using a version of the function scoped to this function call.
      Operations.OperateOnX(a, 200);
    }
}
````

## Lambda As Data Structure (and do not expose underlying logic)
````
public delegate void Operation(int a);

public static class Operations
{    
  public static Operation Standard = MakeOperateOnX(0);

  public static void MakeOperateOnX(int x)
  {
    return a =>
    {
      // a function body using a and x
      . . .
    };
  }
}

public class Usage
{
    Operation Instance = Operations.MakeOperateOnX(100);
    
    void Function(int a)
    {
      // Use of a globally-scoped version of the function.
      Functions.Standard(a);
      
      // Use of a version of the function specific to this class.
      Instance(a);
      
      // Use a version of the function specific to this function call. 
      Operations.MakeOperateOnX(200)(a);
    }
}
````

## Class as data structure
````
public class Function
{
  int X;
  
  public Function(int x)
  {
    X = x;
  }
  
  public void Action(int a)
  {
    // a function body using a and X
    . . .
  }
  
  public static void Action(int a, int X)
  {
  }
}

public static Class Functions
{
    public static class Standard = new Function(0);
}

public class Usage
{
    Operation Instance = a => Operations.OperateOnX(a, 100);
    
    void Function(int a)
    {
      // Using a globally-scoped version of the function.
      Functions.Standard.Action(a);
      
      // Using a version of the function scoped to this class.
      Instance.Action(a);
      
      // Using a version of the function scoped to this function call.
      new Function(200).Action(a);
    }
}
````

## Class as data structure (and do not expose underlying logic)
````
public class Function
{
  int X;
  
  public Function(int x)
  {
    X = x;
  }

  public void Action()
  {
    // a function body using X
    . . .
  }
}

public static Class Functions
{
    public static class Standard = new Function(0);
}

public class Usage
{
    Operation Instance = a => Operations.OperateOnX(a, 100);
    
    void Function(int a)
    {
      // Using a globally-scoped version of the function.
      Functions.Standard.Action(a);
      
      // Using a version of the function scoped to this class.
      Instance.Action(a);
      
      // Using a version of the function scoped to this function call.
      new Function(200).Action(a);
    }
}
````
