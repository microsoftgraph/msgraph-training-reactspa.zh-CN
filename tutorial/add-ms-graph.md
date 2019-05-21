<!-- markdownlint-disable MD002 MD041 -->

在本练习中, 将把 Microsoft Graph 合并到应用程序中。 对于此应用程序, 您将使用[microsoft graph 客户端](https://github.com/microsoftgraph/msgraph-sdk-javascript)库调用 microsoft graph。

## <a name="get-calendar-events-from-outlook"></a>从 Outlook 获取日历事件

首先将新方法添加到`./src/GraphService.js`文件中, 以从日历中获取事件。 添加以下函数。

```js
export async function getEvents(accessToken) {
  const client = getAuthenticatedClient(accessToken);

  const events = await client
    .api('/me/events')
    .select('subject,organizer,start,end')
    .orderby('createdDateTime DESC')
    .get();

  return events;
}
```

考虑此代码执行的操作。

- 将调用的 URL 为`/me/events`。
- 此`select`方法将为每个事件返回的字段限制为只是视图实际使用的字段。
- `orderby`方法按其创建日期和时间对结果进行排序, 最新项目最先开始。

现在, 创建一个响应组件以显示呼叫结果。 在名为`./src` `Calendar.js`的目录中创建一个新文件, 并添加以下代码。

```JSX
import React from 'react';
import { Table } from 'reactstrap';
import moment from 'moment';
import config from './Config';
import { getEvents } from './GraphService';

// Helper function to format Graph date/time
function formatDateTime(dateTime) {
  return moment.utc(dateTime).local().format('M/D/YY h:mm A');
}

export default class Calendar extends React.Component {
  constructor(props) {
    super(props);

    this.state = {
      events: []
    };
  }

  async componentDidMount() {
    try {
      // Get the user's access token
      var accessToken = await window.msal.acquireTokenSilent(config.scopes);
      // Get the user's events
      var events = await getEvents(accessToken);
      // Update the array of events in state
      this.setState({events: events.value});
    }
    catch(err) {
      this.props.showError('ERROR', JSON.stringify(err));
    }
  }

  render() {
    return (
      <pre><code>{JSON.stringify(this.state.events, null, 2)}</code></pre>
    );
  }
}
```

现在, 这只是在页面上呈现 JSON 中的事件数组。 将此新组件添加到应用程序中。 打开`./src/App.js`并将以下`import`语句添加到文件顶部。

```js
import Calendar from './Calendar';
```

然后, 将以下组件添加到现有`<Route>`的后面。

```JSX
<Route exact path="/calendar"
  render={(props) =>
    <Calendar {...props}
      showError={this.setErrorMessage.bind(this)} />
  } />
```

保存更改并重新启动该应用。 登录并单击导航栏中的 "**日历**" 链接。 如果一切正常, 应在用户的日历上看到一个 JSON 转储的事件。

## <a name="display-the-results"></a>显示结果

现在, 您可以更新`Calendar`组件以以更用户友好的方式显示事件。 将中`./src/Calendar.js`的`render`现有函数替换为以下函数。

```JSX
render() {
  return (
    <div>
      <h1>Calendar</h1>
      <Table>
        <thead>
          <tr>
            <th scope="col">Organizer</th>
            <th scope="col">Subject</th>
            <th scope="col">Start</th>
            <th scope="col">End</th>
          </tr>
        </thead>
        <tbody>
          {this.state.events.map(
            function(event){
              return(
                <tr key={event.id}>
                  <td>{event.organizer.emailAddress.name}</td>
                  <td>{event.subject}</td>
                  <td>{formatDateTime(event.start.dateTime)}</td>
                  <td>{formatDateTime(event.end.dateTime)}</td>
                </tr>
              );
            })}
        </tbody>
      </Table>
    </div>
  );
}
```

这将遍历事件集合并为每个事件添加一个表行。 保存所做的更改, 然后重新启动应用程序。 单击 "**日历**" 链接, 应用现在应呈现一个事件表。

![事件表的屏幕截图](./images/add-msgraph-01.png)