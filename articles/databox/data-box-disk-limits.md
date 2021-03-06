---
title: Omezení disku Azure Data Box | Dokumentace Microsoftu
description: Popisuje omezení systému a doporučené velikosti pro Microsoft Azure Data Box Disk.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: article
ms.date: 02/05/2019
ms.author: alkohli
ms.openlocfilehash: 6a7f7943e9d567a953c0e21697dfe4fdedd6e8f0
ms.sourcegitcommit: 947b331c4d03f79adcb45f74d275ac160c4a2e83
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/05/2019
ms.locfileid: "55744785"
---
# <a name="azure-data-box-disk-limits"></a>Omezení disku Azure Data Box


Jak nasadit a provozovat řešení Microsoft Azure Data Box Disk vezměte v úvahu tyto limity. 

## <a name="data-box-service-limits"></a>Omezení služby data Box

 - Služba data Box je dostupná jenom v USA, Evropa, Kanadě a Austrálii ve všech oblastech Azure pro veřejný cloud Azure.
 - Data Box Disk podporuje jeden účet úložiště.

## <a name="data-box-disk-performance"></a>Data Box Disk výkonu

Při testování s disky připojenými přes USB 3.0 byl výkon disku až 430 MB/s. Skutečné hodnoty se liší v závislosti na velikosti použitých souborů. U menších souborů může být výkon nižší.

## <a name="azure-storage-limits"></a>Omezení služby Azure storage

Tato část popisuje omezení pro službu Azure Storage a požadované zásady vytváření názvů pro soubory Azure, objekty BLOB bloku Azure a objekty BLOB stránky Azure, případně ke službě Data Box. Pečlivě zkontrolujte omezení úložiště a postupujte podle všech doporučení.

Nejnovější informace o omezení služby Azure storage a osvědčené postupy pro zadávání názvů sdílených složek, kontejnery a souborů přejděte na:

- [Pojmenování a odkazování na ně kontejnery](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata)
- [Pojmenování sdílených složek a odkazování na ně](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata)
- [Objekty BLOB bloku a vytváření názvů objektů blob stránky](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs)

> [!IMPORTANT]
> Pokud jsou všechny soubory a adresáře, které překračují omezení služby Azure Storage, nebo není v souladu s zásady vytváření názvů souborů a objektů Blob v Azure, pak tyto soubory nebo adresáře se ingestuje do služby Azure Storage pomocí služby Data Box.

## <a name="data-upload-caveats"></a>Odesílání dat upozornění

- Nekopírovat data přímo do disky. Kopírování dat do předem vytvořené *BlockBlob* a *PageBlob* složek.
- Složku ve složce *BlockBlob* a *PageBlob* je kontejner. Například kontejnery jsou vytvořeny jako *BlockBlob/kontejneru* a *PageBlob/kontejneru*.
- Pokud máte existující objekt Azure (třeba jako objekt blob) v cloudu se stejným názvem jako objekt, který je kopírování disku Data Box přepíše soubor v cloudu.
- Každý soubor zapsán do *BlockBlob* a *PageBlob* sdílené složky se nahraje jako objekt blob bloku a objektů blob stránky.
- Všechny prázdné hierarchii adresářů (bez jakékoli soubory) vytvořené v rámci *BlockBlob* a *PageBlob* složky nebylo odesláno.
- Pokud nejsou žádné chyby při odesílání dat do Azure, vytvoří se protokol chyb v účtu cílového úložiště. Cesta k tento protokol chyb je k dispozici na portálu, když se nahrávání dokončí, a vy můžete zkontrolovat protokol k provedení nápravné akce. Neodstraňujte data ze zdroje bez ověření odesílaná data.

## <a name="azure-storage-account-size-limits"></a>Omezení velikosti účtu úložiště Azure

Tady jsou omezení velikosti dat, která je zkopírován do účtu úložiště. Ujistěte se, že data, která nahrajete odpovídá tato omezení. Nejaktuálnější informace o těchto omezeních najdete v části [cíle škálování Azure blob storage](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets#azure-blob-storage-scale-targets) a [soubory Azure škálovat cíle](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets#azure-files-scale-targets).

| Velikost dat zkopírována do účtu úložiště Azure                      | Výchozí omezení          |
|---------------------------------------------------------------------|------------------------|
| Objekt Blob bloku a stránky objektu blob                                            | 500 TB na jeden účet úložiště. <br> To zahrnuje data ze všech zdrojů, včetně disku Data Box.|


## <a name="azure-object-size-limits"></a>Omezení velikosti objektu Azure

Tady jsou velikosti Azure objekty, které je možné zapisovat. Ujistěte se, že všechny soubory, které jsou odeslány odpovídají tyto limity.

| Typ objektu Azure | Výchozí omezení                                             |
|-------------------|-----------------------------------------------------------|
| Objekt blob bloku        | ~ 4,75 TB                                                 |
| Objekt blob stránky         | 8 TiB <br> (Každý soubor odeslat ve formátu objektů Blob stránky musí být zarovnaná 512 bajtů (integrální více), jinak se odeslání nezdaří. <br> VHD a VHDX jsou 512 bajtů zarovnána.) |


## <a name="azure-block-blob-and-page-blob-naming-conventions"></a>Objekt blob bloku Azure a zásady vytváření názvů objektů blob stránky

| Entita                                       | Zásady                                                                                                                                                                                                                                                                                                               |
|----------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Názvy kontejnerů objektů blob bloku a objektů blob stránky | Musí být platný název DNS, který je dlouhý 3 až 63 znaků. <br>  Musí začínat písmenem nebo číslicí. <br> Může obsahovat jenom malá písmena, číslice a pomlčky (-). <br> Každé pomlčce (-) musí bezprostředně předcházet číslice (0–9) nebo malé písmeno (a–z) a také po ní musí následovat. <br> Názvy nesmí obsahovat po sobě jdoucí pomlčky. |
| Názvy objektů blob bloku a objektů blob stránky      | Názvy objektů blob rozlišují velká a malá písmena a smí obsahovat libovolnou kombinaci znaků. <br> Název objektu blob musí mít délku 1 až 1024 znaků. <br> Vyhrazené znaky v adresách URL musí být správně uzavřené do uvozovek. <br>Počet segmentů cesty, ze kterých se název objektu blob skládá, nesmí překročit 254. Segment cesty je řetězec mezi po sobě jdoucími znaky oddělovače (třeba lomítko „/“), který odpovídá názvu virtuálního adresáře. |


## <a name="next-steps"></a>Další postup
* Kontrola [požadavky na systém zařízení Data Box](data-box-system-requirements.md)
