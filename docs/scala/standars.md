1. Logging should be done with `java.slf4j.Logger`
2. For akka inclue `akka-slf4j` and add `akka.event.Sl4jLogger` to `loggers` configuration
3. `logback-classic` as a backend for slf4j, use `fixLogging` to fix conflicts.
   For logging use the parameterized mehtods instead of the ones that takes only a String, this is an optimisation as the former materialises the string objects even if the messages are not logged when the latter doesn't.
   
4. akka actors should be named for clarity (this applies to big-mama-sdk and silver-watch-backend), system and actor names should be lower cased and hyphenated
5. don't add uneccessary dependencies, try to use the minimal set of them
6. for multi build project, use the same structure as big-mama-sdk and silver-watch-backend (we have to create a seed project for that)
7. If one of the sub projects need `scalapb` add
    ```
    retrieveManaged := true
    PB.targets := Seq(scalapb.gen() -> (sourceManaged in Compile).value)
    ```
    to the build.sbt of the subproject (not the root project), if you use want to compile `src/test/protobf` files also
    add the folowing :
    ```
    retrieveManaged := true
    PB.targets in Test := Seq(scalapb.gen() -> (sourceManaged in Test).value)
    Project.inConfig(Test)(sbtprotoc.ProtocPlugin.protobufConfigSettings)
    ```

9. All project should use scoverage tool as follows:
   1. add `addSbtPlugin("org.scoverage" % "sbt-scoverage" % "1.5.1")` to your `project/plugins.sbt`
   2. add the folowwing to your `build.sbt` (or to your `commonConfig` variable if you are in a multi project build)
      ```scala
      coverageEnabled := true,
      coverageMinimum := 50,
      coverageFailOnMinimum := true,
      coverageHighlighting := true,
      ```
   3. To run the test with coverate enabled: `sbt test coverage`
   4. To get The coverage report (for every sub project if you are in multi project build): `sbt test coverateReport`
   5. To get an aggregated report for all sub projects `sbt coverageAggregate`. This should be executed after one of the previous commands

10. Supervision strategy for streams:
    Streams fails by default, if you want to avoid stream failures you need to provide a supervision strategy to the 
    ActorMaterializer. 
    ```scala
    val decider: Supervision.Decider = {
      case x =>
        logger.error("Stream Error", x)
        Supervision.Resume
    }
    implicit val materializer = ActorMaterializer(
      ActorMaterializerSettings(system).withSupervisionStrategy(decider))
    ```
    Note that the supervision strategy is not applied to all the stages/operators, some of them
    not adhere to the supervision strategy, check the docs for that.

11. sbt assembly:
    Sbt assembly needs a mergeStrategy to resolve conflicts (same files in diffrent paths), you need to put it in the
    common settings for multi build project structure
    ```scala
    assemblyMergeStrategy in assembly := {
      //this ensure netty native libraries are not discarded by the second case
      case PathList("META-INF", "native", xs @ _*) => MergeStrategy.first
      //discard all other META-INF duplicated files. Be aware !!
      case PathList("META-INF", xs @ _*) => MergeStrategy.discard
      //merge configuration files
      case "application.conf" => MergeStrategy.concat
      case "reference.conf" => MergeStrategy.concat
      //apply the default strategy for ohters
      case x =>
        val oldStrategy = (assemblyMergeStrategy in assembly).value
          oldStrategy(x)
    }
    ```
   If you don't want to assemble the root project (by default the aggregation of all the subprojects) use this configutaitons
   ```
   lazy val rootSettings = Seq(
     publishArtifact := false,
     publishArtifact in Test := false
   )

   val root = project 
     .settings(rootSettings: _*)
   ```
   if you want to not publish the root project, add the following to the rootSettings
   ```scala
   skip in publish := true
   ```
