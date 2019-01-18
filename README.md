# VSCode_ARM-Blinker-example

ARM STM32F4 Discovery blinker using VSCode

Visual Studio Code is an exceptionally lightened editor from Microsoft that has been designed to make it bendable to work with a variety of programming tools. Having used Eclipse in the past for ARM development, I decided to try this as an editor.

Being a fairly low level installation, I used this as an opportunity to get familiar again with GNU build tools and using the ARM GNU tools. I would have gone for the pre-packaged ARM development environments, but most are Eclipse based with crippled ARM compiler tools (32 kb). 

There are people out there that have successfully complied ARM code with CMAKE; I wanted to have something going as a starting  reference base with GNU build tools and then transition over to CMAKE to get familiar with that eventually.

VS Code Extensions:

There are a number of extensions that I have installed to make this work:
* C/C++
* ARM Support for Visual Studio Code
* Cortex-Debug (ARM Cortex-M GDB Debugger support for VSCode.
* Bookmarks

The Bookmarks extension is optional, but makes exploring the code much easier.

Makefiles:

I initially tried to build my own Makefiles and looked through a variety of blinker examples on the internet and found the best solution was to use the STMCubeMX utility from STM (https://www.st.com/content/st_com/en.html) to select the board or processor type and have it build the makefile. Extracting the defines based on the hardware selected from the newly created makefile, I used that to apply to the VSCode c_cpp_properties.json properties. 

ARM Driver Libraries:

Additional searches also specified that the standard ARM libraries are no longer supported and that designers are to use the HAL Drivers. Deleting the standard libraries from the local repository used by the STMCubeMX utility and updating the HAL Drivers with the most recent version reaped some success.

Building, Uploading, Cleaning and Debugging: 

Build:
Setting up the task.json file for building is straightforward. If you have built from the command line before using make you will be familiar with build all, build clean. Setting the cwd option specifies the reference directory where your makefile will exist and where to reference your build directory.

Clean:
To clean, the makefile was modified to indicate where the MinGW rm files are.

Upload:
The cwd option was set for the "${workspaceFolder}/build" directory to let the ST LINK command line tool find the firmware files to upload. To make the file more generic so I could drop into any new project I used the environment variable: 
      "command": "ST-LINK_CLI -P ${workspaceFolderBasename}.hex 0x08000000",

Debugging:
I used the CORTEX Debug extension from Marus (https://github.com/Marus/cortex-debug). I was able to quickly set it up in the launch.json file. The only caveat is that I was not able to use the ${workspaceFolderBasename} environment variable to make the launcher generic. No big deal to script that when I get time. When I got it up and running I was very pleased and look forward to playing around debugging more when I get time.


