# Modify a module to change each private/protected to public
# Modify a module class to add method
# Modify a module class to add field
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

Code to add 2 value at the end of the enums  PlayTagArrows + PlayTagBombArrows

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
