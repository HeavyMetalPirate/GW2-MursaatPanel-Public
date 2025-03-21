# MursaatPanel
Keep your data in view whereever you go!

### What is MursaatPanel?
MursaatPanel will replace your traditional Experience Bar with a more configurable one, allowing you to put all kinds of widgets to display various data around your game play.

![mursaat_panel](https://github.com/user-attachments/assets/f3301233-493e-47ea-a9c1-ee1606553950)

MursaatPanel starts with a bottom panel. In the options you can enable an additional top panel, and hide the bottom panel, to fully customize your experience.

The following widgets are available:
- Level & Experience
- Mastery Levels
- Achievement Points
- Wallet currency & Gold
- Equipment & Gear Templates in use
- Location with sectorname and coordinates
- Inventory Bag space
- System stats (FPS, Ping, Memory Usage)
- World vs. World status
  - Skirmish & Match scores
  - K/D ratio
  - Participation timer
  - Reward Track
  - Pips Target Calculator
- Map & World Completion 

Something you want to see is missing? Post a suggestion on the Raidcore.gg Discord: https://discord.gg/raidcore

# Installing MursaatPanel
MursaatPanel can be installed via the Nexus addon library. You can also download the DLL from the most recent release and drop it into your `<gw2install>/addons` folder.

MursaatPanel will store its settings under `<gw2install>/addons/MursaatPanel`.

**Important:** MursaatPanel alters the way some of the native UI elements are displayed. To reflect all the changes the addon makes, a full UI refresh is needed. The easiest way to trigger this is by loading into a different map. The same applies to whenever you unload MursaatPanel, the UI will only fully restore after a full refresh.

# Using MursaatPanel
After installation, you will see an empty panel at the bottom. In the options you can configure an additional top panel.

By right clicking on either of the panels, a context menu opens that allows you to add widgets for the respective categories.

After adding a widget, you can customize its appearance by right clicking on the widget. You can set the panel, alignment and order priority, as well as hide it again. Some widgets may allow for further customization.

# FAQ & Known Issues

### After installing, the original experience bar is visible behind the panel, why?
The game renderer updates the entire experience bar every now and then instead of during each frame. Once drawn, the experience bar will remain in its state until another update happens.
This means that after installing, you will see the original experience bar until you change map (or another thing triggers a full refresh).

### After uninstalling, the original experience bar remains hidden, why?
Same as above, the experience bar will continue its working on a full refresh, usually triggered by a map change.

### I am using the Top Panel. The icons for Nexus and BlishHUD are still rendered on top.
For Nexus: in the Nexus options you can configure an offset. Ideally you set the Y-Offset to whatever your bar height is set to.

For BlishHUD: at this time I can give no support due to me not using the overlay. The best course might be engaging with Delta and the Blish-Developers directly to make the NexusShim-Module take into account the offset settings as well.

### Widgets are being weirdly positioned; What gives?
The addon was developed with 1080p, small UI scaling and a Nexus scaling of 1.0 in mind. Other factors might be UI customizations like margins or paddings that are not accounted for. Another thing might be font and font size.
MursaatPanel offers some settings to help with general appearance, but is far from perfect on resolving everything. You can help improving MursaatPanel by reporting any weird rendering issues on the Raidcore.gg Discord!

### Is the addon safe to use after a game update?
Generally speaking, there are some precautionary measures in place to help not breaking whenever Guild Wars 2 updates. However, sometimes things will change around a lot. The addon will log every failed interaction with the game to the Nexus log.
If you encounter any issues, please report them using the log. You can find it here: `<gw2install>/addons/Nexus/Nexus.log`

# Disclaimer
The addon accesses data and functions by Guild Wars 2 using information in memory. As such it has to remain closed source.
All sources have been reviewed by Raidcore.gg and are found to be in accordance with the [Raidcore.gg Addon Policy](https://discord.com/channels/410828272679518241/1259477034959114341/1259544667670712382).

The ArenaNet [Terms of Service](https://www.arena.net/en/legal) and [Third Party Programs Policy](https://help.guildwars2.com/hc/en-us/articles/360013625034-Policy-Third-Party-Programs) apply in their current version.
