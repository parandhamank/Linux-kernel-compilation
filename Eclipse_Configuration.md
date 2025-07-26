# Eclipse Configuration for Kernel
## Create new eclipse project
- file -> New -> C/C++ Project -> C Managed build
- Fill following info
    - project name
    - uncheck use default location
    - choose the location where kernel source code present
    - Make sure to select an empty project
- then click next and finish

## Add Kernel configurations
- Right click on project -> Properties -> C/C++ General -> Preprocessor Include Paths, Macros, etc -> GNU C [in Entries Tab] -> CDT User Setting Entries
- Click on Add -> select (Preprocessor Macro File, Workspace path)
- include file include/generated/autoconf.h
- then also include file include/kenel/kconfig.h

## Define Macros
- Right click on project -> Properties -> C/C++ General -> Preprocessor Include Paths, Macros, etc -> GNU C [in Entries Tab] -> CDT User Setting Entries
- Click on Add -> select (Preprocessor Macro)
- Define
    - __KERNEL__
    - __LINUX_ARM_ARCH__ = 7 (for armv7a arch)

