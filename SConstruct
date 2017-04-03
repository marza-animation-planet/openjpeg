import sys
import excons

env = excons.MakeBaseEnv()

out_incdir = excons.OutputBaseDirectory() + "/include"
out_libdir = excons.OutputBaseDirectory() + "/lib"

staticbuild = (excons.GetArgument("static", 1, int) != 0)

if not env.CMakeConfigure("openjpeg", opts={"BUILD_TESTING": 0,
                                            "BUILD_CODEC": 1,
                                            "BUILD_JPWL": 0,
                                            "BUILD_JPIP": 0,
                                            "BUILD_JP3D": 0,
                                            "BUILD_MJ2": 0,
                                            "BUILD_DOC": 0,
                                            "BUILD_JAVA": 0,
                                            "BUILD_VIEWER": 0,
                                            "BUILD_THIRDPARTY": 0,
                                            "BUILD_LUTS_GENERATOR": 0,
                                            "BUILD_SHARED_LIBS": (1 if not staticbuild else 0),
                                            "BUILD_PKGCONFIG_FILES": 0}):
   sys.exit(1)


target = env.CMake(env.CMakeOutputs(),
                   env.CMakeInputs(dirs=["src/bin/common",
                                         "src/bin/jp2",
                                         "src/lib/openjp2"],
                                   patterns=[".h", ".c", ".cmake.in"]))

env.CMakeClean()

env.Alias("openjpeg", target)

excons.SyncCache()


def RequireOpenJpeg(env):
   env.Append(CPPPATH=[out_incdir])
   env.Append(LIBPATH=[out_libdir])
   if staticbuild:
      env.Append(CPPDEFINES=["OPJ_STATIC"])
      if sys.platform != "win32":
         excons.StaticallyLink(env, "openjp2", silent=True)
      else:
         pass
   else:
      env.Append(LIBS=["openjp2"])

export("RequireOpenJpeg")

