<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="fa041-101">在本节中，您将添加在用户日历上创建事件的功能。</span><span class="sxs-lookup"><span data-stu-id="fa041-101">In this section you will add the ability to create events on the user's calendar.</span></span>

## <a name="add-method-to-graphservice"></a><span data-ttu-id="fa041-102">向 GraphService 添加方法</span><span class="sxs-lookup"><span data-stu-id="fa041-102">Add method to GraphService</span></span>

1. <span data-ttu-id="fa041-103">打开 **/src/GraphService.ts** ，并添加以下函数以创建新事件。</span><span class="sxs-lookup"><span data-stu-id="fa041-103">Open **./src/GraphService.ts** and add the following function to create a new event.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/GraphService.ts" id="createEventSnippet":::

## <a name="create-new-event-form"></a><span data-ttu-id="fa041-104">创建新的事件表单</span><span class="sxs-lookup"><span data-stu-id="fa041-104">Create new event form</span></span>

1. <span data-ttu-id="fa041-105">在名为**NewEvent**的 **/src**目录中创建一个新文件，并添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="fa041-105">Create a new file in the **./src** directory named **NewEvent.tsx** and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/NewEvent.tsx" id="NewEventSnippet":::

1. <span data-ttu-id="fa041-106">打开 **/src/App.tsx** ，并将以下 `import` 语句添加到文件顶部。</span><span class="sxs-lookup"><span data-stu-id="fa041-106">Open **./src/App.tsx** and add the following `import` statement to the top of the file.</span></span>

    ```typescript
    import NewEvent from './NewEvent';
    ```

1. <span data-ttu-id="fa041-107">将新的路由添加到新的事件表单。</span><span class="sxs-lookup"><span data-stu-id="fa041-107">Add a new route to the new event form.</span></span> <span data-ttu-id="fa041-108">紧接着将以下代码添加到其他 `Route` 元素之后。</span><span class="sxs-lookup"><span data-stu-id="fa041-108">Add the following code just after the other `Route` elements.</span></span>

    ```typescript
    <Route exact path="/newevent"
      render={(props) =>
        this.props.isAuthenticated ?
          <NewEvent {...props} /> :
          <Redirect to="/" />
      } />
    ```

    <span data-ttu-id="fa041-109">完整的 `return` 语句现在应如下所示。</span><span class="sxs-lookup"><span data-stu-id="fa041-109">The full `return` statement should now look like this.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/App.tsx" id="renderSnippet" highlight="23-28":::

1. <span data-ttu-id="fa041-110">刷新应用程序并浏览到 "日历" 视图。</span><span class="sxs-lookup"><span data-stu-id="fa041-110">Refresh the app and browse to the calendar view.</span></span> <span data-ttu-id="fa041-111">单击 " **新建事件** " 按钮。</span><span class="sxs-lookup"><span data-stu-id="fa041-111">Click the **New event** button.</span></span> <span data-ttu-id="fa041-112">填写这些字段，然后单击 " **创建**"。</span><span class="sxs-lookup"><span data-stu-id="fa041-112">Fill in the fields and click **Create**.</span></span>

    ![新事件表单的屏幕截图](./images/create-event-01.png)
