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


## Why did you make it?

For pure educational purposes, fun, and to make this game accessible on more platforms.


## Credits

- **Original Game:** All credit for the game's creation goes to **STUDIO INVESTIGRAVE**. Please support the original creator by visiting the [official itch.io page](https://racheldrawsthis.itch.io/married-in-red).
- **Web Port:** This web-based version was ported by [Nick088](https://linktr.ee/nick088).
- **Game Engine:** RPG Maker MV.