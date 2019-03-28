# Sbt 

## Packaging

using \[sb-native-package\]:

in `build.sbt` : `enablePlugins(JavaAppPackaging, DockerPluging`

in `project/plugins.sbt` : `addSbtPlugin("com.typesafe.sbt" % "sbt-native-packager" % "1.3.15")`

and then run:

`sbt docker:publishLocal` to push the docker image locally

`sbt universal:packageBin` to create `target/universal/yourapp.zip` distribution file

