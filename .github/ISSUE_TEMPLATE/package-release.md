---
name: Package release
about: Package release template
title: Release [version]
labels: ''
assignees: ''

---

Releases are driven by tags: CI pushes pre-release tags (`X.Y.Z-alpha.N`, `-beta.N`, `-rc.N`) to the [Feedz.io pre-releases feed](https://feedz.io/org/mauroservienti/repository/pre-releases) (`https://f.feedz.io/mauroservienti/pre-releases/nuget/index.json`), and stable tags (`X.Y.Z`) to NuGet.org.

- Prepare the release
  - [ ] update dependabot configuration to consider any newly created `release-X.Y` branch
  - [ ] optionally, tag a pre-release and try the package from Feedz.io
- Release
  - [ ] tag the repo with `X.Y.Z`
  - [ ] check that the CI release job pushed the package to [NuGet.org](https://www.nuget.org/packages/<PackageId>)
- Release notes
  - [ ] create a GitHub release from the tag (it doesn't publish anything)
  - [ ] edit release notes as necessary, e.g., to mention contributors
  - [ ] publish release notes
- Clean-up
  - [ ] close this issue
  - [ ] close the milestone
