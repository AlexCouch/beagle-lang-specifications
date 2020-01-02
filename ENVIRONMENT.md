# Environment

### General Environment Details
Modules will be generally structured like so
```
root (dir)
| - first_module (dir)
    | - second_module (dir)
        | - mod.bg
        | - build.bg
        | - deploy.bg
        | - vcs.bg
    | - mod.bg
    | - build.bg
    | - deploy.bg
    | - vcs.bg
| - mod.bg
| - build.bg
| - vcs.bg
| - log.bg
```

#### Modules
Modules are defined in one of three ways:

##### File Based Modules
File based modules are modules that are defined by the source code's file name. You can then import this file as a module and call its top level members.

##### Directory Based Modules
Directory based modules are modules that are defined by the directory containing a mod.bg file. The mod.bg file is the source code for the module, and the name of the module is that of the containing directory. This allows you to group similar modules together as child modules of one module (the directory).

##### Declaration Based Modules
Declaration based modules are modules that are defined in code by using the `mod` keyword. More info in the [modules doc](MODULES.md).

#### Build Scripts
Build scripts can be defined for any module, as long as that module is a directory based module. When you write a build script inside a module's directory, the build tool will run it along with any other build scripts. A module's child modules can have their own build scripts, and allows you to do sequential execution of build scripts. Definitions in a parent module's build script can be accessed in a child module's build script.

##### Platform Specific Building
Build scripts can be used to specify platform specific details about a module. If you define a parent module for `x86` implementations of common code, that module's build script can tell the build tool how its code and child modules should be built. A child module called `windows` can have its own build script for windows specific dependencies, linking, filesystem interactions, etc, same with any other platform specific module.

#### Deployment Scripts
A deployment script can be used to specify how you want to deploy your built artifacts in several different ways. This includes, but is not limited to:

* Test deployment
    - Local test installation and deployment
        + Launcing and testing inside a browser (in the case of WASM modules)
        + Install and run as a background service
    - Remote test installation and deployment
        + Send artifact to a remote machine running specialized software (WIP concept) that will download the artifact and receive special test instructions for that artifact
    - Cloud test installation and deployment
    - Device test installation and deployment
        + USB devices like a connected iPhone/Android
        + Raspberry Pi device
* Live deployment
    - Launch artifact to live server
    - Launch artifact to web host
    - Launch artifact to remote machine
    - Launch artifact to connected devices
        + Android
        + iOS
        + Raspberry Pi
        + Etc
    - Launch artifact to downloads repository

#### Version Control Integration Script
Version control integration scripts can be put into a module for telling specialized vcs that wraps around commonly used vcs such as SVN, Git, Mercurial, etc, and tells it how to manage the version control automation specifically for that module. Capabilities include, but not limited to:

* Creating branches for certain modules
* Create repositories for certain modules
* Branch synchronizations
* Automating team boards like trello