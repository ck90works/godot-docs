.. _doc_c_sharp:

C# basics
=========

Introduction
------------

.. warning:: C# support is a new feature available since Godot 3.0.
             As such, you may still run into some issues, or find spots
             where the documentation could be improved.
             Please report issues with C# in Godot on the
             `engine GitHub page <https://github.com/godotengine/godot/issues>`_,
             and any documentation issues on the
             `documentation GitHub page <https://github.com/godotengine/godot-docs/issues>`_.

This page provides a brief introduction to C#, both what it is and
how to use it in Godot. Afterwards, you may want to look at
:ref:`how to use specific features <doc_c_sharp_features>`, read about the
:ref:`differences between the C# and the GDScript API <doc_c_sharp_differences>`
and (re)visit the :ref:`Scripting section <doc_scripting>` of the
step-by-step tutorial.

C# is a high-level programming language developed by Microsoft. In Godot,
it is implemented with the Mono 6.x .NET framework, including full support
for C# 8.0. Mono is an open source implementation of Microsoft's .NET Framework
based on the ECMA standards for C# and the Common Language Runtime.
A good starting point for checking its capabilities is the
`Compatibility <http://www.mono-project.com/docs/about-mono/compatibility/>`_
page in the Mono documentation.

.. note:: This is **not** a full-scale tutorial on the C# language as a whole.
        If you aren't already familiar with its syntax or features,
        see the
        `Microsoft C# guide <https://docs.microsoft.com/en-us/dotnet/csharp/index>`_
        or look for a suitable introduction elsewhere.

Setting up C# for Godot
-----------------------

Prerequisites
~~~~~~~~~~~~~

Install the latest stable version of
`.NET Core SDK <https://dotnet.microsoft.com/download/dotnet-core>`__
(3.1 as of writing).

As of Godot 3.2.3, installing Mono SDK is not a requirement anymore,
except it is required if you are building the engine from source.

Godot bundles the parts of Mono needed to run already compiled games,
however Godot does not include the tools required to build and compile
games, such as MSBuild. These tools need to be installed separately.
The required tools are included in the .NET Core SDK. MSBuild is also
included in the Mono SDK, but it can't build C# projects with the new
``csproj`` format, therefore .NET Core SDK is required for Godot 3.2.3+.

In summary, you must have installed .NET Core SDK
**and** the Mono-enabled version of Godot.

Additional notes
~~~~~~~~~~~~~~~~

Be sure to install the 64-bit version of the SDK(s)
if you are using the 64-bit version of Godot.

If you are building Godot from source, install the latest stable version of
`Mono <https://www.mono-project.com/download/stable/>`__, and make sure to
follow the steps to enable Mono support in your build as outlined in the
:ref:`doc_compiling_with_mono` page.

Configuring an external editor
------------------------------

C# support in Godot's built-in script editor is minimal. Consider using an
external IDE or editor, such as  `Visual Studio Code <https://code.visualstudio.com/>`__
or MonoDevelop. These provide autocompletion, debugging, and other
useful features for C#. To select an external editor in Godot,
click on **Editor → Editor Settings** and scroll down to
**Mono**. Under **Mono**, click on **Editor**, and select your
external editor of choice. Godot currently supports the following
external editors:

- Visual Studio 2019
- Visual Studio Code
- MonoDevelop
- Visual Studio for Mac
- JetBrains Rider

See the following sections for how to configure an external editor:

JetBrains Rider
~~~~~~~~~~~~~~~

After reading the "Prerequisites" section, you can download and install
`JetBrains Rider <https://www.jetbrains.com/rider/download>`__.

In Godot's **Editor → Editor Settings** menu:

- Set **Mono** -> **Editor** -> **External Editor** to **JetBrains Rider**.
- Set **Mono** -> **Builds** -> **Build Tool** to **dotnet CLI**.

In Rider:

- Set **MSBuild version** to **.NET Core**.
- Install the **Godot support** plugin.

Visual Studio Code
~~~~~~~~~~~~~~~~~~

After reading the "Prerequisites" section, you can download and install
`Visual Studio Code <https://code.visualstudio.com/download>`__ (aka VS Code).

In Godot's **Editor → Editor Settings** menu:

- Set **Mono** -> **Editor** -> **External Editor** to **Visual Studio Code**.

In Visual Studio Code:

- Install the `C# <https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp>`__ extension.
- Install the `godot-tools <https://marketplace.visualstudio.com/items?itemName=geequlim.godot-tools>`__ extension.
- Install the `C# Tools for Godot <https://marketplace.visualstudio.com/items?itemName=neikeq.godot-csharp-vscode>`__ extension.

Next, follow the instructions found in the
:ref:`doc_c_sharp_configuring_vs_code_for_debugging` section below.

Visual Studio (Windows only)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Download and install the latest version of
`Visual Studio <https://visualstudio.microsoft.com/downloads/>`__.
Visual Studio will include the required SDKs if you have the correct
workloads selected, so you don't need to manually install the things
listed in the "Prerequisites" section.

While installing Visual Studio, select these workloads:

- Mobile development with .NET
- .NET Core cross-platform development

In Godot's **Editor → Editor Settings** menu:

- Set **Mono** -> **Editor** -> **External Editor** to **Visual Studio**.

Next, follow the instructions found in the
:ref:`doc_c_sharp_configuring_vs_2019_for_debugging` section below.

Creating a C# script
--------------------

After you successfully set up C# for Godot, you should see the following option
when selecting **Attach Script** in the context menu of a node in your scene:

.. image:: img/attachcsharpscript.png

Note that while some specifics change, most concepts work the same
when using C# for scripting. If you're new to Godot, you may want to follow
the tutorials on :ref:`doc_scripting` at this point.
While some places in the documentation still lack C# examples, most concepts
can be transferred easily from GDScript.

Project setup and workflow
--------------------------

When you create the first C# script, Godot initializes the C# project files
for your Godot project. This includes generating a C# solution (``.sln``)
and a project file (``.csproj``), as well as some utility files and folders
(``.mono`` and ``Properties/AssemblyInfo.cs``).
All of these but ``.mono`` are important and should be committed to your
version control system. ``.mono`` can be safely added to the ignore list of your VCS.
When troubleshooting, it can sometimes help to delete the ``.mono`` folder
and let it regenerate.

Example
-------

Here's a blank C# script with some comments to demonstrate how it works.

.. code-block:: csharp

    using Godot;
    using System;

    public class YourCustomClass : Node
    {
        // Member variables here, example:
        private int a = 2;
        private string b = "textvar";

        public override void _Ready()
        {
            // Called every time the node is added to the scene.
            // Initialization here.
            GD.Print("Hello from C# to Godot :)");
        }

        public override void _Process(float delta)
        {
            // Called every frame. Delta is time since the last frame.
            // Update game logic here.
        }
    }

As you can see, functions normally in global scope in GDScript like Godot's
``print`` function are available in the ``GD`` class which is part of
the ``Godot`` namespace. For a list of methods in the ``GD`` class, see the
class reference pages for
:ref:`@GDScript <class_@gdscript>` and :ref:`@GlobalScope <class_@globalscope>`.

.. note::
    Keep in mind that the class you wish to attach to your node should have the same
    name as the ``.cs`` file. Otherwise, you will get the following error
    and won't be able to run the scene:
    *"Cannot find class XXX for script res://XXX.cs"*

General differences between C# and GDScript
-------------------------------------------

The C# API uses ``PascalCase`` instead of ``snake_case`` in GDScript/C++.
Where possible, fields and getters/setters have been converted to properties.
In general, the C# Godot API strives to be as idiomatic as is reasonably possible.

For more information, see the :ref:`doc_c_sharp_differences` page.

.. warning::

    You need to (re)build the project assemblies whenever you want to see new
    exported variables or signals in the editor. This build can be manually
    triggered by clicking the word **Mono** at the bottom of the editor window
    to reveal the Mono panel, then clicking the **Build Project** button.

    You will also need to rebuild the project assemblies to apply changes in
    "tool" scripts.

Current gotchas and known issues
--------------------------------

As C# support is quite new in Godot, there are some growing pains and things
that need to be ironed out. Below is a list of the most important issues
you should be aware of when diving into C# in Godot, but if in doubt, also
take a look over the official
`issue tracker for Mono issues <https://github.com/godotengine/godot/labels/topic%3Amono>`_.

- Writing editor plugins is possible, but it is currently quite convoluted.
- State is currently not saved and restored when hot-reloading,
  with the exception of exported variables.
- Attached C# scripts should refer to a class that has a class name
  that matches the file name.
- There are some methods such as ``Get()``/``Set()``, ``Call()``/``CallDeferred()``
  and signal connection method ``Connect()`` that rely on Godot's ``snake_case`` API
  naming conventions.
  So when using e.g. ``CallDeferred("AddChild")``, ``AddChild`` will not work because
  the API is expecting the original ``snake_case`` version ``add_child``. However, you
  can use any custom properties or methods without this limitation.


As of Godot 3.2.2, exporting Mono projects is supported for desktop platforms
(Linux, Windows and macOS), Android, HTML5, and iOS. The only platform not
supported yet is UWP.

Performance of C# in Godot
--------------------------

According to some preliminary `benchmarks <https://github.com/cart/godot3-bunnymark>`_,
the performance of C# in Godot — while generally in the same order of magnitude
— is roughly **~4×** that of GDScript in some naive cases. C++ is still
a little faster; the specifics are going to vary according to your use case.
GDScript is likely fast enough for most general scripting workloads.
C# is faster, but requires some expensive marshalling when talking to Godot.

Using NuGet packages in Godot
-----------------------------

`NuGet <https://www.nuget.org/>`_ packages can be installed and used with Godot,
as with any C# project. Many IDEs are able to add packages directly.
They can also be added manually by adding the package reference in
the ``.csproj`` file located in the project root:

.. code-block:: xml
    :emphasize-lines: 2

        <ItemGroup>
            <PackageReference Include="Newtonsoft.Json">
              <Version>11.0.2</Version>
            </PackageReference>
        </ItemGroup>
        ...
    </Project>

.. note::
    By default, tools like NuGet put ``Version`` as an attribute of the ```PackageReference``` Node. **You must manually create a Version node as shown above.**  This is because the version of MSBuild used requires this. (This will be fixed in Godot 4.0.)

Whenever packages are added or modified, run ``nuget restore`` (*not* ``dotnet restore``) in the root of the
project directory. To ensure that NuGet packages will be available for
msbuild to use, run:

.. code-block:: none

    msbuild /t:restore

Profiling your C# code
----------------------

- `Mono log profiler <https://www.mono-project.com/docs/debug+profile/profile/profiler/>`_ is available for Linux and macOS. Due to a Mono change, it does not work on Windows currently.
- External Mono profiler like `JetBrains dotTrace <https://www.jetbrains.com/profiler/>`_ can be used as described `here <https://github.com/godotengine/godot/pull/34382>`_.

.. _doc_c_sharp_configuring_vs_2019_for_debugging:

Configuring VS 2019 for debugging
---------------------------------

.. note::

    Godot has built-in support for workflows involving several popular C# IDEs.
    Built-in support for Visual Studio will be including in future versions,
    but in the meantime, the steps below can let you configure VS 2019 for use
    with Godot C# projects.

1. Install VS 2019 with ``.NET desktop development`` and ``Desktop development with C++`` workloads selected.
2. **Ensure that you do not have Xamarin installed.** Do not choose the ``Mobile development with .NET`` workload. Xamarin changes the DLLs used by MonoDebugger, which breaks debugging.
3. Install the `VSMonoDebugger extension <https://marketplace.visualstudio.com/items?itemName=GordianDotNet.VSMonoDebugger0d62>`_.
4. In VS 2019 --> Extensions --> Mono --> Settings:

   - Select ``Debug/Deploy to local Windows``.
   - Leave ``Local Deploy Path`` blank.
   - Set the ``Mono Debug Port`` to the port in Godot --> Project --> Project Settings --> Mono --> Debugger Agent.
   - Also select ``Wait for Debugger`` in the Godot Mono options. `This Godot Addon <https://godotengine.org/asset-library/asset/435>`_ may be helpful.

5. Run the game in Godot. It should hang at the Godot splash screen while it waits for your debugger to attach.
6. In VS 2019, open your project and choose Extensions --> Mono --> Attach to Mono Debugger.

.. _doc_c_sharp_configuring_vs_code_for_debugging:

Configuring Visual Studio Code for debugging
--------------------------------------------

To configure debugging, open Visual Studio Code and download the Mono Debug extension from
Microsoft and the Godot extension by Ignacio. Then open the Godot project folder in VS Code.
Go to the Run tab and click on **create a launch.json file**. Select **C# Godot** from the dropdown
menu. Now, when you start the debugger in VS Code your Godot project will run.
