---
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 02/14/2018
ms.topic: include
ms.prod: azure
ms.technology: azure-cli
ms.openlocfilehash: ddd58efdbc0c2d3ded068a9bebf2466db702566f
ms.sourcegitcommit: f08abf902b48f8173aa6e261084ff2cfc9043305
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/03/2018
---
O [SDK do Azure para Go](https://github.com/Azure/azure-sdk-for-go) é compatível com as versões 1.8 e mais recentes do Go. Para ambientes usando [Perfis do Azure Stack](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-version-profiles), a versão 1.9 do Go é o requisito mínimo.
Se você não tiver Go disponível em seu sistema, siga [as instruções de instalação do Go](https://golang.org/doc/install).

Você pode obter o SDK do Azure para Go e suas dependências via `go get`.

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> Certifique-se de que você coloca `Azure` em letra maiúscula na URL. Fazer o contrário pode causar problemas de importação relacionadas a caso ao trabalhar com o SDK. Você também precisa colocar `Azure` em maiúscula em suas instruções de importação.

