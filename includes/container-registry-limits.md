---
title: Plik dyrektywy include
description: Plik dyrektywy include
services: container-registry
author: mmacy
ms.service: container-registry
ms.topic: include
ms.date: 08/30/2018
ms.author: marsma
ms.custom: include file
ms.openlocfilehash: bddb17a5163333a5aeb86b296a21867f0611d12f
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/30/2018
ms.locfileid: "43302530"
---
| Zasób | Podstawowa | Standardowa (Standard) | Premium |
|---|---|---|---|---|
| Magazyn<sup>1</sup> | 10 GiB | 100 GiB| 500 GiB |
| Maksymalny rozmiar warstwy obrazu | 20 giB | 20 giB | 50 giB |
| ReadOps minutę<sup>2, 3</sup> | 1000 | 3,000 | 10 000 |
| WriteOps minutę<sup>2, 4</sup> | 100 | 500 | 2,000 |
| Pobierz przepustowości w MB/s<sup>2</sup> | 30 | 60 | 100 |
| Przekaż przepustowości w MB/s<sup>2</sup> | 10 | 20 | 50 |
| Elementy webhook | 2 | 10 | 100 |
| Replikacja geograficzna | ND | ND | [Obsługiwane][geo-replication] |
| Zaufanie do zawartości (wersja zapoznawcza) | ND | ND | [Obsługiwane][content-trust] |

<sup>1</sup> limity określonego magazynu są ilość *uwzględnione* magazynu dla każdej warstwy. Opłaty są naliczane dodatkowe stawka dzienna za GiB do przechowywania obrazu powyżej tych limitów. Szybkość informacji, zobacz [ceny Container Registry][pricing].

<sup>2</sup> *ReadOps*, *WriteOps*, i *przepustowości* szacunkowe minimalnej. Rejestru Azure container Registry dokłada wszelkich starań zwiększyć wydajność, ponieważ wymaga użycia.

<sup>3</sup> [polecenie docker pull](https://docs.docker.com/registry/spec/api/#pulling-an-image) przekłada się na wiele operacji odczytu, w oparciu o liczbę warstw obrazu, a także pobierania manifestu.

<sup>4</sup> [docker push](https://docs.docker.com/registry/spec/api/#pushing-an-image) przekłada się na wiele operacji zapisu, na podstawie liczby warstw, które muszą zostać przeniesiony. A `docker push` obejmuje *ReadOps* można pobrać manifestu istniejącego obrazu.

<!-- LINKS - External -->
[pricing]: https://azure.microsoft.com/pricing/details/container-registry/

<!-- LINKS - Internal -->
[geo-replication]: ../articles/container-registry/container-registry-geo-replication.md
[content-trust]: ../articles/container-registry/container-registry-content-trust.md