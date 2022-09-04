---
layout: default
title: CMakeLists File
parent: C++
nav_order: 1
has_children: true
---

# CMakeLists

**In short words (for beginner):** 

CMakeLists is the composer while the compiler e.g. g++, is the musican in building and installing phase.

Compiler write notes, which tells the musican what music to play and how to play. So CMakeLists will build makefiles which tells compiles exactly how to compile each file and link them together.

It's ok to write makefile directly, but normally it's very long and complex. We could write `CMakeLists.txt` in simple commands and use command`cmake` to generate the makefile.

** A general pipeline is:**

```sh
cd /your/project/root

# Use build folder to keep generated files only in this folder, for easy deleting and tidy
mkdir build && cd build

# Find the CMakeList.txt in parent folder to generate makefile
# It's better to clean build folder if you changed your CMakeList, since some file generated here will not be updated
# Some flags could be assigned here
cmake ..

# Optional
# This command could let us change options with a GUI.
# In cmakelists, you could use parameters and logics to control your building process. Here you could use this command to open a simple gui in terminal and change the parameters.
# E.g. If you set a parameter USE_GPU in CMakeLists.txt, and if this is true you will compile GPU file, if not you won't compile them. Then, you could configure this parameter either with -DUSE_GPU during cmake or edit with ccmake here and press c to update your changes. 
ccmake ..

# Optional
# If you configured your CMakeList to generate debian package, now it's the time to build your package
# After this command the package target will be built, so you could see a .deb file in your built folder 
make package

# Make your file with the generated configuration
# A very often used parameter is -j, the number behind means how many thread you want to run, e.g. -j4, use 4
make

# Optional
# Install basically means copy files from build folder to other folders
# If you assgined your install path and the file you want to install, you could run this command to install your file.
sudo make install

```



