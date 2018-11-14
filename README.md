# dotnet-publish-location-repro

Minimal test case for bug 
https://github.com/aspnet/AspNetCore/issues/3437

## Step to reproduce 

- Clone repo
- View original web.config
- Publish web project
- View resulting web.config
  - `<handlers>` and `<aspNetCore>` are incorrectly placed within `<location>` 

## Example

```
C:\>git clone https://github.com/memark/dotnet-publish-location-repro.git
Cloning into 'dotnet-publish-location-repro'...
remote: Enumerating objects: 9, done.
remote: Counting objects: 100% (9/9), done.
remote: Compressing objects: 100% (7/7), done.
remote: Total 9 (delta 0), reused 9 (delta 0), pack-reused 0
Unpacking objects: 100% (9/9), done.

C:\>cd dotnet-publish-location-repro

C:\dotnet-publish-location-repro>type web.config
∩╗┐<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
  </system.webServer>
  <location path="test">
  </location>
</configuration>
C:\dotnet-publish-location-repro>dotnet publish -o dist
Microsoft (R) Build Engine version 15.9.20+g88f5fadfbe for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Restoring packages for C:\dotnet-publish-location-repro\Repro.csproj...
  Generating MSBuild file C:\dotnet-publish-location-repro\obj\Repro.csproj.nuget.g.props.
  Generating MSBuild file C:\dotnet-publish-location-repro\obj\Repro.csproj.nuget.g.targets.
  Restore completed in 979,73 ms for C:\dotnet-publish-location-repro\Repro.csproj.
  Repro -> C:\dotnet-publish-location-repro\bin\Debug\netcoreapp2.1\Repro.dll
  Repro -> C:\dotnet-publish-location-repro\dist\

C:\dotnet-publish-location-repro>type dist\web.config
∩╗┐<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer></system.webServer>
  <location path="test">
    <system.webServer>
      <handlers>
        <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
      </handlers>
      <aspNetCore processPath="dotnet" arguments=".\Repro.dll" stdoutLogEnabled="false" stdoutLogFile=".\logs\stdout" />
    </system.webServer>
  </location>
</configuration>
C:\dotnet-publish-location-repro>dotnet --info
.NET Core SDK (reflecting any global.json):
 Version:   2.1.500
 Commit:    b68b931422

Runtime Environment:
 OS Name:     Windows
 OS Version:  10.0.17134
 OS Platform: Windows
 RID:         win10-x64
 Base Path:   C:\Program Files\dotnet\sdk\2.1.500\

Host (useful for support):
  Version: 2.1.6
  Commit:  3f4f8eebd8

.NET Core SDKs installed:
  1.1.11 [C:\Program Files\dotnet\sdk]
  2.1.302 [C:\Program Files\dotnet\sdk]
  2.1.403 [C:\Program Files\dotnet\sdk]
  2.1.500 [C:\Program Files\dotnet\sdk]

.NET Core runtimes installed:
  Microsoft.AspNetCore.All 2.1.0 [C:\Program Files\dotnet\shared\Microsoft.AspNetCore.All]
  Microsoft.AspNetCore.All 2.1.2 [C:\Program Files\dotnet\shared\Microsoft.AspNetCore.All]
  Microsoft.AspNetCore.All 2.1.4 [C:\Program Files\dotnet\shared\Microsoft.AspNetCore.All]
  Microsoft.AspNetCore.All 2.1.5 [C:\Program Files\dotnet\shared\Microsoft.AspNetCore.All]
  Microsoft.AspNetCore.All 2.1.6 [C:\Program Files\dotnet\shared\Microsoft.AspNetCore.All]
  Microsoft.AspNetCore.App 2.1.0 [C:\Program Files\dotnet\shared\Microsoft.AspNetCore.App]
  Microsoft.AspNetCore.App 2.1.2 [C:\Program Files\dotnet\shared\Microsoft.AspNetCore.App]
  Microsoft.AspNetCore.App 2.1.4 [C:\Program Files\dotnet\shared\Microsoft.AspNetCore.App]
  Microsoft.AspNetCore.App 2.1.5 [C:\Program Files\dotnet\shared\Microsoft.AspNetCore.App]
  Microsoft.AspNetCore.App 2.1.6 [C:\Program Files\dotnet\shared\Microsoft.AspNetCore.App]
  Microsoft.NETCore.App 1.0.13 [C:\Program Files\dotnet\shared\Microsoft.NETCore.App]
  Microsoft.NETCore.App 1.1.10 [C:\Program Files\dotnet\shared\Microsoft.NETCore.App]
  Microsoft.NETCore.App 2.1.2 [C:\Program Files\dotnet\shared\Microsoft.NETCore.App]
  Microsoft.NETCore.App 2.1.5 [C:\Program Files\dotnet\shared\Microsoft.NETCore.App]
  Microsoft.NETCore.App 2.1.6 [C:\Program Files\dotnet\shared\Microsoft.NETCore.App]

To install additional .NET Core runtimes or SDKs:
  https://aka.ms/dotnet-download
```
