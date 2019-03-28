<!-- TITLE: .NET Lifecycle Policy -->
<!-- SUBTITLE: AOSC OS includes several .NET packages, like `dotnet-{sdk,runtime}` and `mono`, which are maintained by Microsoft and the .NET community<sup>[1][1],[2][2]</sup>. This issue exists to clarify our lifecycle policy on them. -->

# .NET Core `dotnet-{sdk,runtime}`

.NET Core has 2 [release types](https://dotnet.microsoft.com/platform/support/policy/dotnet-core). Currently, we have LTS (2.1 at time of the writing) in `stable` branch, and Current (2.2 at time of the writing) in `testing` branch.

## Policy

`stable`: Latest LTS release
`testing`: Latest Current release

# Mono `mono`

Mono has 2 [release channels](https://www.mono-project.com/download/). Currently, we have Stable (5.18 at time of the writing) in `stable` and `testing` branch. Mono also has a [Visual Studio channel](https://www.mono-project.com/download/vs/) (5.16 at time of the writing), which is "the most tested but also the least frequently updated".

## Policy

`stable`: Latest Visual Studio release
`testing`: Latest Stable release

# F# + Mono `fsharp`

## Policy

`stable`: Latest release

<!--# MSBuild + Mono `msbuild`

TODO-->

# NuGet + Mono `nuget`

NuGet has many [distribution versions available](https://www.nuget.org/downloads). Currently, we have v4.7 in `stable` branch, and v4.9 (recommended latest at time of the writing) in `testing` branch.

## Policy

`stable`: Latest recommended release

<!--More packages policy expected-->

[1]: https://docs.microsoft.com/en-us/dotnet/core/
[2]: https://www.mono-project.com/