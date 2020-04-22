<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="9eccf-101">在本练习中，将把 Microsoft Graph 合并到应用程序中。</span><span class="sxs-lookup"><span data-stu-id="9eccf-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="9eccf-102">对于此应用程序，您将使用[microsoft graph 客户端](https://github.com/microsoftgraph/msgraph-sdk-javascript)库调用 microsoft graph。</span><span class="sxs-lookup"><span data-stu-id="9eccf-102">For this application, you will use the [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) library to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="9eccf-103">从 Outlook 获取日历事件</span><span class="sxs-lookup"><span data-stu-id="9eccf-103">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="9eccf-104">打开`./src/GraphService.ts`并添加以下函数。</span><span class="sxs-lookup"><span data-stu-id="9eccf-104">Open `./src/GraphService.ts` and add the following function.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/GraphService.ts" id="getEventsSnippet":::

    <span data-ttu-id="9eccf-105">考虑此代码执行的操作。</span><span class="sxs-lookup"><span data-stu-id="9eccf-105">Consider what this code is doing.</span></span>

    - <span data-ttu-id="9eccf-106">将调用的 URL 为`/me/events`。</span><span class="sxs-lookup"><span data-stu-id="9eccf-106">The URL that will be called is `/me/events`.</span></span>
    - <span data-ttu-id="9eccf-107">此`select`方法将为每个事件返回的字段限制为只是视图实际使用的字段。</span><span class="sxs-lookup"><span data-stu-id="9eccf-107">The `select` method limits the fields returned for each events to just those the view will actually use.</span></span>
    - <span data-ttu-id="9eccf-108">`orderby`方法按其创建日期和时间对结果进行排序，最新项目最先开始。</span><span class="sxs-lookup"><span data-stu-id="9eccf-108">The `orderby` method sorts the results by the date and time they were created, with the most recent item being first.</span></span>

1. <span data-ttu-id="9eccf-109">创建响应组件以显示呼叫结果。</span><span class="sxs-lookup"><span data-stu-id="9eccf-109">Create a React component to display the results of the call.</span></span> <span data-ttu-id="9eccf-110">在名为`./src` `Calendar.tsx`的目录中创建一个新文件，并添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="9eccf-110">Create a new file in the `./src` directory named `Calendar.tsx` and add the following code.</span></span>

    ```typescript
    import React from 'react';
    import { Table } from 'reactstrap';
    import moment from 'moment';
    import { Event } from 'microsoft-graph';
    import { config } from './Config';
    import { getEvents } from './GraphService';
    import withAuthProvider, { AuthComponentProps } from './AuthProvider';

    interface CalendarState {
      events: Event[];
    }

    // Helper function to format Graph date/time
    function formatDateTime(dateTime: string | undefined) {
      if (dateTime !== undefined) {
        return moment.utc(dateTime).local().format('M/D/YY h:mm A');
      }
    }

    class Calendar extends React.Component<AuthComponentProps, CalendarState> {
      constructor(props: any) {
        super(props);

        this.state = {
          events: []
        };
      }

      async componentDidMount() {
        try {
          // Get the user's access token
          var accessToken = await this.props.getAccessToken(config.scopes);
          // Get the user's events
          var events = await getEvents(accessToken);
          // Update the array of events in state
          this.setState({events: events.value});
        }
        catch(err) {
          this.props.setError('ERROR', JSON.stringify(err));
        }
      }

      render() {
        return (
          <pre><code>{JSON.stringify(this.state.events, null, 2)}</code></pre>
        );
      }
    }

    export default withAuthProvider(Calendar);
    ```

    <span data-ttu-id="9eccf-111">现在，这只是在页面上呈现 JSON 中的事件数组。</span><span class="sxs-lookup"><span data-stu-id="9eccf-111">For now this just renders the array of events in JSON on the page.</span></span>

1. <span data-ttu-id="9eccf-112">将此新组件添加到应用程序中。</span><span class="sxs-lookup"><span data-stu-id="9eccf-112">Add this new component to the app.</span></span> <span data-ttu-id="9eccf-113">打开`./src/App.tsx`并将以下`import`语句添加到文件顶部。</span><span class="sxs-lookup"><span data-stu-id="9eccf-113">Open `./src/App.tsx` and add the following `import` statement to the top of the file.</span></span>

    ```typescript
    import Calendar from './Calendar';
    ```

1. <span data-ttu-id="9eccf-114">将以下组件添加到现有`<Route>`的后面。</span><span class="sxs-lookup"><span data-stu-id="9eccf-114">Add the following component just after the existing `<Route>`.</span></span>

    ```typescript
    <Route exact path="/calendar"
      render={(props) =>
        this.props.isAuthenticated ?
          <Calendar {...props} /> :
          <Redirect to="/" />
      } />
    ```

1. <span data-ttu-id="9eccf-115">保存更改并重新启动该应用。</span><span class="sxs-lookup"><span data-stu-id="9eccf-115">Save your changes and restart the app.</span></span> <span data-ttu-id="9eccf-116">登录并单击导航栏中的 "**日历**" 链接。</span><span class="sxs-lookup"><span data-stu-id="9eccf-116">Sign in and click the **Calendar** link in the nav bar.</span></span> <span data-ttu-id="9eccf-117">如果一切正常，应在用户的日历上看到一个 JSON 转储的事件。</span><span class="sxs-lookup"><span data-stu-id="9eccf-117">If everything works, you should see a JSON dump of events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="9eccf-118">显示结果</span><span class="sxs-lookup"><span data-stu-id="9eccf-118">Display the results</span></span>

<span data-ttu-id="9eccf-119">现在，您可以更新`Calendar`组件以以更用户友好的方式显示事件。</span><span class="sxs-lookup"><span data-stu-id="9eccf-119">Now you can update the `Calendar` component to display the events in a more user-friendly manner.</span></span>

1. <span data-ttu-id="9eccf-120">将中`./src/Calendar.js`的`render`现有函数替换为以下函数。</span><span class="sxs-lookup"><span data-stu-id="9eccf-120">Replace the existing `render` function in `./src/Calendar.js` with the following function.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/Calendar.tsx" id="renderSnippet":::

    <span data-ttu-id="9eccf-121">这将遍历事件集合并为每个事件添加一个表行。</span><span class="sxs-lookup"><span data-stu-id="9eccf-121">This loops through the collection of events and adds a table row for each one.</span></span>

1. <span data-ttu-id="9eccf-122">保存所做的更改，然后重新启动应用程序。</span><span class="sxs-lookup"><span data-stu-id="9eccf-122">Save the changes and restart the app.</span></span> <span data-ttu-id="9eccf-123">单击 "**日历**" 链接，应用现在应呈现一个事件表。</span><span class="sxs-lookup"><span data-stu-id="9eccf-123">Click on the **Calendar** link and the app should now render a table of events.</span></span>

    ![事件表的屏幕截图](./images/add-msgraph-01.png)
