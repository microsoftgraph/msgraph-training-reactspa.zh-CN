<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="16bdc-101">在此练习中，你将 Microsoft Graph 合并到应用程序中。</span><span class="sxs-lookup"><span data-stu-id="16bdc-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="16bdc-102">对于此应用程序，你将使用 [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) 库调用 Microsoft Graph。</span><span class="sxs-lookup"><span data-stu-id="16bdc-102">For this application, you will use the [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) library to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="16bdc-103">从 Outlook 获取日历事件</span><span class="sxs-lookup"><span data-stu-id="16bdc-103">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="16bdc-104">打开 `./src/GraphService.ts` 并添加以下函数。</span><span class="sxs-lookup"><span data-stu-id="16bdc-104">Open `./src/GraphService.ts` and add the following function.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/GraphService.ts" id="getUserWeekCalendarSnippet":::

    <span data-ttu-id="16bdc-105">考虑此代码正在做什么。</span><span class="sxs-lookup"><span data-stu-id="16bdc-105">Consider what this code is doing.</span></span>

    - <span data-ttu-id="16bdc-106">将调用的 URL 为 `/me/calendarview` 。</span><span class="sxs-lookup"><span data-stu-id="16bdc-106">The URL that will be called is `/me/calendarview`.</span></span>
    - <span data-ttu-id="16bdc-107">`header`该方法将标头添加到请求中，从而导致响应中的时间在 `Prefer: outlook.timezone=""` 用户的首选时区。</span><span class="sxs-lookup"><span data-stu-id="16bdc-107">The `header` method adds the `Prefer: outlook.timezone=""` header to the request, causing the times in the response to be in the user's preferred time zone.</span></span>
    - <span data-ttu-id="16bdc-108">该方法 `query` 添加 `startDateTime` 和 `endDateTime` 参数，为日历视图定义时间窗口。</span><span class="sxs-lookup"><span data-stu-id="16bdc-108">The `query` method adds the `startDateTime` and `endDateTime` parameters, defining the window of time for the calendar view.</span></span>
    - <span data-ttu-id="16bdc-109">`select`该方法将每个事件返回的字段限制为视图将实际使用的字段。</span><span class="sxs-lookup"><span data-stu-id="16bdc-109">The `select` method limits the fields returned for each events to just those the view will actually use.</span></span>
    - <span data-ttu-id="16bdc-110">该方法按创建结果的日期和时间对结果进行排序，最新 `orderby` 项为第一项。</span><span class="sxs-lookup"><span data-stu-id="16bdc-110">The `orderby` method sorts the results by the date and time they were created, with the most recent item being first.</span></span>
    - <span data-ttu-id="16bdc-111">该方法 `top` 将单个页面中的结果限制为 25 个事件。</span><span class="sxs-lookup"><span data-stu-id="16bdc-111">The `top` method limits the results in a single page to 25 events.</span></span>
    - <span data-ttu-id="16bdc-112">如果响应包含一个值（指示可用的结果更多），则对象用于分页访问集合 `@odata.nextLink` `PageIterator` ，获取所有[](https://docs.microsoft.com/graph/sdks/paging?tabs=typeScript)结果。</span><span class="sxs-lookup"><span data-stu-id="16bdc-112">If the response contains an `@odata.nextLink` value, indicating there are more results available, a `PageIterator` object is used to [page through the collection](https://docs.microsoft.com/graph/sdks/paging?tabs=typeScript) to get all of the results.</span></span>

1. <span data-ttu-id="16bdc-113">创建 React 组件以显示呼叫结果。</span><span class="sxs-lookup"><span data-stu-id="16bdc-113">Create a React component to display the results of the call.</span></span> <span data-ttu-id="16bdc-114">在名为的目录中创建新文件 `./src` 并 `Calendar.tsx` 添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="16bdc-114">Create a new file in the `./src` directory named `Calendar.tsx` and add the following code.</span></span>

    ```typescript
    import React from 'react';
    import { NavLink as RouterNavLink } from 'react-router-dom';
    import { Table } from 'reactstrap';
    import moment, { Moment } from 'moment-timezone';
    import { findOneIana } from "windows-iana";
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
            var ianaTimeZone = findOneIana(this.props.user.timeZone);

            // Get midnight on the start of the current week in the user's timezone,
            // but in UTC. For example, for Pacific Standard Time, the time value would be
            // 07:00:00Z
            var startOfWeek = moment.tz(ianaTimeZone!.valueOf()).startOf('week').utc();

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

    <span data-ttu-id="16bdc-115">现在，这只是在页面上以 JSON 呈现事件数组。</span><span class="sxs-lookup"><span data-stu-id="16bdc-115">For now this just renders the array of events in JSON on the page.</span></span>

1. <span data-ttu-id="16bdc-116">将此新组件添加到应用。</span><span class="sxs-lookup"><span data-stu-id="16bdc-116">Add this new component to the app.</span></span> <span data-ttu-id="16bdc-117">打开 `./src/App.tsx` 以下语句 `import` 并将其添加到文件顶部。</span><span class="sxs-lookup"><span data-stu-id="16bdc-117">Open `./src/App.tsx` and add the following `import` statement to the top of the file.</span></span>

    ```typescript
    import Calendar from './Calendar';
    ```

1. <span data-ttu-id="16bdc-118">将以下组件添加到现有组件之后 `<Route>` 。</span><span class="sxs-lookup"><span data-stu-id="16bdc-118">Add the following component just after the existing `<Route>`.</span></span>

    ```typescript
    <Route exact path="/calendar"
      render={(props) =>
        this.props.isAuthenticated ?
          <Calendar {...props} /> :
          <Redirect to="/" />
      } />
    ```

1. <span data-ttu-id="16bdc-119">保存更改并重新启动该应用。</span><span class="sxs-lookup"><span data-stu-id="16bdc-119">Save your changes and restart the app.</span></span> <span data-ttu-id="16bdc-120">登录并单击导航 **栏中** 的"日历"链接。</span><span class="sxs-lookup"><span data-stu-id="16bdc-120">Sign in and click the **Calendar** link in the nav bar.</span></span> <span data-ttu-id="16bdc-121">如果一切正常，应在用户日历上看到事件的 JSON 转储。</span><span class="sxs-lookup"><span data-stu-id="16bdc-121">If everything works, you should see a JSON dump of events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="16bdc-122">显示结果</span><span class="sxs-lookup"><span data-stu-id="16bdc-122">Display the results</span></span>

<span data-ttu-id="16bdc-123">现在，您可以 `Calendar` 更新组件，以更用户友好的方式显示事件。</span><span class="sxs-lookup"><span data-stu-id="16bdc-123">Now you can update the `Calendar` component to display the events in a more user-friendly manner.</span></span>

1. <span data-ttu-id="16bdc-124">在名为的目录中创建新文件 `./src` 并 `Calendar.css` 添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="16bdc-124">Create a new file in the `./src` directory named `Calendar.css` and add the following code.</span></span>

    :::code language="css" source="../demo/graph-tutorial/src/Calendar.css":::

1. <span data-ttu-id="16bdc-125">创建 React 组件以在一天中将事件呈现为表格行。</span><span class="sxs-lookup"><span data-stu-id="16bdc-125">Create a React component to render events in a single day as table rows.</span></span> <span data-ttu-id="16bdc-126">在名为的目录中创建新文件 `./src` 并 `CalendarDayRow.tsx` 添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="16bdc-126">Create a new file in the `./src` directory named `CalendarDayRow.tsx` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/CalendarDayRow.tsx" id="CalendarDayRowSnippet":::

1. <span data-ttu-id="16bdc-127">将以下 `import` 语句添加到 **Calendar.tsx 的顶部**。</span><span class="sxs-lookup"><span data-stu-id="16bdc-127">Add the following `import` statements to the top of **Calendar.tsx**.</span></span>

    ```typescript
    import CalendarDayRow from './CalendarDayRow';
    import './Calendar.css';
    ```

1. <span data-ttu-id="16bdc-128">将现有 `render` 函数 `./src/Calendar.tsx` 替换为以下函数。</span><span class="sxs-lookup"><span data-stu-id="16bdc-128">Replace the existing `render` function in `./src/Calendar.tsx` with the following function.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/Calendar.tsx" id="renderSnippet":::

    <span data-ttu-id="16bdc-129">这会将事件拆分为其各自的天数，并呈现每天的表格部分。</span><span class="sxs-lookup"><span data-stu-id="16bdc-129">This splits the events into their respective days and renders a table section for each day.</span></span>

1. <span data-ttu-id="16bdc-130">保存更改并重新启动应用。</span><span class="sxs-lookup"><span data-stu-id="16bdc-130">Save the changes and restart the app.</span></span> <span data-ttu-id="16bdc-131">单击 **"日历"** 链接，应用现在应呈现事件表。</span><span class="sxs-lookup"><span data-stu-id="16bdc-131">Click on the **Calendar** link and the app should now render a table of events.</span></span>

    ![事件表的屏幕截图](./images/add-msgraph-01.png)
