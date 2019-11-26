# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="34a22-101">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="34a22-101">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="34a22-102">Presione Ctrl+F5 para ejecutarla sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="34a22-102">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="34a22-103">Visual Studio inicia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) y ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="34a22-103">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="34a22-104">En la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="34a22-104">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="34a22-105">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="34a22-105">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="34a22-106">Localhost solo sirve las solicitudes web del equipo local.</span><span class="sxs-lookup"><span data-stu-id="34a22-106">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="34a22-107">Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web.</span><span class="sxs-lookup"><span data-stu-id="34a22-107">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
 
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="34a22-108">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="34a22-108">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="34a22-109">Presione **Ctrl+F5** para ejecutar sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="34a22-109">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="34a22-110">Visual Studio Code inicia [Kestrel](xref:fundamentals/servers/kestrel) y un explorador, y se desplaza hasta `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="34a22-110">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="34a22-111">En la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="34a22-111">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="34a22-112">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="34a22-112">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="34a22-113">Localhost solo sirve las solicitudes web del equipo local.</span><span class="sxs-lookup"><span data-stu-id="34a22-113">Localhost only serves web requests from the local computer.</span></span>

  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="34a22-114">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="34a22-114">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="34a22-115">En Visual Studio, presione **Alt-Cmd-ENTRAR** para realizar la ejecución sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="34a22-115">From Visual Studio, press **Alt-Cmd-Enter** to run without the debugger.</span></span> <span data-ttu-id="34a22-116">O bien, en la barra de menús, vaya a **Ejecutar > Iniciar sin depurar**.</span><span class="sxs-lookup"><span data-stu-id="34a22-116">Alternatively, navigate to the menu bar and go to **Run>Start Without Debugging**.</span></span>

  <span data-ttu-id="34a22-117">Visual Studio iniciará [Kestrel](xref:fundamentals/servers/kestrel) y un explorador, y se desplazará a `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="34a22-117">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

<!-- End of VS tabs -->

---