# Building an ArkUI-X Project

- Use the **build.sh** script to build an ArkUI-X project. Common options of the build script are as follows:

  ```shell
  --product-name    # (Mandatory) Product name, for example, arkui-x.
  --target-os       # (Mandatory) Target OS platform, for example, android or ios.
  --build-target    # (Optional) One or more targets to build.
  --gn-args         # (Optional) One or more GN parameters.
  --ninja-args      # (Optional) Ninja parameters, for example, --ninja-args=-dkeeprsp.
  --log-level       # (Optional) Log level, for example, info or debug.
  --help, -h        # (Optional) Help command.
  ```

- If new code is downloaded or code is updated, run the following command to download or update the prebuilt toolchain:
  ```
  ./build/prebuilts_download.sh --build-arkuix --skip-ssl
  ```
- Build command examples
  - Display options supported by the build script.
  ```
  ./build.sh -h
  ```

  - Build an ArkUI-X project on Android.

  ```shell
  ./build.sh --product-name arkui-x --target-os android
  ```

  - Build an ArkUI-X project on iOS.

  ```shell
  ./build.sh --product-name arkui-x --target-os ios
  ```
