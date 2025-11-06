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

## Building from source

1. Install **Visual Studio 2022** (or newer) with the *.NET desktop development* workload so that MSBuild 17 and the .NET Framework 4.7.2 targeting pack are available.
2. Clone this repository and open `SearchSetsToQuantification2.sln` in Visual Studio.
3. Make sure **Navisworks Manage 2025** (or the version you target) is installed locally so the Autodesk API assemblies are present under `C:\Program Files\Autodesk\Navisworks Manage 2025`.
4. If Navisworks is installed to a different folder, override the `NavisworksInstallDir` property either by editing `SearchSetsToQuantification2.csproj` or by passing `/p:NavisworksInstallDir="D:\Path\To\Navisworks\"` when building with MSBuild.
5. Choose the **Debug_2025 | x64** configuration in Visual Studio. Press **Build → Build Solution**. The project compiles against the Navisworks API DLLs located in `$(NavisworksInstallDir)`.
6. After a successful build, the post-build event copies the add-in to `$(NavisworksInstallDir)Plugins\SearchSetsToQuantification2\`. Restart Navisworks (or launch it with **F5**) to load the updated plugin.

If the automatic copy fails because of permissions, build once to produce the binaries in `SearchSetsToQuantification2\bin\x64\Debug_2025\` and manually copy the DLL, XAML, and `Images` folder to `$(NavisworksInstallDir)Plugins\SearchSetsToQuantification2\`.

### Where do the Autodesk *.dll files come from?

The assemblies referenced by the project—`AdWindows.dll`, `Autodesk.Navisworks.Api.dll`, `Autodesk.Navisworks.ComApi.dll`, and `Autodesk.Navisworks.Takeoff.dll`—are part of the Navisworks installation and are not distributed in this repository. Install **Navisworks Manage 2025** (or the specific version you plan to target) and point `NavisworksInstallDir` to its installation directory, for example:

```
<NavisworksInstallDir>C:\Program Files\Autodesk\Navisworks Manage 2025\</NavisworksInstallDir>
```

After Navisworks is installed, the required DLLs will be available inside that folder. Visual Studio will load them from there when you build the project. If you only need the API assemblies without a full install, Autodesk also ships them with the Navisworks SDK that can be downloaded from your Autodesk Account alongside the Navisworks installers.
