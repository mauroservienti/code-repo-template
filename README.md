# Repository template for code repositories

This template contains the following:

- Default `.editorconfig` and `.gitignore` for .NET projects
- Dependabot auto-merge GitHub Action
- [MarkdownSnippets](https://github.com/SimonCropp/MarkdownSnippets) GitHub Action
- An issue template to complete the repository configuration once the new repository has been generated
- AppVeyor build configuration
- Build system using [SimpleExec](https://github.com/adamralph/simple-exec/) and [BullsEye](https://github.com/adamralph/bullseye)

Sample build script:

<!-- snippet: default-build-script -->
<a id='snippet-default-build-script'></a>
```cs
public static void Main(string[] args)
{
    var sdk = new DotnetSdkManager();

    Target("default", DependsOn("test"));

    Target("build",
        Directory.EnumerateFiles("src", "*.sln", SearchOption.AllDirectories),
        solution => Run(sdk.GetDotnetCliPath(), $"build \"{solution}\" --configuration Release"));

    Target("test", DependsOn("build"),
        Directory.EnumerateFiles("src", "*Tests.csproj", SearchOption.AllDirectories),
        proj => Run(sdk.GetDotnetCliPath(), $"test \"{proj}\" --configuration Release --no-build"));
    
    RunTargetsAndExit(args);
}
```
<sup><a href='/targets/Program.cs#L8-L25' title='Snippet source file'>snippet source</a> | <a href='#snippet-default-build-script' title='Start of snippet'>anchor</a></sup>
<!-- endSnippet -->
