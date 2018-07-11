---
title: Ferramentas para desenvolvedores Go
description: Ferramentas para trabalhar com o SDK do Azure para linguagem Go e serviços do Azure
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 01/30/2018
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-sdk-go
ms.devlang: go
ms.openlocfilehash: 006d140bffb66fdd769a14511232d4ea5081811d
ms.sourcegitcommit: 181d4e0b164cf39b3feac346f559596bd19c94db
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38066975"
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a><span data-ttu-id="d82f3-103">Ferramentas para desenvolvedores usando o SDK do Azure para linguagem Go</span><span class="sxs-lookup"><span data-stu-id="d82f3-103">Tools for developers using the Azure SDK for Go</span></span>

<span data-ttu-id="d82f3-104">Para escrever código Go de forma eficaz e fazer com que ele funcione perfeitamente com os serviços do Azure, aqui estão algumas ferramentas recomendadas.</span><span class="sxs-lookup"><span data-stu-id="d82f3-104">To be effective at writing Go code and have it work seamlessly with Azure services, here are some recommended tools.</span></span>

## <a name="azure-cli-20"></a><span data-ttu-id="d82f3-105">CLI do Azure 2.0</span><span class="sxs-lookup"><span data-stu-id="d82f3-105">Azure CLI 2.0</span></span>

<span data-ttu-id="d82f3-106">A CLI do Azure 2.0 fornece uma interface de linha de comando para criar e configurar os recursos do Azure em suas assinaturas.</span><span class="sxs-lookup"><span data-stu-id="d82f3-106">The Azure 2.0 CLI provides a command-line interface to create and configure Azure resources in your subscriptions.</span></span> <span data-ttu-id="d82f3-107">A CLI pode ajudá-lo a começar a criar recursos do Azure comuns e compartilhados rapidamente, para que você possa se concentrar no uso de serviços mais complexos.</span><span class="sxs-lookup"><span data-stu-id="d82f3-107">The CLI can help you get started building common and shared Azure resources quickly, so that you can focus on more complex usage of services.</span></span> <span data-ttu-id="d82f3-108">A CLI tem recursos de filtragem e de consulta para que você possa direcionar a saída diretamente para suas ferramentas favoritas de linha de comando.</span><span class="sxs-lookup"><span data-stu-id="d82f3-108">The CLI has query and filtering features so you can pipe output directly to your favorite command-line tools.</span></span> <span data-ttu-id="d82f3-109">A CLI está disponível para instalação em seu sistema local, como uma imagem do Docker ou por meio do [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).</span><span class="sxs-lookup"><span data-stu-id="d82f3-109">The CLI is available for installation on your local system, as a Docker image, or through [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="d82f3-110">Instalar a CLI 2.0 do Azure</span><span class="sxs-lookup"><span data-stu-id="d82f3-110">Install the Azure CLI 2.0</span></span>](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a><span data-ttu-id="d82f3-111">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d82f3-111">Visual Studio Code</span></span>

<span data-ttu-id="d82f3-112">O Visual Studio Code é um editor leve que tem suporte abrangente para a linguagem Go por meio de extensões.</span><span class="sxs-lookup"><span data-stu-id="d82f3-112">Visual Studio Code is a lightweight editor that has comprehensive support for the Go language through extensions.</span></span> <span data-ttu-id="d82f3-113">Essas extensões incluem suporte para recursos como preenchimento automático, modelos `impl`, refatoração e depuração.</span><span class="sxs-lookup"><span data-stu-id="d82f3-113">These extensions include support for features like autocomplete, `impl` templates, refactoring, and debugging.</span></span> <span data-ttu-id="d82f3-114">O Visual Studio Code também oferece várias extensões para ferramentas de desenvolvedor comuns, como controle de origem e até mesmo oferece extensões para interações diretas com os serviços do Azure.</span><span class="sxs-lookup"><span data-stu-id="d82f3-114">Visual Studio Code also offers many extensions for common developer tools such as source control, and even offers extensions for direct interactions with Azure services.</span></span> <span data-ttu-id="d82f3-115">A Microsoft mantém uma meta-extensão oficial incluindo essas extensões do Azure, incluindo uma interface interativa para a CLI do Azure.</span><span class="sxs-lookup"><span data-stu-id="d82f3-115">Microsoft maintains an official meta-extension including these Azure extensions, including an interactive interface for the Azure CLI.</span></span>

* [<span data-ttu-id="d82f3-116">Instalar o Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d82f3-116">Install Visual Studio Code</span></span>](https://code.visualstudio.com/Download)
* [<span data-ttu-id="d82f3-117">Obter a extensão Go do Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d82f3-117">Get the Visual Studio Code Go extension</span></span>](https://code.visualstudio.com/docs/languages/go)
* [<span data-ttu-id="d82f3-118">Obter a extensão de Ferramentas do Azure</span><span class="sxs-lookup"><span data-stu-id="d82f3-118">Get the Azure Tools extension</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="dependency-management-with-dep"></a><span data-ttu-id="d82f3-119">Gerenciamento de dependência com dep</span><span class="sxs-lookup"><span data-stu-id="d82f3-119">Dependency management with dep</span></span>

<span data-ttu-id="d82f3-120">Há muitas maneiras de gerenciar suas dependências de pacote e fazer vendoring com Go, porque não há nenhuma solução oficial ainda.</span><span class="sxs-lookup"><span data-stu-id="d82f3-120">There are many ways to manage your package dependencies and do vendoring with Go, since there is no official solution yet.</span></span> <span data-ttu-id="d82f3-121">É a maneira recomendada para executar o gerenciamento com o gerenciador de dependência `dep`.</span><span class="sxs-lookup"><span data-stu-id="d82f3-121">The recommended way to perform this management is with the `dep` dependency manager.</span></span> <span data-ttu-id="d82f3-122">O SDK do Azure para linguagem Go usa dep para seu vendoring e garante obter corretamente dependências para qualquer outro projeto usando o dep.</span><span class="sxs-lookup"><span data-stu-id="d82f3-122">The Azure SDK for Go uses dep for its vendoring, and is guaranteed to correctly get dependencies for any other project using dep.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="d82f3-123">Obter o gerente de dependência de dep</span><span class="sxs-lookup"><span data-stu-id="d82f3-123">Get the dep dependency manager</span></span>](https://github.com/golang/dep)
