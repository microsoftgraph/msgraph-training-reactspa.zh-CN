<!-- markdownlint-disable MD002 MD041 -->

在此练习中，你将 Microsoft Graph 合并到应用程序中。 对于此应用程序，你将使用 [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) 库调用 Microsoft Graph。

## <a name="get-calendar-events-from-outlook"></a>从 Outlook 获取日历事件

1. 打开 `./src/GraphService.ts` 并添加以下函数。

    :::code language="typescript" source="../demo/graph-tutorial/src/GraphService.ts" id="getUserWeekCalendarSnippet":::

    考虑此代码正在做什么。

    - 将调用的 URL 为 `/me/calendarview` 。
    - `header`该方法将标头添加到请求中，从而导致响应中的时间在 `Prefer: outlook.timezone=""` 用户的首选时区。
    - 该方法 `query` 添加 `startDateTime` 和 `endDateTime` 参数，为日历视图定义时间窗口。
    - `select`该方法将每个事件返回的字段限制为视图将实际使用的字段。
    - 该方法按创建结果的日期和时间对结果进行排序，最新 `orderby` 项为第一项。
    - 该方法 `top` 将单个页面中的结果限制为 25 个事件。
    - 如果响应包含一个值（指示可用的结果更多），则对象用于分页访问集合 `@odata.nextLink` `PageIterator` ，获取所有[](https://docs.microsoft.com/graph/sdks/paging?tabs=typeScript)结果。

1. 创建 React 组件以显示呼叫结果。 在名为的目录中创建新文件 `./src` 并 `Calendar.tsx` 添加以下代码。

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

    现在，这只是在页面上以 JSON 呈现事件数组。

1. 将此新组件添加到应用。 打开 `./src/App.tsx` 以下语句 `import` 并将其添加到文件顶部。

    ```typescript
    import Calendar from './Calendar';
    ```

1. 将以下组件添加到现有组件之后 `<Route>` 。

    ```typescript
    <Route exact path="/calendar"
      render={(props) =>
        this.props.isAuthenticated ?
          <Calendar {...props} /> :
          <Redirect to="/" />
      } />
    ```

1. 保存更改并重新启动该应用。 登录并单击导航 **栏中** 的"日历"链接。 如果一切正常，应在用户日历上看到事件的 JSON 转储。

## <a name="display-the-results"></a>显示结果

现在，您可以 `Calendar` 更新组件，以更用户友好的方式显示事件。

1. 在名为的目录中创建新文件 `./src` 并 `Calendar.css` 添加以下代码。

    :::code language="css" source="../demo/graph-tutorial/src/Calendar.css":::

1. 创建 React 组件以在一天中将事件呈现为表格行。 在名为的目录中创建新文件 `./src` 并 `CalendarDayRow.tsx` 添加以下代码。

    :::code language="typescript" source="../demo/graph-tutorial/src/CalendarDayRow.tsx" id="CalendarDayRowSnippet":::

1. 将以下 `import` 语句添加到 **Calendar.tsx 的顶部**。

    ```typescript
    import CalendarDayRow from './CalendarDayRow';
    import './Calendar.css';
    ```

1. 将现有 `render` 函数 `./src/Calendar.tsx` 替换为以下函数。

    :::code language="typescript" source="../demo/graph-tutorial/src/Calendar.tsx" id="renderSnippet":::

    这会将事件拆分为其各自的天数，并呈现每天的表格部分。

1. 保存更改并重新启动应用。 单击 **"日历"** 链接，应用现在应呈现事件表。

    ![事件表的屏幕截图](./images/add-msgraph-01.png)
