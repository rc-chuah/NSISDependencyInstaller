# NSIS Dependency Installer

**Nullsoft Scriptable Install System (NSIS) Dependency Installer** Can Download And Install Any Dependency Such As .NET, Visual C++ Or SQL Server During Your Application's Installation. In Addition, It Is Easy To Add Your Own Dependencies As Well.

## Installation And Usage

1. Download And Install [NSIS 3.11+](https://nsis.sourceforge.io/Main_Page).
2. Download [This Repository](https://github.com/rc-chuah/NSISDependencyInstaller/archive/main.zip) Or Clone It `git clone https://github.com/rc-chuah/NSISDependencyInstaller`.
3. Open The Extracted _ExampleSetup.nsi_ File.
4. Comment Out Dependency Macro Calls Inside _.onInit_ Function To Disable Installing Them:
    ```nsis
    !insertmacro Dependency_AddVC2013 ; Installed In Example Setup
    ;!insertmacro Dependency_AddVC2013 ; Commented Out And Not Installed In Example Setup
    ```
5. Modify Other Sections Like _[MainSection] [-AdditionalIcons] [-Post] [Uninstall]_ As Necessary.
6. Build The Setup Using Nullsoft Scriptable Install System (NSIS) Compiler.

## Integration

You Can Also Just Include _CodeDependencies.nsh_ File Into Your Setup And Call The Desired _Dependency_Add_ Macros (Some May Need Defining Their Exe File Path Before The Include):

```nsis
!include "CodeDependencies.nsh"

; Define ...

Function .onInit
  ; Add The Dependencies You Need
  !insertmacro Dependency_AddDotNet90
  ; ...
FunctionEnd

; Section ...
```

## Details

You Have Two Ways To Distribute The Dependency Installers. By Default, Most Dependencies Will Be Downloaded From The Official Website. Another Way Is To Pack The Dependency Into A Single Executable Setup Like So:

* Include The Dependency Setup File By Defining The Source:

    ```nsis
    File "dxwebsetup.exe"
    ```

* Call _ExtractTemporaryFile_ Macro Before The Corresponding _Dependency_Add_ Macros

    ```nsis
    !insertmacro ExtractTemporaryFile "dxwebsetup.exe"
    ```

The Dependencies Are Installed Based On The System Architecture. If You Want To Install 32-Bit Dependencies On A 64-Bit System You Can Force 32-Bit Mode Like So:

```nsis
!insertmacro Dependency_ForceX86 True ; Force 32-Bit Install Of Next Dependencies
!insertmacro Dependency_AddVC2013
!insertmacro Dependency_ForceX86 False ; Disable Forced 32-Bit Install Again
```

If You Only Deploy 32-Bit Binaries And Dependencies You Can Also Instead Just Not Define [${If} ${IsNativeARM64} ${ElseIf} ${RunningX64} ${Else} ${EndIf}], [${If} ${IsNativeARM64} ${ElseIf} ${IsNativeAMD64} ${ElseIf} ${IsNativeIA32} ${Else} Abort "Unsupported CPU Architecture!" ${EndIf}], [${If} ${RunningX64} ${Else} ${EndIf}], [SetRegView 64], [SetRegView 32], [${DisableX64FSRedirection}], [${EnableX64FSRedirection}] And [${IsWow64}] In [MainSection], [-AdditionalIcons], [-Post] And [Uninstall] Sections.

## Dependencies

* .NET
    * .NET Framework 3.5 Service Pack 1
    * .NET Framework 4.0
    * .NET Framework 4.5.2
    * .NET Framework 4.6.2
    * .NET Framework 4.7.2
    * .NET Framework 4.8
    * .NET Framework 4.8.1
    * .NET Core 3.1 (Runtime, ASP.NET, Desktop) - **EOL**
    * .NET 5.0 (Runtime, ASP.NET, Desktop) - **EOL**
    * .NET 6.0 (Runtime, ASP.NET, Desktop) - **EOL**
    * .NET 7.0 (Runtime, ASP.NET, Desktop) - **EOL**
    * .NET 8.0.24 (Runtime, ASP.NET, Desktop)
    * .NET 9.0.13 (Runtime, ASP.NET, Desktop)
    * .NET 10.0.3 (Runtime, ASP.NET, Desktop)
* C++
    * Visual C++ 2005 Service Pack 1 Redistributable
    * Visual C++ 2008 Service Pack 1 Redistributable
    * Visual C++ 2010 Service Pack 1 Redistributable
    * Visual C++ 2012 Update 4 Redistributable
    * Visual C++ 2013 Update 5 Redistributable
    * Visual C++ 2015-2022 Redistributable
* SQL
    * SQL Server 2008 R2 Service Pack 2 Express
    * SQL Server 2012 Service Pack 4 Express
    * SQL Server 2014 Service Pack 3 Express
    * SQL Server 2016 Service Pack 3 Express
    * SQL Server 2017 Express
    * SQL Server 2019 Express
    * SQL Server 2022 Express
* Access
    * Access Database Engine 2010
    * Access Database Engine 2016
* DirectX End-User Runtime
* WebView2 Runtime

## Credits

Thanks To The Community For Sharing Many Fixes And Improvements. To Contribute Please [Create A Pull Request](https://github.com/rc-chuah/NSISDependencyInstaller/pulls).

## License

[The Mozilla Public License (MPL) 2.0](https://github.com/rc-chuah/NSISDependencyInstaller/blob/main/LICENSE.md)
