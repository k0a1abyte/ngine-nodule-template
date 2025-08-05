#!/usr/bin/env python
import os
import sys

# requires godot-cpp (official GDExtension C++ bindings) to be imported
env = SConscript("godot-cpp/SConstruct")

# For reference:
# - CCFLAGS are compilation flags shared between C and C++
# - CFLAGS are for C-specific compilation flags
# - CXXFLAGS are for C++-specific compilation flags
# - CPPFLAGS are for pre-processor flags
# - CPPDEFINES are for pre-processor defines
# - LINKFLAGS are for linking flags

# tweak this if you want to use different folders, or more folders, to store your source code in
env.Append(CPPPATH=["nodule/"])

# removed because it restricted all .cpp files to be purely in the extensions folder (i.e., no sub folders)
# sources = Glob("nodule/*.cpp")
sources = []

# obtain every .cpp file located in the extensions folder (may not be efficient but it works)
for working_directory, directory_names, file_names in os.walk("nodule", topdown=True):
    for file_name in file_names:
        if file_name.endswith(".cpp"):
            sources.append(os.path.normpath(os.path.join(working_directory, file_name)))

# build options dependent on chosen platform
if env["platform"] == "macos":
    library = env.SharedLibrary(
        "game/bin/libgdexample.{}.{}.framework/libgdexample.{}.{}".format(
            env["platform"], env["target"], env["platform"], env["target"]
        ),
        source=sources,
    )
elif env["platform"] == "ios":
    if env["ios_simulator"]:
        library = env.StaticLibrary(
            "game/bin/libgdexample.{}.{}.simulator.a".format(env["platform"], env["target"]),
            source=sources,
        )
    else:
        library = env.StaticLibrary(
            "game/bin/libgdexample.{}.{}.a".format(env["platform"], env["target"]),
            source=sources,
        )
else:
    library = env.SharedLibrary(
        "game/bin/libgdexample{}{}".format(env["suffix"], env["SHLIBSUFFIX"]),
        source=sources,
    )

Default(library)
