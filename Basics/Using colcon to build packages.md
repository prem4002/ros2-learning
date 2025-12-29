for easier access to all the extensions using `tab`
`sudo apt install python3-colcon-common-extensions`

source this bitch in the bashrc

- The `build` directory will be where intermediate files are stored. For each package a subfolder will be created in which e.g. CMake is being invoked.
- The `install` directory is where each package will be installed to. By default each package will be installed into a separate subdirectory.
- The `log` directory contains various logging information about each colcon invocation.