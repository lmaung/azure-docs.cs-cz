---
title: Předvolby H264 Jeden s přenosovou rychlostí 720p zvuk 5.1 | Dokumentace Microsoftu
description: Téma s přehledem **s jednou přenosovou rychlostí H264 720p zvuk 5.1** přednastavení úloh.
author: Juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.assetid: ba2bfa57-3c87-4b1b-b91b-58f9108f4e85
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2019
ms.author: juliako
ms.openlocfilehash: bbc1da0430656a866bc9022c694f7e70de039c3f
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/11/2019
ms.locfileid: "56000025"
---
# <a name="h264-single-bitrate-720p-audio-51"></a>H264 Single Bitrate 720p Audio 5.1
`Media Encoder Standard` definuje sadu kódování přednastavení, která můžete použít při vytváření úloh kódování. Můžete použít `preset name` zadat do formátu, který chcete zakódovat váš soubor média. Nebo můžete vytvořit vlastní JSON nebo XML na základě předvoleb (pomocí kódování UTF-8 nebo UTF-16. By pak úspěšně prošel zpracováním vlastní přednastaveny tak, aby kodér. Seznam přednastavených názvy všech podporovaných touto `Media Encoder Standard` kodér, naleznete v tématu [předvolby úloh pro Media Encoder Standard](media-services-mes-presets-overview.md).  
  
 Toto téma ukazuje, `H264 Single Bitrate 720p Audio 5.1` přednastavení ve formátu XML a JSON.  
  
 Tato předvolba vytváří jeden soubor MP4 s přenosovou rychlostí 4500 kb/s a zvuku AAC 5.1. Podrobné informace o profilech s přenosovou rychlostí, vzorkování rychlost atd. to přednastavení, zkontrolujte XML nebo JSON, definovaná níže. Vysvětlení znamená, co každý prvek a platné hodnoty pro každý prvek, najdete v článku [schéma Media Encoderu Standard](media-services-mes-schema.md).  
  
 XML  
  
```  
<?xml version="1.0" encoding="utf-16"?>  
<Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">  
  <Encoding>  
    <H264Video>  
      <KeyFrameInterval>00:00:02</KeyFrameInterval>  
      <SceneChangeDetection>true</SceneChangeDetection>  
      <H264Layers>  
        <H264Layer>  
          <Bitrate>4500</Bitrate>  
          <Width>1280</Width>  
          <Height>720</Height>  
          <FrameRate>0/1</FrameRate>  
          <Profile>Auto</Profile>  
          <Level>auto</Level>  
          <BFrames>3</BFrames>  
          <ReferenceFrames>3</ReferenceFrames>  
          <Slices>0</Slices>  
          <AdaptiveBFrame>true</AdaptiveBFrame>  
          <EntropyMode>Cabac</EntropyMode>  
          <BufferWindow>00:00:05</BufferWindow>  
          <MaxBitrate>4500</MaxBitrate>  
        </H264Layer>  
      </H264Layers>  
      <Chapters />  
    </H264Video>  
    <AACAudio>  
      <Profile>AACLC</Profile>  
      <Channels>6</Channels>  
      <SamplingRate>48000</SamplingRate>  
      <Bitrate>384</Bitrate>  
    </AACAudio>  
  </Encoding>  
  <Outputs>  
    <Output FileName="{Basename}_{Width}x{Height}_{VideoBitrate}.mp4">  
      <MP4Format />  
    </Output>  
  </Outputs>  
</Preset>  
```  
  
 JSON  
  
```  
{  
  "Version": 1.0,  
  "Codecs": [  
    {  
      "KeyFrameInterval": "00:00:02",  
      "SceneChangeDetection": true,  
      "H264Layers": [  
        {  
          "Profile": "Auto",  
          "Level": "auto",  
          "Bitrate": 4500,  
          "MaxBitrate": 4500,  
          "BufferWindow": "00:00:05",  
          "Width": 1280,  
          "Height": 720,  
          "BFrames": 3,  
          "ReferenceFrames": 3,  
          "AdaptiveBFrame": true,  
          "Type": "H264Layer",  
          "FrameRate": "0/1"  
        }  
      ],  
      "Type": "H264Video"  
    },  
    {  
      "Profile": "AACLC",  
      "Channels": 6,  
      "SamplingRate": 48000,  
      "Bitrate": 384,  
      "Type": "AACAudio"  
    }  
  ],  
  "Outputs": [  
    {  
      "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",  
      "Format": {  
        "Type": "MP4Format"  
      }  
    }  
  ]  
}  
```
