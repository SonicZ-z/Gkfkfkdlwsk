# WindUI Script Analysis

> **Source:** [`wind (1).txt`](https://github.com/SonicZ-z/Gkfkfkdlwsk/blob/main/wind%20(1).txt)  
> **Repository:** [`SonicZ-z/Gkfkfkdlwsk`](https://github.com/SonicZ-z/Gkfkfkdlwsk)  
> **Analyzed size:** 16,382 lines / approximately 583 KiB

## 1. What this file is

`wind (1).txt` is a bundled and heavily minified/obfuscated Lua library for Roblox UI creation. Its structure and embedded metadata identify it as a fork/bundle of **WindUI v1.6.61**.

It is primarily a **UI framework**, not a complete standalone script with a user-facing feature or game automation routine. The file ends with `return ac`, where `ac` is the exported `WindUI` library object.

The library can be loaded by a Roblox Lua executor or another environment that provides the required executor APIs:

```lua
local WindUI = loadstring(game:HttpGet(
    "https://raw.githubusercontent.com/SonicZ-z/Gkfkfkdlwsk/main/wind%20%281%29.txt"
))()
```

A typical consumer then creates a window and adds tabs/elements:

```lua
local Window = WindUI:CreateWindow({
    Title = "Example UI",
    Icon = "layout-dashboard",
    Folder = "ExampleUI",
    ToggleKey = Enum.KeyCode.RightControl
})

local Tab = Window:Tab({
    Title = "Main",
    Icon = "home"
})

Tab:Button({
    Title = "Run action",
    Desc = "Runs a callback when clicked",
    Callback = function()
        print("Button clicked")
    end
})
```

## 2. How the script works

### Startup sequence

1. The file creates an internal lazy-loader/cache (`a.load(...)`) for its bundled modules.
2. It obtains Roblox services including `RunService`, `UserInputService`, `TweenService`, `LocalizationService`, and `HttpService`.
3. It downloads and executes an external icon library from:
   `https://raw.githubusercontent.com/Footagesus/Icons/main/Main-v2.lua`
4. It initializes default UI properties, colors, shapes, themes, signals, localization, and helper functions.
5. It initializes the default **Dark** theme and the system language.
6. It creates the global notification/dropdown UI containers.
7. `WindUI:CreateWindow(options)` creates the only allowed window, initializes its folders/config manager, optionally performs key validation, and opens the window.
8. `Window:Tab(options)` creates a tab. The tab receives factory methods for the supported UI elements.
9. Each element constructor creates Roblox `Instance` objects, connects input signals, stores its current value, and returns an element object.
10. Callbacks are invoked through protected calls (`pcall`), so callback errors can be displayed in debug mode instead of immediately breaking the UI.

### Runtime behavior

- UI updates use Roblox tweens and `Heartbeat`/`RenderStepped` where appropriate.
- Elements generally expose `Set`, `Get`, `Lock`, `Unlock`, `SetTitle`, `SetDesc`, `Highlight`, and `Destroy` methods where applicable.
- A window can be dragged, resized, minimized, toggled, made fullscreen, and controlled with a keyboard toggle key.
- The library supports themes, gradients, icons, localization, notifications, popups, dialogs, tags, acrylic mode, and configuration files.
- `Flag` values on elements can be registered with the Config Manager for saving/loading.

## 3. Window options

Pass these fields to `WindUI:CreateWindow({...})`:

| Option | Type | Meaning / default |
|---|---|---|
| `Title` | string | Window title; default `"UI Library"`. |
| `Author` | string | Optional author text in the top bar. |
| `Icon` | string | Icon name or asset identifier. |
| `IconSize` | number | Top-bar icon size; default `22`. |
| `IconThemed` | boolean | Whether the icon follows the theme. |
| `Folder` | string | Folder name used for assets and config files. Strongly recommended for persistence. |
| `Size` | `UDim2` | Initial window size; default `UDim2.new(0, 580, 0, 460)`. |
| `MinSize` | `Vector2` | Minimum size; default `Vector2.new(560, 350)`. |
| `MaxSize` | `Vector2` | Maximum size; default `Vector2.new(850, 560)`. |
| `Resizable` | boolean | Enables resizing; default `true`. Set `false` to disable. |
| `Background` | Color/gradient-compatible value | Window background. |
| `BackgroundImageTransparency` | number | Background image transparency; default `0`. |
| `ShadowTransparency` | number | Shadow transparency; default `0.7`. |
| `Transparent` | boolean | Enables transparent-window behavior. |
| `Acrylic` | boolean | Enables acrylic/blur-style rendering when supported. |
| `ToggleKey` | `Enum.KeyCode` | Key that toggles the window. |
| `OpenButton` | table | Configuration for the floating open button. |
| `SideBarWidth` | number | Sidebar width; default `200`. |
| `ScrollBarEnabled` | boolean | Enables the sidebar scrollbar; default `false`. |
| `HideSearchBar` | boolean | Search bar visibility setting; the source defaults this to `true`. |
| `HidePanelBackground` | boolean | Hides the main panel background. |
| `IgnoreAlerts` | boolean | Suppresses some UI alerts. |
| `AutoScale` | boolean | Enables automatic scaling; default `true`. |
| `Radius` | number | Window corner radius; default `16`. |
| `ElementsRadius` | number | Corner radius used by elements. |
| `NewElements` | boolean | Enables the newer element visual style. |
| `TopBarButtonIconSize` | number | Top-bar button icon size; default `16`. |
| `User` | table | Optional user profile/sidebar data. |
| `Parent` | Instance | Optional Roblox UI parent. |
| `KeySystem` | table | Optional key-validation configuration; see [Key systems](#7-key-systems). |

Useful window methods include:

```lua
Window:Open()
Window:Close()
Window:Toggle()
Window:Destroy()
Window:ToggleFullscreen()
Window:SetTitle("New title")
Window:SetAuthor("Author")
Window:SetToggleKey(Enum.KeyCode.RightShift)
Window:SetBackgroundImage("rbxassetid://123")
Window:SetBackgroundTransparency(0.25)
Window:SetIconSize(20)
Window:LockAll()
Window:UnlockAll()
Window:SelectTab(Tab)
Window:IsResizable(true)
Window:OnOpen(function() end)
Window:OnClose(function() end)
Window:OnDestroy(function() end)
```

## 4. Tabs and layout helpers

### Tab

```lua
local Tab = Window:Tab({
    Title = "Settings",
    Desc = "Configuration controls",
    Icon = "settings",
    IconThemed = true,
    ShowTabTitle = true,
    Locked = false
})
```

Tab options:

- `Title` — tab title.
- `Desc` — optional description.
- `Icon` — icon name or asset.
- `IconThemed` — theme the icon.
- `ShowTabTitle` — show/hide the tab title.
- `Locked` — disable tab selection.

Useful tab methods include `Tab:Select()`, `Tab:Lock()`, `Tab:Unlock()`, `Tab:SetIcon(icon)`, `Tab:Visible(boolean)`, `Tab:Edit(options)`, and `Tab:Destroy()`.

### Other layout methods

- `Window:Section({...})` — adds a sidebar section.
- `Window:Divider()` — adds a sidebar divider.
- `Window:SidebarParagraph({...})` — adds a richer sidebar card with title/content, thumbnail, icon, buttons, badge, and progress bar.
- `Tab:Section({...})` — adds a section inside a tab where supported.
- `Tab:Space()` — inserts spacing.
- `Tab:MultiSection({...})` — creates a multi-section layout where supported.

## 5. Every element that can be added

All element methods are exposed through the tab element loader. The source registers these element types:

`Paragraph`, `Button`, `Toggle`, `Slider`, `Keybind`, `ToggleKeybind`, `ButtonKeybind`, `Input`, `Dropdown`, `Code`, `Colorpicker`, `Section`, `MultiSection`, `Divider`, `Space`, `Image`, and `Label`.

The common fields `Title`, `Desc`, `Icon`, `Locked`, `Callback`, and `Flag` are not accepted identically by every element. Use the per-element fields below.

### 5.1 Paragraph

Displays rich text and can include action buttons.

```lua
Tab:Paragraph({
    Title = "Information",
    Desc = "<b>RichText</b> content is supported.",
    Align = "Left", -- Left, Center, or Right
    Buttons = {
        {
            Title = "Open",
            Icon = "external-link",
            Callback = function()
                print("Open clicked")
            end
        }
    }
})
```

Options: `Title`, `Desc`, `Align`, `Buttons`, `Icon`, `Locked`.

Each button supports `Title`, `Icon`, and `Callback`.

### 5.2 Button

Runs a callback when clicked.

```lua
Tab:Button({
    Title = "Execute",
    Desc = "Run an operation",
    Icon = "play",
    IconThemed = true,
    Color = Color3.fromRGB(80, 160, 255),
    Justify = "Between", -- commonly Between or Center/other supported layout values
    IconAlign = "Right", -- or Left
    Locked = false,
    Callback = function()
        print("Executed")
    end
})
```

Options: `Title`, `Desc`, `Icon`, `IconThemed`, `Color`, `Justify`, `IconAlign`, `Locked`, `Callback`.

Methods: `SetTitle`, `SetDesc`, `Lock`, `Unlock`, `Highlight`, `Destroy`.

### 5.3 Toggle

Boolean on/off control.

```lua
local AutoFarm = Tab:Toggle({
    Title = "Auto farm",
    Desc = "Enable or disable the feature",
    Icon = "zap",
    Value = false,
    Type = "Toggle",
    Callback = function(enabled)
        print("Enabled:", enabled)
    end,
    Flag = "AutoFarm"
})

AutoFarm:Set(true)
local enabled = AutoFarm:Get()
```

Options: `Title`, `Desc`, `Icon`, `Value` (boolean), `Type`, `Locked`, `Callback`, `Flag`.

Methods: `Set(value)`, `Get()`, `Lock()`, `Unlock()`, `SetTitle`, `SetDesc`, `Destroy`.

### 5.4 Checkbox

The source contains checkbox rendering as an internal component. It is used by toggle-style controls; the public element registry exposes `Toggle` rather than a separate `Checkbox` factory.

Use `Toggle` for a public checkbox-like boolean control.

### 5.5 Slider

Numeric control with minimum, maximum, default value, and step size.

```lua
local Volume = Tab:Slider({
    Title = "Volume",
    Desc = "Choose a value from 0 to 100",
    Value = {
        Min = 0,
        Max = 100,
        Default = 50
    },
    Step = 1,
    Callback = function(value)
        print("Volume:", value)
    end,
    Flag = "Volume"
})

Volume:Set(75)
Volume:SetMin(10)
Volume:SetMax(90)
```

Options: `Title`, `Desc`, `Value.Min`, `Value.Max`, `Value.Default`, `Step`, `Locked`, `Callback`, `Flag`.

`Step` may be fractional; fractional values are formatted to two decimal places. Values are clamped to the configured min/max range.

Methods: `Set(value)`, `SetMin(value)`, `SetMax(value)`, `Lock()`, `Unlock()`, `Destroy`.

### 5.6 Keybind

A key selector that invokes a callback when the selected key is pressed.

```lua
Tab:Keybind({
    Title = "Toggle menu",
    Desc = "Choose a keyboard key",
    Value = "F",
    CanChange = true,
    Callback = function()
        print("Key pressed")
    end,
    Flag = "ToggleMenuKey"
})
```

Options: `Title`, `Desc`, `Value` (key name/key value), `CanChange`, `Locked`, `Callback`, `Flag`.

Methods include `Set(value)`, `SetKey(key)`, `Lock()`, `Unlock()`, `Destroy`.

### 5.7 ToggleKeybind

Combines a boolean toggle with a changeable keybind.

```lua
Tab:ToggleKeybind({
    Title = "Feature hotkey",
    Desc = "Enable a feature and choose its key",
    Value = false,
    Key = "G",
    Callback = function(enabled)
        print("Enabled:", enabled)
    end,
    Flag = "FeatureHotkey"
})
```

Options: `Title`, `Desc`, `Value` (boolean), `Key`, `Locked`, `Callback`, `Flag`.

Methods include `Set(value)`, `SetKey(key)`, `Lock()`, `Unlock()`, `Destroy`.

### 5.8 ButtonKeybind

The registry includes `ButtonKeybind`. It is a button action with a keybind-style trigger. Because this bundled file uses internal/minified module names, exact visual options may vary by version; use the same common fields as `Button` plus a key field:

```lua
Tab:ButtonKeybind({
    Title = "Run action",
    Desc = "Click the button or press the key",
    Key = "H",
    Callback = function()
        print("Action")
    end
})
```

Recommended fields: `Title`, `Desc`, `Key`, `Icon`, `Locked`, `Callback`, `Flag`.

### 5.9 Input

Text-entry control.

```lua
local NameInput = Tab:Input({
    Title = "Name",
    Desc = "Enter a value",
    Placeholder = "Enter text...",
    Value = "Default",
    InputIcon = "pencil",
    ClearTextOnFocus = false,
    Type = "Input",
    Callback = function(text)
        print("Text:", text)
    end,
    Flag = "Name"
})

NameInput:Set("New value")
```

Options: `Title`, `Desc`, `Type`, `InputIcon`, `Placeholder`, `Value`, `Callback`, `ClearTextOnFocus`, `Locked`, `Flag`.

The callback receives the entered text. The source also provides `Set`, `Get`, `Lock`, `Unlock`, `SetTitle`, `SetDesc`, and `Destroy` behavior.

### 5.10 Dropdown

Single-select or multi-select list.

```lua
local Mode = Tab:Dropdown({
    Title = "Mode",
    Desc = "Choose a mode",
    Values = {"Safe", "Fast", "Manual"},
    Value = "Safe",
    AllowNone = false,
    SearchBarEnabled = true,
    Multi = false,
    Callback = function(value)
        print("Selected:", value)
    end,
    Flag = "Mode"
})

Mode:Select("Fast")
Mode:Refresh({"Safe", "Fast", "Manual", "Experimental"})
```

Options: `Title`, `Desc`, `Values` (array), `Value`, `AllowNone`, `SearchBarEnabled`, `Multi`, `MenuWidth`, `Callback`, `Locked`, `Flag`.

- With `Multi = false`, `Value` is normally one selected value.
- With `Multi = true`, the value is an array/table of selected values.
- `Display`, `Refresh`, `Select`, `Open`, and `Close` are exposed by the underlying dropdown implementation.

### 5.11 Code

Displays syntax-highlighted code and provides a copy action.

```lua
Tab:Code({
    Title = "Example code",
    Code = 'print("Hello")',
    OnCopy = function()
        print("Copied")
    end
})
```

Options: `Title`, `Code`, `OnCopy`.

The source includes a Lua/Roblox keyword highlighter and copies the code using `toclipboard` when available. Method: `SetCode(code)`.

### 5.12 Colorpicker

Color selection control with optional alpha/transparency.

```lua
local Accent = Tab:Colorpicker({
    Title = "Accent color",
    Desc = "Choose a color",
    Default = Color3.fromRGB(80, 140, 255),
    Transparency = 0, -- optional alpha representation used by the source
    Callback = function(color, transparency)
        print(color, transparency)
    end,
    Flag = "AccentColor"
})

Accent:Set(Color3.fromRGB(255, 80, 80), 0)
```

Options: `Title`, `Desc`, `Default` (`Color3`), `Transparency`, `Callback`, `Locked`, `Flag`.

The picker supports hue, saturation/value, hex input, RGB input, and optional alpha input. Methods include `Set(color, transparency)`, `Update`, `Lock`, `Unlock`, and `Destroy`.

### 5.13 Section

Adds a labeled section/header to organize controls.

```lua
Tab:Section({
    Title = "Combat settings",
    Subtitle = "Options related to combat",
    Icon = "swords",
    TextXAlignment = "Left",
    TextSize = 17,
    Opened = true,
    Box = true
})
```

Options: `Title`, `Subtitle` (or `Desc`), `Icon`, `TextXAlignment`, `TextSize`, `Box`, `FontWeight`, `TextTransparency`, `Opened`.

Methods include `SetTitle`, `SetSubtitle`, `SetIcon`, `Open`, `Close`, and `Destroy` where available.

### 5.14 MultiSection

The element registry includes `MultiSection`, intended to group multiple sections in one layout. Its exact constructor is bundled under an internal module name, so use the same section-style metadata and verify the specific build before relying on version-specific fields.

Recommended fields: `Title`, `Subtitle`/`Desc`, `Icon`, `Sections`, and any child section definitions supported by the installed build.

### 5.15 Divider

Adds a visual separator. It generally does not need options:

```lua
Tab:Divider()
```

### 5.16 Space

Adds blank layout space. Depending on the build, it may accept a size/amount option:

```lua
Tab:Space()
-- or, if supported by the wrapper:
Tab:Space({Size = 10})
```

### 5.17 Image

Displays an image with an aspect-ratio constraint.

```lua
Tab:Image({
    Image = "rbxassetid://123456789",
    AspectRatio = "16:9",
    Radius = 12
})
```

Options: `Image`, `AspectRatio` (for example `"16:9"`), and `Radius`.

Method: `Destroy()`.

### 5.18 Label

Displays a value, optionally with an icon/color, and can be clickable when a callback is supplied.

```lua
local Status = Tab:Label({
    Title = "Status",
    Desc = "Current state",
    Value = "Ready",
    Icon = "check",
    IconThemed = true,
    Color = Color3.fromRGB(80, 200, 120),
    Callback = function(value)
        print("Label clicked:", value)
    end
})

Status:Set("Running")
print(Status:Get())
```

Options: `Title`, `Desc`, `Value`, `Color` (`Color3` or theme color name), `Icon`, `IconThemed`, `Callback`.

Methods: `Set(value)`, `Get()`, `SetTitle`, `SetDesc`, `SetColor`, `Lock`, `Unlock`, `Destroy`.

## 6. Common element lifecycle

Most interactive elements follow this pattern:

```lua
local element = Tab:Toggle({
    Title = "Example",
    Value = false,
    Callback = function(value)
        -- Consumer code runs here.
    end
})

element:Set(true)       -- change its value
-- element:Get()         -- read its value where supported
element:Lock()          -- disable interaction
element:Unlock()        -- enable interaction
element:SetTitle("Updated")
element:SetDesc("Updated description")
element:Destroy()
```

Callbacks are protected by the library's `SafeCallback` helper. In debug mode, callback errors are routed to a WindUI notification.

## 7. Key systems

`CreateWindow` can optionally receive `KeySystem`. The bundled code supports these providers:

- **Platoboost** — service ID and secret; uses `api.platoboost.app`/`.net` endpoints.
- **Panda Development** — service ID; validates through Panda Development endpoints.
- **Luarmor** — script ID and Discord/link data; loads the Luarmor SDK remotely.

The key system may save a valid key under the configured `Folder` and local user identifier when `SaveKey` is enabled. A key system can block window creation until validation succeeds.

Example shape (provider-specific fields must match the provider dashboard):

```lua
local Window = WindUI:CreateWindow({
    Title = "Protected UI",
    Folder = "ProtectedUI",
    KeySystem = {
        SaveKey = true,
        -- Provider-specific configuration goes here.
        -- Key = "..." or API/provider entries are handled by the bundled build.
    }
})
```

**Security note:** this is an external authorization workflow. Do not embed private secrets in publicly shared scripts. Review the provider API and the executor's HTTP permissions before enabling it.

## 8. Themes, notifications, gradients, and popups

The exported library also provides:

```lua
WindUI:Notify({
    Title = "Saved",
    Content = "Your settings were saved.",
    Icon = "check",
    Duration = 3
})

WindUI:SetFont("rbxassetid://...")
WindUI:SetTheme("Dark")
local themes = WindUI:GetThemes()
local current = WindUI:GetCurrentTheme()
local gradient = WindUI:Gradient({
    [0] = {Color = Color3.fromRGB(255, 0, 0)},
    [100] = {Color = Color3.fromRGB(0, 0, 255)}
})
```

Other exported helpers include `SetNotificationLower`, `OnThemeChange`, `AddTheme`, `GetTransparency`, `GetWindowSize`, `Localization`, `SetLanguage`, `ToggleAcrylic`, `Popup`, and `Dialog`.

The default theme fallback maps UI roles such as `Background`, `Text`, `Icon`, `Accent`, `Button`, `Hover`, `Dialog`, `Toggle`, and `Checkbox` to theme values. The source also defines built-in color names: `Red`, `Orange`, `Green`, `Blue`, `White`, and `Grey`.

## 9. Configuration and flags

To persist values:

1. Give the window a `Folder`.
2. Give supported elements a string `Flag`.
3. Create or select a config using the window's `ConfigManager`.
4. Call the config's `Save()` and `Load()` methods.

The bundled parser supports these element types directly:

- `Colorpicker`
- `Dropdown`
- `Input`
- `Keybind`
- `ToggleKeybind`
- `Slider`
- `Toggle`

Config files are written as JSON under a path similar to:

```text
WindUI/<Folder>/config/<ConfigName>.json
```

The config object supports methods including `SetAsCurrent`, `Register`, `Set`, `Get`, `Save`, `Load`, `Delete`, and `GetData`. The source depends on executor filesystem functions such as `isfolder`, `makefolder`, `isfile`, `readfile`, `writefile`, `listfiles`, and `delfile`.

## 10. External dependencies and required environment

The script is not normal Roblox Studio code. It assumes a compatible executor/runtime because it references or conditionally uses:

- `loadstring`
- `request`, `http_request`, or `syn.request`
- `setclipboard`/`toclipboard`
- `gethwid`
- `isfolder`, `makefolder`, `isfile`, `readfile`, `writefile`, `listfiles`, `delfile`
- `gethui`/executor UI-parent behavior in surrounding code

It also performs external HTTP loads for icons and may contact key-system services. In Roblox Studio, many of these functions are unavailable, so the complete script will not run unchanged.

## 11. Important caveats

- The code is obfuscated/minified, so maintenance and debugging are difficult.
- The source dynamically executes code downloaded from GitHub and, depending on options, third-party key SDKs. Inspect and pin dependencies before using it.
- The public repository is a fork and the linked file may change without notice. Pin a commit if reproducibility matters.
- `CreateWindow` warns and refuses to create a second window if one already exists.
- UI assets are cached into local folders when a `Folder` is supplied.
- Some defaults use Lua expressions such as `x or true`, which means a value intended to explicitly disable a feature may still resolve to true in parts of this bundle. Test options such as `Box` and `CanChange` against the exact version.
- `Checkbox`, `MultiSection`, `ButtonKeybind`, and `Space` are present in the internal registry/components, but their bundled constructors are less straightforward than the primary controls. Treat the examples for these as version-sensitive and verify them in a test place.

## 12. Recommended usage checklist

1. Review the source and all remote URLs before executing it.
2. Use a pinned raw GitHub commit instead of the mutable `main` branch for production use.
3. Confirm that the runtime provides HTTP, clipboard, filesystem, and (if needed) HWID functions.
4. Create a unique `Folder` if using cached assets or configs.
5. Add `Flag` only to controls that the bundled Config Manager can serialize.
6. Keep callbacks short or use `task.spawn` for longer operations.
7. Test every UI option in a private Roblox place because this bundle contains version-specific and obfuscated internals.
8. Avoid placing provider secrets or private keys in a public script.

## 13. Source observations

The conclusions above are based on the linked file's implementation, including:

- default theme and UI settings near lines 12–156;
- exported element registry near lines 11,761–11,780;
- window defaults near lines 13,403–13,501;
- public tab/section/layout methods near lines 15,024–15,086;
- exported notification/theme/gradient functions near lines 16,143–16,260;
- `CreateWindow` and key-system flow near lines 16,272–16,381.
