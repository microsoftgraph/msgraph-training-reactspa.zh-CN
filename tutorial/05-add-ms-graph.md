<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="de71a-101">在此练习中，你将将 Microsoft Graph合并到应用程序中。</span><span class="sxs-lookup"><span data-stu-id="de71a-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="de71a-102">对于此应用程序，你将使用[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript)库调用 Microsoft Graph。</span><span class="sxs-lookup"><span data-stu-id="de71a-102">For this application, you will use the [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) library to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="de71a-103">从 Outlook 获取日历事件</span><span class="sxs-lookup"><span data-stu-id="de71a-103">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="de71a-104">打开 `./src/GraphService.ts` 并添加以下函数。</span><span class="sxs-lookup"><span data-stu-id="de71a-104">Open `./src/GraphService.ts` and add the following function.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/GraphService.ts" id="getUserWeekCalendarSnippet":::

    <span data-ttu-id="de71a-105">考虑此代码将执行什么工作。</span><span class="sxs-lookup"><span data-stu-id="de71a-105">Consider what this code is doing.</span></span>

    - <span data-ttu-id="de71a-106">将调用的 URL 为 `/me/calendarview`。</span><span class="sxs-lookup"><span data-stu-id="de71a-106">The URL that will be called is `/me/calendarview`.</span></span>
    - <span data-ttu-id="de71a-107">方法将 标头添加到请求中，导致响应中的时间在 `header` `Prefer: outlook.timezone=""` 用户的首选时区。</span><span class="sxs-lookup"><span data-stu-id="de71a-107">The `header` method adds the `Prefer: outlook.timezone=""` header to the request, causing the times in the response to be in the user's preferred time zone.</span></span>
    - <span data-ttu-id="de71a-108">`query`方法添加 `startDateTime` 和 `endDateTime` 参数，定义日历视图的时间窗口。</span><span class="sxs-lookup"><span data-stu-id="de71a-108">The `query` method adds the `startDateTime` and `endDateTime` parameters, defining the window of time for the calendar view.</span></span>
    - <span data-ttu-id="de71a-109">`select`方法将每个事件返回的字段限定为视图将实际使用的字段。</span><span class="sxs-lookup"><span data-stu-id="de71a-109">The `select` method limits the fields returned for each events to just those the view will actually use.</span></span>
    - <span data-ttu-id="de71a-110">方法按创建结果的日期和时间对结果进行排序，最新项 `orderby` 首先排序。</span><span class="sxs-lookup"><span data-stu-id="de71a-110">The `orderby` method sorts the results by the date and time they were created, with the most recent item being first.</span></span>
    - <span data-ttu-id="de71a-111">`top`方法将单个页面中的结果限制为 25 个事件。</span><span class="sxs-lookup"><span data-stu-id="de71a-111">The `top` method limits the results in a single page to 25 events.</span></span>
    - <span data-ttu-id="de71a-112">如果响应包含一个值，该值指示可用的结果更多，则使用对象在集合中分页获取 `@odata.nextLink` `PageIterator` 所有结果。 [](https://docs.microsoft.com/graph/sdks/paging?tabs=typeScript)</span><span class="sxs-lookup"><span data-stu-id="de71a-112">If the response contains an `@odata.nextLink` value, indicating there are more results available, a `PageIterator` object is used to [page through the collection](https://docs.microsoft.com/graph/sdks/paging?tabs=typeScript) to get all of the results.</span></span>

1. <span data-ttu-id="de71a-113">创建React组件以显示调用的结果。</span><span class="sxs-lookup"><span data-stu-id="de71a-113">Create a React component to display the results of the call.</span></span> <span data-ttu-id="de71a-114">在目录中新建一个名为 `./src` 的文件 `Calendar.tsx` 并添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="de71a-114">Create a new file in the `./src` directory named `Calendar.tsx` and add the following code.</span></span>

    ```typescript
    import React from 'react';
    import { NavLink as RouterNavLink } from 'react-router-dom';
    import { Table } from 'reactstrap';
    import moment, { Moment } from 'moment-timezone';
    import { findIana } from "windows-iana";
    import { Event } from 'microsoft-graph';
    import { config } from './Config';
    import { getUserWeekCalendar } from './GraphService';
    import withAuthProvider, { AuthComponentProps } from './AuthProvider';

    interface CalendarState {
      eventsLoaded: boolean;
      events: Event[];
      startOfWeek: Moment | undefined;
    }

    class Calendar extends React.Component<AuthComponentProps, CalendarState> {
      constructor(props: any) {
        super(props);

        this.state = {
          eventsLoaded: false,
          events: [],
          startOfWeek: undefined
        };
      }

      async componentDidUpdate() {
        if (this.props.user && !this.state.eventsLoaded)
        {
          try {
            // Get the user's access token
            var accessToken = await this.props.getAccessToken(config.scopes);

            // Convert user's Windows time zone ("Pacific Standard Time")
            // to IANA format ("America/Los_Angeles")
            // Moment needs IANA format
            var ianaTimeZones = findIana(this.props.user.timeZone);

            // Get midnight on the start of the current week in the user's timezone,
            // but in UTC. For example, for Pacific Standard Time, the time value would be
            // 07:00:00Z
            var startOfWeek = moment.tz(ianaTimeZones![0].valueOf()).startOf('week').utc();

            // Get the user's events
            var events = await getUserWeekCalendar(accessToken, this.props.user.timeZone, startOfWeek);

            // Update the array of events in state
            this.setState({
              eventsLoaded: true,
              events: events,
              startOfWeek: startOfWeek
            });
          }
          catch (err) {
            this.props.setError('ERROR', JSON.stringify(err));
          }
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

    <span data-ttu-id="de71a-115">现在，这只是在页面上以 JSON 呈现事件数组。</span><span class="sxs-lookup"><span data-stu-id="de71a-115">For now this just renders the array of events in JSON on the page.</span></span>

1. <span data-ttu-id="de71a-116">将此新组件添加到应用。</span><span class="sxs-lookup"><span data-stu-id="de71a-116">Add this new component to the app.</span></span> <span data-ttu-id="de71a-117">打开 `./src/App.tsx` 并添加 `import` 以下语句到文件顶部。</span><span class="sxs-lookup"><span data-stu-id="de71a-117">Open `./src/App.tsx` and add the following `import` statement to the top of the file.</span></span>

    ```typescript
    import Calendar from './Calendar';
    ```

1. <span data-ttu-id="de71a-118">将以下组件添加到现有 组件之后 `<Route>` 。</span><span class="sxs-lookup"><span data-stu-id="de71a-118">Add the following component just after the existing `<Route>`.</span></span>

    ```typescript
    <Route exact path="/calendar"
      render={(props) =>
        this.props.isAuthenticated ?
          <Calendar {...props} /> :
          <Redirect to="/" />
      } />
    ```

1. <span data-ttu-id="de71a-119">保存更改并重新启动该应用。</span><span class="sxs-lookup"><span data-stu-id="de71a-119">Save your changes and restart the app.</span></span> <span data-ttu-id="de71a-120">登录并单击导航 **栏中** 的"日历"链接。</span><span class="sxs-lookup"><span data-stu-id="de71a-120">Sign in and click the **Calendar** link in the nav bar.</span></span> <span data-ttu-id="de71a-121">如果一切正常，应在用户日历上看到事件被 JSON 卸载。</span><span class="sxs-lookup"><span data-stu-id="de71a-121">If everything works, you should see a JSON dump of events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="de71a-122">显示结果</span><span class="sxs-lookup"><span data-stu-id="de71a-122">Display the results</span></span>

<span data-ttu-id="de71a-123">现在，你可以 `Calendar` 更新组件以更用户友好的方式显示事件。</span><span class="sxs-lookup"><span data-stu-id="de71a-123">Now you can update the `Calendar` component to display the events in a more user-friendly manner.</span></span>

1. <span data-ttu-id="de71a-124">在目录中新建一个名为 `./src` 的文件 `Calendar.css` 并添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="de71a-124">Create a new file in the `./src` directory named `Calendar.css` and add the following code.</span></span>

    :::code language="css" source="../demo/graph-tutorial/src/Calendar.css":::

1. <span data-ttu-id="de71a-125">创建一React组件，将一天中的事件呈现为表格行。</span><span class="sxs-lookup"><span data-stu-id="de71a-125">Create a React component to render events in a single day as table rows.</span></span> <span data-ttu-id="de71a-126">在目录中新建一个名为 `./src` 的文件 `CalendarDayRow.tsx` 并添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="de71a-126">Create a new file in the `./src` directory named `CalendarDayRow.tsx` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/CalendarDayRow.tsx" id="CalendarDayRowSnippet":::

1. <span data-ttu-id="de71a-127">将以下 `import` 语句添加到 **Calendar.tsx 的顶部**。</span><span class="sxs-lookup"><span data-stu-id="de71a-127">Add the following `import` statements to the top of **Calendar.tsx**.</span></span>

    ```typescript
    import CalendarDayRow from './CalendarDayRow';
    import './Calendar.css';
    ```

1. <span data-ttu-id="de71a-128">将 中的 `render` 现有函数 `./src/Calendar.tsx` 替换为以下函数。</span><span class="sxs-lookup"><span data-stu-id="de71a-128">Replace the existing `render` function in `./src/Calendar.tsx` with the following function.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/Calendar.tsx" id="renderSnippet":::

    <span data-ttu-id="de71a-129">这会将事件拆分为其各自的天数，并呈现每天的表格部分。</span><span class="sxs-lookup"><span data-stu-id="de71a-129">This splits the events into their respective days and renders a table section for each day.</span></span>

1. <span data-ttu-id="de71a-130">保存更改并重新启动应用。</span><span class="sxs-lookup"><span data-stu-id="de71a-130">Save the changes and restart the app.</span></span> <span data-ttu-id="de71a-131">单击" **日历"** 链接，应用现在应呈现一个事件表。</span><span class="sxs-lookup"><span data-stu-id="de71a-131">Click on the **Calendar** link and the app should now render a table of events.</span></span>

    ![事件表的屏幕截图](./images/add-msgraph-01.png)
