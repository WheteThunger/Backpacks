## Features

Allows players to have backpacks that provide them with extra inventory space.

- Customizable capacity per player (using permissions)
- Option to drop or erase contents on death
- Option to clear on map wipe
- Optional item restrictions
- Optional GUI button to access the backpack

**Note:** To bind a key to open the backpack, use: `bind <key> backpack.open` in your F1 client console. For example, `bind b backpack.open`.

## Commands

- `backpack` / `backpack.open` -- Opens your own backpack. Requires the `backpacks.use` permission. If the backpack is already open, this will advance to the next page, or will close the player inventory if there are no more pages.
- `backpack.fetch <item short name or id> <amount>` -- Fetches an item from your backpack.
- `backpackgui` -- Toggles whether you can see the backpack GUI button.

## Admin commands

- `viewbackpack <name or steam id>` -- open another player's backpack (requires `backpacks.admin` permission)

## Server commands

- `backpack.erase <id>` -- forcibly erase the contents of a specific player's backpack

## Permissions

- `backpacks.use` -- Required to open your own backpack.
- `backpacks.admin` -- Required to use the `/viewbackpack` command.
- `backpacks.gui` -- Required to see the GUI button.
- `backpacks.fetch` -- required to use the `backpack.fetch` command.
- `backpacks.keepondeath` -- Exempts players from having their backpack erased or dropped on death.
- `backpacks.keeponwipe` -- Exempts players from having their backpack erased on map wipe.

### Size permissions

If you want to grant specific players or groups more backpack capacity than the default (`Backpack size` -> `Default size`), then you may do so via permissions. Each number present in the `Backpack size` -> `Permission sizes` config option will cause the plugin to generate a permission of the format `backpacks.size.<number>`, which assigns the corresponding players or groups that much capacity. For example, `backpacks.size.18` would assign 18 slots of capacity (3 rows).

The following permissions come with the plugin's **default configuration**.
 
- `backpacks.size.6` -- 1 rows
- `backpacks.size.12` -- 2 rows
- `backpacks.size.18` -- 3 rows
- `backpacks.size.24` -- 4 rows
- `backpacks.size.30` -- 5 rows
- `backpacks.size.36` -- 6 rows
- `backpacks.size.42` -- 7 rows
- `backpacks.size.48` -- 8 rows

Additional permissions may be defined by simply adding them to the `Backpack size` -> `Permission sizes` config option and reloading the plugin.

Note: If a player is granted multiple size permissions, the highest will apply.

### Item restriction permissions

If you want to allow backpacks of specific players or groups to accept different items than the default (`Item restrictions` -> `Default ruleset`), then you may do so via permissions. Each ruleset defined in the `Item restrictions` -> `Rulesets by permission` config option will cause the plugin to generate a permission of the format `backpacks.restrictions.<name>`, which assigns that ruleset to the corresponding players or groups.

The following permissions come with the plugin's **default configuration**.

- `backpacks.restrictions.allowall` -- Allows all items in the player's backpack. Only useful if you have customized the default ruleset to restrict items. 

### Legacy permissions

- `backpacks.use.1 - 8` -- These permissions will be generated when the `"Enable legacy backpacks.use.1-8 row permissions": true` config option is present, which will be automatically added to your config when upgrading from a previous version of the plugin, if you have the `"Backpack Size (1-8 Rows)"` config option present at that time. These permissions function like `backpacks.size.*` but assign the specified number of rows rather than number of slots.
- `backpacks.noblacklist` -- This permission is present when the `"Enable legacy noblacklist permission": true` config option is present, which will be automatically added to your config when upgrading from a previous version of the plugin, if you have the `"Use Whitelist (true/false)": true` or `"Use Blacklist (true/false)": true` config options present at that time. This permission exempts players from item restrictions, allowing any item to be placed in their backpack.

## Configuration

Default configuration:

```json
{
  "Backpack size": {
    "Default size": 6,
    "Max size per page": 48,
    "Permission sizes": [
      6,
      12,
      18,
      24,
      30,
      36,
      42,
      48
    ]
  },
  "Drop on Death (true/false)": true,
  "Erase on Death (true/false)": false,
  "Clear Backpacks on Map-Wipe (true/false)": false,
  "Minimum Despawn Time (Seconds)": 300.0,
  "GUI Button": {
    "Enabled by default (for players with permission)": true,
    "Skin Id": 0,
    "Image": "https://i.imgur.com/CyF0QNV.png",
    "Background Color": "0.969 0.922 0.882 0.035",
    "GUI Button Position": {
      "Anchors Min": "0.5 0.0",
      "Anchors Max": "0.5 0.0",
      "Offsets Min": "185 18",
      "Offsets Max": "245 78"
    }
  },
  "Softcore": {
    "Reclaim Fraction": 0.5
  },
  "Item restrictions": {
    "Enabled": false,
    "Default ruleset": {
      "Allowed item categories": [
        "All"
      ],
      "Disallowed item categories": [],
      "Allowed item short names": [],
      "Disallowed item short names": [],
      "Allowed skin IDs": [],
      "Disallowed skin IDs": []
    },
    "Rulesets by permission": [
      {
        "Name": "allowall",
        "Allowed item categories": [
          "All"
        ],
        "Disallowed item categories": [],
        "Allowed item short names": [],
        "Disallowed item short names": [],
        "Allowed skin IDs": [],
        "Disallowed skin IDs": []
      }
    ]
  }
}
```

### Backpack size

- `Backpack size`
  - `Default size` (Default: `6`) -- Determines the capacity (in slots) of backpacks for players who have the `backpacks.use` permission. Players who have the `backpacks.size.<number>` permissions may have greater capacity.
  - `Max size per page` (Default: `48`, Max: `48`) -- Determines the capacity (in slots) per backpack page. For example, if you grant a player `60` backpack capacity, and set the max size per page to `48`, their backpack will have two pages: one page with `48` capacity, and another page with `12` capacity.
  - `Permission sizes` -- Each number in this list generates a permission of the format `backpacks.size.<number>`. Granting that permission to a player or group assigns that much capacity to their backpack. This is useful if you want some players to have more capacity than the above default. Note: If a player is granted multiple size permissions, the highest will apply.

### Item restrictions

- `Item restrictions`
  - `Enabled (true/false)` -- Determines whether player backpacks are subject to item restrictions. Set to `false` to disable item restrictions for all players. Set to `true` to make the below rulesets will apply. Note: Regardless of these settings, other plugins can prevent specific items from being added to Backpacks using the `CanBackpackAcceptItem` hook.
  - `Default ruleset` -- The default ruleset applies to all players' backpacks, except for players who have been granted `backpacks.restrictions.<name>` permissions (which are generated via `"Rulesets by permission"` below).
    - `Allowed item categories` -- Determines which item categories are allowed in backpacks that are assigned this ruleset, **in addition** to any allowed item short names and skin IDs.
      - If you want to allow only specific item short names, leave this option blank (`[]`) and instead add those item short names to `"Allowed item short names"`.
      - If you want to allow most items, with specific exemptions, set this option to `"Allowed item categories": ["All"]`, then use the `"Disallowed item categories"`, `"Disallowed item short names"`, and/or `"Disallowed skin IDs"` options.
      - Allowed values: `"All"`, `"Ammunition"`, `"Attire"`, `"Common"`, `"Component"`, `"Construction"`, `"Electrical"`, `"Favourite"`, `"Food"`, `"Fun"`, `"Items"`, `"Medical"`, `"Misc"`, `"Resources"`, `"Search"`, `"Tool"`, `"Traps"`, `"Weapon"`.
    - `Disallowed item categories` -- Determines which item categories are disallowed in backpacks that are assigned this ruleset.
      - The only correct way to use this option is by setting `"Allowed item categories": ["All"]` and using this option to specify categories that you want to disallow.
      - If an item is in a disallowed category, that item may still be allowed if it's short name is specified in `"Allowed item short names"`, or if it's skin ID is specified in `"Allowed skin IDs"`.
    - `Allowed item short names` -- Determines which item short names are allowed in backpacks that are assigned this ruleset, **in addition** to any item categories and skin IDs that you have allowed.
      - Item short names are evaluated with higher priority than item categories, meaning you can use this option to allow items that are in disallowed categories.
    - `Disallowed item short names` -- Determines which item short names are disallowed in backpacks that are assigned this ruleset.
      - This option has no effect if `"Allowed item categories"` is empty.
      - If an item has a disallowed short name, that item may still be allowed if it's skin ID is specified in `"Allowed skin IDs"`. 
    - `Allowed skin IDs` -- Determines which item skin IDs are allowed in backpacks that are assigned this ruleset, **in addition** to any item categories or short names that you have allowed.
      - Item skin IDs are evaluated with higher priority than item categories and item short names, meaning you can use this option to allow items that are in disallowed categories or that have disallowed short names.
    - `Disallowed skin IDs` -- Determines which item skin IDs are disallowed in backpacks that are assigned this ruleset.
      - This option has no effect if both `Allowed item categories` and `Allowed item short names` are blank.
      - If an item has a disallowed skin ID, it will not be allowed under any circumstances, even if that item is in an allowed category or has an allowed short name.
  - `Rulesets by permission` -- Each ruleset in this list generates a permission of the format `backpacks.restrictions.<name>`. Besides the `Name` option, the rest of the options are the same as for `"Default ruleset"`.

Example rulesets:

```json
"Rulesets by permission": [
  {
    "Name": "no_fun_except_trumpet",
    "Allowed item categories": [
      "All"
    ],
    "Disallowed item categories": [
      "Fun"
    ],
    "Allowed item short names": [
      "fun.trumpet"
    ],
    "Disallowed item short names": [],
    "Allowed skin IDs": [],
    "Disallowed skin IDs": []
  },
  {
    "Name": "no_c4",
    "Allowed item categories": [
      "All"
    ],
    "Disallowed item categories": [],
    "Allowed item short names": [],
    "Disallowed item short names": [
      "explosive.timed"
    ],
    "Allowed skin IDs": [],
    "Disallowed skin IDs": []
  },
  {
    "Name": "only_food_but_no_apples",
    "Allowed item categories": [
      "Food"
    ],
    "Disallowed item categories": [],
    "Allowed item short names": [],
    "Disallowed item short names": [
      "apple",
      "apple.spoiled"
    ],
    "Allowed skin IDs": [],
    "Disallowed skin IDs": []
  }
]
```

### GUI Button

- `GUI Button` -- Determines the display of the GUI button which players can click on to open and close their backpack.
  - `Enabled by default (for players with permission)` -- Default: `true`.
  - `Skin Id` (Default: `0`) -- Determines the skin ID used to display the GUI button, as an alternative to the `Image` URL.
  - `Image` (Default: `"https://i.imgur.com/CyF0QNV.png"`) -- Determines the URL of the image to display on the GUI button, as an alternative to `Skin Id`. 
  - `Background Color` -- Default: `"0.969 0.922 0.882 0.035"`.
  - `GUI Button Position` -- Determines the position and size of the button.
    - `Anchors Min` -- Default: `"0.5 0.0"` (bottom center of screen). Don't change this unless you know what you are doing.
    - `Anchors Max` -- Default: `"0.5 0.0"` (bottom center of screen). Don't change this unless you know what you are doing.
    - `Offsets Min` -- Determines the bottom-left position of the button. Default: `"185 18"` (right side of belt).
    - `Offsets Max` -- Determines the top-right position of the button. Default: `"245 78"` (right side of belt).

Alternative backpacks images:

- [Right-side](https://i.imgur.com/h1HQEAB.png)
- [Left-side](https://i.imgur.com/wLR9Z6V.png)
- [Universal](https://i.imgur.com/5RE9II5.png)

### Miscellaneous options

- `Drop on Death (true/false)` (Default: `true`) -- While `true`, the contents of players' backpacks will be dropped in a backpack by their corpse when they die. When the player respawns, their backpack will be empty, but can find their dropped backpack to recover its contents. Players with the `backpacks.keepondeath` permission will keep their backpack contents on death.
  - Note: Even while this option is enabled, players with the `backpacks.keepondeath` permission will keep their backpack contents on death.
  - Note: Even if dropping backpacks is disabled, other plugins such as Raidable Bases may forcibly drop the player's backpack if configured to do so, via the `API_DropBackpack` API.
- `Erase on Death (true/false)` (Default: `false`) -- While `true`, the contents of players' backpacks will be erased when they die. Erased backpack contents cannot be recovered under any circumstances. Players with the `backpacks.keepondeath` permission will keep their backpack contents on death.
  - Note: Even while this option is enabled, players with the `backpacks.keepondeath` permission will keep their backpack contents on death.
- `Clear Backpacks on Map-Wipe (true/false)` (Default: `false`) -- While `true`, the contents of all players' backpacks will be wiped when the map is wiped.
  - Note: Even while this option is enabled, players with the `backpacks.keeponwipe` permission will keep their backpack contents on map wipe.
  - Note: In order for this feature to work, Oxide must be installed and the plugin must be loaded when the server boots for the first time with the new map. In rare cases, server owners make the mistake of booting their server after a Rust update without Oxide installed or with the plugin out of date (e.g., failing to compile), causing backpacks to not be wiped. If you make this mistake, it is advised that you wipe your server again by deleting your server save file and restarting the server again, because other plugins that rely on detecting map wipes would have also been affected. Alternatively, you may manually wipe all backpacks by unloading the plugin, deleting the `oxide/data/Backpacks` directory, then loading the plugin.
- `Minimum Despawn Time (Seconds)` (Default: `300.0`) -- Determines the minimum time (in seconds) that dropped backpacks will be protected from despawning. If the backpack contents are moderately rare, as determined by vanilla Rust, the backpack may take longer to despawn than this duration.

## Localization

```json
{
  "No Permission": "You don't have permission to use this command.",
  "May Not Open Backpack In Event": "You may not open a backpack while participating in an event!",
  "View Backpack Syntax": "Syntax: /viewbackpack <name or id>",
  "User ID not Found": "Could not find player with ID '{0}'",
  "User Name not Found": "Could not find player with name '{0}'",
  "Multiple Players Found": "Multiple matching players found:\n{0}",
  "Backpack Items Rejected": "Your backpack rejected some items. They have been added to your inventory or dropped.",
  "Backpack Over Capacity": "Your backpack was over capacity. Overflowing items were added to your inventory or dropped.",
  "Blacklisted Items Removed": "Your backpack contained blacklisted items. They have been added to your inventory or dropped.",
  "Backpack Fetch Syntax": "Syntax: backpack.fetch <item short name or id> <amount>",
  "Invalid Item": "Invalid Item Name or ID.",
  "Invalid Item Amount": "Item amount must be an integer greater than 0.",
  "Item Not In Backpack": "Item \"{0}\" not found in backpack.",
  "Items Fetched": "Fetched {0} \"{1}\" from backpack.",
  "Fetch Failed": "Couldn't fetch \"{0}\" from backpack. Inventory may be full.",
  "Toggled Backpack GUI": "Toggled backpack GUI button."
}
```

## FAQ

#### Why are backpacks not dropping when players die?

There are three possible reasons backpacks won't drop.

1. The configuration option `"Drop on Death (true/false)"` is `false`.
2. Players have the `backpacks.keepondeath` permission.
3. Another plugin prevented the backpack from dropping via the `CanDropBackpack` hook. Arena plugins will often do this for players in the arena.

#### How do I move the backpack button to the left side of the hotbar?

Replace the `"GUI Button Position"` section of the plugin configuration with the following.

```json
"GUI Button Position": {
  "Anchors Min": "0.5 0.0",
  "Anchors Max": "0.5 0.0",
  "Offsets Min": "-265 18",
  "Offsets Max": "-205 78"
}
```

## Known limitations

Paintable entities, photos, pagers, mobile phones, and cassettes will lose their data on map wipe. There are currently no plans to persist this data across wipes, but such a feature can be considered upon request.

## Developer API

### API_GetBackpackContainer

```csharp
ItemContainer API_GetBackpackContainer(ulong backpackOwnerID)
```

Returns a reference to the underlying `ItemContainer` of a player's backpack. Returns `null` if the player essentially has no backpack (no data file and no backpack in-memory).

Notes:
- This will create the container entity if it doesn't exist. This can add load to the server, so it's recommended to use this API only if the other API methods do not meet your needs. For example, if you want to know only the quantity of an item in the player's backpack, you can use `API_GetBackpackItemAmount` which can count the items without creating the container.
- You should avoid caching the container because several events may cause the backpack's underlying container to be replaced or deleted, which would make the cached reference useless.

### API_GetBackpackItemAmount

```csharp
int API_GetBackpackItemAmount(ulong backpackOwnerID, int itemId, ulong skinId = 0)
```

Returns the quantity of a given item in the player's backpack. Returns `0` if the player has no backpack. This API is more performant than `API_GetBackpackContainer` because it does not require creating the backpack container.

### API_DropBackpack

```csharp
DroppedItemContainer API_DropBackpack(BasePlayer player, List<DroppedItemContainer> collect = null)
```

Drop the player's backpack at their current position. This can be used, for example, to only drop the player's backpack when they die in a PvP zone.

If the `collect` list is provided, every dropped container for the backpack will be added to that list. This is useful if the backpack has multiple pages.

Note: This intentionally ignores the player's `backpacks.keepondeath` permission in order to provide maximum flexibility to other plugins, so it's recommended that other plugins provide a similar permission to allow exemptions.

### API_EraseBackpack

```csharp
void API_EraseBackpack(ulong backpackOwnerID)
```

Erases the contents of a specific player's backpack.

Note: This cannot be blocked by the `CanEraseBackpack` hook.

### API_GetBackpackOwnerId

```csharp
ulong API_GetBackpackOwnerId(ItemContainer container)
```

- Returns the Steam ID of the backpack owner if the `ItemContainer` is a backpack.
- Returns `0` if the `ItemContainer` is **not** a backpack.

### API_GetExistingBackpacks

```csharp
Dictionary<ulong, ItemContainer> API_GetExistingBackpacks()
```

Returns all backpack containers that are cached in the plugin's memory, keyed by the Steam IDs of the backpack owners. This was originally contributed so that item cleaner plugins could determine which items were in backpacks in order to ignore them. However, as of Backpacks v3.7.0, all item cleaner plugins should automatically be compatible if they verify that the container has a valid `entityOwner`.

### API_TryOpenBackpack

```csharp
object API_TryOpenBackpack(BasePlayer player, ulong backpackOwnerID = 0, ItemContainer container = null)
```

- Makes the specified player open the specified owner's backpack.
- If `ownerId` is `0`, the player will open their own backpack.
- Returns `true` if successful.
- Returns `false` if unsuccessful.

## Developer Hooks

### CanOpenBackpack

```csharp
string CanOpenBackpack(BasePlayer player, ulong backpackOwnerID)
```

Called when a player tries to open a backpack.
Returning a string will cancel backpack opening and send the string as a chat message to the player trying to open the backpack.

### OnBackpackOpened

```csharp
void OnBackpackOpened(BasePlayer player, ulong backpackOwnerID, ItemContainer backpackContainer)
```

- Called when a player successfully opens a backpack.
- No return behaviour.

### OnBackpackClosed

```csharp
void OnBackpackClosed(BasePlayer player, ulong backpackOwnerID, ItemContainer backpackContainer)
```

- Called when a player closes a backpack.
- No return behaviour.

### CanBackpackAcceptItem

```csharp
bool CanBackpackAcceptItem(ulong backpackOwnerID, ItemContainer backpackContainer, Item item)
```

- Called when a player tries to move an item into a backpack.
- Returning `false` prevents the item being moved.

### CanDropBackpack

```csharp
bool CanDropBackpack(ulong backpackOwnerID, Vector3 position)
```

- Called when a player dies while the `"Drop on Death (true/false)"` option is set to `true`.
- Returning `false` prevents the backpack from dropping.

### CanEraseBackpack

```csharp
bool CanEraseBackpack(ulong backpackOwnerID)
```

- Called when a player dies while the `"Erase on Death (true/false)"` option is set to `true`.
- Returning `false` prevents the backpack from being erased.
