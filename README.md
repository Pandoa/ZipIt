# ZipIt - Documentation
![ZipIt features](https://github.com/Pandoa/ZipIt/blob/master/Images/Features.PNG?raw=true)
# Table of content
1. [Getting started](#1-getting-started)</br>
2. [Blueprints](#2-blueprints)</br>
    2.1. [Zipping](21-zipping)</br>
    2.2. [Unzipping](22-unzipping)</br>
3. [C++](#3-c)</br>
    3.1. [Zipping](31-zipping)</br>
    3.2. [Unzipping](32-unzipping)</br>
4. [Support](#4-support)</br>
# 1. Getting started
ZipIt is separated into 3 parts:
|Class|Functionality|Header
|:---|:---| :---|
|`UZipper`|Zip files into a zip archive.|`Zipper.h`
|`UUnzipper`|Unzip a zip archive.|`Unzipper.h`
|`UZipLibrary`|Uses the Zipper/Unzipper internally for easy to use functions.|`ZipLibrary.h`
For C++, don't forget to include `ZipIt` to your project's module include list in `<Project>.Build.cs`.

# 2. Blueprints
## 2.1. Zipping
### 2.1.1. Zipping a directory
To zip a directory, we can use the helper node to do it in a single node:

![Zip Directory Example](https://github.com/Pandoa/ZipIt/blob/master/Images/BpZipDirectory.png?raw=true)
### 2.1.2. Zipping files from different locations
A node to zip several files from different locations into a single zip archive exists:

![Zip files example](https://github.com/Pandoa/ZipIt/blob/master/Images/BpZipFiles.png?raw=true)
## 2.2. Unzipping
To unzip a file, use the `Unzip Zip Archive` node:

![Unzip zip example](https://github.com/Pandoa/ZipIt/blob/master/Images/BpUnzip.png?raw=true)
# 3. C++
## 3.1. Zipping
### 3.1.1. Zipping a directory
Using ZipLibrary, we can easily zip a whole directory into a single archive:
```cpp
// Don't forget to #include "ZipLibrary.h"

FOnFilesZipped OnFilesZipped;
FOnFileZipped  OnFileZipped;

// Lambda as an example, you can bind a UObject's method with 
// OnFilesZipped.AddUObject(Obj, &UMyClass:Func);
OnFilesZipped.AddLambda([](const bool bSuccess, const int64 NumberOfFilesUnzipped) -> void
{
    if (bSuccess)
    {
        // "NumberOfFilesUnzipped" have been zipped.
    }
    else
    {
        // An error occurred. 
    }
});

// Lambda as an example, you can bind a UObject's method with 
// OnFileZipped.AddUObject(Obj, &UMyClass:Func);
OnFileZipped.AddLambda([](const FString& ArchivePath, const FString& FilePath, const int64 FilesZippedCount, const int64 TotalFilesCount) -> void
{
    // We just zipped the file "FilePath" to "ArchivePath" in the archive.
    // And "FilesZippedCount" / "TotalFilesCount" have been zipped.
});

// Launch the zip task.
UZipLibrary::ZipDirectory
(
    TEXT("./MyDir/"),            // Directory to zip.
    TEXT("./Result/Zipped.zip"), // Resulting .zip archive.
    TEXT("ZipPassword"),         // Archive password
    EZipCompressLevel::Level9,   // Compression level, 9 is max.
    EZipCreationFlag::Overwrite, // Overwrite existing files.
    OnFilesZipped,               // Delegate binded before.
    OnFileZipped                 // Delegate binded before.
);
```
### 3.1.2. Zipping files and folder from different locations
```cpp

```
## 3.2. Unzipping
# 4. Support
