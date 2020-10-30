<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="21932-101">在本练习中，将把 Microsoft Graph 合并到应用程序中。</span><span class="sxs-lookup"><span data-stu-id="21932-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="21932-102">对于此应用程序，您将使用 [microsoft graph 客户端](https://github.com/microsoftgraph/msgraph-sdk-javascript) 库调用 microsoft graph。</span><span class="sxs-lookup"><span data-stu-id="21932-102">For this application, you will use the [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) library to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="21932-103">从 Outlook 获取日历事件</span><span class="sxs-lookup"><span data-stu-id="21932-103">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="21932-104">打开 `./src/GraphService.ts` 并添加以下函数。</span><span class="sxs-lookup"><span data-stu-id="21932-104">Open `./src/GraphService.ts` and add the following function.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/GraphService.ts" id="getUserWeekCalendarSnippet":::

    <span data-ttu-id="21932-105">考虑此代码执行的操作。</span><span class="sxs-lookup"><span data-stu-id="21932-105">Consider what this code is doing.</span></span>

    - <span data-ttu-id="21932-106">将调用的 URL 为 `/me/calendarview` 。</span><span class="sxs-lookup"><span data-stu-id="21932-106">The URL that will be called is `/me/calendarview`.</span></span>
    - <span data-ttu-id="21932-107">该 `header` 方法将 `Prefer: outlook.timezone=""` 标头添加到请求，从而导致响应中的时间位于用户的首选时区中。</span><span class="sxs-lookup"><span data-stu-id="21932-107">The `header` method adds the `Prefer: outlook.timezone=""` header to the request, causing the times in the response to be in the user's preferred time zone.</span></span>
    - <span data-ttu-id="21932-108">`query`方法添加 `startDateTime` 和 `endDateTime` 参数，定义日历视图的时间窗口。</span><span class="sxs-lookup"><span data-stu-id="21932-108">The `query` method adds the `startDateTime` and `endDateTime` parameters, defining the window of time for the calendar view.</span></span>
    - <span data-ttu-id="21932-109">此 `select` 方法将为每个事件返回的字段限制为只是视图实际使用的字段。</span><span class="sxs-lookup"><span data-stu-id="21932-109">The `select` method limits the fields returned for each events to just those the view will actually use.</span></span>
    - <span data-ttu-id="21932-110">`orderby`方法按其创建日期和时间对结果进行排序，最新项目最先开始。</span><span class="sxs-lookup"><span data-stu-id="21932-110">The `orderby` method sorts the results by the date and time they were created, with the most recent item being first.</span></span>
    - <span data-ttu-id="21932-111">该 `top` 方法将结果限制为前50个事件。</span><span class="sxs-lookup"><span data-stu-id="21932-111">The `top` method limits the results to the first 50 events.</span></span>
    - <span data-ttu-id="21932-112">如果响应包含一个 `@odata.nextLink` 值，指示有更多的可用结果， `PageIterator` 则使用对象在集合中进行 [分页](https://docs.microsoft.com/graph/sdks/paging?tabs=typeScript) 以获取所有结果。</span><span class="sxs-lookup"><span data-stu-id="21932-112">If the response contains an `@odata.nextLink` value, indicating there are more results available, a `PageIterator` object is used to [page through the collection](https://docs.microsoft.com/graph/sdks/paging?tabs=typeScript) to get all of the results.</span></span>

1. <span data-ttu-id="21932-113">创建响应组件以显示呼叫结果。</span><span class="sxs-lookup"><span data-stu-id="21932-113">Create a React component to display the results of the call.</span></span> <span data-ttu-id="21932-114">在名为的目录中创建一个新文件 `./src` `Calendar.tsx` ，并添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="21932-114">Create a new file in the `./src` directory named `Calendar.tsx` and add the following code.</span></span>

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

    <span data-ttu-id="21932-115">现在，这只是在页面上呈现 JSON 中的事件数组。</span><span class="sxs-lookup"><span data-stu-id="21932-115">For now this just renders the array of events in JSON on the page.</span></span>

1. <span data-ttu-id="21932-116">将此新组件添加到应用程序中。</span><span class="sxs-lookup"><span data-stu-id="21932-116">Add this new component to the app.</span></span> <span data-ttu-id="21932-117">打开 `./src/App.tsx` 并将以下 `import` 语句添加到文件顶部。</span><span class="sxs-lookup"><span data-stu-id="21932-117">Open `./src/App.tsx` and add the following `import` statement to the top of the file.</span></span>

    ```typescript
    import Calendar from './Calendar';
    ```

1. <span data-ttu-id="21932-118">将以下组件添加到现有的后面 `<Route>` 。</span><span class="sxs-lookup"><span data-stu-id="21932-118">Add the following component just after the existing `<Route>`.</span></span>

    ```typescript
    <Route exact path="/calendar"
      render={(props) =>
        this.props.isAuthenticated ?
          <Calendar {...props} /> :
          <Redirect to="/" />
      } />
    ```

1. <span data-ttu-id="21932-119">保存更改并重新启动该应用。</span><span class="sxs-lookup"><span data-stu-id="21932-119">Save your changes and restart the app.</span></span> <span data-ttu-id="21932-120">登录并单击导航栏中的 " **日历** " 链接。</span><span class="sxs-lookup"><span data-stu-id="21932-120">Sign in and click the **Calendar** link in the nav bar.</span></span> <span data-ttu-id="21932-121">如果一切正常，应在用户的日历上看到一个 JSON 转储的事件。</span><span class="sxs-lookup"><span data-stu-id="21932-121">If everything works, you should see a JSON dump of events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="21932-122">显示结果</span><span class="sxs-lookup"><span data-stu-id="21932-122">Display the results</span></span>

<span data-ttu-id="21932-123">现在，您可以更新 `Calendar` 组件以以更用户友好的方式显示事件。</span><span class="sxs-lookup"><span data-stu-id="21932-123">Now you can update the `Calendar` component to display the events in a more user-friendly manner.</span></span>

1. <span data-ttu-id="21932-124">在名为的目录中创建一个新文件 `./src` `Calendar.css` ，并添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="21932-124">Create a new file in the `./src` directory named `Calendar.css` and add the following code.</span></span>

    :::code language="css" source="../demo/graph-tutorial/src/Calendar.css":::

1. <span data-ttu-id="21932-125">创建一个响应组件以在一个天内以表行的形式呈现事件。</span><span class="sxs-lookup"><span data-stu-id="21932-125">Create a React component to render events in a single day as table rows.</span></span> <span data-ttu-id="21932-126">在名为的目录中创建一个新文件 `./src` `CalendarDayRow.tsx` ，并添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="21932-126">Create a new file in the `./src` directory named `CalendarDayRow.tsx` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/CalendarDayRow.tsx" id="CalendarDayRowSnippet":::

1. <span data-ttu-id="21932-127">将以下 `import` 语句添加到 " **tsx** " 顶部。</span><span class="sxs-lookup"><span data-stu-id="21932-127">Add the following `import` statements to the top of **Calendar.tsx** .</span></span>

    ```typescript
    import CalendarDayRow from './CalendarDayRow';
    import './Calendar.css';
    ```

1. <span data-ttu-id="21932-128">将中的现有 `render` 函数替换 `./src/Calendar.tsx` 为以下函数。</span><span class="sxs-lookup"><span data-stu-id="21932-128">Replace the existing `render` function in `./src/Calendar.tsx` with the following function.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/Calendar.tsx" id="renderSnippet":::

    <span data-ttu-id="21932-129">这会将事件拆分为各自的天，并为每天呈现一个表节。</span><span class="sxs-lookup"><span data-stu-id="21932-129">This splits the events into their respective days and renders a table section for each day.</span></span>

1. <span data-ttu-id="21932-130">保存所做的更改，然后重新启动应用程序。</span><span class="sxs-lookup"><span data-stu-id="21932-130">Save the changes and restart the app.</span></span> <span data-ttu-id="21932-131">单击 " **日历** " 链接，应用现在应呈现一个事件表。</span><span class="sxs-lookup"><span data-stu-id="21932-131">Click on the **Calendar** link and the app should now render a table of events.</span></span>

    ![事件表的屏幕截图](./images/add-msgraph-01.png)
