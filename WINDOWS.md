# Installing and Running Curv on Windows 10
These instructions will install Curv on Windows 10 as a native 64 bit Windows
program.

This is accomplished by installing MSYS2, which is a Linux-like operating
environment. Then, using the MSYS2 interactive command line shell, Curv is
installed as an MSYS2 application. The MSYS2 installation will reside
in C:/msys64, which includes the entire Curv installation.

Once everything is installed, you can type the "curv" command
in a standard terminal window, either the Command Prompt or the PowerShell.
Follow the usage instructions for Linux in the regular documentation.

If you uninstall MSYS2 using the System Settings app,
then you will remove both MSYS2 and Curv.

## Building and Installing Curv

 1. On Windows, install [MSYS2 64bit](https://www.msys2.org/)
    Just follow the instructions from the web site.

 2. At the end of the installation, an "MSYS" window will open.
    The prompt says "MSYS", it does not say "MinGW64".
    We don't want to use this shell, so just close the window,
    or type "exit" and press ENTER.

 3. Open a "MinGW64" shell from the start menu:
    ```
    Start -> MSYS2 64bit -> MSYS2 MinGW 64bit
    ```

 4. Upgrade MSYS2.
    Within the MinGW64 shell, type:
    ```
    pacman -Syuu
    ```
    Reopen the shell if asked to do so.

 5. Install `git`:
    ```
    pacman -S --needed git
    ```
    Note: One time when I tried this, I got weird error messages due to
    a network problem. Running the command a second time succeeded.
 
 6. Use `git` to install the Curv source code:
    ```
    git clone https://github.com/curv3d/curv
    cd curv
    git submodule update --init
    ```
 
 7. Install build tools and libraries:
    ```
    pacman -S --needed git diffutils mingw-w64-x86_64-clang make mingw-w64-x86_64-cmake mingw-w64-x86_64-boost mingw-w64-x86_64-mesa mingw-w64-x86_64-openexr mingw-w64-x86_64-intel-tbb mingw-w64-x86_64-glm mingw-w64-x86_64-glew mingw-w64-x86_64-dbus patch mingw-w64-x86_64-openvdb
    ```

 8. Build curv:
    ```
    cd
    cd curv
    make
    ```
    This creates `release/curv.exe`.
    The full pathname of this executable is:
    `C:\msys64\home\User\curv\release\curv.exe`.

 9. In order to run Curv from the Command Prompt or the PowerShell,
    you need to add Curv to your PATH variable:
    * In the "Start search" box in the Windows 10 task bar,
      type "env" and hit ENTER.
    * Select "Edit environment variables for your account".
    * In the control panel window that pops up, click on "Path"
      then click the "Edit..." button.
    * In the "Edit environment variable" window that pops up,
      add two additional directories using the "New" button:
      * `C:\msys64\mingw64\bin`
      * `C:\msys64\home\User\curv\release`

10. In order to run Curv from the MSYS2 terminal window,
    you need to install `winpty`:
    ```
    pacman -S winpty
    ```
    The Curv REPL (which you get by running `curv` with no arguments)
    will not work correctly in the MSYS2 terminal due to a bug in that terminal.
    The official workaround is to run Curv using the `winpty` command.

    Next, edit your `.bashrc` file:
    ```
    notepad ~/.bashrc
    ```
    and add the following line to the end:
    ```
    alias curv="winpty ~/curv/release/curv"
    ```
    Finally, tell your shell to reload the `.bashrc` definitions:
    ```
    source ~/.bashrc
    ```
    Now the `curv` command will work in your MSYS2 terminal window.

    Note that you also need `winpty` in order to make the command line
    `gdb` debugger work (in case you want to debug `curv` using `gdb`
    in the MSYS2 shell).

## Running Curv
Here are some commands to try:

  * Print version information:
    ```
    curv --version
    ```
    This version information should be included in any bug report.
    The command works even when a lot of other things are broken.
    It will also describe your graphics card, which is critical information
    if the graphics are broken or slow.

  * Render some 3D graphics:
    ```
    cd c:/msys64/home/User/curv/examples
    curv kaboom.curv
    ```

  * Livemode with editor:
    ```
    cd c:/msys64/home/User/curv/examples
    curv -le liquid_paint.curv
    ```

  * Export 3D model to an OBJ file (polygon mesh) for 3D printing.
    ```
    curv -o klein.obj -O jit c:/msys64/home/User/curv/examples/mesh_only/klein.curv
    ```
    Note, this command uses the MSYS2 C++ compiler that you installed
    as part of the build instructions.
