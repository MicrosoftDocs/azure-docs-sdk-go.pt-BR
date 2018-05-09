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
<span data-ttu-id="0f60b-101">O [SDK do Azure para Go](https://github.com/Azure/azure-sdk-for-go) é compatível com as versões 1.8 e mais recentes do Go.</span><span class="sxs-lookup"><span data-stu-id="0f60b-101">The [Azure SDK for Go](https://github.com/Azure/azure-sdk-for-go) is compatible with Go versions 1.8 and later.</span></span> <span data-ttu-id="0f60b-102">Para ambientes usando [Perfis do Azure Stack](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-version-profiles), a versão 1.9 do Go é o requisito mínimo.</span><span class="sxs-lookup"><span data-stu-id="0f60b-102">For environments using [Azure Stack Profiles](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-version-profiles), Go version 1.9 is the minimum requirement.</span></span>
<span data-ttu-id="0f60b-103">Se você não tiver Go disponível em seu sistema, siga [as instruções de instalação do Go](https://golang.org/doc/install).</span><span class="sxs-lookup"><span data-stu-id="0f60b-103">If you do not have Go available on your system, follow [the Go installation instructions](https://golang.org/doc/install).</span></span>

<span data-ttu-id="0f60b-104">Você pode obter o SDK do Azure para Go e suas dependências via `go get`.</span><span class="sxs-lookup"><span data-stu-id="0f60b-104">You can obtain the Azure SDK for Go and its dependencies via `go get`.</span></span>

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> <span data-ttu-id="0f60b-105">Certifique-se de que você coloca `Azure` em letra maiúscula na URL.</span><span class="sxs-lookup"><span data-stu-id="0f60b-105">Make sure that you capitalize `Azure` in the URL.</span></span> <span data-ttu-id="0f60b-106">Fazer o contrário pode causar problemas de importação relacionadas a caso ao trabalhar com o SDK.</span><span class="sxs-lookup"><span data-stu-id="0f60b-106">Doing otherwise can cause case-related import problems when working with the SDK.</span></span> <span data-ttu-id="0f60b-107">Você também precisa colocar `Azure` em maiúscula em suas instruções de importação.</span><span class="sxs-lookup"><span data-stu-id="0f60b-107">You also need to capitalize `Azure` in your import statements.</span></span>

