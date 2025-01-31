# FreeRTOS User Guide

-----
*****Copyright &copy; Amazon Web Services, Inc. and/or its affiliates. All rights reserved.*****

-----
Amazon's trademarks and trade dress may not be used in 
     connection with any product or service that is not Amazon's, 
     in any manner that is likely to cause confusion among customers, 
     or in any manner that disparages or discredits Amazon. All other 
     trademarks not owned by Amazon are the property of their respective
     owners, who may or may not be affiliated with, connected to, or 
     sponsored by Amazon.

-----
## Contents
+ [What is FreeRTOS?](what-is-freertos.md)
+ [FreeRTOS kernel fundamentals](dev-guide-freertos-kernel.md)
+ [FreeRTOS console](freertos-ocw.md)
+ [AWS IoT Device SDK for Embedded C](c-sdk.md)
+ [Getting Started with FreeRTOS](freertos-getting-started.md)
   + [FreeRTOS demo application](freertos-getting-started-demo.md)
   + [First steps](freertos-prereqs.md)
   + [First steps with the console Quick Connect workflow](freertos-console-quickstart.md)
   + [Troubleshooting getting started](gsg-troubleshooting.md)
   + [Using CMake with FreeRTOS](getting-started-cmake.md)
   + [Developer-mode key provisioning](dev-mode-key-provisioning.md)
   + [Board-specific getting started guides](getting-started-guides.md)
      + [Getting started with the Cypress CYW943907AEVAL1F Development Kit](getting_started_cypress_43.md)
      + [Getting started with the Cypress CYW954907AEVAL1F Development Kit](getting_started_cypress_54.md)
      + [Getting started with the Cypress CY8CKIT-064S0S2-4343W kit](getting_started_cypress_psoc64.md)
      + [Getting started with the Microchip ATECC608A Secure Element with Windows simulator](getting_started_atecc608a.md)
      + [Getting started with the Espressif ESP32-DevKitC and the ESP-WROVER-KIT](getting_started_espressif.md)
      + [Getting started with the Espressif ESP32-WROOM-32SE](getting_started_esp32wroom-32se.md)
      + [Getting started with the Espressif ESP32-S2](getting_started_esp32-s2.md)
      + [Getting started with the Infineon XMC4800 IoT Connectivity Kit](getting_started_infineon.md)
      + [Getting started with the Infineon OPTIGA Trust X and XMC4800 IoT Connectivity Kit](getting_started_infineon_trust_x.md)
      + [Getting started with the MW32x AWS IoT Starter Kit](getting_started_mw32x.md)
      + [Getting started with the MediaTek MT7697Hx development kit](getting_started_mediatek.md)
      + [Getting started with the Microchip Curiosity PIC32MZ EF](getting_started_mch.md)
      + [Getting started with the Nordic nRF52840-DK](getting_started_nordic.md)
      + [Getting started with the Nuvoton NuMaker-IoT-M487](getting-started-nuvoton-m487.md)
      + [Getting started with the NXP LPC54018 IoT Module](getting_started_nxp.md)
      + [Getting started with the Renesas Starter Kit+ for RX65N-2MB](getting_started_renesas.md)
      + [Getting started with the STMicroelectronics STM32L4 Discovery Kit IoT Node](getting_started_st.md)
      + [Getting started with the Texas Instruments CC3220SF-LAUNCHXL](getting_started_ti.md)
      + [Getting started with the Windows Device Simulator](getting_started_windows.md)
      + [Getting started with the Xilinx Avnet MicroZed Industrial IoT Kit](getting_started_xilinx.md)
   + [Next steps with FreeRTOS](getting-started-next-steps.md)
+ [FreeRTOS Over-the-Air Updates](freertos-ota-dev.md)
   + [OTA update prerequisites](ota-prereqs.md)
      + [Create an Amazon S3 bucket to store your update](dg-ota-bucket.md)
      + [Create an OTA Update service role](create-service-role.md)
      + [Create an OTA user policy](create-ota-user-policy.md)
      + [Create a code-signing certificate](ota-code-sign-cert.md)
         + [Creating a code-signing certificate for the Texas Instruments CC3220SF-LAUNCHXL](ota-code-sign-cert-ti.md)
         + [Creating a code-signing certificate for the Espressif ESP32](ota-code-sign-cert-esp.md)
         + [Creating a code-signing certificate for the Nordic nrf52840-dk](ota-code-sign-cert-nordic.md)
         + [Creating a code-signing certificate for the FreeRTOS Windows simulator](ota-code-sign-cert-win.md)
         + [Creating a code-signing certificate for custom hardware](ota-code-sign-cert-other.md)
      + [Grant access to code signing for AWS IoT](code-sign-policy.md)
      + [Download FreeRTOS with the OTA library](ota-download-freertos.md)
      + [Prerequisites for OTA updates using MQTT](ota-mqtt-freertos.md)
      + [Prerequisites for OTA updates using HTTP](ota-http-freertos.md)
   + [OTA tutorial](dev-guide-ota-workflow.md)
      + [Installing the initial firmware](dg-ota-initial-firmware.md)
         + [Install the initial version of firmware on the Texas Instruments CC3220SF-LAUNCHXL](burn-initial-firmware-ti.md)
         + [Install the initial version of firmware on the Espressif ESP32](burn-initial-firmware-esp.md)
         + [Install the initial version of firmware on the Nordic nRF52840 DK](burn-initial-firmware-nordic.md)
         + [Initial firmware on the Windows simulator](burn-initial-firmware-windows.md)
         + [Install the initial version of firmware on a custom board](burn-initial-firmware-other.md)
      + [Update the version of your firmware](dg-ota-update-firmware.md)
      + [Creating an OTA update (AWS IoT console)](ota-console-workflow.md)
      + [Creating an OTA update with the AWS CLI](ota-cli-workflow.md)
   + [OTA Update Manager service](ota-manager.md)
   + [Integrating the OTA Agent into your application](integrate-ota-agent.md)
   + [OTA security](dev-guide-ota-security.md)
   + [OTA troubleshooting](ota-troubleshooting.md)
      + [Set up CloudWatch Logs for OTA updates](ota-logging.md)
      + [Log AWS IoT OTA API calls with AWS CloudTrail](iot-using-cloudtrail-afr.md)
      + [Get CreateOTAUpdate failure details using the AWS CLI](ota-create-failure.md)
      + [Get OTA failure codes with the AWS CLI](ota-failure-codes.md)
      + [Troubleshoot OTA updates of multiple devices](ota-troubleshooting-multi-thing.md)
      + [Troubleshoot OTA updates with the Texas Instruments CC3220SF Launchpad](ota-troubleshooting-ti.md)
+ [FreeRTOS Libraries](dev-guide-freertos-libraries.md)
   + [backoffAlgorithm library](backoffalgorithm-library.md)
   + [Bluetooth Low Energy library](freertos-ble-library.md)
      + [Mobile SDKs for FreeRTOS Bluetooth devices](freertos-ble-mobile.md)
   + [Cellular Interface library](cellular-interface.md)
   + [Common I/O](common-io.md)
   + [AWS IoT Device Defender library](afr-device-defender-library.md)
   + [AWS IoT Greengrass Discovery library](freertos-lib-gg-connectivity.md)
   + [coreHTTP library](core-http.md)
   + [coreJSON library](freertos-lib-corejson.md)
   + [coreMQTT library](coremqtt.md)
   + [coreMQTT Agent library](coremqtt-agent.md)
   + [AWS IoT Over the air (OTA) library](ota-update-library.md)
   + [corePKCS11 library](security-pkcs.md)
   + [Secure Sockets library](secure-sockets.md)
   + [AWS IoT Device Shadow library](freertos-lib-cloud-shadows.md)
   + [AWS IoT Jobs library](freertos-lib-jobs.md)
   + [Transport Layer Security](security-tls.md)
   + [Wi-Fi library](freertos-wifi.md)
+ [FreeRTOS demos](freertos-next-steps.md)
   + [Bluetooth Low Energy demo applications](ble-demo.md)
   + [Demo bootloader for the Microchip Curiosity PIC32MZEF](microchip-bootloader.md)
   + [AWS IoT Device Defender demo](dd-demo.md)
   + [AWS IoT Greengrass discovery demo application](gg-demo.md)
   + [coreHTTP demos](core-http-demo.md)
      + [coreHTTP mutual authentication demo](core-http-ma-demo.md)
      + [coreHTTP basic Amazon S3 upload demo](core-http-s3-upload-demo.md)
      + [coreHTTP basic S3 download demo](core-http-s3-download-demo.md)
      + [coreHTTP basic multithreaded demo](core-http-bmt-demo.md)
   + [AWS IoT Jobs library demo](freertos-jobs-demo.md)
   + [coreMQTT demos](mqtt-demo.md)
      + [coreMQTT mutual authentication demo](mqtt-demo-ma.md)
      + [coreMQTT Agent connection sharing demo](mqtt-demo-cs.md)
   + [Over-the-air updates demo application](ota-demo.md)
      + [Over-the-air demo configurations](ota-demo-specific-config.md)
      + [Download, build, flash, and run the FreeRTOS OTA demo on the Texas Instruments CC3220SF-LAUNCHXL](download-ota-ti.md)
      + [Download, build, flash, and run the FreeRTOS OTA demo on the Microchip Curiosity PIC32MZEF](download-ota-mchip.md)
      + [Download, build, flash, and run the FreeRTOS OTA demo on the Espressif ESP32](download-ota-esp.md)
      + [Download, build, flash and run the FreeRTOS OTA demo on the Renesas RX65N](download-rx65n-ota.md)
      + [Tutorial: Perform OTA updates on Espressif ESP32 using FreeRTOS Bluetooth Low Energy](ota-updates-esp32-ble.md)
   + [AWS IoT Device Shadow demo application](shadow-demo.md)
   + [Secure Sockets echo client demo](secure-sockets-demo.md)
+ [AWS IoT Device Tester for FreeRTOS](device-tester-for-freertos-ug.md)
   + [Supported versions of AWS IoT Device Tester for FreeRTOS](dev-test-versions-afr.md)
   + [Unsupported IDT versions for FreeRTOS](idt-unsupported-versions-afr.md)
   + [Download IDT for FreeRTOS](idt-programmatic-download.md)
   + [Use IDT with the FreeRTOS qualification suite (FRQ)](idt-freertos-qualification.md)
      + [Prerequisites](dev-tester-prereqs.md)
      + [Preparing to test your microcontroller board for the first time](qual-steps.md)
      + [Use the IDT for FreeRTOS user interface to run the FreeRTOS qualification suite](device-tester-ui.md)
         + [Prerequisites](dev-tester-ui-prereqs.md)
         + [Getting started with the IDT-FreeRTOS UI](dev-tester-ui-getting-started.md)
      + [Running Bluetooth Low Energy tests](afr-bridgekeeper-dt-bt.md)
      + [Running the FreeRTOS qualification suite](run-tests.md)
         + [Test for re-qualification](requal-test.md)
      + [AWS IoT Device Tester for FreeRTOS test suite versions](idt-test-suite-versions.md)
      + [Understanding results and logs](results-logs.md)
   + [Use IDT to develop and run your own test suites](idt-custom-tests.md)
      + [Tutorial: Build and run the sample IDT test suite](build-sample-suite.md)
      + [Tutorial: Develop a simple IDT test suite](create-custom-tests.md)
         + [Create IDT test suite configuration files](idt-json-config.md)
         + [Configure the IDT test orchestrator](idt-test-orchestrator.md)
         + [Configure the IDT state machine](idt-state-machine.md)
         + [Create IDT test case executables](test-executables.md)
         + [Use the IDT context](idt-context.md)
         + [Configure settings for test runners](set-config-custom.md)
         + [Debug and run custom test suites](run-tests-custom.md)
         + [Review IDT test results and logs](idt-review-results-logs.md)
         + [IDT usage metrics](idt-usage-metrics.md)
   + [Troubleshooting](dt-afr-troubleshooting.md)
   + [Support policy for AWS IoT Device Tester for FreeRTOS](idt-support-policy.md)
+ [Security in AWS](security.md)
   + [Identity and Access Management for AWS resources](security-iam.md)
      + [How AWS services work with IAM](security_iam_service-with-iam.md)
      + [Identity-based policy examples](security_iam_id-based-policy-examples.md)
      + [Troubleshooting identity and access](security_iam_troubleshoot.md)
   + [Compliance validation](freertos-compliance.md)
   + [Resilience in AWS](disaster-recovery-resiliency.md)
   + [Infrastructure security in FreeRTOS](infrastructure-security.md)
+ [FreeRTOS User Guide Archive](freertos-ug-archive.md)