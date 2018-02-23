<span data-ttu-id="8c5f5-101">O SDK do Azure para Go é compatível com as versões 1.8 e mais recentes do Go.</span><span class="sxs-lookup"><span data-stu-id="8c5f5-101">The Azure SDK for Go is compatible with Go versions 1.8 and later.</span></span> <span data-ttu-id="8c5f5-102">Para ambientes usando [Perfis do Azure Stack](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-version-profiles), a versão 1.9 do Go é o requisito mínimo.</span><span class="sxs-lookup"><span data-stu-id="8c5f5-102">For environments using [Azure Stack Profiles](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-version-profiles), Go version 1.9 is the minimum requirement.</span></span> <span data-ttu-id="8c5f5-103">Se você não tiver Go disponível em seu sistema, siga [as instruções de instalação do Go](https://golang.org/doc/install).</span><span class="sxs-lookup"><span data-stu-id="8c5f5-103">If you do not have Go available on your system, follow [the Go installation instructions](https://golang.org/doc/install).</span></span>

<span data-ttu-id="8c5f5-104">Você pode obter o SDK do Azure para Go e suas dependências via `go get`.</span><span class="sxs-lookup"><span data-stu-id="8c5f5-104">You can obtain the Azure SDK for Go and its dependencies via `go get`.</span></span>

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> <span data-ttu-id="8c5f5-105">Certifique-se de que você coloca `Azure` em letra maiúscula na URL.</span><span class="sxs-lookup"><span data-stu-id="8c5f5-105">Make sure that you capitalize `Azure` in the URL.</span></span> <span data-ttu-id="8c5f5-106">Fazer o contrário pode causar problemas de importação relacionadas a caso ao trabalhar com o SDK.</span><span class="sxs-lookup"><span data-stu-id="8c5f5-106">Doing otherwise can cause case-related import problems when working with the SDK.</span></span> <span data-ttu-id="8c5f5-107">Você também precisa colocar `Azure` em maiúscula em suas instruções de importação.</span><span class="sxs-lookup"><span data-stu-id="8c5f5-107">You also need to capitalize `Azure` in your import statements.</span></span>

