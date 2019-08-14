---
title: 调试 ASP.NET Core Blazor
author: guardrex
description: 了解如何调试 Blazor 应用程序。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/31/2019
uid: blazor/debug
ms.openlocfilehash: 37c6009727a4f62b61793adca0d83cdd53be4b9a
ms.sourcegitcommit: 3204bc89ae6354b61ee0a9b2770ebe5214b7790c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2019
ms.locfileid: "68948367"
---
# <a name="debug-aspnet-core-blazor"></a><span data-ttu-id="f225e-103">调试 ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="f225e-103">Debug ASP.NET Core Blazor</span></span>

[<span data-ttu-id="f225e-104">Daniel Roth</span><span class="sxs-lookup"><span data-stu-id="f225e-104">Daniel Roth</span></span>](https://github.com/danroth27)

<span data-ttu-id="f225e-105">针对在 Chrome 上调试在 WebAssembly 上运行的 Blazor 客户端应用, 存在*早期*支持。</span><span class="sxs-lookup"><span data-stu-id="f225e-105">*Early* support exists for debugging Blazor client-side apps running on WebAssembly in Chrome.</span></span>

<span data-ttu-id="f225e-106">调试器功能受到限制。</span><span class="sxs-lookup"><span data-stu-id="f225e-106">Debugger capabilities are limited.</span></span> <span data-ttu-id="f225e-107">可用方案包括:</span><span class="sxs-lookup"><span data-stu-id="f225e-107">Available scenarios include:</span></span>

* <span data-ttu-id="f225e-108">设置和删除断点。</span><span class="sxs-lookup"><span data-stu-id="f225e-108">Set and remove breakpoints.</span></span>
* <span data-ttu-id="f225e-109">单步执行 (`F10`), 通过代码或恢复 (`F8`) 代码执行。</span><span class="sxs-lookup"><span data-stu-id="f225e-109">Single-step (`F10`) through the code or resume (`F8`) code execution.</span></span>
* <span data-ttu-id="f225e-110">在 "*局部变量*" 显示中, 观察类型`int`为、 `string`和`bool`的任何本地变量的值。</span><span class="sxs-lookup"><span data-stu-id="f225e-110">In the *Locals* display, observe the values of any local variables of type `int`, `string`, and `bool`.</span></span>
* <span data-ttu-id="f225e-111">请参阅调用堆栈, 其中包括从 JavaScript 到 .NET 和从 .NET 到 JavaScript 的调用链。</span><span class="sxs-lookup"><span data-stu-id="f225e-111">See the call stack, including call chains that go from JavaScript into .NET and from .NET to JavaScript.</span></span>

<span data-ttu-id="f225e-112">*不能*:</span><span class="sxs-lookup"><span data-stu-id="f225e-112">You *can't*:</span></span>

* <span data-ttu-id="f225e-113">观察所有不是`int`、 `string`或`bool`的局部变量的值。</span><span class="sxs-lookup"><span data-stu-id="f225e-113">Observe the values of any locals that aren't an `int`, `string`, or `bool`.</span></span>
* <span data-ttu-id="f225e-114">观察任何类属性或字段的值。</span><span class="sxs-lookup"><span data-stu-id="f225e-114">Observe the values of any class properties or fields.</span></span>
* <span data-ttu-id="f225e-115">将鼠标悬停在变量上以查看其值。</span><span class="sxs-lookup"><span data-stu-id="f225e-115">Hover over variables to see their values.</span></span>
* <span data-ttu-id="f225e-116">在控制台中计算表达式的值。</span><span class="sxs-lookup"><span data-stu-id="f225e-116">Evaluate expressions in the console.</span></span>
* <span data-ttu-id="f225e-117">跨异步调用单步执行。</span><span class="sxs-lookup"><span data-stu-id="f225e-117">Step across async calls.</span></span>
* <span data-ttu-id="f225e-118">执行大多数其他普通调试方案。</span><span class="sxs-lookup"><span data-stu-id="f225e-118">Perform most other ordinary debugging scenarios.</span></span>

<span data-ttu-id="f225e-119">开发进一步的调试方案是工程团队不断关注的重点。</span><span class="sxs-lookup"><span data-stu-id="f225e-119">Development of further debugging scenarios is an on-going focus of the engineering team.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f225e-120">系统必备</span><span class="sxs-lookup"><span data-stu-id="f225e-120">Prerequisites</span></span>

<span data-ttu-id="f225e-121">调试需要以下任一浏览器:</span><span class="sxs-lookup"><span data-stu-id="f225e-121">Debugging requires either of the following browsers:</span></span>

* <span data-ttu-id="f225e-122">Google Chrome (版本70或更高版本)</span><span class="sxs-lookup"><span data-stu-id="f225e-122">Google Chrome (version 70 or later)</span></span>
* <span data-ttu-id="f225e-123">Microsoft Edge 预览 ([边缘开发渠道](https://www.microsoftedgeinsider.com))</span><span class="sxs-lookup"><span data-stu-id="f225e-123">Microsoft Edge preview ([Edge Dev Channel](https://www.microsoftedgeinsider.com))</span></span>

## <a name="procedure"></a><span data-ttu-id="f225e-124">过程</span><span class="sxs-lookup"><span data-stu-id="f225e-124">Procedure</span></span>

1. <span data-ttu-id="f225e-125">在配置中`Debug`运行 Blazor 客户端应用。</span><span class="sxs-lookup"><span data-stu-id="f225e-125">Run a Blazor client-side app in `Debug` configuration.</span></span> <span data-ttu-id="f225e-126">将选项传递到[dotnet run](/dotnet/core/tools/dotnet-run)命令: `dotnet run --configuration Debug`。 `--configuration Debug`</span><span class="sxs-lookup"><span data-stu-id="f225e-126">Pass the `--configuration Debug` option to the [dotnet run](/dotnet/core/tools/dotnet-run) command: `dotnet run --configuration Debug`.</span></span>
1. <span data-ttu-id="f225e-127">在浏览器中访问应用程序。</span><span class="sxs-lookup"><span data-stu-id="f225e-127">Access the app in the browser.</span></span>
1. <span data-ttu-id="f225e-128">将键盘焦点置于应用上, 而不是开发人员工具面板上。</span><span class="sxs-lookup"><span data-stu-id="f225e-128">Place the keyboard focus on the app, not the developer tools panel.</span></span> <span data-ttu-id="f225e-129">启动调试时可关闭 "开发人员工具" 面板。</span><span class="sxs-lookup"><span data-stu-id="f225e-129">The developer tools panel can be closed when debugging is initiated.</span></span>
1. <span data-ttu-id="f225e-130">选择以下 Blazor 特定的键盘快捷方式:</span><span class="sxs-lookup"><span data-stu-id="f225e-130">Select the following Blazor-specific keyboard shortcut:</span></span>
   * <span data-ttu-id="f225e-131">`Shift+Alt+D`在 Windows/Linux 上</span><span class="sxs-lookup"><span data-stu-id="f225e-131">`Shift+Alt+D` on Windows/Linux</span></span>
   * <span data-ttu-id="f225e-132">`Shift+Cmd+D`在 macOS 上</span><span class="sxs-lookup"><span data-stu-id="f225e-132">`Shift+Cmd+D` on macOS</span></span>

## <a name="enable-remote-debugging"></a><span data-ttu-id="f225e-133">启用远程调试</span><span class="sxs-lookup"><span data-stu-id="f225e-133">Enable remote debugging</span></span>

<span data-ttu-id="f225e-134">如果远程调试处于禁用状态, 则**无法查找可调试浏览器选项卡**错误页面由 Chrome 生成。</span><span class="sxs-lookup"><span data-stu-id="f225e-134">If remote debugging is disabled, an **Unable to find debuggable browser tab** error page is generated by Chrome.</span></span> <span data-ttu-id="f225e-135">"错误" 页包含有关运行 Chrome 的说明, 其中打开了调试端口, 以便 Blazor 调试代理可以连接到应用。</span><span class="sxs-lookup"><span data-stu-id="f225e-135">The error page contains instructions for running Chrome with the debugging port open so that the Blazor debugging proxy can connect to the app.</span></span> <span data-ttu-id="f225e-136">*关闭所有 Chrome 实例*, 并按指示重新启动 chrome。</span><span class="sxs-lookup"><span data-stu-id="f225e-136">*Close all Chrome instances* and restart Chrome as instructed.</span></span>

## <a name="debug-the-app"></a><span data-ttu-id="f225e-137">调试应用</span><span class="sxs-lookup"><span data-stu-id="f225e-137">Debug the app</span></span>

<span data-ttu-id="f225e-138">在启用了远程调试的情况下运行 Chrome 后, 调试键盘快捷方式将打开一个新的 "调试器" 选项卡。一段时间后, "**源**" 选项卡显示应用程序中的 .net 程序集列表。</span><span class="sxs-lookup"><span data-stu-id="f225e-138">Once Chrome is running with remote debugging enabled, the debugging keyboard shortcut opens a new debugger tab. After a moment, the **Sources** tab shows a list of the .NET assemblies in the app.</span></span> <span data-ttu-id="f225e-139">展开每个程序集, 找到可用于调试的 *.cs*/文件。</span><span class="sxs-lookup"><span data-stu-id="f225e-139">Expand each assembly and find the *.cs*/*.razor* source files available for debugging.</span></span> <span data-ttu-id="f225e-140">设置断点, 切换回应用的选项卡, 并在代码执行时命中断点。</span><span class="sxs-lookup"><span data-stu-id="f225e-140">Set breakpoints, switch back to the app's tab, and the breakpoints are hit when the code executes.</span></span> <span data-ttu-id="f225e-141">命中断点后, 通过代码或恢复 (`F10``F8`) 代码执行的单步执行 ()。</span><span class="sxs-lookup"><span data-stu-id="f225e-141">After a breakpoint is hit, single-step (`F10`) through the code or resume (`F8`) code execution normally.</span></span>

<span data-ttu-id="f225e-142">Blazor 提供了一个实现[Chrome DevTools 协议](https://chromedevtools.github.io/devtools-protocol/)的调试代理, 并使用对该协议进行了补充。特定于网络的信息。</span><span class="sxs-lookup"><span data-stu-id="f225e-142">Blazor provides a debugging proxy that implements the [Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/) and augments the protocol with .NET-specific information.</span></span> <span data-ttu-id="f225e-143">按下调试键盘快捷方式时, Blazor 会将 Chrome DevTools 指向代理。</span><span class="sxs-lookup"><span data-stu-id="f225e-143">When debugging keyboard shortcut is pressed, Blazor points the Chrome DevTools at the proxy.</span></span> <span data-ttu-id="f225e-144">代理连接到要调试的浏览器窗口 (因此需要启用远程调试)。</span><span class="sxs-lookup"><span data-stu-id="f225e-144">The proxy connects to the browser window you're seeking to debug (hence the need to enable remote debugging).</span></span>

## <a name="browser-source-maps"></a><span data-ttu-id="f225e-145">浏览器源映射</span><span class="sxs-lookup"><span data-stu-id="f225e-145">Browser source maps</span></span>

<span data-ttu-id="f225e-146">浏览器源映射允许浏览器将已编译的文件映射回其原始源文件, 并且通常用于客户端调试。</span><span class="sxs-lookup"><span data-stu-id="f225e-146">Browser source maps allow the browser to map compiled files back to their original source files and are commonly used for client-side debugging.</span></span> <span data-ttu-id="f225e-147">但 Blazor 当前不会直接C#映射到 JAVASCRIPT/WASM。</span><span class="sxs-lookup"><span data-stu-id="f225e-147">However, Blazor doesn't currently map C# directly to JavaScript/WASM.</span></span> <span data-ttu-id="f225e-148">相反, Blazor 会在浏览器中执行 IL 解释, 因此源映射不相关。</span><span class="sxs-lookup"><span data-stu-id="f225e-148">Instead, Blazor does IL interpretation within the browser, so source maps aren't relevant.</span></span>

## <a name="troubleshooting-tip"></a><span data-ttu-id="f225e-149">故障排除提示</span><span class="sxs-lookup"><span data-stu-id="f225e-149">Troubleshooting tip</span></span>

<span data-ttu-id="f225e-150">如果遇到错误, 下列提示可能会有所帮助:</span><span class="sxs-lookup"><span data-stu-id="f225e-150">If you're running into errors, the following tip may help:</span></span>

<span data-ttu-id="f225e-151">在 "**调试器**" 选项卡上, 在浏览器中打开开发人员工具。</span><span class="sxs-lookup"><span data-stu-id="f225e-151">In the **Debugger** tab, open the developer tools in your browser.</span></span> <span data-ttu-id="f225e-152">在控制台中, 执行`localStorage.clear()`以删除所有断点。</span><span class="sxs-lookup"><span data-stu-id="f225e-152">In the console, execute `localStorage.clear()` to remove any breakpoints.</span></span>