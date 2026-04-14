# DynamoVisualProgramming.Revit
NuGet package repository for DynamoVisualProgramming.Revit

This package provides the Dynamo for Revit libraries needed to build Dynamo packages that target Revit. It contains the following assemblies:
- `RevitNodes.dll`
- `RevitServices.dll`
- `DynamoRevitDS.dll`
- `DSRevitNodesUI.dll`

## Usage

### Package Reference in `.csproj`

Reference this package in your `.csproj` using a version that matches your target Revit version. The recommended approach (as used by projects like [Rhythm for Dynamo](https://github.com/johnpierson/RhythmForDynamo)) is to define the Dynamo/Revit version as a property and use a wildcard for the patch version:

```xml
<PackageReference Include="DynamoVisualProgramming.Revit" Version="$(DynRevitVersion).*" />
```

Where `$(DynRevitVersion)` is set per build configuration. For example:

```xml
<PropertyGroup Condition="$(Configuration.Contains('R25'))">
    <DynRevitVersion>3.0</DynRevitVersion>
    <RevitVersion>2025</RevitVersion>
    <TargetFramework>net8.0-windows</TargetFramework>
</PropertyGroup>

<PropertyGroup Condition="$(Configuration.Contains('R24'))">
    <DynRevitVersion>2.19</DynRevitVersion>
    <RevitVersion>2024</RevitVersion>
    <TargetFramework>net48</TargetFramework>
</PropertyGroup>
```

### Version Matrix

Use the table below to find the correct package version for your target Revit version:

| Revit Version | DynamoRevit Version | Package Version (major.minor) |
|---------------|---------------------|-------------------------------|
| 2020.0        | 2.3                 | 2.3.x                         |
| 2020.1        | 2.6                 | 2.6.x                         |
| 2020.2        | 2.6                 | 2.6.x                         |
| 2021.0        | 2.5                 | 2.5.x                         |
| 2021.1        | 2.6                 | 2.6.x                         |
| 2022.0        | 2.12                | 2.12.x                        |
| 2022.1        | 2.12                | 2.12.x                        |
| 2023.0        | 2.13                | 2.13.x                        |
| 2023.1        | 2.16                | 2.16.x                        |
| 2024.0        | 2.17                | 2.17.x                        |
| 2024.1        | 2.18                | 2.18.x                        |
| 2024.2        | 2.19                | 2.19.x                        |
| 2025.0        | 3.0                 | 3.0.x                         |
| 2026.x        | 3.6                 | 3.6.x                         |
| 2027.0        | 27.0                | 27.0.x                        |

### Full Multi-Version `.csproj` Example

Below is a complete example showing how to set up a project that targets multiple Revit versions, following the same pattern used by [Rhythm for Dynamo](https://github.com/johnpierson/RhythmForDynamo/blob/master/src/RhythmRevit/RhythmRevit.csproj):

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Library</OutputType>
    <Configurations>
      Debug R24;Debug R25;Debug R26;
      Release R24;Release R25;Release R26
    </Configurations>
  </PropertyGroup>

  <!-- Revit 2024 -->
  <PropertyGroup Condition="$(Configuration.Contains('R24'))">
    <DynRevitVersion>2.19</DynRevitVersion>
    <RevitVersion>2024</RevitVersion>
    <TargetFramework>net48</TargetFramework>
  </PropertyGroup>

  <!-- Revit 2025 -->
  <PropertyGroup Condition="$(Configuration.Contains('R25'))">
    <DynRevitVersion>3.0</DynRevitVersion>
    <RevitVersion>2025</RevitVersion>
    <TargetFramework>net8.0-windows</TargetFramework>
  </PropertyGroup>

  <!-- Revit 2026 -->
  <PropertyGroup Condition="$(Configuration.Contains('R26'))">
    <DynRevitVersion>3.6</DynRevitVersion>
    <RevitVersion>2026</RevitVersion>
    <TargetFramework>net8.0-windows</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="DynamoVisualProgramming.Revit" Version="$(DynRevitVersion).*" />
  </ItemGroup>

</Project>
```

### Package Source

The packages are published to [GitHub Packages](https://github.com/johnpierson/DynamoVisualProgramming.Revit/pkgs/nuget/DynamoVisualProgramming.Revit).

#### Adding the GitHub Packages source

Before restoring or installing, add the GitHub Packages feed. You will need a [GitHub Personal Access Token (classic)](https://github.com/settings/tokens) with the `read:packages` scope.

**Via the .NET CLI:**

```shell
dotnet nuget add source "https://nuget.pkg.github.com/johnpierson/index.json" \
  --name "github-johnpierson" \
  --username YOUR_GITHUB_USERNAME \
  --password YOUR_GITHUB_PAT \
  --store-password-in-clear-text
```

**Via a `nuget.config` file** (recommended for checked-in projects):

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <packageSources>
    <add key="github-johnpierson"
         value="https://nuget.pkg.github.com/johnpierson/index.json" />
  </packageSources>
  <packageSourceCredentials>
    <github-johnpierson>
      <add key="Username" value="YOUR_GITHUB_USERNAME" />
      <add key="ClearTextPassword" value="YOUR_GITHUB_PAT" />
    </github-johnpierson>
  </packageSourceCredentials>
</configuration>
```

> **Tip:** Store credentials as environment variables or CI secrets rather than committing them. You can reference them with `%GITHUB_PAT%` (Windows) or `$GITHUB_PAT` (Linux/macOS) in your `nuget.config`.

#### Installing a specific version

Via the .NET CLI:

```shell
dotnet add package DynamoVisualProgramming.Revit --version 2.19.* --source github-johnpierson
```

Via the NuGet Package Manager Console in Visual Studio:

```powershell
Install-Package DynamoVisualProgramming.Revit -Version 2.19.3.10292 -Source github-johnpierson
```

