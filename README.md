## Objective and environment

Set up coding environment for C++ in the following environment, which can be demonstrated by the successful execution of Hello World program.

- Editor for coding: Visual Studio Code (not to be confused with **Visual Studio Community**)
- Compiler for building: g++ via MinGW (Minimalist GNU for **Windows**)

## Build Process
The following knowledge is *good to know* for understanding purposes, sufficient explanation will be given as appropriate at every stage.

Build process
* *Input*: Code on .h .cpp files, external libraries.
* *Compilation* from input files to individual object files (.obj).
* *Linking* from object, external libraries to output.
* *Output*: Executable (.exe) or Library (Static(.a,.lib)/Dynamic(.dll/.so))

In general, the life cycle of a C++ application can be split into the above four processes. Input is where our code goes, and output is the product (*fulfilling the purpose*) of the project. The compilation and linking processes are performed by passing inputs and commands to a compiler.

Every time the code is changed, recompilation of the modified file and linking with the other object files is necessary to update the output. Compiling by typing manually will then be a frustrating process, especially when there are a large number of source files, located at different directories.

This problem is mitigated by *build systems* present in IDE (integrated Development Environment). IDEs allow for automation of build process and include debugging tools to simplify the development process.

## Prerequisites

### Command Line Interface (CLI)
A CLI such as *command prompt* (cmd.exe) or *Windows Powershell* will be necessary. They come preinstalled with windows and are accessible by Windows Search.

Familiarity of the following commands will be useful.

|command|description|example|
|:-|:-|-:|
|`cd <folder name>`|change directory|`cd C:\`|
|`dir`|list directory| |
|`mkdir <name>`|make the subfolder `name`|`mkdir "Test"`|
|`rmdir <name> /s`|remove the specified directory and all sub-directories|`rmdir "Test"`|

The above commands enables free navigation in the file system, but only *inwards* (towards subfolder).
* `..` represents the parent directory while `.` represents the current directory.
    > Use `cd ..` to get to the parent of the current folder. You do not need to `cd C:\` and start again.
* You can navigate to deeper subfolders in one command
    > `cd <subfolder>\<subsubfolder>` goes two folders *in*, while `cd ..\..` goes two folders *out*.

By default, autocomplete is enabled in powershell, but not in command prompt. Autocomplete is triggered using **tab**.
* Repeated presses of *tab* cycles through the possible candidates, *shift+tab* does so in reverse.

The rest of this tutorial is closely related to the [official documentation by Visual Studio Code](https://code.visualstudio.com/docs/cpp/config-mingw). Use it as additional reference to this guide.
If you suspect that you have some of the following components installed, skip ahead to the verification section to confirm.

### Editor

Install VSCode and C/C++ Extension as per documentation.

### Compiler

Download an installer for [MinGW](https://sourceforge.net/projects/mingw/files/latest/download) rather than the one specified by the tutorial. This installer conveniently shows all packages, which can then be customised in the future.

* Install the installer under C:/MinGW
* Open MinGW Installation Manager, select Basic Installation on the left.
* Install: developer-toolkit, base (there are two) and g++ packages.
> This is done by first marking the desired packages for installation then selecting update catalogue under the installation menu tab.

### Git
A revision control system such as [Git](https://github.com/git/git/blob/master/README.md) is essential for collaboration purposes. While Github is where the files are stored, git enables the access and sharing of files *from your computer*. There is automatic integration with VSCode, but you need to [install git](https://git-scm.com/downloads) to your computer.

## Verification

### PATH variable
Every program is run by an executable (.exe). For integration between programs to occur, they need to know where the corresponding .exe files are located. The **PATH** variable is a list of directories that specify where .exe files are, and is in turn used by programs to search for executables.

> If an executable is part of the path variable, the program can be run from the CLI from any directory.

Verify that the programs are installed **and** the path variable includes it.

* Open up a CLI (cmd or powershell) and run the following commands, which should give a similar output.
> An *unsuccessful* output has the form "`name` is not recognised as an internal or external command...".
* `code --version`
    >`1.55.0`  
    `c185983a683d14c396952dd432459097bc7f757f`  
    `x64`
* `g++ --version`
    >`g++ (MinGW.org GCC Build-2) 9.2.0 Copyright (C) 2019 Free Software Foundation, Inc.`  
    `This is free software; see the source for copying conditions.  There is NO warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.`
* `git --version`
    >`git version 2.27.0.windows.1`

If you have installed the program (verified under Start > Apps and Features) but don't get the output in the CLI, then you will need to edit the path variable, which basically boils down to two things.
* Identify the directory to include
* Add it to the variable.

For identifying the directory, the default locations are:
* VSCode: `C:\Users\<name>\AppData\Local\Programs\Microsoft VS Code\bin`
    * Note the replacement of `name`
* g++: `C:\MinGW\bin`
* Git: `C:\Program Files\Git\bin`
* Otherwise, windows Search for the application > Right click > Open file location.
    * If the actual .exe, this folder is the directory.
    * If a shortcut to the .exe, Right click > Properties > Target shows where the .exe is, use target directory instead.

Adding to PATH
* Windows search for `path` > Edit the system environment varibles > Environment variables > Path > Edit > New.

## Initial Setup
There is no initial setup necessary for g++.

For git, perform [these steps](https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup).
* There are different scopes of customisation: system (affects all users on computer), global (your account on the computer) and local (changes the *repository* settings, not necessary for now).
> `git config --list --show-origin` shows all settings and where they are stored, which in turn shows their scope.

Verify that C/C++ extension is installed in VSCode as per documentation, then close VSCode.

## First Project
### Making Directory
Note: More actions are performed manually for a deeper understanding, with hints to automation at the end.

Perform the following actions in a CLI.

* Navigate to where you want to store all your projects, and create a folder named "projects"
* Go into "projects", create a folder named "helloworld"
* Go into "helloworld" and open Visual Studio Code via `code .`

The commands to do so are 
```
mkdir projects  
cd projects  
mkdir helloworld  
cd helloworld  
code .
```

`code .` opens VSCode and sets the current folder (`.`) as the working directory.

### Adding Code
Create a new file main.cpp (see VSCode documentation) but with the following code instead.

```
#include <iostream>

int main(){
    std::cout << "Hello World" << std::endl;
}
```
This program prints out "Hello World" to the console.
> A simple search of "C++ Hello World" should be sufficient to understand the code and features of C++. This guide is meant for build process not how to code.

### Compiling, linking and executing
Make a subfolder named `build`. This is where we will store the outputs of our compilation.
> The create folder icon is next to create file icon, process similar to making main.cpp.

Open the terminal (View > Terminal, shortcut Ctrl+\`). We will perform the following steps via the terminal, which is the same as the windows CLI.
* Compile, but do not link, main.cpp to main.o in the build directory.
* Link main.o in the build to get main.exe in the build directory.

The commands to do so are
```
g++ -c main.cpp -o build\main.o
g++ build\main.o -o build\main.exe
```

* -c specifies compile but do not link
* -o specifies the form of output
* build\ specifies the location to be in build subdirectory, since the command is executed from HelloWorld folder.
> -c/-o are known as *options*, the most common and useful ones can be displayed by `g++ --help`, or searching for "g++ flags options"


At this point, the project structure is
```
helloworld
|_____build
|       |_____main.exe
|       |_____main.o
|_____main.cpp
```

Run the main.exe in terminal via `build\main.exe`, and you will see an output!

### Simplifications
* Compilation and linking (Collectively known as *building*) can be performed in one step.
    * `g++ main.cpp -o build\main.exe` skips the generation of main.o.
    * `g++ main.cpp` alone will produce a.exe in the same directory by default. While simple, this builds the wrong habit.
* VSCode offers a *building task* to automate build process. The details are in the documentation.
    * Configure tasks.json such that it performs "g++ main.cpp -o build\main.exe" every time you press the **build** button. Setting `args` parameter to ["main.cpp","-o","build\\\\main.exe"] achieves this.
        * Note "\\\\", since \\ is an escape character.
    * Variable substitution is necessary as file gets larger. The details are in the documentation.
* VSCode offers a *launch task* to automate debugging process. The details are in the documentation.
    * For simplicity, use Windows over gdb for environment.
    * Configure the launch file to launch the .exe everytime you press the **debug** button. Setting `program` to "${workspaceFolder}\\\\build\\\\main.exe" achieves this.
        * Variable substitution of workspaceFolder is used.

## Cloning
In this example, you coded your files on your own and they are all stored in your machine. This is impossible when working with other people, and is where git comes into play. Github stores the files and distributes them to collaborators.

To simulate this, *clone* this repository to your `project` folder.
* While in projects directory, run `git clone https://github.com/kubrian/HelloWorldBase`.

Compile and run the program. Good luck!

## Documentation and further reading
* Github structure and git
    * [Github Guide](https://guides.github.com/)
    * [Basics of git](https://git-scm.com/book/en/v2)
* C++
    * [Devdocs Documentation](https://devdocs.io/cpp/)
        * Based on [cppreference](https://en.cppreference.com/w/)
* Moving forward
    * Advanced building via [Cmake] (https://cmake.org/)
        * CMake makes Makefile which is used to build complex projects (Think of it as a file that specify the args for g++)
        * CMake is crossplatform by its ability to work with native compilers.
    * GUI via [OpenGL](https://www.opengl.org/)
        * Implemented by [GLFW](https://www.glfw.org/), [GLAD](https://github.com/Dav1dde/glad), [GLEW](http://glew.sourceforge.net/), etc.