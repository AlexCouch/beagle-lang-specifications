# Environment

Notes: `.ext` is a placehold for the actual file extension.

### General Environment Details
Modules will be structured like so
```
root (dir)
| - first_module (dir)
    | - second_module (dir)
        | - mod.ext
        | - build.ext
        | - deploy.ext
        | - vcs.ext
    | - mod.ext
    | - build.ext
    | - deploy.ext
    | - vcs.ext
| - mod.ext
| - build.ext
| - vcs.ext
```

`mod.ext` -> The actual module code that will be compiled. The name of the module is the name of the containing directory.<br>
`build.ext` -> Build script for specifying specific actions for the containing module<br>
`deploy.ext` -> Deploy script for specifying specific actions, details, and locations for deploying the module after being built for both testing and releasing<br>
`vcs.ext` -> VCS script for specifying how your vcs should handle this module, such as specific branches to be made and pushed. This will help with organizing modules vcs wise to keep tasks for each module well managed.<br>