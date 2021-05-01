import sys
import excons
import SCons.Script

env = excons.MakeBaseEnv()

out_incdir  = excons.OutputBaseDirectory() + "/include"
out_libdir  = excons.OutputBaseDirectory() + "/lib"
staticbuild = (excons.GetArgument("openjpeg-static", 1, int) != 0)
libsuffix = excons.GetArgument("openjpeg-suffix", "")

def OpenjpegName():
   name = "openjp2" + libsuffix
   if sys.platform == "win32" and staticbuild:
       name = "lib" + name
   return name

def OpenjpegPath():
   name = OpenjpegName()
   if sys.platform == "win32":
      libname = name + ".lib"
   else:
      libname = "lib" + name + (".a" if staticbuild else excons.SharedLibraryLinkExt())
   return out_libdir + "/" + libname

def RequireOpenjpeg(env):
   if staticbuild:
      env.Append(CPPDEFINES=["OPJ_STATIC"])
   env.Append(CPPPATH=[out_incdir])
   env.Append(LIBPATH=[out_libdir])
   excons.Link(env, OpenjpegPath(), static=staticbuild, force=True, silent=True)

prjs = [
   {  "name": "openjpeg",
      "type": "cmake",
      "cmake-opts": {"BUILD_JPWL": 0,
                     "BUILD_JPIP": 0,
                     "BUILD_JP3D": 0,
                     "BUILD_MJ2": 0,
                     "BUILD_DOC": 0,
                     "BUILD_JAVA": 0,
                     "OPJ_USE_DSYMUTIL": 0,
                     "BUILD_LUTS_GENERATOR": 0,
                     "BUILD_BENCH_DWT": 0,
                     "BUILD_SHARED_LIBS": (1 if not staticbuild else 0),
                     "BUILD_PKGCONFIG_FILES": 0,
                     "BUILD_TESTING": 0,
                     "BUILD_CODEC": excons.GetArgument("openjpeg-tools", 0, int),
                     "BUILD_THIRDPARTY": 0,
                     "BUILD_VIEWER": 0},
      "cmake-cfgs": excons.CollectFiles(["src/bin/common", "src/bin/jp2", "src/lib/openjp2"], patterns=["CMakeLists.txt"]),
      "cmake-srcs": excons.CollectFiles(["src/bin/common", "src/bin/jp2", "src/lib/openjp2"], patterns=["*.c"]),
      "cmake-outputs": ["include/openjpeg-2.3/openjpeg.h",
                        "include/openjpeg-2.3/opj_config.h",
                        "include/openjpeg-2.3/opj_stdint.h",
                        OpenjpegPath()]
   }
]

excons.AddHelpOptions(openjpeg="""OPENJPEG OPTIONS
  openjpeg-static=0|1   : Toggle between static and shared library build [1]
  openjpeg-suffix=<str> : OpenJpeg Library name suffix                   ['']
  openjpeg-tools=0|1    : Build OpenJpeg command line tools              [0]
                          (requires JPEG, PNG and TIFF libraries)""")
excons.DeclareTargets(env, prjs)

SCons.Script.Export("OpenjpegName OpenjpegPath RequireOpenjpeg")
