# Prerequisites<a name="dev-tester-ui-prereqs"></a>

This section describes the prerequisites for testing microcontrollers with AWS IoT Device Tester\.

**Topics**
+ [Use a supported web browser](#idt-ui-supported-web-browser)
+ [Download FreeRTOS](#ui-download-afr)
+ [Download IDT for FreeRTOS](#ui-download-dev-tester-afr)
+ [Create and configure an AWS account](#ui-config-aws-account)
+ [AWS IoT Device Tester managed policy](#ui-managed-policy)

## Use a supported web browser<a name="idt-ui-supported-web-browser"></a>

The IDT\-FreeRTOS UI supports the following web browsers\. 


| Browser | Version | 
| --- | --- | 
| Google Chrome | Latest three major versions | 
| Mozilla Firefox | Latest three major versions | 
| Microsoft Edge | Latest three major versions | 
| Apple Safari for macOS | Latest three major versions | 

We recommend that you use Google Chrome or Mozilla Firefox for a better experience\.

**Note**  
The IDT\-FreeRTOS UI doesn't support Microsoft Internet Explorer\.

## Download FreeRTOS<a name="ui-download-afr"></a>

You can download a release of FreeRTOS from [GitHub](https://github.com/aws/amazon-freertos) with the following command:

```
git clone --branch <FREERTOS_RELEASE_VERSION> --recurse-submodules https://github.com/aws/amazon-freertos.git
cd amazon-freertos
git submodule update --checkout --init --recursive
```

where <FREERTOS\_RELEASE\_VERSION> is a version of FreeRTOS \(for example, 202007\.00\) corresponding to an IDT version listed in [Supported versions of AWS IoT Device Tester for FreeRTOS](dev-test-versions-afr.md)\. This ensures you have the full source code, including submodules, and are using the correct version of IDT for your version of FreeRTOS, and vice versa\.

Windows has a path length limitation of 260 characters\. The path structure of FreeRTOS is many levels deep, so if you're using Windows, keep your file paths under the 260\-character limit\. For example, clone FreeRTOS to `C:\FreeRTOS` rather than `C:\Users\username\programs\projects\myproj\FreeRTOS\`\.

### Considerations for LTS qualification \(qualification for FreeRTOS that uses LTS libraries\)<a name="ui-lts-qualification-dev-tester-afr"></a>
+ In order for your microcontroller to be designated as supporting long\-term support \(LTS\) based versions of FreeRTOS in the AWS Partner Device Catalog, you must provide a manifest file\. For more information, see the [FreeRTOS Qualification Checklist](https://docs.aws.amazon.com/freertos/latest/qualificationguide/afq-checklist.html) in the *FreeRTOS Qualification Guide*\.
+ In order to validate that your microcontroller supports LTS based versions of FreeRTOS and qualify it for submission to the AWS Partner Device Catalog, you must use AWS IoT Device Tester \(IDT\) with FreeRTOS Qualification \(FRQ\) test suite version v1\.4\.x\.
+ Support for LTS based versions of FreeRTOS is limited to the 202012\.xx version of FreeRTOS\. 

## Download IDT for FreeRTOS<a name="ui-download-dev-tester-afr"></a>

Every version of FreeRTOS has a corresponding version of IDT for FreeRTOS for performing qualification tests\. Download the appropriate version of IDT for FreeRTOS from [Supported versions of AWS IoT Device Tester for FreeRTOS](dev-test-versions-afr.md)\.

Extract IDT for FreeRTOS to a location on the file system where you have read and write permissions\. Because Microsoft Windows has a character limit for the path length, extract IDT for FreeRTOS into a root directory such as `C:\` or `D:\`\.

**Note**  
We recommend that you extract the IDT package to a local drive\.Allowing multiple users to run IDT from a shared location, such as an NFS directory or a Windows network shared folder, might result in the system not responding or data corruption\. 

## Create and configure an AWS account<a name="ui-config-aws-account"></a>

Follow these steps to create and configure an AWS account, an IAM user, and an IAM policy that grants IDT for FreeRTOS permission to access resources on your behalf while running tests\.

1. If you already have an AWS account, skip to the next step\. Create an [AWS account](http://aws.amazon.com/premiumsupport/knowledge-center/create-and-activate-aws-account/)\.

1. Create an IAM policy that grants IDT for FreeRTOS the IAM permissions to create service roles with specific permissions\. 

   1. Sign in to the [IAM console](https://console.aws.amazon.com/iam)\.

   1. In the navigation pane, choose **Policies**\.

   1. In the content pane, choose **Create policy**\.

   1. Choose the **JSON** tab and copy the following permissions into the **JSON** text box\.
**Important**  
The following policy template grants IDT permission to create roles, create policies, and attach policies to roles\. IDT for FreeRTOS uses these permissions for tests that create roles\. Although the policy template doesn't provide administrator permissions to the user, the permissions could potentially be used to gain administrator access to your AWS account\.

------
#### [ Most Regions ]

      ```
      {
          "Version": "2012-10-17",
          "Statement": [
              {
                  "Effect": "Allow",
                  "Action": [
                      "iam:CreatePolicy",
                      "iam:DetachRolePolicy",
                      "iam:DeleteRolePolicy",
                      "iam:DeletePolicy",
                      "iam:CreateRole",
                      "iam:DeleteRole",
                      "iam:AttachRolePolicy"
                  ],
                  "Resource": [
                      "arn:aws:iam::*:policy/idt*",
                      "arn:aws:iam::*:role/idt*"
                  ]
              }
          ]
      }
      ```

------
#### [ Beijing and Ningxia Regions ]

      The following policy template can be used in the Beijing and Ningxia Regions\.

      ```
      {
          "Version": "2012-10-17",
          "Statement": [
              {
                  "Effect": "Allow",
                  "Action": [
                      "iam:CreatePolicy",
                      "iam:DetachRolePolicy",
                      "iam:DeleteRolePolicy",
                      "iam:DeletePolicy",
                      "iam:CreateRole",
                      "iam:DeleteRole",
                      "iam:AttachRolePolicy"
                  ],
                  "Resource": [
                      "arn:aws-cn:iam::*:policy/idt*",
                      "arn:aws-cn:iam::*:role/idt*"
                  ]
              }
          ]
      }
      ```

------

   1. When you're finished, choose **Review policy**\.

   1. On the **Review** page, enter `IDTFreeRTOSIAMPermissions` for the policy name\. Review the policy **Summary** to verify the permissions granted by your policy\.

   1. Choose **Create policy**\.

1. Create an IAM user with the necessary permissions to run AWS IoT Device Tester\. 

   1. Follow steps 1 through 5 in [ Creating IAM Users \(Console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html#id_users_create_console)\.

   1. To attach the necessary permissions to your IAM user:

      1. On the **Set permissions** page, choose **Attach existing policies to user directly**\.

      1. Search for the **IDTFreeRTOSIAMPermissions** policy that you created in step 2\. Select the check box\.

      1. Search for the **AWSIoTDeviceTesterForFreeRTOSFullAccess** policy\. Select the check box\.

   1. Choose **Next: Tags**\.

   1. To view a summary of your choices, choose **Next: Review**\.

   1. Choose **Create user**\.

   1. To view the users' access keys \(access key IDs and secret access keys\), choose **Show** next to each password and access key and then choose **Download\.csv**\. Save the file to a safe location\.

## AWS IoT Device Tester managed policy<a name="ui-managed-policy"></a>

To enable device tester to run and to collect metrics, the `AWSIoTDeviceTesterForFreeRTOSFullAccess` managed policy contains the following permissions:
+ `iot-device-tester:SupportedVersion`

  Grants permission to get the list of FreeRTOS versions and test suite versions supported by IDT, so that they're available from the AWS CLI\.
+ `iot-device-tester:LatestIdt`

  Grants permission to get the latest AWS IoT Device Tester version that is available for download\.
+ `iot-device-tester:CheckVersion`

  Grants permission to check that a combination of product, test suite, and AWS IoT Device Tester versions are compatible\.
+ `iot-device-tester:DownloadTestSuite`

  Grants permission to AWS IoT Device Tester to download test suites\.
+ `iot-device-tester:SendMetrics`

  Grants permission to publish AWS IoT Device Tester usage metrics data\.