# ESP-IDF Development Environment Using VS Code on MS Windows 7
Version: 1.0

This walkthrough using esp-idf version **3.3.1**, as for version 4, it has different setup so this method might not work.

Compared with official setup from Espressif There is some different with using this setup method:
1. we can't use `make` command to perform operation like `build`, `flash`, and `monitor`, we would use `idf.py` script provided by esp-idf.
  Because, for esp-idf, `make` command could only work properly inside MSYS console. There may some configuration to make it work, but I don't find it yet.
  Insted we use `idf.py` script provided by esp-idf.
  **Example:**
   ```console
   > idf.py menuconfig
   > idf.py -p COM5 -b 115200 build flash monitor
   ```
2. When executing `menuconfig` action with `idf.py` script, there is some option that not available, compared to `make` command. Especially in **Serial flasher config**, there is no option to set serial port and baud rate. So we have to include `-p` and `-b` parameter for serial port and baud rate, like above example.

## Prerequisite
1. Python 2.7 already installed and could be called from console.
2. Download esp-idf v3.3.1 from [espressif site](https://dl.espressif.com/dl/esp-idf/releases/esp-idf-v3.3.1.zip).
3. VS Code, you could use your existing installed VS Code or setup dedicated VS Code for esp-idf development. I personally prefer dedicated VS Code (a.k.a.[portable mode](https://code.visualstudio.com/docs/editor/portable)) for ESP development.
4. Working internet connection to download tools artifact needed by esp-idf. Theoretically, we could install each of those manually one-ny-one, but I have not tried it. 

## Setup esp-idf and python modules
1. Open console(`cmd`) and check if python already installed and have the right version.
   ```console
   > python --version
   Python 2.7.15
   ```
1. Extract `esp-idf-v3.3.1.zip` file. To prevent any undesired effect, avoid any directory that has space on its name.
2. Go to the extraction target directory
   ```console
   C:\Users\FULAN> cd e:\workspace\esp-idf-v3.3.1
   E:\workspace\esp-idf-v3.3.1>
   ```
3. Install the required Python package.
   ```console
   > python -m pip install --user -r requirements.txt
   ```
4. Execute `install.bat` script. That script would download several files, so make sure that our internet connection is working.
 
   ```console
   > install.bat
   ```

   **WARNING :** **DON'T** execute `export.bat` file after this script finished !

After this step finished, a new directory named `.espressif` would be created inside our home directory. Go to that directory and check if it's have a correct structure :
```
|- C:\Users\FULAN
|   |- dist\
|   |- python_env\
|      |- idf3.3_py2.7_env
|   |- tools\
|      |- ccache\
|      |- cmake\
|      |- esp32ulp-elf\
|      |- idf-exe\
|      |- mconf\
|      |- ninja\
|      |- openocd-esp32\
|      |- xtensa-esp32-elf\
```
These directories is needed to set environment variables in the next step.


## Setup VS Code
For ESP-IDF development, you could setup a [dedicated portable VS Code](#setup-dedicated) or [using system-wide installation](#setup-existing).

### <a name="setup-dedicated"></a>Setup Dedicated VS Code
1. Extract vs-code archive, and then go to the extraction target directory.
2. Create a `data` folder within VS Code folder to enable VS Code [Portable Mode](https://code.visualstudio.com/docs/editor/portable). Our VS Code directory structure would look like this:
   ```
   |- VSCode For ESP directory
   |   |- bin\
   |   |- data\
   |   |- ...
   |   |- Code.exe
   |   |- ...
   ```
3. Go to `bin` directory, and edit `code.cmd` file using text editor.
4. Append command to set new environment variable, `IDF_PATH`, `IDF_PYTHON_ENV_PATH`, `OPENOCD_SCRIPTS`, and `PATH`.
   Your `code.cmd` file would look like this (**note**: the path sould be adjusted to your own):
   ```batch
   @echo off
   set IDF_PATH=E:\Workspaces\esp-idf-v3.3.1
   set IDF_PYTHON_ENV_PATH=C:\Users\FULAN\.espressif\python_env\idf3.3_py2.7_env
   set OPENOCD_SCRIPTS=C:\Users\FULAN\.espressif\tools\openocd-esp32\v0.10.0-esp32-20190313\openocd-esp32\share\openocd\scripts
   set PATH=C:\Users\FULAN\.espressif\tools\xtensa-esp32-elf\1.22.0-80-g6c4433a5-5.2.0\xtensa-esp32-elf\bin;C:\Users\FULAN\.espressif\tools\esp32ulp-elf\2.28.51.20170517\esp32ulp-elf-binutils\bin;C:\Users\FULAN\.espressif\tools\cmake\3.13.4\bin;C:\Users\FULAN\.espressif\tools\openocd-esp32\v0.10.0-esp32-20190313\openocd-esp32\bin;C:\Users\FULAN\.espressif\tools\mconf\v4.6.0.0-idf-20190628\;C:\Users\FULAN\.espressif\tools\ninja\1.9.0\;C:\Users\FULAN\.espressif\tools\idf-exe\1.0.1\;C:\Users\FULAN\.espressif\tools\ccache\3.7\;C:\Users\FULAN\.espressif\python_env\idf3.3_py2.7_env\Scripts;%PATH%
   
   setlocal
   set VSCODE_DEV=
   set ELECTRON_RUN_AS_NODE=1
   "%~dp0..\Code.exe" "%~dp0..\resources\app\out\cli.js" %*
   endlocal
   ```
5. To make our new environment variable available inside VS Code, you **must** run VS Code via `bin\code.cmd` script from now on, don't execute `Code.exe` directly.

If you don't want to edit any file, skip step **3** onward and persist those variable in system environment variable. Then, you can run `Code.exe` normally.

### <a name="setup-existing"></a>Use Existing VS Code
We just need to set three new environment variable on windows.
**Note**: the path sould be adjusted to your own.

| Variable Name | Variable Value |
| ------------- | -------------- |
| IDF_PATH      | E:\Workspaces\esp-idf-v3.3.1 |
| IDF_PYTHON_ENV_PATH | C:\Users\FULAN\.espressif\python_env\idf3.3_py2.7_env |
| OPENOCD_SCRIPTS | C:\Users\FULAN\.espressif\tools\openocd-esp32\v0.10.0-esp32-20190313\openocd-esp32\share\openocd\scripts    |
| PATH          | C:\Users\FULAN\.espressif\tools\xtensa-esp32-elf\1.22.0-80-g6c4433a5-5.2.0\xtensa-esp32-elf\bin;C:\Users\FULAN\.espressif\tools\esp32ulp-elf\2.28.51.20170517\esp32ulp-elf-binutils\bin;C:\Users\FULAN\.espressif\tools\cmake\3.13.4\bin;C:\Users\FULAN\.espressif\tools\openocd-esp32\v0.10.0-esp32-20190313\openocd-esp32\bin;C:\Users\FULAN\.espressif\tools\mconf\v4.6.0.0-idf-20190628\;C:\Users\FULAN\.espressif\tools\ninja\1.9.0\;C:\Users\FULAN\.espressif\tools\idf-exe\1.0.1\;C:\Users\FULAN\.espressif\tools\ccache\3.7\;C:\Users\FULAN\.espressif\python_env\idf3.3_py2.7_env\Scripts;%PATH% |

For now your VS Code is ready for create an ESP system program. You could use VS Code own integrated terminal to build and flash program. Just remember, instead of using `make`, you should use `idf.py`.

## Additional Note
You can use VS Code extension like C/C++ extension to provide intellisene and several additional feature for c/c++ development. You could also create workspace task to build and flashing to board. But, all of those is not covered in this walkthrough. Feel free to give any sugestion to improve this guide :).