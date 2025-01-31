# Porting the Secure Sockets library<a name="afr-porting-ss"></a>

You can use the FreeRTOS Secure Sockets library to create and configure a TCP socket, connect to an MQTT broker, and send and receive TCP data\. The Secure Sockets library also encapsulates TLS functionality\. Only a standard TCP socket is required to create a TLS\-protected socket\. For more information, see [FreeRTOS Secure Sockets Library](https://docs.aws.amazon.com/freertos/latest/userguide/secure-sockets.html) in the FreeRTOS User Guide\.

FreeRTOS includes a Secure Sockets implementation for the [FreeRTOS\+TCP](https://freertos.org/FreeRTOS-Plus/FreeRTOS_Plus_TCP/index.html) and [lightweight IP \(lwIP\)](https://savannah.nongnu.org/projects/lwip/) TCP/IP stacks, which are used in conjunction with [mbedTLS](https://tls.mbed.org/)\. If you are using either the FreeRTOS\+TCP or the lwIP TCP/IP stack, you do not need to port the Secure Sockets library\.

**Note**  
Even if you do not need to create a port of the Secure Sockets library, your platform must still pass the qualification tests for the Secure Sockets library\. Qualification is based on results from AWS IoT Device Tester\.
Your TLS implementation should support the [TLS cipher suites that are supported by AWS IoT](https://docs.aws.amazon.com/iot/latest/developerguide/transport-security.html#tls-cipher-suite-support)\.
If you modify the TLS configuration of the Secure Sockets library, this may cause a failure when you attempt to connect to the echo server\. Not all cipher suites supported by the echo server are supported by the Secure Sockets library, and if you enable a cipher suite that the library does not currently support \(for example, SHA384\) you may be unable to connect\.
If your platform offloads TCP/IP functionality to a separate network chip, you need to port the FreeRTOS Secure Sockets library to your device\.

## Prerequisites<a name="porting-prereqs-ss"></a>

To port the Secure Sockets library, you need the following:
+ A port of the Wi\-Fi library \(required only if you are using Wi\-Fi for network connectivity\)\.

  For information about porting the Wi\-Fi library, see [Porting the Wi\-Fi library](afr-porting-wifi.md)\.
+ If your board has cellular connectivity, you must provide a Secure Sockets library that uses the cellular modem as transport\. You can implement the Secure Sockets library in two ways:
  + Implement the Secure Sockets API by using the AT commands of your modem directly\.
  + Use the Cellular Interface library provided by FreeRTOS, which hides the AT commands and provides a socket\-like API\. For more information, see [Porting the Cellular Interface library](freertos-porting-cellular.md)\.
+ A port of a TCP/IP stack\.

  For information about porting a TCP/IP stack, see [Porting a TCP/IP stack](afr-porting-tcp.md)\.
+ An echo server\.

  FreeRTOS includes an echo server, written in Go, in the `freertos/tools/echo_server` directory\. For more information, see [Setting up an echo server](afr-echo-server.md)\.

## Porting<a name="porting-steps-ss"></a>

If your platform offloads TCP/IP functionality to a separate network chip, you need to implement all the functions for which stubs exist in `freertos/vendors/vendor/boards/board/ports/secure_sockets/iot_secure_sockets.c`\.

## Testing<a name="porting-testing-ss"></a>

If you are using an IDE to build test projects, you need to set up your library port in the IDE project\.

### Setting up the IDE test project<a name="testing-ide-ss"></a>

If you are using an IDE for porting and testing, you need to add some source files to the IDE test project before you can test your ported code\.

**Important**  
In the following steps, make sure that you add the source files to your IDE project from their on\-disk location\. Do not create duplicate copies of source files\.

**To set up the Secure Sockets library in the IDE project**

1. If you are using the FreeRTOS\+TCP TCP/IP stack, add `freertos/libraries/abstractions/secure_sockets/freertos_plus_tcp/iot_secure_sockets.c` to the `aws_tests` IDE project\.

   If you are using the lwIP TCP/IP stack, add `freertos/libraries/abstractions/secure_sockets/lwip/iot_secure_sockets.c` to the `aws_tests` IDE project\.

   If you are using your own TCP/IP port, add `freertos/vendors/vendor/boards/board/ports/secure_sockets/iot_secure_sockets.c` to the `aws_tests` IDE project\.

1. Add `secure_sockets/test/aws_test_tcp.c` to the `aws_tests` IDE project\.

### Configuring the `CMakeLists.txt` file<a name="testing-cmake-ss"></a>

If you are using CMake to build your test project, you need to define a portable layer target for the library in your CMake list file\.

To define a library's portable layer target in `CMakeLists.txt`, follow the instructions in [FreeRTOS portable layers](cmake-template.md#cmake-portable)\.

The `CMakeLists.txt` template list file under `freertos/vendors/vendor/boards/board/CMakeLists.txt` includes example portable layer target definitions\. You can uncomment the definition for the library that you are porting, and modify it to fit your platform\.

See below for an example portable layer target definition for the Secure Sockets library\.

```
# Secure sockets
afr_mcu_port(secure_sockets)
# Link to AFR::secure_sockets_freertos_tcp if you want use default implementation based on
# FreeRTOS-Plus-TCP.
target_link_libraries(
AFR::pkcs11::mcu_port
INTERFACE AFR::secure_sockets_freertos_tcp
)
# Or provide your own implementation.
target_sources(
AFR::secure_sockets::mcu_port
INTERFACE "$path/iot_secure_sockets.c"
)
```

### Setting up your local testing environment<a name="testing-local-ss"></a>

After you set up the library in the IDE project, you need to configure some other files for testing\.

**To configure the source and header files for the Secure Sockets tests**

1. Open `freertos/libraries/freertos_plus/standard/utils/src/iot_system_init.c`, and in the function `SYSTEM_Init()`, make sure that the line that calls `SOCKETS_Init()` is not commented out\.

1. Start an echo server\.

   If you have not ported the TLS library to your platform, you can only test your Secure Sockets port using an unsecured echo server \(`freertos/tools/echo_server/echo_server.go`\)\. For instructions on setting up and running an unsecured echo server, see [Setting up an echo server](afr-echo-server.md)\.

1. In `aws_test_tcp.h`, set the IP address to the correct values for your server\. For example, if your server's IP address is `192.168.2.6`, set the following values in `aws_test_tcp.h`:    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/freertos/latest/portingguide/afr-porting-ss.html)

1. Open `aws_test_tcp.h`, and set the `tcptestSECURE_SERVER` macro to `0` to run the Secure Sockets tests without TLS\.

1. Open `freertos/vendors/vendor/boards/board/aws_tests/config_files/aws_test_runner.config.h`, and set the `testrunnerFULL_TCP_ENABLED` macro to `1` to enable the sockets tests\.

1. Open `freertos/vendors/vendor/boards/board/aws_tests/application_code/main.c`, and and delete the `#if 0` and `#endif` compiler directives in the `vApplicationIPNetworkEventHook ( void )` definition to enable the testing task\.
**Note**  
This change is required to port the remaining libraries\.

**Important**  
For qualification, you must pass the Secure Sockets tests with TLS\. After you port the TLS library, rerun the Secure Sockets tests with TLS tests enabled, using a TLS\-capable echo server\.  
To port the TLS library, see [Porting the TLS library](afr-porting-tls.md)\.

**To set up testing for Secure Sockets after porting the TLS library**

1. Start a secure echo server\.

   For information, see [Setting up an echo server](afr-echo-server.md)\.

1. Set the IP address and port in `freertos/tests/include/aws_test_tcp.h` to correct values for your server\. For example, if your server's IP address is `192.168.2.6`, and the server is listening on `9000`, set the following values in `freertos/tests/include/aws_test_tcp.h`:    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/freertos/latest/portingguide/afr-porting-ss.html)

1. Open `freertos/tests/include/aws_test_tcp.h`, and set the `tcptestSECURE_SERVER` macro to `1` to enable TLS tests\.

1. Download a trusted root certificate\. For information about accepted root certificates and download links, see [Server Authentication](https://docs.aws.amazon.com/iot/latest/developerguide/managing-device-certs.html#server-authentication) in the AWS IoT Developer Guide\. We recommend that you use Amazon Trust Services certificates\.

1. In a browser window, open `freertos/tools/certificate_configuration/PEMfileToCString.html`\.

1. Under **PEM Certificate or Key**, choose the root CA file that you downloaded\.

1. Choose **Display formatted PEM string to be copied into aws\_clientcredential\_keys\.h**, and then copy the certificate string\.

1. Open `aws_test_tcp.h`, and paste the formatted certificate string into the definition for `tcptestECHO_HOST_ROOT_CA`\.

1. Use the second set of OpenSSL commands in `freertos/tools/echo_server/readme-gencert.txt` to generate a client certificate and private key that is signed by the certificate authority\. The certificate and key allow the custom echo server to trust the client certificate that your device presents during TLS authentication\.

1. Format the certificate and key with the `freertos/tools/certificate_configuration/PEMfileToCString.html` formatting tool\.

1. Before you build and run the test project on your device, open `aws_clientcredential_keys.h`, and copy the client certificate and private key, in PEM format, into the definitions for `keyCLIENT_CERTIFICATE_PEM` and `keyCLIENT_PRIVATE_KEY_PEM`\.

### Running the tests<a name="testing-run-ss"></a>

**To execute the Secure Sockets tests**

1. Build the test project, and then flash it to your device for execution\.

1. Check the test results in the UART console\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/freertos/latest/portingguide/images/porting-ss-tests1.png)

   `...`  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/freertos/latest/portingguide/images/porting-ss-tests2.png)

   If all tests pass, then testing is complete\.

## Validation<a name="testing-validation-ss"></a>

To officially qualify a device for FreeRTOS, you need to validate the device's ported source code with AWS IoT Device Tester\. Follow the instructions in [Using AWS IoT Device Tester for FreeRTOS](https://docs.aws.amazon.com/freertos/latest/userguide/device-tester-for-freertos-ug.html) in the FreeRTOS User Guide to set up Device Tester for port validation\. To test a specific library's port, the correct test group must be enabled in the `device.json` file in the Device Tester `configs` folder\.

After you finish porting the FreeRTOS Secure Sockets library to your device, you can start porting the corePKCS11 library\. See [Porting the corePKCS11 library](afr-porting-pkcs.md) for instructions\.