# Married in Red - Web Port

This is an **Unofficial Web Port** of the free RPG Maker MV horror game [**Married in Red by STUDIO INVESTIGRAVE**](https://racheldrawsthis.itch.io/married-in-red). This port allows the game to be played directly in a web browser without needing any downloads/installations.

## How to play

*Fullscreen is recommended!*

There are 2 methods:

### Github Pages
Go to https://nick088official.github.io/Married-in-Red-Web-Port/www/

### Local Server
1. Click Code -> Download ZIP -> Extract the Zip.
2. [Get Python](https://www.python.org/downloads/) installed and added to your system's PATH.
3. Open a CMD/Terminal **inside the root game folder** (the one that contains the `www` folder).
4.  Run the following command to start a simple web server:
    ```bash
    python -m http.server
    ```
5.  Open your web browser and go to the following address:
    **http://localhost:8000/www/**


## How did you make it?

This guide provides step-by-step instructions for modifying the game to run correctly in a web browser.

### Prerequisites

1. **Get the Game Files:** [Download the original game from the creator's itch.io page](https://racheldrawsthis.itch.io/married-in-red) and extract the files.
2. **A Text Editor:** To modify files (e.g., VS Code, Sublime Text, Notepad).
3. **Python (for running):** Having [Python](https://www.python.org/downloads/) installed and added to your system's PATH, to run the local web server.
4. **FFmpeg (Optional, for mobile audio):** For the mobile audio fix, you will need [FFmpeg](https://ffmpeg.org/download.html) installed and added to your system's PATH.


### Part A: Remove Unused Files

1. Delete everything else except the `www` folder in the root game folder.
2. Inside the `www` folder, you can delete the `package.json`, `RPG Maker MV.url` and `supertoolsengine.html`.

### Part B: Core Browser Compatibility Fix

There's a preload js plugin that is used for improving performance, but some parameters makes it load files that don't exist, which doesn't matter for the game contained browser but cause issues when running it in a normal browser.

1. Navigate to the `www/js/` folder and open **`plugins.js`** with a text editor.
2. Search for `{"name":"Aetherflow_PreloadEverything","status":true,"description":"v1.0.2 Preload All The Things. (IF YOU HAVE v1.6.1)","parameters":{"ReleaseMapChange":"true","PreloadSystem":"true","ImageCacheSize":"10 * 1000 * 1000","AudioCacheSize":"20 * 1000 * 1000","PreloadGlobal":"{\"Images\":\"[\\\"layers/maplowercolor\\\",\\\"characters/$rodywalkingwine5%(5)\\\",\\\"characters/$rodywalking6%(5)\\\",\\\"characters/$chefcooking2%(4)\\\",\\\"characters/$chefcooking3%(4)\\\",\\\"characters/$chefcooking4%(4)\\\",\\\"characters/$chefcooking5%(4)\\\",\\\"menu/SumRndmDde/$test\\\"]\",\"Audio\":\"\"}","PreloadMaps":""}},`.
3. Replace `"PreloadGlobal":"{\"Images\":\"[\\\"layers/maplowercolor\\\",\\\"characters/$rodywalkingwine5%(5)\\\",\\\"characters/$rodywalking6%(5)\\\",\\\"characters/$chefcooking2%(4)\\\",\\\"characters/$chefcooking3%(4)\\\",\\\"characters/$chefcooking4%(4)\\\",\\\"characters/$chefcooking5%(4)\\\",\\\"menu/SumRndmDde/$test\\\"]\",\"Audio\":\"\"}"` with `"PreloadGlobal":""`.
4. Save the file.

### Part C: Mobile Audio Compatibility Fix (Optional)

On mobile, the game tries to load `.m4a` audio files, but the game only contains `.ogg` files, causing a crash. So you have to create the `.m4a` audio files for maximum compatibility.

1. Ensure you have **FFmpeg installed and added to your system's PATH**.
2. Open a CMD/Terminal in your **root game folder** (the one containing the `www` folder).
3. Paste and run the following one-liner command. It will find every `.ogg` file and create an `.m4a` copy next to it:
- For Windows:
    ```cmd
    for /R "www\audio" %F in (*.ogg) do ffmpeg -i "%F" -v quiet -stats "%~dpnF.m4a"
    ```
- For macOS & Linux:
    ```bash
    find www/audio -type f -name "*.ogg" -exec sh -c 'ffmpeg -i "$0" -v quiet -stats "${0%.ogg}.m4a"' {} \;
    ```

### Part D: Fix Game Scaling on Mobile (Optional)

You need to modify a to your `index.html` file to ensure proper game scaling on mobile.

1. Open `www/index.html` in your text editor.
2. Before the `</head>` tag, Replace the previous `<meta name="viewport"...` with:
    ```html
    <!-- This tag ensures the game scales correctly on mobile devices -->
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    ```

### Part E: Running the Game Locally

Once you have made the modifications, you need to run them using a local web server. Simply opening the `index.html` file directly will not work due to browser security restrictions.

1. Open a CMD/Terminal **inside the root game folder** (the one that contains the `www` folder).
2. Run the following command to start a simple web server:
    ```bash
    python -m http.server
    ```
3. Open your web browser and go to the following address:
    **http://localhost:8000/www/**

The game should now start and be fully playable.

### Part F: Fix Problematic Filenames for Web Servers (Optional)

While the game works on a local server after the previous fixes, some image files have names with special characters (like `$` or `%()`) and inconsistent capitalization. Local Windows servers can handle this, but web servers like GitHub Pages are much stricter and case-sensitive. This can lead to "Failed to load" errors for specific images when the game is hosted online.

The solution is to rename these files to a web-safe format and then update the game's events to use these new names.

1. This one-liner script will automatically find and rename all image files with problematic characters to a safe format (e.g., `$Boksu%(5).png` becomes `$Boksu_5.png`). Open a Powershell/Terminal in your **root game folder** and run:
    - For Windows (in PowerShell):
        ```powershell
        Rename-Item -Path 'www\img\characters\$Boksu%(5).png' -NewName '$Boksu_5.png'; Rename-Item -Path 'www\img\characters\$BoksuBloody%(5).png' -NewName '$BoksuBloody_5.png'; Rename-Item -Path 'www\img\characters\$BoksuCoatOff%(5).png' -NewName '$BoksuCoatOff_5.png'; Rename-Item -Path 'www\img\characters\$BoksuCoatOffLighting%(5).png' -NewName '$BoksuCoatOffLighting_5.png'; Rename-Item -Path 'www\img\characters\$BoksuWhite%(5).png' -NewName '$BoksuWhite_5.png'; Rename-Item -Path 'www\img\characters\$BoksuWhiteLighting%(5).png' -NewName '$BoksuWhiteLighting_5.png'; Rename-Item -Path 'www\img\characters\$KillAnimation%(8).png' -NewName '$KillAnimation_8.png'; Rename-Item -Path 'www\audio\bgm\(Menu) Time and Silence.ogg' -NewName 'Menu_Time_and_Silence.ogg'; Rename-Item -Path 'www\audio\bgm\(Menu) Time and Silence.m4a' -NewName 'Menu_Time_and_Silence.m4a'
        ```
    - For macOS & Linux:
        ```bash
        mv "www/img/characters/\$Boksu%(5).png" "www/img/characters/\$Boksu_5.png" && mv "www/img/characters/\$BoksuBloody%(5).png" "www/img/characters/\$BoksuBloody_5.png" && mv "www/img/characters/\$BoksuCoatOff%(5).png" "www/img/characters/\$BoksuCoatOff_5.png" && mv "www/img/characters/\$BoksuCoatOffLighting%(5).png" "www/img/characters/\$BoksuCoatOffLighting_5.png" && mv "www/img/characters/\$BoksuWhite%(5).png" "www/img/characters/\$BoksuWhite_5.png" && mv "www/img/characters/\$BoksuWhiteLighting%(5).png" "www/img/characters/\$BoksuWhiteLighting_5.png" && mv "www/img/characters/\$KillAnimation%(8).png" "www/img/characters/\$KillAnimation_8.png" && mv "www/audio/bgm/(Menu) Time and Silence.ogg" "www/audio/bgm/Menu_Time_and_Silence.ogg" && mv "www/audio/bgm/(Menu) Time and Silence.m4a" "www/audio/bgm/Menu_Time_and_Silence.m4a"
        ```
2. This one-liner script will automatically find and rename all references to image files with problematic characters to a safe format (e.g., `$Boksu%(5).png` becomes `$Boksu_5.png`) inside of the `js` folder. Open a Powershell/Terminal in your **root game folder** and run:
    - For Windows (in PowerShell):
        ```powershell
        Get-ChildItem -Path 'www\data' -Filter '*.json' | ForEach-Object { $originalContent = Get-Content -Path $_.FullName -Raw; $newContent = $originalContent -replace '\$Boksu%\(5\)', '$Boksu_5' -replace '\$BoksuBloody%\(5\)', '$BoksuBloody_5' -replace '\$BoksuCoatOff%\(5\)', '$BoksuCoatOff_5' -replace '\$BoksuCoatOffLighting%\(5\)', '$BoksuCoatOffLighting_5' -replace '\$BoksuWhite%\(5\)', '$BoksuWhite_5' -replace '\$BoksuWhiteLighting%\(5\)', '$BoksuWhiteLighting_5' -replace '\$KillAnimation%\(8\)', '$KillAnimation_8' -replace '\(Menu\) Time and Silence', 'Menu_Time_and_Silence'; if ($originalContent -ne $newContent) { Write-Host "Updating references in: $($_.FullName)"; Set-Content -Path $_.FullName -Value $newContent -NoNewline } }
        ```
    - For macOS & Linux:
        ```bash
        find www/data -name "*.json" -exec sed -i.bak -e 's/$Boksu%(5)/$Boksu_5/g' -e 's/$BoksuBloody%(5)/$BoksuBloody_5/g' -e 's/$BoksuCoatOff%(5)/$BoksuCoatOff_5/g' -e 's/$BoksuCoatOffLighting%(5)/$BoksuCoatOffLighting_5/g' -e 's/$BoksuWhite%(5)/$BoksuWhite_5/g' -e 's/$BoksuWhiteLighting%(5)/$BoksuWhiteLighting_5/g' -e 's/$KillAnimation%(8)/$KillAnimation_8/g' -e 's/(Menu) Time and Silence/Menu_Time_and_Silence/g' {} +
        ```
        You can remove the .bak files later.

### Part F: Bypass Jekyll GitHub Pages for Underscore Files (Optional)

This is only for people who are going to use GitHub Pages, which automatically uses Jekyll to build static blog websites. The issue is that Jekyll ignores files that start with '_', which some of the game assets do, so we will not use Jekyll.

1. In the root project folder, add an emtpy file named `.nojekyll`.


## Why did you make it?

For pure educational purposes, fun, and to make this game accessible on more platforms.


## Credits

- **Original Game:** All credit for the game's creation goes to **STUDIO INVESTIGRAVE**. Please support the original creator by visiting the [official itch.io page](https://racheldrawsthis.itch.io/married-in-red).
- **Web Port:** This web-based version was ported by [Nick088](https://linktr.ee/nick088).
- **Game Engine:** RPG Maker MV.