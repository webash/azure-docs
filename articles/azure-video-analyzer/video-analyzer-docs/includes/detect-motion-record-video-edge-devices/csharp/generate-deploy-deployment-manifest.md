---
author: fvneerden
ms.service: azure-video-analyzer
ms.topic: include
ms.date: 05/03/2021
ms.author: naiteeks
---

The deployment manifest defines what modules are deployed to an edge device. It also defines configuration settings for those modules.

Follow these steps to generate the manifest from the template file and then deploy it to the edge device.

1.  Open Visual Studio Code.
1.  Next to the **AZURE IOT HUB** pane, select the **More actions** icon to set the IoT Hub connection string. You can copy the string from the _src/cloud-to-device-console-app/appsettings.json_ file.

    ![Set IOT Connection String](../../../media/vscode-common-screenshots/set-connection-string.png)

    > [!NOTE]
    > You might be asked to provide Built-in endpoint information for the IoT Hub. To get that information, in Azure portal, navigate to your IoT Hub and look for **Built-in endpoints** option in the left navigation pane. Click there and look for the **Event Hub-compatible endpoint** under **Event Hub compatible endpoint** section. Copy and use the text in the box. The endpoint will look something like this: <br/>`Endpoint=sb://iothub-ns-xxx.servicebus.windows.net/;SharedAccessKeyName=iothubowner;SharedAccessKey=XXX;EntityPath=<IoT Hub name>`

1.  Right-click **src/edge/deployment.template.json** and select **Generate IoT Edge Deployment Manifest**.

    ![Generate the IoT Edge deployment manifest](../../../media/quickstarts/generate-iot-edge-deployment-manifest.png)

    This action should create a manifest file named _deployment.amd64.json_ in the _src/edge/config_ folder.

1.  Right-click **src/edge/config/deployment.amd64.json**, select **Create Deployment for Single Device**, and then select the name of your edge device.

    ![Create a deployment for a single device](../../../media/quickstarts/create-deployment-single-device.png)

1.  When you're prompted to select an IoT Hub device, choose **avasample-iot-edge-device** from the drop-down menu.
1.  After about 30 seconds, in the lower-left corner of the window, refresh Azure IoT Hub. The edge device now shows the following deployed modules:

    - Video Analyzer edge module (module name `avaedge`)
    - Real-Time Streaming Protocol (RTSP) simulator (module name `rtspsim`)
