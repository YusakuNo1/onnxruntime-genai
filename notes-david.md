SLM server start:
    cd examples/slm_engine
    ./build_scripts/builds/Darwin-arm64/install/bin/slm-server -m phi-3-mini-4k -v -p 8000

SLM server test:
    cd examples/slm_engine
    python test_slm_server.py --server_binary_path ../build_scripts/builds/Darwin-arm64/install/bin/slm-server --model_path ../phi-3-mini-4k

Build Android:
    python build.py --build_java --android --android_home [your-home-folder]/Library/Android/sdk --android_ndk_path [your-home-folder]/Library/Android/sdk/ndk/27.0.12077973 --android_abi arm64-v8a --config Release
* Build error `exception in phase 'semantic analysis' in source unit '_BuildScript_' Unsupported class file major version`
    * Solution: [from this post](https://stackoverflow.com/questions/68597899/bug-exception-in-phase-semantic-analysis-in-source-unit-buildscript-unsup), update version of distributionUrl and distributionSha256Sum, e.g. for Java version 24, update to 8.14 
* Build error `Caused by: [CIRCULAR REFERENCE: java.lang.NullPointerException: Cannot invoke "String.length()" because "<parameter1>" is null]`
    * Update gradle version from `'com.android.tools.build:gradle:7.4.2'` to `'com.android.tools.build:gradle:8.2.2'`, update from `src/java` folder instead of the generated folder within `build`


# How to build iOS
* Build
    * Command: `build.py --cmake_generator Xcode --config Debug --build_dir build_xcode --osx_arch arm64`
* In folder `build_xcode/Debug`
    * Command: `/opt/homebrew/bin/ctest --build-config Debug --verbose --timeout 10800`
* Update "header search path" and "runpath search pather" include path to the "absolute path" by searching for "header" and "runpath search paths" respectively -- TODO: not sure how to do it with relative path yet.
* How to run unit tests. Set the following variables as run env var:
    * Option 1, disable `ORTGENAI_LOG_ORT_LIB`: set `ORTGENAI_LOG_ORT_LIB` to 0
    * Option 2, enable `ORTGENAI_LOG_ORT_LIB` by setting it to 1, and then set `ORT_LIB_PATH` to the dylib path, e.g. `build_xcode/Debug/dependencies/ort/runtimes/osx-arm64/native/libonnxruntime.dylib`


# Convert to ONNX
* Command: `optimum-cli export onnx --model Qwen/Qwen2.5-0.5B-Instruct output`
* Success Models:
    * Qwen/Qwen2.5-0.5B-Instruct
