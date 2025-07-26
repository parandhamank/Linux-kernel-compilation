# Eclipse Configuration for Kernel
## Create new eclipse project
- file -> New -> C/C++ Project -> C Managed build
- Fill following info
    - project name
    - uncheck use default location
    - choose the location where kernel source code present
    - Make sure to select an empty project
- then click next and finish

## Disable CDT Cross GCC Built-in Compiler Settings
- Right click on project -> Properties -> C/C++ General -> Preprocessor Include Paths, Macros, etc -> click on providers tab
- Uncheck CDT Cross GCC Built-in Compiler Settings

## Add Kernel configurations
- Right click on project -> Properties -> C/C++ General -> Preprocessor Include Paths, Macros, etc -> GNU C [in Entries Tab] -> CDT User Setting Entries
- Click on Add -> select (Preprocessor Macro File, Workspace path)
- include file include/generated/autoconf.h
- then also include file include/kenel/kconfig.h

## Define Macros
- Right click on project -> Properties -> C/C++ General -> Preprocessor Include Paths, Macros, etc -> GNU C [in Entries Tab] -> CDT User Setting Entries
- Click on Add -> select (Preprocessor Macro)
- Define
    - \_\_KERNEL\_\_
    - \_\_LINUX_ARM_ARCH\_\_ = 7 (for armv7a arch)

## Add Include Paths
- Right click on project -> Properties -> C/C++ General -> Preprocessor Include Paths, Macros, etc -> GNU C [in Entries Tab] -> CDT User Setting Entries
- Click on Add -> select (Include path, Workspace path)
- include following paths,
    - include
    - arch/arm/include
    - arch/arm/mach-\<MACH>/include
    - arch/arm/plat-\<MACH>/include
