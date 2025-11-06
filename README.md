# Auto_Quantification
Automated Quantification creator for Autodesk Navisworks

Autodesk University session (Russian): https://youtu.be/FROfd0oOZe4

## Installation (Navisworks Manage 2025)

1. Download the latest release from https://github.com/Babinoff/Auto_Quantification/releases.
2. Extract the archive and copy the files to the Navisworks plugin directory:
   * `C:\Program Files\Autodesk\Navisworks Manage 2025\Plugins\SearchSetsToQuantification2\SearchSetsToQuantification2.dll`
   * `C:\Program Files\Autodesk\Navisworks Manage 2025\Plugins\SearchSetsToQuantification2\ru-RU\SearchSetsToQuantification2.xaml`
   * `C:\Program Files\Autodesk\Navisworks Manage 2025\Plugins\SearchSetsToQuantification2\Images\q32.png`
   * `C:\Program Files\Autodesk\Navisworks Manage 2025\Plugins\SearchSetsToQuantification2\Images\q16.png`

> The plugin folder may need to be created manually the first time. Keep the folder names exactly as shown so Navisworks can locate the ribbon layout and icons.

## Using the plugin

1. Launch **Navisworks Manage 2025** and open a model containing the search sets you want to quantify.
2. In the **Selection Tree**, organise the search sets that should become Quantification items inside a folder. Remember the folder name— the command will ask for it.
3. Open the **Quantification** workspace (Home tab → Workspaces → Quantification) if it is not already visible.
4. Switch to the **Авто Q** ribbon tab and click **Генерация Quantification**. The plugin appears as a toggle button with the Auto Q icon.
5. When prompted, enter the exact name of the search-set folder prepared in step 2 and press **OK**.
6. Wait for the confirmation dialog. The plugin copies the structure of the search sets into the Quantification workbook and creates takeoff items for every matched model item. Processing progress is written to `%TEMP%\NW_QUNTIFICATION_LOG`.

If the command reports an error, inspect `%TEMP%\NW_QUNTIFICATION_LOG\FAIL_LOG_Navisworks_SearchsToQuantification.log` for details.

## Building for other Navisworks versions

When building from source, adjust the `NavisworksInstallDir` property in `SearchSetsToQuantification2.csproj` so that the post-build step copies the add-in to the correct Navisworks installation folder.

### Where do the Autodesk *.dll files come from?

The assemblies referenced by the project—`AdWindows.dll`, `Autodesk.Navisworks.Api.dll`, `Autodesk.Navisworks.ComApi.dll`, and `Autodesk.Navisworks.Takeoff.dll`—are part of the Navisworks installation and are not distributed in this repository. Install **Navisworks Manage 2025** (or the specific version you plan to target) and point `NavisworksInstallDir` to its installation directory, for example:

```
<NavisworksInstallDir>C:\Program Files\Autodesk\Navisworks Manage 2025\</NavisworksInstallDir>
```

After Navisworks is installed, the required DLLs will be available inside that folder. Visual Studio will load them from there when you build the project. If you only need the API assemblies without a full install, Autodesk also ships them with the Navisworks SDK that can be downloaded from your Autodesk Account alongside the Navisworks installers.
