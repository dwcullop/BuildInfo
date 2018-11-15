# Easy and Automated Extended Build Info for Dot Net / C#
## Introduction

If you've ever wanted a way to programmatically have access to Build Information (build date, branch, etc.) but needed it to be easy to maintain, flexible, and best of all, automated, then this is your lucky day.

## What You Get

It's always been trivial to get the version number of any Dot Net Assembly.  But what if you wanted more detailed information, such as the build date, git branch or hash, or whatever else?  So you may write something like this:

```csharp
// SomeProject v0.9.0.0 (Build Date: August 9, 2018 7:57:21 PM UTC)
Console.WriteLine( BuildInfo.DisplayText ); 
```

## How It Works

This amazingly easy to use code leverages the power of T4, a vastly underused part of DotNet development, which allows one to write C# code that emits C# code when it's built.  This code captures the current date/time and other information about the build and generates a static C# class that exposes this information as properties.

## How To Use It

All you really need to do is copy the `BuildInfo.tt` file to your project and tweak it as necessary.  When you build the project, it will emit a `BuildInfo.cs` file that will be compiled with the rest of your code.

However, you may prefer to harness the power of Git by adding my simple repo as a submodule of your repo, which will allow you to easily get any updates and improvements that may come up.  To do that, use these steps:

 1) Add this project as a submodule to your project with this command:

     `git submodule add -f https://github.com/dwcullop/BuildInfo BuildInfo`
     
 2) Add the `BuildInfo\BuildInfo.tt`  file to your C# project
 3) Enjoy the automated buildinfo goodness

[More information on Git Submodules](https://git-scm.com/docs/git-submodule)

If you want to include the Git Hash of the current checked out branch in the information that is available via the static class, use the `feature/githash` branch, like this:

    git submodule add -b feature/githash -f https://github.com/dwcullop/BuildInfo BuildInfo
    
**NOTICE**: If you do not use the submodule option, make sure you specify the `BuildInfo.cs` in your `.gitignore` file.  It is usually considered bad-form to check generated files into source control.

### :bulb: Pro Tip

To ensure that the Build Timestamp is updated on every single build, I recommend using one of the Visual Studio extensions that forces all T4 code to re-execute on every build, [such as this one](https://marketplace.visualstudio.com/items?itemName=BennorMcCarthy.AutoT4).

## Warranties

This works for me using Visual Studio 2015 and 2017.  I hope it works for you, but I can't make any promises.  Your mileage may vary.  Offer void where prohibited.  If you're not here, you're probably someplace else.  Do not taunt happy fun ball.  If at first you DO succeed, try not to look surprised.

If you make any useful changes, please submit a PR so everyone can enjoy the benefits.


## Example

Here is actual output from the code (from the branch that adds the Git information):
```csharp
/////////////////////////////////////////////////////////////////////////////////////////////////////////////
// NOTICE: DO NOT EDIT THIS FILE!
// 
// This file is autogenerated and your changes will be OVERWRITTEN! 
// Edit the corresponding .tt file instead.
//
// Or, better yet, make a lasting contribution by submitting a Pull Request:  
//      https://github.com/dwcullop/BuildInfo
/////////////////////////////////////////////////////////////////////////////////////////////////////////////
using System;
using System.Reflection;

namespace DareWare.SWGoH.ModMaster
{
    public static class BuildInfo
    {
        private const long              BUILD_DATE_BINARY_UTC       = 0x48d60eb549dbbe85;    // August 30, 2018 8:15:11.051123 PM UTC

        private static AssemblyName     BuildAssemblyName { get; }  = Assembly.GetExecutingAssembly().GetName();
        public static DateTimeOffset    BuildDateUtc { get; }       = DateTime.FromBinary(BUILD_DATE_BINARY_UTC);
        public static string            ModuleText { get; }         = BuildAssemblyName.Name;
        public static string            CommitHash { get; }         = "d76760b57ceb7b3b3e7cdfa9b0427cdf1bac901a";
        public static string            CommitHashAbbrev { get; }   = "d76760b";
        public static string            VersionText { get; }        = "v" + BuildAssemblyName.Version.ToString()
                                                                                + "." + CommitHashAbbrev
#if DEBUG
                                                                                + " [DEBUG]"
#endif
                                                                                ;

        public static string            BuildDateText { get; }      = "Thursday, August 30, 2018 8:15:11 PM UTC";
        public static string            DisplayText { get; }        = $"{ModuleText} {VersionText} (Build Date: {BuildDateText})";
    }
}
```




