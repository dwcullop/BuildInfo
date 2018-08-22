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

However, you may prefer to harness the power of Git by adding my simple repo as a submodule of your repo, which will allow you to easily get any updates and improvements that may come up.


## Example

Here is actual output from the code:
```csharp
/////////////////////////////////////////////////////////////////////////////////////
// This file is auto-generated!  Do not make any changes because they will be overwritten!
// Edit the corresponding .tt file if you want to make changes
/////////////////////////////////////////////////////////////////////////////////////
using System;
using System.Reflection;

namespace DareWare.SomeProject
{
    public static class BuildInfo
    {
        private const long              BUILD_DATE_BINARY_UTC       = 0x48d6085b065f4766;    // August 22, 2018 6:13:56.145751 PM UTC

        private static AssemblyName     BuildAssemblyName { get; }  = Assembly.GetExecutingAssembly().GetName();
        public static DateTimeOffset    BuildDateUtc { get; }       = DateTime.FromBinary(BUILD_DATE_BINARY_UTC);
        public static string            ModuleText { get; }         =  BuildAssemblyName.Name;
        public static string            VersionText { get; }        = "v" + BuildAssemblyName.Version.ToString()
#if DEBUG
                                            + " [DEBUG]"
#endif
                                            ;

        public static string            BuildDateText { get; }      = "August 22, 2018 6:13:56 PM UTC";
        public static string            DisplayText { get; }        = $"{ModuleText} {VersionText} (Build Date: {BuildDateText})";
    }
}
```




