---
title: Instale o SDK do Azure para linguagem Go
description: Como instalar, fornecer e configurar o SDK do Azure para linguagem Go.
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 03/14/2018
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-sdk-go
ms.devlang: go
ms.openlocfilehash: ad77bdff881770512a828b19dc7af4821f4a55ad
ms.sourcegitcommit: f08abf902b48f8173aa6e261084ff2cfc9043305
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/03/2018
---
# <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="ee7be-103">Instale o SDK do Azure para linguagem Go</span><span class="sxs-lookup"><span data-stu-id="ee7be-103">Install the Azure SDK for Go</span></span>

<span data-ttu-id="ee7be-104">Bem-vindo ao SDK do Azure para linguagem Go!</span><span class="sxs-lookup"><span data-stu-id="ee7be-104">Welcome to the Azure SDK for Go!</span></span> <span data-ttu-id="ee7be-105">O SDK permite gerenciar e interagir com os serviços do Azure em seus aplicativos Go.</span><span class="sxs-lookup"><span data-stu-id="ee7be-105">The SDK allows you to manage and interact with Azure services from your Go applications.</span></span>

## <a name="get-the-azure-sdk-for-go"></a><span data-ttu-id="ee7be-106">Obtenha o SDK do Azure para linguagem Go</span><span class="sxs-lookup"><span data-stu-id="ee7be-106">Get the Azure SDK for Go</span></span>

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

<span data-ttu-id="ee7be-107">Trabalhar com os Azure Storage Blobs requer um SDK separado.</span><span class="sxs-lookup"><span data-stu-id="ee7be-107">Working with Azure Storage Blobs requires a separate SDK.</span></span>

```bash
go get -u -d github.com/Azure/azure-storage-blob-go/...
```

## <a name="vendor-the-azure-sdk-for-go"></a><span data-ttu-id="ee7be-108">Fornecimento do SDK do Azure para linguagem Go</span><span class="sxs-lookup"><span data-stu-id="ee7be-108">Vendor the Azure SDK for Go</span></span>

<span data-ttu-id="ee7be-109">O SDK do Azure para linguagem Go pode usar a funcionalidade de vendoring pelo [dep](https://github.com/golang/dep).</span><span class="sxs-lookup"><span data-stu-id="ee7be-109">The Azure SDK for Go may be vendored through [dep](https://github.com/golang/dep).</span></span> <span data-ttu-id="ee7be-110">Por motivos de estabilidade, é recomendado usar vendoring.</span><span class="sxs-lookup"><span data-stu-id="ee7be-110">For stability reasons, vendoring is recommended.</span></span> <span data-ttu-id="ee7be-111">Para usar o suporte `dep`, adicione o `github.com/Azure/azure-sdk-for-go` a uma seção de `[[constraint]]` do seu `Gopkg.toml`.</span><span class="sxs-lookup"><span data-stu-id="ee7be-111">In order to use `dep` support, add `github.com/Azure/azure-sdk-for-go` to a `[[constraint]]` section of your `Gopkg.toml`.</span></span> <span data-ttu-id="ee7be-112">Por exemplo, para o vendor na versão `14.0.0`, adicione a seguinte entrada:</span><span class="sxs-lookup"><span data-stu-id="ee7be-112">For example, to vendor on version `14.0.0`, add the following entry:</span></span>

```
[[constraint]]
name = "github.com/Azure/azure-sdk-for-go"
version = "14.0.0"
```

## <a name="include-the-azure-sdk-for-go-in-your-project"></a><span data-ttu-id="ee7be-113">Incluir o SDK do Azure para linguagem Go em seu projeto</span><span class="sxs-lookup"><span data-stu-id="ee7be-113">Include the Azure SDK for Go in your project</span></span>

<span data-ttu-id="ee7be-114">Para usar os serviços do Azure a partir de seu código Go, importe todos os serviços com os quais você interage e os módulos `autorest` necessários.</span><span class="sxs-lookup"><span data-stu-id="ee7be-114">To use Azure services from your Go code, import any services you interact with and the required `autorest` modules.</span></span>
<span data-ttu-id="ee7be-115">Você obterá uma lista completa dos módulos disponíveis do GoDoc para os [serviços disponíveis](https://godoc.org/github.com/Azure/azure-sdk-for-go) e [pacotes AutoRest](https://godoc.org/github.com/Azure/go-autorest).</span><span class="sxs-lookup"><span data-stu-id="ee7be-115">You get a complete list of the available modules from GoDoc for [available services](https://godoc.org/github.com/Azure/azure-sdk-for-go) and [AutoRest packages](https://godoc.org/github.com/Azure/go-autorest).</span></span> <span data-ttu-id="ee7be-116">Os pacotes mais comuns do `go-autorest` de que você precisa são:</span><span class="sxs-lookup"><span data-stu-id="ee7be-116">The most common packages you need from `go-autorest` are:</span></span>

| <span data-ttu-id="ee7be-117">Pacote</span><span class="sxs-lookup"><span data-stu-id="ee7be-117">Package</span></span> | <span data-ttu-id="ee7be-118">DESCRIÇÃO</span><span class="sxs-lookup"><span data-stu-id="ee7be-118">Description</span></span> |
|---------|-------------|
| <span data-ttu-id="ee7be-119">[github.com/Azure/go-autorest/autorest][autorest]</span><span class="sxs-lookup"><span data-stu-id="ee7be-119">[github.com/Azure/go-autorest/autorest][autorest]</span></span> | <span data-ttu-id="ee7be-120">Objetos para lidar com a autenticação de cliente de serviço</span><span class="sxs-lookup"><span data-stu-id="ee7be-120">Objects for handling service client authentication</span></span> |
| <span data-ttu-id="ee7be-121">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span><span class="sxs-lookup"><span data-stu-id="ee7be-121">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span></span> | <span data-ttu-id="ee7be-122">Constantes para interações com os serviços do Azure</span><span class="sxs-lookup"><span data-stu-id="ee7be-122">Constants for interactions with Azure services</span></span> |
| <span data-ttu-id="ee7be-123">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span><span class="sxs-lookup"><span data-stu-id="ee7be-123">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span></span> | <span data-ttu-id="ee7be-124">Mecanismos de autenticação para acessar os serviços do Azure</span><span class="sxs-lookup"><span data-stu-id="ee7be-124">Authentication mechanisms for accessing Azure services</span></span> |
| <span data-ttu-id="ee7be-125">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span><span class="sxs-lookup"><span data-stu-id="ee7be-125">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span></span> | <span data-ttu-id="ee7be-126">Digite os auxiliares de asserção para trabalhar com as estruturas de dados do SDK do Azure</span><span class="sxs-lookup"><span data-stu-id="ee7be-126">Type assertion helpers for working with Azure SDK data structures</span></span> |

[autorest]: https://godoc.org/github.com/Azure/go-autorest/autorest
[autorest/azure]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure
[autorest/adal]: https://godoc.org/github.com/Azure/go-autorest/autorest/adal
[autorest/to]: https://godoc.org/github.com/Azure/go-autorest/autorest/to

<span data-ttu-id="ee7be-127">Os módulos para os serviços do Azure têm controle de versão independentemente das APIs do SDK para eles.</span><span class="sxs-lookup"><span data-stu-id="ee7be-127">Modules for Azure services are versioned independently from the SDK APIs for them.</span></span> <span data-ttu-id="ee7be-128">Essas versões são parte do caminho de importação do módulo e são de uma _versão do serviço_ ou um _perfil_.</span><span class="sxs-lookup"><span data-stu-id="ee7be-128">These versions are part of the module import path, and are from either a _service version_ or a _profile_.</span></span> <span data-ttu-id="ee7be-129">No momento, é recomendável que você use uma versão de serviço específica para desenvolvimento e versão.</span><span class="sxs-lookup"><span data-stu-id="ee7be-129">Currently, it is recommended that you use a specific service version for both development and release.</span></span> <span data-ttu-id="ee7be-130">Os serviços são localizados no módulo `services`.</span><span class="sxs-lookup"><span data-stu-id="ee7be-130">Services are located under the `services` module.</span></span> <span data-ttu-id="ee7be-131">O caminho completo para a importação é o nome do serviço, seguido da versão no formato `YYYY-MM-DD`, seguido do nome do serviço novamente.</span><span class="sxs-lookup"><span data-stu-id="ee7be-131">The full path for the import is the name of the service, followed by the version in `YYYY-MM-DD` format, followed by the service name again.</span></span> <span data-ttu-id="ee7be-132">Por exemplo, para incluir a versão `2017-03-30` do serviço de computação:</span><span class="sxs-lookup"><span data-stu-id="ee7be-132">For example, to include the `2017-03-30` version of the Compute service:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/services/compute/mgmt/2017-03-30/compute"
```

<span data-ttu-id="ee7be-133">No momento, é recomendável que você use a versão mais recente de um serviço, a menos que você tenha um motivo para fazer o contrário.</span><span class="sxs-lookup"><span data-stu-id="ee7be-133">Right now it is recommended that you use the latest version of a service, unless you have a reason to do otherwise.</span></span>

<span data-ttu-id="ee7be-134">Se você precisar de um instantâneo coletivo dos serviços, você também pode selecionar uma versão de perfil.</span><span class="sxs-lookup"><span data-stu-id="ee7be-134">If you need a collective snapshot of services, you can also select a single profile version.</span></span> <span data-ttu-id="ee7be-135">Agora, o único perfil bloqueado é a versão `2017-03-09`, que pode não ter os recursos mais recentes dos serviços.</span><span class="sxs-lookup"><span data-stu-id="ee7be-135">Right now, the only locked profile is version `2017-03-09`, which may not have the latest features of services.</span></span> <span data-ttu-id="ee7be-136">Os perfis estão localizados no módulo `profiles`, com a versão no formato `YYYY-MM-DD`.</span><span class="sxs-lookup"><span data-stu-id="ee7be-136">Profiles are located under the `profiles` module, with their version in the `YYYY-MM-DD` format.</span></span> <span data-ttu-id="ee7be-137">Os serviços são agrupados em sua versão de perfil.</span><span class="sxs-lookup"><span data-stu-id="ee7be-137">Services are grouped under their profile version.</span></span> <span data-ttu-id="ee7be-138">Por exemplo, para importar o módulo de gerenciamento de recursos do Azure a partir do perfil `2017-03-09`:</span><span class="sxs-lookup"><span data-stu-id="ee7be-138">For example, to import the Azure Resources management module from the `2017-03-09` profile:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/resources/mgmt/resources"
```

> [!WARNING]
> <span data-ttu-id="ee7be-139">Os perfis `preview` e `latest` também estão disponíveis.</span><span class="sxs-lookup"><span data-stu-id="ee7be-139">There are also `preview` and `latest` profiles available.</span></span> <span data-ttu-id="ee7be-140">Porém, não é recomendado usá-los.</span><span class="sxs-lookup"><span data-stu-id="ee7be-140">Using them is not recommended.</span></span> <span data-ttu-id="ee7be-141">Esses perfis são versões progressivas, e o comportamento do serviço poderá mudar a qualquer momento.</span><span class="sxs-lookup"><span data-stu-id="ee7be-141">These profiles are rolling versions and service behavior may change at any time.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ee7be-142">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="ee7be-142">Next steps</span></span>

<span data-ttu-id="ee7be-143">Para começar a usar o SDK do Azure para linguagem Go, experimente um início rápido.</span><span class="sxs-lookup"><span data-stu-id="ee7be-143">To begin using the Azure SDK for Go, try out a quickstart.</span></span>

* [<span data-ttu-id="ee7be-144">Implantar uma máquina virtual no Azure a partir de um modelo</span><span class="sxs-lookup"><span data-stu-id="ee7be-144">Deploy a virtual machine from a template</span></span>](azure-sdk-go-qs-vm.md)
* [<span data-ttu-id="ee7be-145">Transferir objetos para o Armazenamento de Blobs do Azure usando o SDK de Blobs do Azure para linguagem Go</span><span class="sxs-lookup"><span data-stu-id="ee7be-145">Transfer objects to Azure Blob Storage with the Azure Blob SDK for Go</span></span>](/azure/storage/blobs/storage-quickstart-blobs-go?toc=%2fgo%2fazure%2ftoc.json)
* [<span data-ttu-id="ee7be-146">Conectar-se ao Banco de Dados do Azure para PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="ee7be-146">Connect to Azure Database for PostgreSQL</span></span>](/azure/postgresql/connect-go?toc=%2fgo%2fazure%2ftoc.json)

<span data-ttu-id="ee7be-147">Se você quiser começar com outros serviços no SDK Go imediatamente, dê uma olhada em alguns códigos de exemplo disponíveis.</span><span class="sxs-lookup"><span data-stu-id="ee7be-147">If you want to get started with other services in the Go SDK immediately, take a look at some of the available sample code.</span></span>

* [<span data-ttu-id="ee7be-148">Autenticar com os serviços do Azure</span><span class="sxs-lookup"><span data-stu-id="ee7be-148">Authenticate with Azure services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/iam)
* [<span data-ttu-id="ee7be-149">Implantar novas máquinas virtuais com autenticação SSH</span><span class="sxs-lookup"><span data-stu-id="ee7be-149">Deploy new virtual machines with SSH authentication</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)
* [<span data-ttu-id="ee7be-150">Implantar uma imagem de contêiner nas Instâncias de Contêiner do Azure</span><span class="sxs-lookup"><span data-stu-id="ee7be-150">Deploy a container image to Azure Container Instances</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerinstance)
* [<span data-ttu-id="ee7be-151">Criar um cluster no Serviço Kubernetes do Azure</span><span class="sxs-lookup"><span data-stu-id="ee7be-151">Create a cluster in Azure Kubernetes Service</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerservice)
* [<span data-ttu-id="ee7be-152">Trabalhar com serviços de armazenamento do Azure</span><span class="sxs-lookup"><span data-stu-id="ee7be-152">Work with Azure Storage services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage)
* [<span data-ttu-id="ee7be-153">Todos os exemplos do SDK do Azure para linguagem Go</span><span class="sxs-lookup"><span data-stu-id="ee7be-153">All samples for the Azure SDK for Go</span></span>](https://github.com/azure-samples/azure-sdk-for-go-samples)
