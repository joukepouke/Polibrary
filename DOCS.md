# Manual for Polibrary 2.0

## Linking it to your Mod
Before you can use the vast amount of premade abilities, or the GLD tools, you must list **Polibrary** as a dependency in your `manifest.json` file.  
(More info in the Polymod wiki)

```json
"dependencies": [
    {
        "id": "polibrary"
    }
]
```

---

## Improvement Abilities

### Constant

| ID | Description |
|----|-------------|
| `polib_block` | *Impassable obstacle.* (No units can move to this tile, not even yours) |
| `polib_indestructible` | *Can not be crushed.* (Nature Bunny can still crush it, only units with `polib_crush` can’t) |
| `polib_isolated` | *Can not be built next to itself.* (Including diagonals) |
| `polib_native` | *Can only be built in your own terrain.* (In your own climate/biome) |
| `polib_foreign` | *Can not be built in your own terrain.* (On tiles that are different from your home biome) |
| `polib_cleanse` | *Removes all effects from the unit stepping on this improvement.* |
| `polib_demolishable` | *Can be demolished even without the necessary tech requirements.* |
| `polib_healall` | *Heals all units that step on this tile.* |
| `polib_healfriendly` | *Heals units belonging to the owner of this improvement once they step onto this tile.* (Same as healall, but only heals your units) |
| `polib_preventagent` | *Prevents agent units from being trained on neighbouring tiles.* (Originally agents couldn’t spawn near temples, this recreates that) |

### One-Time  
*(Triggers only upon building the improvement. Pairs well with Manual-Discrete improvements)*  

| ID | Description |
|----|-------------|
| `polib_healonce` | *Heals unit on the tile upon doing this.* |
| `polib_cleanseonce` | *Removes all effects from the unit on the tile upon doing this.* |
| `polib_killunit` | *Kills the unit on the tile upon doing this.* (In exchange for the building being built) |
| `polib_gainxp` | *The unit on the tile gains 3 XP upon doing this.* (Usually enough to promote to a veteran, will be displayed as “kills”) |
| `polib_research` | *Gain a random technology upon doing this.* |

---

## Situational Requirements (Manual-Discrete Improvements)

| ID | Description |
|----|-------------|
| `polib_fullhealthbuilder` | *The unit on this tile must have full health to do this.* (For manual improvements this means the unit doing the action) |
| `polib_woundedbuilder` | *The unit on this tile must not have full health to do this.* (Useful if a healing ability should only be present when the unit needs it) |

---

## GLD Tools
To help you make one-time abilities, Polibrary includes four helpful parameters that you can use for each improvement inside `improvementData`.  
All of the following GLD Tools can be written both in `CamelCase` and `camelCase`.

- **BuiltBySpecific**  
  Allows you to specify the circumstances in which you allow the improvement to be built.  
  The improvement can only be built on tiles where a unit with the specific ability stands.  
  For manual improvements, this means only units with this ability can perform the improvement.  
  *(You can define dummy abilities in the `unitAbility` part of your `patch.json`.)*  

- **NotBuiltBySpecific**  
  Allows you to specify if a manual improvement can’t be built by specific units.  
  Requires a `unitAbility`, and those units with it can’t perform the improvement.  

- **BuiltOnSpecific**  
  Similar to BuiltBySpecific. Allows you to specify an **improvement ability** that this discrete improvement can be built on.  
  *(You can define dummy improvement abilities in `improvementAbility` part of `patch.json`.)*  

- **Unblock**  
  Used inside improvements with the `polib_block` ability.  
  Specify the `unitAbility` that, if applied, allows the unit to move onto this improvement.  
  *(If multiple improvements have `polib_block` and you want your unit to traverse all of them, apply Unblock to all of those improvements.)*  

---

## Unit Abilities
In addition to improvement abilities, Polibrary offers unit abilities as well.  
These have custom localizations, but you can redefine them if you don’t like the names.  
To deal with the underscore you must place **two** in the localization file:  

```json
"unit_abilities_polib__bounded"
```

| ID | Description |
|----|-------------|
| `polib_crush` | *Destroys most improvements upon stepping on them.* (Even yours. Will destroy everything unless the improvement is a city, ruin, lighthouse, or has the `polib_indestructible` ability) |
| `polib_rummager` | *Collects ruins immediately upon stepping on them.* (Without waiting a turn) |
| `polib_blind` | *Can't reveal undiscovered tiles.* |
| `polib_bounded` | *Can not leave the territory of the city it was trained in.* (Can move outside the city bounds via winning attacks, unless unit also has `polib_lazy`) |
| `polib_homesick` | *Can not leave the territory of the tribe it was trained in.* (Works similar to `polib_bounded`, can “teleport” between cities if movement allows) |
| `polib_lazy` | *Does not move to the tile of the defeated unit after combat.* |
| `polib_agent` | *Can only train this unit inside enemy territory.* (Cut Polytopia ability recoded. The AI LOVES doing this if the unit is cheap—they’ll go nuts. Recommended to pair with `independent`.) |
| `polib_scary` | *Units attacked by this unit can't attack next turn.* (They can still move) |
| `polib_loyal` | *This unit can not be converted.* (Press F for Ai-Mo) |
| `polib_cantembark` | *This unit can not embark or move onto water.* |

---

## Example

### Example of an ability system set up:

<div style="display:flex; gap:20px; margin-bottom:30px;">
  <pre style="background:#1e1e1e; color:#dcdcdc; padding:10px; border-radius:6px; flex:1; overflow-x:auto;">
{
  "unitability": {
    "magician": {
      "idx": 1
    }
  }
}
  </pre>
  <div style="flex:1; font-family:sans-serif;">
    <p>First we’re defining a dummy unit ability. You can and should localize this ability so that the player sees what it does. We’re going to tie Fertility Rite, a new ability of the mindbenders, to this dummy unit ability.</p>
  </div>
</div>

<div style="display:flex; gap:20px; margin-bottom:30px;">
  <pre style="background:#1e1e1e; color:#dcdcdc; padding:10px; border-radius:6px; flex:1; overflow-x:auto;">
"unitData": {
  "mindbender": {
    "health": 100,
    "defence": 10,
    "movement": 1,
    "range": 1,
    "attack": 0,
    "cost": 5,
    "unitabilities": [
      "heal",
      "convert",
      "disfr",
      "land",
      "magician"
    ],
    "weapon": 4,
    "promotionLimit": 3,
    "idx": 10
  }
}
  </pre>
  <div style="flex:1; font-family:sans-serif;">
    <p>Then we’re adding the dummy ability to a unit we’d like. This unit will be the only one that can perform the fertility rite. In our case this is the vanilla mindbender.</p>
  </div>
</div>

<div style="display:flex; gap:20px; margin-bottom:30px;">
  <pre style="background:#1e1e1e; color:#dcdcdc; padding:10px; border-radius:6px; flex:1; overflow-x:auto;">
"improvementData": {
  "fertilityrite": {
    "parameters": {
      "sacrifice": "true"
    },
    "result": {
      "resource": "fruit"
    },
    "category": 1,
    "price": 5,
    "requirement": "temple",
    "builtBySpecific": "magician",
    "autoReplace": [ "historic", "polib_fullhealthbuilder" ]
  }
}
  </pre>
  <div style="flex:1; font-family:sans-serif;">
    <p>And finally we create the Fertility Rite improvement. We’ve added <code>builtBySpecific: magician</code>, so only the mindbender can perform it. It has manual, freelance and discrete. It can only be built adjacent to temples, and spawns a fruit on the ground. Costs 5 stars. With <code>polib_fullhealthbuilder</code> only mindbenders…</p>
  </div>
</div>
