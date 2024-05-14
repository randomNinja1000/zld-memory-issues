# Purpose

Demonstrate that on macos + apple silicon, zig ld uses prohibitive amounts of memory.

```
bazel build @llvm-project//llvm:opt --sandbox_debug -s
```

Copy the `cd` from last command printed by bazel, it resembles:
```
cd  /private/var/tmp/_bazel_${USER}/<hash>/execroot/_main
```

Then run:
```
time external/zig_sdk/tools/aarch64-macos-none/c++ -o /tmp/opt bazel-out/darwin_arm64-fastbuild/bin/external/llvm-project/llvm/libopt-driver.a bazel-out/darwin_arm64-fastbuild/bin/external/llvm-project/llvm/libAArch64AsmParser.a bazel-out/darwin_arm64-fastbuild/bin/external/llvm-project/llvm/libAArch64CodeGen.a bazel-out/darwin_arm64-fastbuild/bin/external/llvm-project/llvm/libAArch64UtilsAndDesc.a bazel-out/darwin_arm64-fastbuild/bin/external/llvm-project/llvm/libMCDisassembler.a bazel-out/darwin_arm64-fastbuild/bin/external/llvm-project/llvm/libAArch64Info.a bazel-out/darwin_arm64-fastbuild/bin/external/llvm-project/llvm/libPasses.a bazel-out/darwin_arm64-fastbuild/bin/external/llvm-project/llvm/libCodeGen.a bazel-out/darwin_arm64-fastbuild/bin/external/llvm-project/llvm/libCodeGenTypes.a bazel-out/darwin_arm64-fastbuild/bin/external/llvm-project/llvm/libHipStdPar.a bazel-out/darwin_arm64-fastbuild/bin/external/llvm-project/llvm/libCFGuard.a bazel-out/darwin_arm64-fastbuild/bin/external/llvm-project/llvm/libCoroutines.a bazel-out/darwin_arm64-fastbuild/bin/external/llvm-project/llvm/libIPO.a bazel-out/darwin_arm64-fastbuild/bin/external/llvm-project/llvm/libFrontendOpenMP.a bazel-out/darwin_arm64-fastbuild/bin/external/llvm-project/llvm/libFrontendOffloading.a bazel-out/darwin_arm64-fastbuild/bin/external/llvm-project/llvm/libIRPrinter.a bazel-out/darwin_arm64-fastbuild/bin/external/llvm-project/llvm/libLinker.a bazel-out/darwin_arm64-fastbuild/bin/external/llvm-project/llvm/libInstrumentation.a bazel-out/darwin_arm64-fastbuild/bin/external/llvm-project/llvm/libObjCARC.a bazel-out/darwin_arm64-fastbuild/bin/external/llvm-project/llvm/libScalar.a bazel-out/darwin_arm64-fastbuild/bin/external/llvm-project/llvm/libAggressiveInstCombine.a bazel-out/darwin_arm64-fastbuild/bin/external/llvm-project/llvm/libInstCombine.a bazel-out/darwin_arm64-fastbuild/bin/external/llvm-project/llvm/libVectorize.a bazel-out/darwin_arm64-fastbuild/bin/external/llvm-project/llvm/libTransformUtils.a bazel-out/darwin_arm64-fastbuild/bin/external/llvm-project/llvm/libBitWriter.a bazel-out/darwin_arm64-fastbuild/bin/external/llvm-project/llvm/libTarget.a bazel-out/darwin_arm64-fastbuild/bin/external/llvm-project/llvm/libAnalysis.a bazel-out/darwin_arm64-fastbuild/bin/external/llvm-project/llvm/libProfileData.a bazel-out/darwin_arm64-fastbuild/bin/external/llvm-project/llvm/libSymbolize.a bazel-out/darwin_arm64-fastbuild/bin/external/llvm-project/llvm/libDebugInfoDWARF.a bazel-out/darwin_arm64-fastbuild/bin/external/llvm-project/llvm/libDebugInfoPDB.a bazel-out/darwin_arm64-fastbuild/bin/external/llvm-project/llvm/libDebugInfoBTF.a bazel-out/darwin_arm64-fastbuild/bin/external/llvm-project/llvm/libObject.a bazel-out/darwin_arm64-fastbuild/bin/external/llvm-project/llvm/libIRReader.a bazel-out/darwin_arm64-fastbuild/bin/external/llvm-project/llvm/libAsmParser.a bazel-out/darwin_arm64-fastbuild/bin/external/llvm-project/llvm/libBitReader.a bazel-out/darwin_arm64-fastbuild/bin/external/llvm-project/llvm/libMCParser.a bazel-out/darwin_arm64-fastbuild/bin/external/llvm-project/llvm/libTextAPI.a bazel-out/darwin_arm64-fastbuild/bin/external/llvm-project/llvm/libCore.a bazel-out/darwin_arm64-fastbuild/bin/external/llvm-project/llvm/libRemarks.a bazel-out/darwin_arm64-fastbuild/bin/external/llvm-project/llvm/libBitstreamReader.a bazel-out/darwin_arm64-fastbuild/bin/external/llvm-project/llvm/libMC.a bazel-out/darwin_arm64-fastbuild/bin/external/llvm-project/llvm/libDebugInfoCodeView.a bazel-out/darwin_arm64-fastbuild/bin/external/llvm-project/llvm/libBinaryFormat.a bazel-out/darwin_arm64-fastbuild/bin/external/llvm-project/llvm/libDebugInfoMSF.a bazel-out/darwin_arm64-fastbuild/bin/external/llvm-project/llvm/libTargetParser.a bazel-out/darwin_arm64-fastbuild/bin/external/llvm-project/llvm/libSupport.a bazel-out/darwin_arm64-fastbuild/bin/external/llvm-project/llvm/libDemangle.a bazel-out/darwin_arm64-fastbuild/bin/external/llvm_zlib/libzlib.a bazel-out/darwin_arm64-fastbuild/bin/external/llvm_zstd/libzstd.a -pthread -ldl -Wl,-headerpad_max_install_names -Wl,-S -fno-lto '-fsanitize-undefined-strip-path-components=-1'
```

The activity monitor reports more than 14GB for and time says:
```
real    0m12.499s
user    0m10.766s
sys     0m3.580s
```

By comparison, the same on a linux box takes:
```
```
