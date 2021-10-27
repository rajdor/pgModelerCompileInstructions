#How to Compile pgModeler
=====

Building upon [https://www.pgmodeler.io/support/installation](pgModeler "A quick step-by-step on how to build, install and run the tool")


## Windows 10 - successful compile - 2020.04.18
### Install MSYS2
1. Download 64bit MSYS2 from [https://www.msys2.org/](https://www.msys2.org/)  *This example uses msys2-x86_64-20190524.exe*
2. Install to default directory C:\msys64
3. Complete the installation, by running *MSYS2 64bit now* from the last step of the installation wizard.

### Install libraries, tools, setup directories
1. Update MSYS2
   ```bash
   pacman -Suy
   ```

2. On first update you will get the following warning.  Close the window.
   ```bash
   warning: terminate MSYS2 without returning to shell and check for updates again
   warning: for example close your terminal window instead of calling exit
   ```

3. Restart MSYS2 from the Windows 10 Start menu 'MSYS2 MSYS' and continue with updates
   ```bash
   pacman -Suy
   ```

4. Installation of libraries 
   ```bash
   pacman -S base-devel mingw-w64-x86_64-make mingw-w64-x86_64-gcc mingw-w64-x86_64-postgresql mingw-w64-x86_64-qt5
   pacman -S mingw-w64-x86_64-libxml2
   pacman -S mingw-w64-x86_64-double-conversion
   pacman -S mingw-w64-x86_64-zstd
   ```
   
5. Install git
   ```bash
   pacman -S git
   ```

6. Make directory for output 
   ```bash
   mkdir ~/pg_compiled
   ```

### Clone and prepare pgModeler
1. Clone pgModeler from github

   ```bash
   git clone https://github.com/pgmodeler/pgmodeler.git
   cd pgmodeler
   git checkout master
   ```
   
2. Add binaries to path

   ```bash
   export PATH="/mingw64/bin:$PATH"
   ```
   
3. Set environment variable for output of compile

   ```bash
     export INSTALLATION_ROOT="$HOME/pg_compiled"
   ```
   
4. Edit C:\msys64\mingw64\share\qt5\mkspecs\common\windows-vulkan.conf

   Change:
   ```text
      load(windows_vulkan_sdk)
   ```
   To:
   ```text
      load(win32/windows_vulkan_sdk)
   ```

5. Edit "C:\msys64\home\<user>\pgmodeler\pgmodeler.pri"

   Change:
   ```text
       windows {
         !defined(PGSQL_LIB, var): PGSQL_LIB = C:/msys_64/mingw64/bin/libpq.dll
         !defined(PGSQL_INC, var): PGSQL_INC = C:/msys_64/mingw64/include
         !defined(XML_INC, var): XML_INC = C:/msys_64/mingw64/include/libxml2
         !defined(XML_LIB, var): XML_LIB = C:/msys_64/mingw64/bin/libxml2-2.dll
   ```
   To:
   ```text
        windows {
          !defined(PGSQL_LIB, var): PGSQL_LIB = C:/msys64/mingw64/bin/libpq.dll
          !defined(PGSQL_INC, var): PGSQL_INC = C:/msys64/mingw64/include
          !defined(XML_INC, var): XML_INC = C:/msys64/mingw64/include/libxml2
          !defined(XML_LIB, var): XML_LIB = C:/msys64/mingw64/bin/libxml2-2.dll
   ```



### Compile steps
1. Compile it!

   ```bash
   cd ~/pgmodeler
   /mingw64/bin/qmake.exe -r CONFIG+=release PREFIX=$INSTALLATION_ROOT pgmodeler.pro
   make
   make install
   cd $INSTALLATION_ROOT
   windeployqt pgmodeler.exe pgmodeler_ui.dll
   ```

2. Copy dependancies

   ```bash
   cd $MSYS2_ROOT/mingw64/bin/
   cp libicuin*.dll libicuuc*.dll libicudt*.dll libpcre2-16-0.dll libharfbuzz-0.dll \
      libpng16-16.dll libfreetype-6.dll libgraphite2.dll libglib-2.0-0.dll libpcre-1.dll \
      libbz2-1.dll libssl-1_1-x64.dll libcrypto-1_1-x64.dll libgcc_s_seh-1.dll \
      libstdc++-6.dll libwinpthread-1.dll zlib1.dll libpq.dll libxml2-2.dll liblzma-5.dll \
      libiconv-2.dll libintl-8.dll $INSTALLATION_ROOT
   cp libdouble-conversion.dll $INSTALLATION_ROOT
   cp libzstd.dll $INSTALLATION_ROOT
   cp libmd4c.dll $INSTALLATION_ROOT
   cp libbrotlidec.dll $INSTALLATION_ROOT
   cp libbrotlicommon.dll $INSTALLATION_ROOT
   ```

### Run pgModeler
1. From Windows Explorer double click
   ```
   C:\msys64\home\<user>\pg_compiled\pgmodeler.exe
   ```
