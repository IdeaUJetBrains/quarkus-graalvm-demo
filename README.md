Project:  https://github.com/IdeaUJetBrains/quarkus-graalvm-demo

<h2>Run/debug the Quarkus application in native (no JVM) mode inside docker container</h2>

1. Create a maven run configuration and run it on Docker target  to build our native application
   - Run on target: Docker
      - Image to pull: jetbrains/graalvm-debugger:21  or jetbrains/graalvm-debugger:17
      - Optional:
         - Run options: add `-p 8080:8080`
   - "Run": `package`  or `-P native package`

2. Create `GraalVM Native Image`"` run configuration to debug application
   - Run on: select the same Docker target as for the maven run config
   - Executable: `target/quarkus-graalvm-demo-1.0.0-SNAPSHOT-runner`
   - Symbol file: `target/quarkus-graalvm-demo-1.0.0-SNAPSHOT-runner.debug`
   - Use classpath for module: `quarkus-graalvm-demo`


4. Press debug on this run configuration
5. Set desired breakpoints e.g. in `com/demo/HelloResource.java`
6. `curl http://localhost:8080/hello/add/minamoto/44`
7. `http://localhost:8080/hello/users`



//=====================================================================================================//
OLD WAY, build 233.*

1. Run maven target `package` to build native application
   profile: `native`
   additional VM options: 
      - `-Dquarkus.native.container-build=true` 
      - `-Dquarkus.native.debug.enabled=true`
      - `-Dquarkus.native.builder-image=quay.io/quarkus/ubi-quarkus-mandrel-builder-image:23.0-jdk-17`
   </br>So, we have as a result this command: </br>
   `mvn package -P native -Dquarkus.native.container-build=true  -Dquarkus.native.debug.enabled=true  -Dquarkus.native.builder-image=quay.io/quarkus/ubi-quarkus-mandrel-builder-image:23.0-jdk-17`
2. Edit Dockerfile, to prepare container with gdb server installed e.g:
   ```Dockerfile
   FROM registry.access.redhat.com/ubi8/ubi:8.6
   CMD ["yum", "install", "gdb"]
   ```
3. Create `GraalVM Native Image`"` run configuration to debug application
   - Executable: `target/quarkus-graalvm-demo-1.0.0-SNAPSHOT-runner`
   - Use classpath for module: `quarkus-graalvm-demo`
   - Create target from your docker file 'e.g. src/main/docerfile/Dockerfile.native'
      - add optional flag `-p 8080:8080`
      - add image tag, because without it there are problems (which???? )
4. Press debug on this run configuration
5. Set desired breakpoints e.g. in `com/demo/HelloResource.java`
6. `curl http://localhost:8080/hello/add/minamoto/44`
7. `http://localhost:8080/hello/users`


Troubleshooting
https://youtrack.jetbrains.com/issue/IDEA-331760/ It is not possible to set breakpoints for some classes/methods
...


