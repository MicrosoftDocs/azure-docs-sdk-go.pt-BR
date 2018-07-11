---
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 02/14/2018
ms.topic: include
ms.prod: azure
ms.technology: azure-cli
ms.openlocfilehash: 0febc2cc42ad95a1b7a5032c7987e37cc82f374e
ms.sourcegitcommit: 181d4e0b164cf39b3feac346f559596bd19c94db
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38067026"
---
O [SDK do Azure para Go](https://github.com/Azure/azure-sdk-for-go) é compatível com as versões 1.8 e mais recentes do Go. Para ambientes usando [Perfis do Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-version-profiles), a versão 1.9 do Go é o requisito mínimo.
Se você não tiver Go disponível em seu sistema, siga [as instruções de instalação do Go](https://golang.org/doc/install).

Você pode obter o SDK do Azure para Go e suas dependências via `go get`.

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> Certifique-se de que você coloca `Azure` em letra maiúscula na URL. Fazer o contrário pode causar problemas de importação relacionadas a caso ao trabalhar com o SDK. Você também precisa colocar `Azure` em maiúscula em suas instruções de importação.

