---
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 02/14/2018
ms.topic: include
ms.prod: azure
ms.technology: azure-cli
ms.openlocfilehash: d021dd8ef4744b7c50b296b231bf63481f92411a
ms.sourcegitcommit: 2a3bd491e087a1d0e7d269bed896c029357d62a6
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38988052"
---
O [SDK do Azure para Go](https://github.com/Azure/azure-sdk-for-go) é compatível com as versões 1.8 e mais recentes do Go. Para ambientes usando [Perfis do Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-version-profiles), a versão 1.9 do Go é o requisito mínimo.
Se você não tiver Go disponível em seu sistema, siga [as instruções de instalação do Go](https://golang.org/doc/install).

Você pode obter o SDK do Azure para Go e suas dependências via `go get`.

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> Certifique-se de que você coloca `Azure` em letra maiúscula na URL. Fazer o contrário pode causar problemas de importação relacionadas a caso ao trabalhar com o SDK. Você também precisa colocar `Azure` em maiúscula em suas instruções de importação.
