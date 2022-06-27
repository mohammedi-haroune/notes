# sbt-native-packer

## Instalation
add to `plugins.sbt`: `addSbtPlugin("com.typesafe.sbt" % "sbt-native-packager" % "1.3.17")`

## Configuration
### Archtypes:
- Java Application
- Java Server Application

### Formats:
```
          SbtNativePackager
                +
                |
                |
  +-------+  Universal  +--------|-------------|----------------+
  |             +                |             |                |
  |             |                |             |                |
  |             |                |             |                |
  +             +                +             +                +
Docker    +-+ Linux +-+       Windows     JDKPackager   GraalVM native-image
          |           |
          |           |
          +           +
        Debian       RPM
```
- Universal
- Linux
- Debian
- Rpm
- Docker
- Windows
- JdkPackager
- GraallVM Native image packager


## Create a package
We can generate other packages via the following tasks. Note that each packaging format may needs some additional configuration and native tools available. Here’s a complete list of current formats.

- `universal:packageBin`- Generates a universal zip file
- `universal:packageZipTarball` - Generates a universal tgz file
- `debian:packageBin` - Generates a deb
- `docker:publishLocal` - Builds a Docker image using the local Docker server
- `universal:packageOsxDmg` - Generates a DMG file with the same contents as the universal zip/tgz.
- `windows:packageBin` - Generates an MSI

## Running the application
In order to pass application arguments you need to separate the jvm arguments from the application arguments with --. For example
