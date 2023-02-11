# MultiWorkspace

Setup for a multi-mod workspace

## Steps to run in intellij

- Clone this repo as usual
- Run following steps for submodules

```
git submodule init
git submodule update
```

- Open as project in IntelliJ using the build.gradle in the root folder
- Run `getIntellijRuns` in forgegradle runs
- Runs get created, but they don't have module selected so when running it for the first time just select `.main` module of the project that's being run
- `workspace ...` runs run minecraft with all the mods in workspace

## Additional mods setup

Modify `settings.gradle` as well as `workspace/build.gradle` for all your mods.
Inside the `mods/build.gradle` you should have the following construct to make sure the mods are still compilable standalone as well as inside the multi-project:

```groovy
if (findProject(':ExNihiloSequentia') != null) {
    implementation project(':ExNihiloSequentia') {
        transitive = false
    }
} else {
    implementation fg.deobf (project.dependencies.create("novamachina.exnihilosequentia:ExNihiloSequentia:${exnihilo_version}") {
        transitive = false
    })
}
```

To set up this project, just open the root (empty) `build.gradle` as a project in IDEA. Run genIntellijRuns and select the one for the 'workspace' module

## Updating Tool Version Properties

Included in `workspace` and the other subprojects is the task `updateToolingProperties`. This task is only available in the subprojects when opened in the context of this MultiWorkspace.
Running this task will copy properties from the root `gradle.properties` file into the subprojects, if the property exists in the subproject. This is currently used to update the following properties:

- `forgeVersion`
- `mappingsChannel`
- `mappingsVersion`
- `minecraftVersion`

This task is useful for updating all projects to a new version of Forge, Minecraft, or Parchment mappings.

## Running Data Generation

At this time, data generation does not work properly from the MultiWorkspace project. In order to run data generation, the individual project will have to be opened on its own and the `runData` task executed in that project. If the project is dependent on other subprojects, be sure to push the dependencies first to the artifact repo (run the Jenkins pipeline) and update the dependency in the project you want to run data generation.