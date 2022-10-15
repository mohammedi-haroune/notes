# Sbt

## Install
```bash
echo "deb https://dl.bintray.com/sbt/debian /" | sudo tee -a /etc/apt/sources.list.d/sbt.list
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 2EE0EA64E40A89B84B2DF73499E82A75642AC823
sudo apt-get update
sudo apt-get install sbt
```

## Packaging

using \[sb-native-package\]:

in `build.sbt` : `enablePlugins(JavaAppPackaging, DockerPluging`

in `project/plugins.sbt` : `addSbtPlugin("com.typesafe.sbt" % "sbt-native-packager" % "1.3.15")`

and then run:

`sbt docker:publishLocal` to push the docker image locally

`sbt universal:packageBin` to create `target/universal/yourapp.zip` distribution file
