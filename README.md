# Modify a module to change each private/protected to public
# Modify a module class to add method

Mandatory : always add a return instruction even if the method is empty, else the module will crash at the atching time or in executing or launching
```
.....Body.Instructions.Add(Instruction.Create(OpCodes.Ret));
```

```
Mono.Cecil;
using Mono.Cecil.Cil;
using System;
using System.Linq;
using System.Timers;

namespace Patcher
{
  public static class MyPlayer
  {
    public static void PatchModule(ModuleDefinition baseModule)
    {
      MethodDefinition OnTimedEventPlayTag = new MethodDefinition("OnTimedEventPlayTag", Mono.Cecil.MethodAttributes.Public | Mono.Cecil.MethodAttributes.Virtual, baseModule.TypeSystem.Void);
      OnTimedEventPlayTag.Parameters.Add(new ParameterDefinition("source", ParameterAttributes.None, baseModule.ImportReference(typeof(Object))));
      OnTimedEventPlayTag.Parameters.Add(new ParameterDefinition("e", ParameterAttributes.None, baseModule.ImportReference(typeof(System.Timers.ElapsedEventArgs))));

      type.Methods.Add(OnTimedEventPlayTag);
      //var ilProcessor = StartPlayTag.Body.GetILProcessor();
      //Instruction msg = ilProcessor.Create(OpCodes.Ldstr, "!");
      OnTimedEventPlayTag.Body.Instructions.Add(Instruction.Create(OpCodes.Ret));


    }
  }
}
```


## With return Void
      MethodDefinition OnTimedEventPlayTag = new MethodDefinition("OnTimedEventPlayTag", Mono.Cecil.MethodAttributes.Public | Mono.Cecil.MethodAttributes.Virtual, baseModule.TypeSystem.Void);

## With return String
MethodDefinition GetPlayTagCounterValue = new MethodDefinition("GetPlayTagCounterValue", Mono.Cecil.MethodAttributes.Public | Mono.Cecil.MethodAttributes.Virtual, baseModule.TypeSystem.String);

## With return int
      MethodDefinition GetPlayTagCountDown = new MethodDefinition("GetPlayTagCountDown", Mono.Cecil.MethodAttributes.Public | Mono.Cecil.MethodAttributes.Virtual, baseModule.TypeSystem.Int32); 

## With method parameters
```
      OnTimedEventPlayTag.Parameters.Add(new ParameterDefinition("source", ParameterAttributes.None, baseModule.ImportReference(typeof(Object))));
      OnTimedEventPlayTag.Parameters.Add(new ParameterDefinition("e", ParameterAttributes.None, baseModule.ImportReference(typeof(System.Timers.ElapsedEventArgs))));
```      
# Modify a module class to add field

```
Mono.Cecil;
using Mono.Cecil.Cil;
using System;
using System.Linq;
using System.Timers;

namespace Patcher
{
  public static class MyPlayer
  {
    public static void PatchModule(ModuleDefinition baseModule)
    {
      var type = baseModule.AllNestedTypes().Single(t => t.FullName == "TowerFall.Player");
      type.Fields.Add(new FieldDefinition("PlayTag", Mono.Cecil.FieldAttributes.Public , baseModule.ImportReference(typeof(Boolean))));
      //type.Fields.Add(new FieldDefinition("PlayTagTimer", Mono.Cecil.FieldAttributes.Public, baseModule.ImportReference(typeof(System.Timers.Timer))));
      type.Fields.Add(new FieldDefinition("PlayTagCountDown", Mono.Cecil.FieldAttributes.Public, baseModule.ImportReference(typeof(int))));
      type.Fields.Add(new FieldDefinition("creationTime", Mono.Cecil.FieldAttributes.Public, baseModule.ImportReference(typeof(DateTime))));
    }
  }
}
```

## With return Boolean

type.Fields.Add(new FieldDefinition("PlayTag", Mono.Cecil.FieldAttributes.Public , baseModule.ImportReference(typeof(Boolean))));

## With return Timer

//type.Fields.Add(new FieldDefinition("PlayTagTimer", Mono.Cecil.FieldAttributes.Public, baseModule.ImportReference(typeof(System.Timers.Timer))));

## With return int

type.Fields.Add(new FieldDefinition("PlayTagCountDown", Mono.Cecil.FieldAttributes.Public, baseModule.ImportReference(typeof(int))));

## With return DateTime

type.Fields.Add(new FieldDefinition("creationTime", Mono.Cecil.FieldAttributes.Public, baseModule.ImportReference(typeof(DateTime))));

# Modify a module to add new value in an enum
```
public enum Pickups
{
  Arrows,
  BombArrows,
  SuperBombArrows,
  LaserArrows,
  BrambleArrows,
  DrillArrows,
  BoltArrows,
  FeatherArrows,
  TriggerArrows,
  PrismArrows,
  Shield,
  Wings,
  SpeedBoots,
  Mirror,
  TimeOrb,
  DarkOrb,
  LavaOrb,
  SpaceOrb,
  ChaosOrb,
  Bomb,
  Gem,
}
```

Code to add 2 value **at the end** of the enums  PlayTagArrows + PlayTagBombArrows
We can't add value in the middle or at the start (if we did and re-numberer each value, sill not works)

```
namespace Patcher
{
  public static class MyPickups
  {
    public static void PatchModule(ModuleDefinition baseModule)
    {
      var type = baseModule.AllNestedTypes().Single(t => t.FullName == "TowerFall.Pickups");
      FieldDefinition lastPlayTagArrows = new FieldDefinition("PlayTagArrows", FieldAttributes.Static | FieldAttributes.Literal | FieldAttributes.Public | FieldAttributes.HasDefault, type) { Constant = 21 };
      type.Fields.Add(lastPlayTagArrows);
      FieldDefinition lastPlayTagBombArrows = new FieldDefinition("PlayTagBombArrows", FieldAttributes.Static | FieldAttributes.Literal | FieldAttributes.Public | FieldAttributes.HasDefault, type) { Constant = 22 };
      type.Fields.Add(lastPlayTagBombArrows);
    }
  }
}
```

Result

```
public enum Pickups
{
  Arrows,
  BombArrows,
  SuperBombArrows,
  LaserArrows,
  BrambleArrows,
  DrillArrows,
  BoltArrows,
  FeatherArrows,
  TriggerArrows,
  PrismArrows,
  Shield,
  Wings,
  SpeedBoots,
  Mirror,
  TimeOrb,
  DarkOrb,
  LavaOrb,
  SpaceOrb,
  ChaosOrb,
  Bomb,
  Gem,
  PlayTagArrows,
  PlayTagBombArrows,
}
```

# static method issue


