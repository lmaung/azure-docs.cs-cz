---
title: Nahrání souborů ze zařízení do služby Azure IoT Hub pomocí Pythonu | Dokumentace Microsoftu
description: Postup nahrání souborů ze zařízení do cloudu pomocí zařízení Azure IoT SDK pro Python. Nahrané soubory se ukládají v kontejneru objektů blob v Azure storage.
author: kgremban
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: conceptual
ms.date: 01/22/2019
ms.author: kgremban
ms.openlocfilehash: 295f96258b2f5d6612ae7c5f86c9f360232111f6
ms.sourcegitcommit: fea5a47f2fee25f35612ddd583e955c3e8430a95
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/31/2019
ms.locfileid: "55507837"
---
# <a name="upload-files-from-your-device-to-the-cloud-with-iot-hub"></a>Nahrání souborů ze zařízení do cloudu pomocí služby IoT Hub

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

Tento článek popisuje, jak používat [soubor nahrát možnosti služby IoT Hub](iot-hub-devguide-file-upload.md) nahrát soubor do [úložiště objektů blob v Azure](../storage/index.yml). V tomto kurzu získáte informace o následujících postupech:

- Zabezpečeně dodávají kontejner úložiště pro nahrání souboru.
- Pomocí Pythonového klienta k nahrání souboru do služby IoT hub.

[Odesílání telemetrických dat do služby IoT Hub](quickstart-send-telemetry-python.md) rychlý start ukazuje základní funkce zasílání zpráv typu zařízení cloud ze služby IoT Hub. Nicméně v některých scénářích nelze mapovat snadno data, která vaše zařízení odesílají do poměrně málo početnému zpráv typu zařízení cloud, které služby IoT Hub přijímá. Když budete potřebovat hornatých souborů ze zařízení, můžete stále použít zabezpečení a spolehlivost služby IoT Hub.

> [!NOTE]
> Python SDK pro IoT Hub aktuálně podporuje jenom jako nahrávání souborů znakový **.txt** soubory.

Na konci tohoto kurzu Spusťte konzolovou aplikaci v Pythonu:

* **FileUpload.py**, který nahraje soubor do úložiště pomocí sady Python SDK zařízení.

> [!NOTE]
> IoT Hub podporuje mnoho platforem zařízení a jazyků (včetně C, .NET, Javascript, Python a Java) prostřednictvím sady SDK pro zařízení Azure IoT. Odkazovat [centrum pro vývojáře Azure IoT] podrobné pokyny o tom, jak připojit zařízení ke službě Azure IoT Hub.

Pro absolvování tohoto kurzu potřebujete:

* [Python 2.x nebo 3.x][lnk-python-download]. Ujistěte se, že používáte 32bitovou, nebo 64bitovou instalaci podle požadavků vašeho nastavení. Po zobrazení výzvy v průběhu instalace nezapomeňte přidat Python do proměnné prostředí pro konkrétní platformu. Pokud používáte Python 2.x, možná bude nutné [nainstalovat nebo upgradovat *pip*, systém pro správu balíčků Pythonu][lnk-install-pip].
* Pokud používáte operační systém Windows, je k povolení používání nativních knihoven DLL z Pythonu potřeba [balíček distribuovatelných součástí Visual C++][lnk-visual-c-redist].
* Aktivní účet Azure. Pokud účet nemáte, můžete vytvořit [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/) během několika minut.
* V účtu Azure pomocí identity zařízení pro testování funkci odesílání souborů služby IoT hub. 

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]


## <a name="upload-a-file-from-a-device-app"></a>Nahrajte soubor z aplikace pro zařízení

V této části vytvoříte aplikaci pro nahrání souboru do služby IoT hub zařízení.

1. Na příkazovém řádku spusťte následující příkaz k instalaci **azure-iothub-device-client** balíčku:

    ```cmd/sh
    pip install azure-iothub-device-client
    ```

1. Pomocí textového editoru vytvořte soubor testu, který nahrajete do úložiště objektů blob. 

    > [!NOTE]
    > Python SDK pro IoT Hub aktuálně podporuje jenom jako nahrávání souborů znakový **.txt** soubory.

1. Pomocí textového editoru, vytvořte **FileUpload.py** souboru ve své pracovní složce.

1. Přidejte následující `import` příkazy a proměnné na začátku **FileUpload.py** souboru. 

    ```python
    import time
    import sys
    import iothub_client
    import os
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult, IoTHubError

    CONNECTION_STRING = "[Device Connection String]"
    PROTOCOL = IoTHubTransportProvider.HTTP

    PATHTOFILE = "[Full path to file]"
    FILENAME = "[File name for storage]"
    ```

1. V souboru nahraďte `[Device Connection String]` připojovacím řetězcem zařízení IoT hub. Nahraďte `[Full path to file]` cestou k souboru testu, který jste vytvořili nebo všechny soubory na vašem zařízení, který chcete nahrát. Nahraďte `[File name for storage]` názvem, který chcete přiřadit k souboru po ho nahráli do úložiště objektů blob. 

1. Vytvořte zpětné volání pro **upload_blob** funkce:

    ```python
    def blob_upload_conf_callback(result, user_context):
        if str(result) == 'OK':
            print ( "...file uploaded successfully." )
        else:
            print ( "...file upload callback returned: " + str(result) )
    ```

1. Přidejte následující kód k připojení klienta a nahrání souboru. Také `main` rutinu:

    ```python
    def iothub_file_upload_sample_run():
        try:
            print ( "IoT Hub file upload sample, press Ctrl-C to exit" )

            client = IoTHubClient(CONNECTION_STRING, PROTOCOL)

            f = open(PATHTOFILE, "r")
            content = f.read()

            client.upload_blob_async(FILENAME, content, len(content), blob_upload_conf_callback, 0)

            print ( "" )
            print ( "File upload initiated..." )

            while True:
                time.sleep(30)

        except IoTHubError as iothub_error:
            print ( "Unexpected error %s from IoTHub" % iothub_error )
            return
        except KeyboardInterrupt:
            print ( "IoTHubClient sample stopped" )
        except:
            print ( "generic error" )

    if __name__ == '__main__':
        print ( "Simulating a file upload using the Azure IoT Hub Device SDK for Python" )
        print ( "    Protocol %s" % PROTOCOL )
        print ( "    Connection string=%s" % CONNECTION_STRING )

        iothub_file_upload_sample_run()
    ```

1. Uložte a zavřete **UploadFile.py** souboru.


## <a name="run-the-application"></a>Spuštění aplikace

Nyní jste připraveni ke spuštění aplikace.

1. Na příkazovém řádku ve své pracovní složce spusťte následující příkaz:

    ```cmd/sh
    python FileUpload.py
    ```

1. Následující snímek obrazovky ukazuje výstup z **FileUpload** aplikace:

    ![Výstup z aplikace simulovaného zařízení](./media/iot-hub-python-python-file-upload/1.png)

1. Na portálu můžete použít k zobrazení nahraných souborů v kontejneru úložiště, které jste nakonfigurovali:

    ![Nahraný soubor](./media/iot-hub-python-python-file-upload/2.png)


## <a name="next-steps"></a>Další postup

V tomto kurzu jste zjistili, jak zjednodušit nahrávání souborů ze zařízení pomocí možnosti nahrávání souborů služby IoT Hub. Můžete pokračovat k prozkoumání funkcí služby IoT hub a scénáře najdete v následujících článcích:

* [Vytvoření centra IoT prostřednictvím kódu programu][lnk-create-hub]
* [Seznámení s C SDK][lnk-c-sdk]
* [Sady Azure IoT SDK][lnk-sdks]

<!-- Links -->
[Centrum pro vývojáře Azure IoT]: http://azure.microsoft.com/develop/iot

[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-python-download]: https://www.python.org/downloads/
[lnk-visual-c-redist]: http://www.microsoft.com/download/confirmation.aspx?id=48145
[lnk-install-pip]: https://pip.pypa.io/en/stable/installing/
