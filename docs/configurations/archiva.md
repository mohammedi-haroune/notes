# Archiva

1. Create a user in Archiva to use for deployment \(or use guest if you wish to deploy without a username and password\)
2. The deployment user needs the Role 'Repository Manager' for each repository that you want to deploy to
3. To configure sbt's `publish` add the folowing to build.sbt or the commonSettings for multi project build:

```scala
publishTo := Some("snapshots" at "http://localhost:8080/repository/snapshots/"),
  credentials += Credentials(
    "Repository Archiva Managed snapshots Repository",
    "localhost",
    "gitlab-ci",
    "gitlab-ci1"
  )
```

## run it using docker
```
docker run -v ~/existing-archiva-base:/var/archiva -p 8080:8080 -d ninjaben/archiva-docker
```

Users should be: confirmed, not blocked and not required to change password
