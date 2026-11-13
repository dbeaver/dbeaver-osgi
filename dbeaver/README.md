# DBeaver Equinox framework

This fork is based on Eclipse Equinox tag `R4_41` (Eclipse 2026-09).
The `org.eclipse.osgi` sources match the framework version `3.24.300.v20260721-1251` used by DBeaver's target platform.
Upstream history, source layout, and license notices are retained.
The DBeaver development branch is `devel`.

## Build

Clone `dbeaver-common` alongside this repository.
Use its Maven wrapper and the DBeaver build entry point, rather than the upstream root POM:

```text
../dbeaver-common/mvnw.cmd -f dbeaver/pom.xml package
```

On Linux and macOS use `../dbeaver-common/mvnw` instead.
The output is `bundles/org.eclipse.osgi/target/org.eclipse.osgi-3.24.301-SNAPSHOT.jar`.
The bundle retains its upstream symbolic name and public API.
The fork uses version `3.24.301`, explicitly required by DBeaver's RCP feature, to distinguish it from upstream `3.24.300`.
The `org.jkiss.dbeaver.osgi.feature` P2 feature patch replaces the exact framework dependency in Eclipse's `org.eclipse.e4.rcp` feature for the 4.41 release stream.
This is required because upstream RCP features pin the original framework to an exact version.
No startup or cache behavior is changed by this initial integration.

DBeaver's `plugins/pom.xml` includes the framework as a reactor `eclipse-plugin` module, so Community product builds compile and package the fork.
The remaining upstream bundles are not part of the DBeaver build.

## IDEA debugging

Regenerate the workspace using `idea-workspace-pro` after checking out this repository.
The generator selects `bundles/org.eclipse.osgi` as the framework location, generates its source module, and adds `target/classes` to `dev.properties`.
Build the `org.eclipse.osgi` module before launching a product or debugging framework startup.
Root-level framework resources remain available from the bundle directory.

## Updating upstream

Merge the Equinox release tag corresponding to DBeaver's managed Eclipse target into `devel`.
Keep the DBeaver build parent and IDEA output-directory integration when resolving conflicts.
Update the feature patch's applicability range to the new Eclipse release stream.
Verify both a packaged Community launch and an IDEA launch before changing the baseline.
