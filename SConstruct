import sys
import excons

env = excons.MakeBaseEnv()

out_incdir  = excons.OutputBaseDirectory() + "/include"
out_libdir  = excons.OutputBaseDirectory() + "/lib"
staticbuild = (excons.GetArgument("static", 1, int) != 0)

prjs = [
   {  "name": "openjpeg",
      "type": "cmake",
      "cmake-opts": {"BUILD_JPWL": 0,
                     "BUILD_JPIP": 0,
                     "BUILD_JP3D": 0,
                     "BUILD_MJ2": 0,
                     "BUILD_DOC": 0,
                     "BUILD_JAVA": 0,
                     "BUILD_LUTS_GENERATOR": 0,
                     "BUILD_SHARED_LIBS": (1 if not staticbuild else 0),
                     "BUILD_PKGCONFIG_FILES": 0,
                     "BUILD_TESTING": 0,
                     "BUILD_CODEC": excons.GetArgument("openjpeg-tools", 0, int), # Requires libpng, libjpeg, libtiff
                     "BUILD_THIRDPARTY": 0,
                     "BUILD_VIEWER": 0},
      "cmake-srcs": excons.CollectFiles(["src/bin/common", "src/bin/jp2", "src/lib/openjp2"], patterns=["*.c", "CMakeLists.txt"])
   }
]

excons.DeclareTargets(env, prjs)

def RequireOpenjpeg(env):
   env.Append(CPPPATH=[out_incdir])
   env.Append(LIBPATH=[out_libdir])
   if staticbuild:
      env.Append(CPPDEFINES=["OPJ_STATIC"])
      if sys.platform != "win32":
         excons.StaticallyLink(env, "openjp2", silent=True)
      else:
         env.Append(LIBS=["openjp2"])
   else:
      env.Append(LIBS=["openjp2"])

def OpenjpegName():
   if sys.platform == "win32":
      basename = "openjp2.lib"
   else:
      basename = "libopenjp2" + (".a" if staticbuild else ".so")
   return out_libdir + "/" + basename

Export("RequireOpenjpeg OpenjpegName")

