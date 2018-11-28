---
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/05/2018
ms.topic: include
ms.prod: azure
ms.technology: azure-cli
ms.openlocfilehash: 5df14f939efdd0550b49261c88c8dc6518ada459
ms.sourcegitcommit: 887b15afcdeaf926a5f3d21b64e4045167fd062c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/23/2018
ms.locfileid: "52293466"
---
<span data-ttu-id="09009-101">O [SDK do Azure para linguagem Go](https://github.com/Azure/azure-sdk-for-go) é compatível com as versões 1.8 e mais recentes da linguagem Go.</span><span class="sxs-lookup"><span data-stu-id="09009-101">The [Azure SDK for Go](https://github.com/Azure/azure-sdk-for-go) is compatible with Go versions 1.8 and higher.</span></span> <span data-ttu-id="09009-102">Para ambientes usando [Perfis do Azure Stack](/azure/azure-stack/user/azure-stack-version-profiles-go), a versão 1.9 do Go é o requisito mínimo.</span><span class="sxs-lookup"><span data-stu-id="09009-102">For environments using [Azure Stack Profiles](/azure/azure-stack/user/azure-stack-version-profiles-go), Go version 1.9 is the minimum requirement.</span></span>
<span data-ttu-id="09009-103">Se você precisar instalar a linguagem Go, siga [as instruções de instalação da linguagem Go](https://golang.org/doc/install).</span><span class="sxs-lookup"><span data-stu-id="09009-103">If you need to install Go, follow [the Go installation instructions](https://golang.org/doc/install).</span></span>

<span data-ttu-id="09009-104">Você pode obter o SDK do Azure para linguagem Go e suas dependências via `go get`.</span><span class="sxs-lookup"><span data-stu-id="09009-104">You can download the Azure SDK for Go and its dependencies via `go get`.</span></span>

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> <span data-ttu-id="09009-105">Certifique-se de que você coloca `Azure` em letra maiúscula na URL.</span><span class="sxs-lookup"><span data-stu-id="09009-105">Make sure that you capitalize `Azure` in the URL.</span></span> <span data-ttu-id="09009-106">Fazer o contrário pode causar problemas de importação relacionadas a caso ao trabalhar com o SDK.</span><span class="sxs-lookup"><span data-stu-id="09009-106">Doing otherwise can cause case-related import problems when working with the SDK.</span></span> <span data-ttu-id="09009-107">Você também precisa colocar `Azure` em maiúscula em suas instruções de importação.</span><span class="sxs-lookup"><span data-stu-id="09009-107">You also need to capitalize `Azure` in your import statements.</span></span>
