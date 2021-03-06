---
title: "title:'生成首个 Blazor 应用' author: guardrex description:“逐步生成 Blazor 应用。”"
author: guardrex
description: "monikerRange: '>= aspnetcore-3.0' ms.author: riande ms.custom: mvc ms.date:2020 年 5 月 19 日 no-loc:"
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 05/19/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: tutorials/first-blazor-app
ms.openlocfilehash: 8cf356b5c9876a4114942af27a22df6bd324d0e2
ms.sourcegitcommit: 829dca1d5a7dcccbfe90644101c6be1d1c94ac62
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84355209"
---
# <a name="build-your-first-blazor-app"></a>'Blazor'

'Identity'

'Let's Encrypt' 'Razor'

> [!div class="checklist"]
> * 'SignalR' uid: tutorials/first-blazor-app
> * 生成首个 Blazor 应用
> * 作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)
> * 本教程演示如何生成和修改 Blazor 应用。

您将学习如何：

## <a name="build-components"></a>创建待办事项列表 Blazor 应用项目

1. 修改 Razor 组件 在组件中使用事件处理和数据绑定

1. 在 Blazor 应用中使用依赖关系注入 (DI) 和路由 在本教程结束时，你将拥有一个正常运行的聊天应用。

1. 生成组件 按照 <xref:blazor/get-started> 文章中的指南创建用于本教程的 Blazor 项目。 将项目命名为 ToDoList。

1. 在 Pages 文件夹中浏览应用的三个页面：主页、计数器和提取数据。

   这些页面由 Razor 组件文件（Index.razor、Counter.razor 和 FetchData.razor）实现  。

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Counter1.razor)]

   在“计数器”页上，选择“单击我”按钮，在不刷新页面的情况下增加计数器值。 增加网页的计数器值通常需要编写 JavaScript。 通过 Blazor，可以改为编写 C#。 检查 Counter.razor 文件中 `Counter` 组件的实现。

   *Pages/Counter.razor*： 使用 HTML 定义 `Counter`组件的 UI。 动态呈现逻辑（例如，循环、条件、表达式）是使用名为 [Razor](xref:mvc/views/razor) 的嵌入式 C# 语法添加的。

   HTML 标记和 C# 呈现逻辑在构建时转换为组件类。

   * 生成的 .NET 类的名称与文件名匹配。
   * 组件类的成员在 `@code` 块中定义。
   * 在 `@code` 块中，可以指定组件状态（属性、字段）和方法用于处理事件或定义其他组件逻辑。
   * 然后，可以将这些成员用作组件呈现逻辑的一部分，并用于处理事件。 选中“单击我”按钮时：

1. 调用 `Counter` 组件的已注册 `onclick` 处理程序（`IncrementCount` 方法）。

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. `Counter` 组件重新生成其呈现树。 将新的呈现树与前一个呈现树进行比较。 仅应用对文档对象模型 (DOM) 的修改。

## <a name="use-components"></a>显示的计数将会更新。

修改 `Counter` 组件的 C# 逻辑，使计数递增 2 而不是 1。

1. 重新生成并运行应用以查看更改。

   选择“单击我”按钮。 计数器的值将增加 2。 使用组件

   使用 HTML 语法将组件加入到另一个组件中。

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Index1.razor?highlight=7)]

1. 通过向 `Index` 组件 (Index.razor) 添加 `<Counter />` 元素，将 `Counter` 组件添加到应用的 `Index` 组件。 如果在此体验中使用的是 Blazor WebAssembly，则 `Index` 组件使用 `SurveyPrompt` 组件。

## <a name="component-parameters"></a>将 `<SurveyPrompt>` 元素替换为 `<Counter />` 元素。

如果在此体验中使用的是 Blazor Server 应用，请向 `Index` 组件添加 `<Counter />` 元素： *Pages/Index.razor*： 重新生成并运行应用。

1. `Index` 组件有其自己的计数器。

   * 组件参数
   * 组件也可以有参数。

   组件参数是使用组件类中包含 [`[Parameter]`](xref:Microsoft.AspNetCore.Components.ParameterAttribute) 特性的公共属性定义的。

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Counter.razor?highlight=13,17)]

   <!-- Add back when supported.
       > [!NOTE]
       > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
   -->

1. 使用这些属性在标记中为组件指定参数。 更新组件的 `@code` C# 代码，如下所示：

   添加包含 [`[Parameter]`](xref:Microsoft.AspNetCore.Components.ParameterAttribute) 特性的公共 `IncrementAmount` 属性。

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Index2.razor?highlight=7)]

1. 增加 `currentCount` 的值时，更改 `IncrementCount` 方法以使用 `IncrementAmount` 属性。 *Pages/Counter.razor*： 使用属性在 `Index` 组件的 `<Counter>` 元素中指定 `IncrementAmount` 参数。

## <a name="route-to-components"></a>将计数器递增值设置为 10。

*Pages/Index.razor*： 重新加载 `Index` 组件。 每次选择“单击我”按钮时，计数器值递增 10。

## <a name="dependency-injection"></a>`Counter` 组件中的计数器继续递增 1。

### <a name="blazor-server-experience"></a>路由到组件

Counter.razor 文件顶部的 `@page` 指令指定 `Counter` 组件是路由终结点。 `Counter` 组件处理发送到 `/counter` 的请求。

[!code-csharp[](build-your-first-blazor-app/samples_snapshot/3.x/Startup.cs?highlight=5)]

如果没有 `@page` 指令，组件将无法处理路由的请求，但该组件仍可以被其他组件使用。

依赖关系注入

[!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

Blazor Server 体验

[!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

### <a name="blazor-webassembly-experience"></a>如果使用的是 Blazor Server 应用，则 `WeatherForecastService` 服务在 `Startup.ConfigureServices` 中注册为[单一实例](xref:fundamentals/dependency-injection#service-lifetimes)。

可通过[依赖关系注入 (DI)](xref:fundamentals/dependency-injection) 在整个应用中使用服务的实例：

[`@inject`](xref:mvc/views/razor#inject) 指令用于将 `WeatherForecastService` 服务的实例注入到 `FetchData` 组件中。

[!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1_client.razor?highlight=7-9)]

*Pages/FetchData.razor*：

[!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]

## <a name="build-a-todo-list"></a>`FetchData` 组件使用注入的服务（作为 `ForecastService`）来检索 `WeatherForecast` 对象的数组：

Blazor WebAssembly 体验

1. 如果使用的是 Blazor WebAssembly 应用，则注入了 <xref:System.Net.Http.HttpClient>，以从 wwwroot/sample-data 文件夹的 weather.json 文件中获取天气预测数据。  *Pages/FetchData.razor*： [`@foreach`](/dotnet/csharp/language-reference/keywords/foreach-in) 循环用于将每个预测实例呈现为“天气”数据表中的一行： 生成待办项列表

1. 向应用添加一个实现简单待办事项列表的新组件。

   ```razor
   @page "/todo"

   <h3>Todo</h3>
   ```

1. 向 Pages 文件夹中的应用添加一个新的 `Todo` Razor 组件。

   如果你使用的是 Visual Studio，请右键单击 Pages 文件夹，然后选择“添加” > “新项目” > “Razor 组件”   。 将组件的文件命名为 Todo.razor。

   在其他开发环境中，将空白文件添加到名为 Todo.razor 的“页面”文件夹中。

   ```razor
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. 为组件提供初始标记： 将 `Todo` 组件添加到导航栏。

1. `NavMenu` 组件 (Shared/NavMenu.razor) 用于应用的布局。 布局是可以避免应用中出现重复内容的组件。

   [!code-csharp[](build-your-first-blazor-app/samples_snapshot/3.x/TodoItem.cs)]

1. 通过在“Shared/NavMenu.razor”文件中的现有列表项下添加以下列表项标记，为 `Todo` 组件添加一个 `<NavLink>` 元素：

   * 重新生成并运行应用。 访问新的“待办事项”页面，确认指向 `Todo` 组件的链接有效。
   * 向项目的根目录添加“TodoItem.cs”文件，以保存一个用于表示待办项的类。

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. 为 `TodoItem` 类使用以下 C# 代码： 返回到 `Todo` 组件 (Pages/Todo.razor)：

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. 在 `@code` 块中为待办项添加一个字段。 `Todo` 组件使用此字段来维护待办项列表的状态。

1. 添加无序列表标记和 `foreach` 循环，以将每个待办项呈现为列表项 (`<li>`)。 该应用需要 UI 元素来将待办项添加到列表。

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

1. 在未排序列表 (`<ul>...</ul>`) 下方添加一个文本输入 (`<input>`) 和一个按钮 (`<button>`)：

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```razor
   <input placeholder="Something todo" @bind="newTodo" />
   ```

1. 重新生成并运行应用。 选择“添加待办项”按钮时没有任何反应，因为没有事件处理程序连接到该按钮。

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. 向 `Todo` 组件添加 `AddTodo` 方法，并使用 `@onclick` 属性注册该方法以选择按钮。 选择按钮时，会调用 `AddTodo` C# 方法：

1. 要获得新待办项标题，请在 `@code` 块顶部添加 `newTodo` 字符串字段，并使用 `<input>` 元素中的 `bind` 属性将其绑定到文本输入的值： 更新 `AddTodo` 方法，将具有指定标题的 `TodoItem` 添加到列表。 通过将 `newTodo` 设置为空字符串来清除文本输入的值：

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. 重新生成并运行应用。

   ```razor
   <h3>Todo (@todos.Count(todo => !todo.IsDone))</h3>
   ```

1. 在待办项列表中添加一些待办项以测试新代码。

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Todo.razor)]

1. 每个待办项的标题文本都可以编辑，复选框可以帮助用户跟踪已完成的项。 为每个待办项添加一个复选框输入，并将它的值绑定到 `IsDone` 属性。

## <a name="next-steps"></a>将 `@todo.Title` 更改为绑定到 `@todo.Title` 的 `<input>` 元素：

若要验证这些值是否已绑定，请更新 `<h3>` 标头以显示尚未完成的待办项计数（`IsDone` 是 `false`）。

> [!div class="checklist"]
> * 完成的 `Todo` 组件 (Pages/Todo.razor)：
> * 重新生成并运行应用。
> * 添加待办项以测试新代码。
> * 后续步骤

在本教程中，你将了解：

> [!div class="nextstepaction"]
> <xref:blazor/components>
