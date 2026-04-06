# Zone Controller

**Author:** Rifle_AK
**Version:** 1.0.0
**Game:** Rust (Oxide/uMod)

A high-performance zone management plugin for Rust. Complete rewrite of ZoneManager with full backward compatibility, extensive bug fixes, and significant performance improvements.

## Overview

Zone Controller allows server administrators to create in-game zones with customizable flags that control gameplay behavior. Zones can be spherical (radius-based) or rectangular (box-based), and support parent/child relationships, temporary zones owned by other plugins, and a comprehensive developer API.

**Drop-in replacement** - Uses the same class name (`ZoneManager`), data format, config format, permissions, commands, API methods, and hooks as the original. Other plugins that reference ZoneManager will work without modification.

> **Important:** Remove the old `ZoneManager.cs` from your plugins folder before adding `ZoneController.cs`. Both cannot be loaded simultaneously.

## Installation

1. Remove `ZoneManager.cs` from your `oxide/plugins/` directory
2. Place `ZoneController.cs` into your `oxide/plugins/` directory
3. Your existing config (`oxide/config/ZoneManager.json`) and data (`oxide/data/ZoneManager/zone_data.json`) will be loaded automatically

## Permissions

| Permission | Description |
|---|---|
| `zonemanager.zone` | Access to all chat/console commands |
| `zonemanager.ignoreflag.<flag>` | Allows a player to bypass a specific zone flag |

> **Warning:** Do not blindly grant ignore flag permissions. Granting `zonemanager.ignoreflag.pvpgod` would let that player take damage in PvpGod zones, for example.

## Chat Commands

All commands require `zonemanager.zone` permission or auth level 2.

| Command | Description |
|---|---|
| `/zone_add` | Create a new zone at your current position |
| `/zone_edit <zone ID>` | Start editing a zone (or the zone you're standing in if only one) |
| `/zone_list` | List all zones with IDs and locations |
| `/zone_remove <zone ID>` | Remove a zone |
| `/zone_wipe` | Delete all non-temporary zones |
| `/zone_player [player]` | Display zone info for a player |
| `/zone_entity` | Display zone info for the entity you're looking at |
| `/zone_stats` | Show zone/player/entity counts |
| `/zone_flags` | Open the UI flag editor |
| `/zone <option> <value>` | Set a zone option or flag (must be editing a zone) |

You can set multiple flags in one command:
```
/zone eject true nobuild true undestr true name "Admin Base"
```

## Console Commands

| Command | Description |
|---|---|
| `zone <zoneID> <arg> <value>` | Set zone options from console |
| `zone_list` | List all zones |

## Zone Options

These are set via `/zone <option> <value>` while editing a zone.

| Option | Values | Description |
|---|---|---|
| `name` | `"text"` | Set the zone name |
| `id` | `"text"` | Change the zone ID |
| `location` | `"here"` or `"x y z"` | Move the zone |
| `radius` | number | Set sphere radius (switches to sphere mode) |
| `size` | `"w h l"` | Set box dimensions (switches to box mode) |
| `rotation` | number or empty | Set Y rotation (empty = your facing direction) |
| `radiation` | number | Add radiation to the zone |
| `comfort` | number | Set comfort level |
| `temperature` | number | Set temperature override |
| `safezone` | true/false | Mark as a safe zone (like Compound) |
| `enter_message` | `"text"` | Message shown when entering |
| `leave_message` | `"text"` | Message shown when leaving |
| `ejectspawns` | `"spawnfile"` | Spawnfile for eject destinations |
| `permission` | `"perm.name"` | Required permission to enter |
| `enabled` | true/false | Enable or disable the zone |
| `parentid` | `"zone ID"` | Set a parent zone for flag inheritance |

## Zone Flags

All flags are true/false values. Set via `/zone <flag> true` or the UI flag editor (`/zone flags`).

### Player Protection
| Flag | Description |
|---|---|
| `PvpGod` | Players immune to PVP damage |
| `PveGod` | Players immune to PVE damage |
| `SleepGod` | Sleeping players immune to damage |
| `NoFallDamage` | No fall damage |
| `NoWounded` | Skip wounded state on death |
| `NoBleed` | Prevent bleeding |
| `NoDrown` | Prevent drowning |
| `NoPoison` | Prevent poison |
| `NoStarvation` | Prevent starvation |
| `NoThirst` | Prevent dehydration |
| `NoRadiation` | Prevent radiation |
| `NoPve` | Animals are invincible to player attacks |
| `NoSuicide` | Prevent suicide command |

### Building & Structures
| Flag | Description |
|---|---|
| `NoBuild` | Prevent building (admin bypass) |
| `NoDeploy` | Prevent deploying items (admin bypass) |
| `NoCup` | Prevent placing tool cupboards (admin bypass) |
| `NoUpgrade` | Prevent upgrading structures (admin bypass) |
| `NoStability` | Disable structure stability |
| `UnDestr` | Structures immune to all damage |
| `NoBuildingDamage` | Building blocks immune to damage |
| `NoDecay` | Prevent decay damage |

### Loot & Items
| Flag | Description |
|---|---|
| `NoBoxLoot` | Prevent looting storage containers |
| `NoPlayerLoot` | Prevent looting other players |
| `NoNPCLoot` | Prevent looting NPC corpses |
| `LootSelf` | Allow looting own body when NoPlayerLoot is active |
| `NoPickup` | Prevent picking up dropped items |
| `NoCollect` | Prevent collecting world items (hemp, stone, etc.) |
| `NoEntityPickup` | Prevent picking up deployables |
| `NoDrop` | Destroy dropped items |
| `NoGather` | Prevent resource gathering |
| `NoLootSpawns` | Prevent loot containers from spawning |

### NPCs & Targeting
| Flag | Description |
|---|---|
| `NoHeliTargeting` | Helicopters won't target players |
| `NoTurretTargeting` | Auto turrets won't target players |
| `NoAPCTargeting` | Bradley APC won't target players |
| `NoNPCTargeting` | NPCs won't target players |
| `NpcFreeze` | Freeze all NPC movement |
| `NoNPCSpawns` | Prevent NPCs from spawning |

### Zone Entry & Exit
| Flag | Description |
|---|---|
| `Eject` | Eject unauthorized players (admin bypass) |
| `EjectSleepers` | Eject players when they go to sleep (admin bypass) |
| `KillSleepers` | Kill players when they go to sleep (admin bypass) |
| `Kill` | Kill players on entry (admin bypass) |

### Vehicles
| Flag | Description |
|---|---|
| `KeepVehiclesIn` | Prevent vehicles from leaving |
| `KeepVehiclesOut` | Prevent vehicles from entering |
| `NoVehicleMounting` | Prevent mounting vehicles |
| `NoVehicleDismounting` | Prevent dismounting vehicles |

### Communication
| Flag | Description |
|---|---|
| `NoChat` | Prevent text chat (admin bypass) |
| `NoVoice` | Prevent voice chat (admin bypass) |

### Miscellaneous
| Flag | Description |
|---|---|
| `AutoLights` | Toggle lights automatically based on time of day |
| `AlwaysLights` | Keep all lights on permanently |
| `InfiniteTrapAmmo` | Turrets/traps don't consume ammo |
| `PoweredSwitches` | Force switches/receivers to powered state |
| `NoCraft` | Prevent crafting |
| `NoVending` | Prevent using vending machines |
| `NoStash` | Prevent hiding stashes |
| `NoOvenToggle` | Prevent toggling ovens/lights/fires |
| `NoSignUpdates` | Prevent updating signs (admin bypass) |
| `NoDoorAccess` | Prevent opening doors |
| `NoCorpse` | Remove player corpses on spawn |
| `NoSprays` | Prevent spray painting |

### Plugin Integration
| Flag | Description |
|---|---|
| `NoKits` | Prevent redeeming kits (Kits plugin) |
| `NoTp` | Prevent teleportation (Teleportation plugin) |
| `NoRemove` | Prevent using remover tool (RemoverTool plugin) |
| `NoTrade` | Prevent trading (Trade plugin) |
| `NoShop` | Prevent using shops (GUIShop/ServerRewards) |

### Custom Flags
| Flag | Description |
|---|---|
| `Custom1` - `Custom5` | Available for other plugins to use with custom behavior |

Custom flags have no built-in behavior. They exist as flag slots that other plugins can set and check via the API. For example, a plugin can call `AddFlag(zoneId, "Custom1")` and then check `PlayerHasFlag(player, "Custom1")` to implement its own zone-based logic.

## Parent/Child Zones

By default, smaller zones inside larger zones inherit all parent flags. To override this, assign a parent zone:

```
/zone parentid <parent_zone_id>
```

When a player enters the child zone, the parent's flags are removed and only the child's flags apply. To keep specific parent flags, add them to the child zone too.

## Temporary Zones

Other plugins can create temporary zones that are automatically cleaned up:

```csharp
ZoneManager.Call("CreateOrUpdateTemporaryZone", this, "myzone_1", args, position);
```

These zones are not saved to disk and are removed when either Zone Controller or the owning plugin is unloaded.

## Configuration

Located at `oxide/config/ZoneManager.json`:

```json
{
  "Autolight Options": {
    "Time to turn lights on": 18.0,
    "Time to turn lights off": 6.0,
    "Lights require fuel to activate automatically": true
  },
  "Notification Options": {
    "Display notifications via PopupNotifications": false,
    "Chat prefix": "[Zone Manager] :",
    "Chat color (hex)": "#d85540"
  },
  "NPC players can deal player damage in zones with PvpGod flag": false,
  "Allow decay damage in zones with Undestr flag": false
}
```

## Developer API

### Zone Management

```csharp
// Create or update a zone
bool CreateOrUpdateZone(string zoneId, string[] args, Vector3 position = default)

// Bulk create/update
void CreateOrUpdateZones(List<(string, string[], Vector3)> zones, List<bool> results = null)

// Delete a zone
bool EraseZone(string zoneId)

// Bulk delete
void EraseZones(List<string> zoneIds, List<bool> results = null)
```

### Temporary Zone Management

```csharp
bool CreateOrUpdateTemporaryZone(Plugin owner, string zoneId, string[] args, Vector3 position = default)
void CreateOrUpdateTemporaryZones(Plugin owner, List<(string, string[], Vector3)> zones, List<bool> results = null)
bool EraseTemporaryZone(Plugin owner, string zoneId)
void EraseTemporaryZones(Plugin owner, List<string> zoneIds, List<bool> results = null)
```

### Zone Queries

```csharp
List<BasePlayer> GetPlayersInZone(string zoneId)
void GetPlayersInZoneNoAlloc(string zoneId, List<BasePlayer> list)
List<BaseEntity> GetEntitiesInZone(string zoneId)
void GetEntitiesInZoneNoAlloc(string zoneId, List<BaseEntity> list)

bool IsPlayerInZone(string zoneId, BasePlayer player)
bool IsEntityInZone(string zoneId, BaseEntity entity)

string[] GetPlayerZoneIDs(BasePlayer player)
void GetPlayerZoneIDsNoAlloc(BasePlayer player, List<string> list)
string[] GetEntityZoneIDs(BaseEntity entity)
void GetEntityZoneIDsNoAlloc(BaseEntity entity, List<string> list)

bool IsPositionInAnyZone(Vector3 position)
bool IsPositionInZone(string zoneId, Vector3 position)
void GetZonesAtPosition(Vector3 position, List<string> results)
```

### Zone Properties

```csharp
void SetZoneStatus(string zoneId, bool active)
object GetZoneRadius(string zoneId)
object GetZoneSize(string zoneId)
object GetZoneName(string zoneId)
object CheckZoneID(string zoneId)
object GetZoneIDs()
void GetZoneIDsNoAlloc(List<string> list)
Vector3 GetZoneLocation(string zoneId)
Dictionary<string, string> ZoneFieldList(string zoneId)
```

### Flag Operations

```csharp
bool HasFlag(string zoneId, string flagName)
bool PositionHasFlag(Vector3 position, string flagName)
void AddFlag(string zoneId, string flagName)
void RemoveFlag(string zoneId, string flagName)

// Disabled flags (temporary, not persisted through reloads)
bool HasDisabledFlag(string zoneId, string flagName)
void AddDisabledFlag(string zoneId, string flagName)
void RemoveDisabledFlag(string zoneId, string flagName)

bool EntityHasFlag(BaseEntity entity, string flagName)
bool PlayerHasFlag(BasePlayer player, string flagName)
```

### Player Management

```csharp
bool AddPlayerToZoneKeepinlist(string zoneId, BasePlayer player)
bool RemovePlayerFromZoneKeepinlist(string zoneId, BasePlayer player)
bool AddPlayerToZoneWhitelist(string zoneId, BasePlayer player)
bool RemovePlayerFromZoneWhitelist(string zoneId, BasePlayer player)
```

## Hooks

### Zone Events
```csharp
void OnEnterZone(string zoneId, BasePlayer player)
void OnExitZone(string zoneId, BasePlayer player)
void OnEntityEnterZone(string zoneId, BaseEntity entity)
void OnEntityExitZone(string zoneId, BaseEntity entity)
```

### Zone Lifecycle
```csharp
void OnZoneCreated(string zoneId)
void OnZoneUpdated(string zoneId)
void OnZoneErased(string zoneId)
void OnZoneInitialize(string zoneId)
void OnZoneDestroyed(string zoneId)
```

### Spawn Control
```csharp
// Return non-null to allow an entity to spawn in a zone that would normally prevent it
object CanSpawnInZone(BaseEntity entity)
```

## Changes from ZoneManager

### Bug Fixes
- **RemoveDisabledFlag** was adding the flag instead of removing it
- **NoCollect** flag had no handler (completely non-functional)
- **NoPickup** was only active when NoEntityPickup was also set on a zone
- **NoNPCLoot** was missing from hook subscription conditions
- **NoSprays** hook was not managed by the subscription system
- **KeepVehiclesIn/Out** was non-functional for mounted players
- **SetFlags(string[])** had inverted logic that broke string-based flag operations
- **OnBetterChat** was not properly subscribed with NoChat flag

### Performance Improvements
- HashSet-based zone membership tracking (O(1) lookups vs O(n) list scans)
- HashSet-backed player update queue eliminates O(n) Contains checks
- `OnEntitySpawned` only subscribed when NoCorpse/NoLootSpawns/NoNPCSpawns/NoDrop flags are active
- `OnPlayerSleep` only subscribed when KillSleepers/EjectSleepers are active
- Position-based fallback for spawn checks on newly created entities
- Dirty-flag system for efficient hook subscription recalculation

## License

GNU General Public License v3.0
