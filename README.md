# Repository template for code repositories

This template contains the following:

- Default `.editorconfig` and `.gitignore` for .NET projects
- Dependabot GitHub Action to enable auto-merge for Dependabot PRs
- [MarkdownSnippets](https://github.com/SimonCropp/MarkdownSnippets) GitHub Action
- An issue template to complete the repository configuration once the new repository has been generated
- CI GitHub Actions to build, run tests, and release from tags: pre-release tags go to Feedz.io, stable tags to NuGet.org with trusted publishing
- The included sample project defines item groups to produce NuGet packages

## Package sources

`src/nuget.config` restores packages from NuGet.org only. To restore from another feed, for example a Feedz.io pre-releases feed:

1. Add it to `src/nuget.config`:

   ```xml
   <add key="feedz-pre-releases" value="https://f.feedz.io/<org>/<repository>/nuget/index.json" />
   ```

2. Declare it as a registry in `.github/dependabot.yml`, and list it in the NuGet update. Dependabot can only reach declared registries: an undeclared feed makes every NuGet update job fail with `private_source_authentication_failure`.

   ```yaml
   registries:
     feedz-pre-releases:
       type: nuget-feed
       url: https://f.feedz.io/<org>/<repository>/nuget/index.json
   updates:
   - package-ecosystem: nuget
     directory: "/src"
     registries:
     - feedz-pre-releases
   ```

3. Optionally, map packages to sources with [package source mapping](https://learn.microsoft.com/nuget/consume-packages/package-source-mapping), to restore only the packages that need the feed from it, and the others from NuGet.org only. Mapping everything to `*` restricts nothing. A package comes only from the sources mapping its most specific pattern, so a prefix mapped to the feed alone blocks that prefix's stable versions on NuGet.org: map it to both.

   ```xml
   <packageSourceMapping>
     <packageSource key="nuget.org">
       <package pattern="*" />
       <package pattern="MyPackages.*" />
     </packageSource>
     <packageSource key="feedz-pre-releases">
       <package pattern="MyPackages.*" />
     </packageSource>
   </packageSourceMapping>
   ```

   With source mapping, every source a package can come from needs a mapping, including sources defined outside the repository, for example in a user-level `nuget.config`.

