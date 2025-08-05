# ngine-nodule-template (work in progress)
The purpose of this repository is to assist with future game development that depends on the official GDExtension C++ bindings. This is merely a blueprint that serves to provide a nice project structure. Primarily for personal usage, but open to public.

## Introduction
The C++ bindings for GDExtension are built on top of the C GDExtension API and provide a nicer way to "extend" nodes and other built-in classes in Godot using C++. This new system allows the extension of Godot to nearly the same level as statically linked C++ modules.

You can download the included example in the test folder of the godot-cpp repository [on GitHub](https://github.com/godotengine/godot-cpp).

## Setting up the Blueprint
There are a few prerequisites you'll need:
- a Godot 4 executable,
- a C++ compiler,
- SCons as a build tool,
- a copy of the [godot-cpp repository](https://github.com/godotengine/godot-cpp).

See also [Configuring an IDE](https://docs.godotengine.org/en/4.4/contributing/development/configuring_an_ide/index.html#toc-devel-configuring-an-ide) and [Compiling](https://docs.godotengine.org/en/4.4/contributing/development/compiling/index.html#toc-devel-compiling) as the build tools are identical to the ones you need to compile Godot from source.

You can download the [godot-cpp repository](https://github.com/godotengine/godot-cpp) from GitHub or let Git do the work for you. Note that this repository has different branches for different versions of Godot. GDExtensions will not work in older versions of Godot (only Godot 4 and up) and vice versa, so make sure you download the correct branch.

If you are versioning your project using Git, it is recommended to add it as a Git submodule:
```
mkdir root_folder
cd root_folder
git init
git submodule add -b 4.x https://github.com/godotengine/godot-cpp
cd godot-cpp
git submodule update --init --recursive
```

Alternatively, you can also clone it to the project folder:
```
mkdir root_folder
cd root_folder
git clone -b 4.x https://github.com/godotengine/godot-cpp
```

If you cloned the example from the link specified in the introduction, the submodules are not automatically initialized. You will need to execute the following commands:
```
cd gdextension_cpp_example
git submodule update --init
```
This will initialize the godot-cpp repository in your root folder.

You may finally clone this repository to your root folder, as shown:
```
git clone https://github.com/k0a1abyte/godot-cpp-blueprint.git .
```

### WARNING IN REGARDS TO godot-cpp
godot-cpp repository's master branch is only usable with GDExtension from Godot's master branch.

For users of stable branches, switch to the branch matching your target Godot version:
- [4.4](https://github.com/godotengine/godot-cpp/tree/4.4)
- [4.3](https://github.com/godotengine/godot-cpp/tree/4.3)
- [4.2](https://github.com/godotengine/godot-cpp/tree/4.2)
- [4.1](https://github.com/godotengine/godot-cpp/tree/4.1)
- [4.0](https://github.com/godotengine/godot-cpp/tree/4.0)

Or check out the Git tag matching your Godot version (e.g. godot-4.4.1-stable):
```
mkdir root_folder
cd root_folder
git init
git submodule add https://github.com/godotengine/godot-cpp
cd godot-cpp
git checkout godot-4.4.1-stable
cd ..
git submodule update --init --recursive
```

## Building the C++ Bindings
Now that we've downloaded the prerequisites, it is time to build the C++ bindings.

The godot-cpp repository contains a copy of the metadata for the your chosen Godot release, but if you need to build these bindings for a newer version of Godot, call the Godot executable:
```
godot --dump-extension-api
```

The resulting ```extension_api.json``` file will be created in the executable's directory. Copy it to the project folder and add ```custom_api_file=<PATH_TO_FILE>``` to the scons command below.

To generate and compile the bindings, use this command (replacing ```<platform>``` with ```windows```, ```linux``` or ```macos``` depending on your OS):

The build process automatically detects the number of CPU threads to use for parallel builds. To specify a number of CPU threads to use, add -jN at the end of the SCons command line where N is the number of CPU threads to use.

```
cd godot-cpp
scons platform=<platform> custom_api_file=<PATH_TO_FILE>
cd ..
```

This step will take a while. When it is completed, you should have static libraries that can be compiled into your project stored in ```godot-cpp/bin/```. You may need to add ```bits=64``` to the command on Windows or Linux.

## Creating Extensions
Now you're ready to build actual extensions!

Your folder structure should now look like this:
```
root_folder/
|
+--game/                  # project files (this is where your actual game resides! that is, res://)
|
+--godot-cpp/             # C++ bindings
|
+--nodule/               # source code for GDExtension C++ extensions
```
