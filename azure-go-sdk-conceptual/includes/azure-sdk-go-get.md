---
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/05/2018
ms.topic: include
ms.prod: azure
ms.technology: azure-cli
ms.openlocfilehash: 5df14f939efdd0550b49261c88c8dc6518ada459
ms.sourcegitcommit: 8b9e10b960150dc08f046ab840d6a5627410db29
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44059235"
---
O [SDK do Azure para linguagem Go](https://github.com/Azure/azure-sdk-for-go) é compatível com as versões 1.8 e mais recentes da linguagem Go. Para ambientes usando [Perfis do Azure Stack](/azure/azure-stack/user/azure-stack-version-profiles-go), a versão 1.9 do Go é o requisito mínimo.
Se você precisar instalar a linguagem Go, siga [as instruções de instalação da linguagem Go](https://golang.org/doc/install).

Você pode obter o SDK do Azure para linguagem Go e suas dependências via `go get`.

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> Certifique-se de que você coloca `Azure` em letra maiúscula na URL. Fazer o contrário pode causar problemas de importação relacionadas a caso ao trabalhar com o SDK. Você também precisa colocar `Azure` em maiúscula em suas instruções de importação.
