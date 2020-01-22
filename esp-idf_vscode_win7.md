# ESP-IDF Development Environment Using VS Code on MS Windows 7
This walkthrough using esp-idf version **3.3.1**.

## Prerequisite
1. Python 2.7 already installed and could be called from console.
2. Download esp-idf v3.3.1 from [espressif site](https://dl.espressif.com/dl/esp-idf/releases/esp-idf-v3.3.1.zip).
3. VS Code installer or binary. I prefer binary(zip) version, so we can setup a dedicated VS Code for ESP development.
4. Working internet connection to download artifact needed by esp-idf.

## Setup esp-idf and python modules
1. Open console(`cmd`) and check if python already installed and have the right version.
   ```bat
   > python --version
   Python 2.7.15
   ```
1. Extract `esp-idf-v3.3.1.zip` file. To prevent any undesired effect, avoid any directory that has space on its name.
2. Go to the extraction target directory
   ```bat
   C:\Users\FULAN> cd e:\workspace\esp-idf-v3.3.1
   E:\workspace\esp-idf-v3.3.1>
   ```
3. Install the required Python package.
   ```bat
   > python -m pip install --user -r requirements.txt
   ```
4. Execute `install.bat` script. That script would download several files, so make sure that our internet connection is working.
 
   ```bat
   > install.bat
   ```

   **WARNING :** **DON'T** execute `export.bat` file after this script finished !

After this step finished, a new directory named `.espressif` would be created inside our home directory. Go to that directory and check if it's have a correct structure :
```
|- C:\Users\FULAN
|   |- dist
|   |- python_env
|      |- idf3.3_py2.7_env
|   |- tools
|      |- ccache
|      |- cmake
|      |- esp32ulp-elf
|      |- idf-exe
|      |- mconf
|      |- ninja
|      |- openocd-esp32
|      |- xtensa-esp32-elf
```
These directories is needed to set environment variables in the next step.


## Setup VS Code
If you don't want to setup dedicated VS Code, start from point **3**.
1. Extract vs-code archive, and then go to the extraction target directory.
2. Create a `data` folder within VS Code folder to enable VS Code [Portable Mode](https://code.visualstudio.com/docs/editor/portable). Our VS Code directory structure would look like this:
   ```
   |- VSCode For ESP directory
   |   |- Code.exe
   |   |- data
   |   |- ...
   ```
3. Run VSCode IDE, and then open setting editor (from menu: `File`->`Preferences`->`Settings`, or use `Ctrl+,` shortcut).
4. Select `User` tab and then switch to settings JSON editor.
5. VS Code has built-in integrated terminal feature, lets use it to compile and downloading/flashing our program to the development board.

   Add/modify `terminal.integrated.env.windows` setting and add environment variable for: `IDF_PATH`, `IDF_PYTHON_ENV_PATH`, `OPENOCD_SCRIPTS`, and `PATH` :
   ```json
   {
        ...
        "terminal.integrated.env.windows": {
            "IDF_PATH": "E:\\Workspaces\\esp-idf-v3.3.1",
            "IDF_PYTHON_ENV_PATH": "C:\\Users\\FULAN\\.espressif\\python_env\\idf3.3_py2.7_env",
            "OPENOCD_SCRIPTS": "C:\\Users\\FULAN\\.espressif\\tools\\openocd-esp32\\v0.10.0-esp32-20190313\\openocd-esp32\\share\\openocd\\scripts",
            "PATH": "C:\\Users\\FULAN\\.espressif\\tools\\xtensa-esp32-elf\\1.22.0-80-g6c4433a5-5.2.0\\xtensa-esp32-elf\\bin;C:\\Users\\FULAN\\.espressif\\tools\\esp32ulp-elf\\2.28.51.20170517\\esp32ulp-elf-binutils\\bin;C:\\Users\\FULAN\\.espressif\\tools\\cmake\\3.13.4\\bin;C:\\Users\\FULAN\\.espressif\\tools\\openocd-esp32\\v0.10.0-esp32-20190313\\openocd-esp32\\bin;C:\\Users\\FULAN\\.espressif\\tools\\mconf\\v4.6.0.0-idf-20190628\\;C:\\Users\\FULAN\\.espressif\\tools\\ninja\\1.9.0\\;C:\\Users\\FULAN\\.espressif\\tools\\idf-exe\\1.0.1\\;C:\\Users\\FULAN\\.espressif\\tools\\ccache\\3.7\\;C:\\Users\\FULAN\\.espressif\\python_env\\idf3.3_py2.7_env\\Scripts;${env:PATH}"
        },
        ...
    }
   ```
   Save that file, now you can build and flash using `idf.py` script on VS Code integrated terminal.