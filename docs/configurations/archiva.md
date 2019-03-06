# Archiva

1. Create a user in Archiva to use for deployment \(or use guest if you wish to deploy without a username and password\)
2. The deployment user needs the Role 'Repository Manager' for each repository that you want to deploy to
3. To configure sbt's `publish` add the folowing to build.sbt or the commonSettings for multi project build:

```
publishTo := Some("snapshots" at "http://localhost:8080/repository/snapshots/"),
  credentials += Credentials(
    "Repository Archiva Managed snapshots Repository",
    "localhost",
    "gitlab-ci",
    "gitlab-ci1"
  )
```







