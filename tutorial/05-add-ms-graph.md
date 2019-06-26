<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="b042f-101">在本练习中, 将把 Microsoft Graph 合并到应用程序中。</span><span class="sxs-lookup"><span data-stu-id="b042f-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="b042f-102">对于此应用程序, 您将使用[microsoft graph 客户端](https://github.com/microsoftgraph/msgraph-sdk-javascript)库调用 microsoft graph。</span><span class="sxs-lookup"><span data-stu-id="b042f-102">For this application, you will use the [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) library to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="b042f-103">从 Outlook 获取日历事件</span><span class="sxs-lookup"><span data-stu-id="b042f-103">Get calendar events from Outlook</span></span>

<span data-ttu-id="b042f-104">首先将新方法添加到`./src/GraphService.js`文件中, 以从日历中获取事件。</span><span class="sxs-lookup"><span data-stu-id="b042f-104">Start by adding a new method to the `./src/GraphService.js` file to get the events from the calendar.</span></span> <span data-ttu-id="b042f-105">添加以下函数。</span><span class="sxs-lookup"><span data-stu-id="b042f-105">Add the following function.</span></span>

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

<span data-ttu-id="b042f-106">考虑此代码执行的操作。</span><span class="sxs-lookup"><span data-stu-id="b042f-106">Consider what this code is doing.</span></span>

- <span data-ttu-id="b042f-107">将调用的 URL 为`/me/events`。</span><span class="sxs-lookup"><span data-stu-id="b042f-107">The URL that will be called is `/me/events`.</span></span>
- <span data-ttu-id="b042f-108">此`select`方法将为每个事件返回的字段限制为只是视图实际使用的字段。</span><span class="sxs-lookup"><span data-stu-id="b042f-108">The `select` method limits the fields returned for each events to just those the view will actually use.</span></span>
- <span data-ttu-id="b042f-109">`orderby`方法按其创建日期和时间对结果进行排序, 最新项目最先开始。</span><span class="sxs-lookup"><span data-stu-id="b042f-109">The `orderby` method sorts the results by the date and time they were created, with the most recent item being first.</span></span>

<span data-ttu-id="b042f-110">现在, 创建一个响应组件以显示呼叫结果。</span><span class="sxs-lookup"><span data-stu-id="b042f-110">Now create a React component to display the results of the call.</span></span> <span data-ttu-id="b042f-111">在名为`./src` `Calendar.js`的目录中创建一个新文件, 并添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="b042f-111">Create a new file in the `./src` directory named `Calendar.js` and add the following code.</span></span>

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
      var accessToken = await window.msal.acquireTokenSilent({
        scopes: config.scopes
      });
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

<span data-ttu-id="b042f-112">现在, 这只是在页面上呈现 JSON 中的事件数组。</span><span class="sxs-lookup"><span data-stu-id="b042f-112">For now this just renders the array of events in JSON on the page.</span></span> <span data-ttu-id="b042f-113">将此新组件添加到应用程序中。</span><span class="sxs-lookup"><span data-stu-id="b042f-113">Add this new component to the app.</span></span> <span data-ttu-id="b042f-114">打开`./src/App.js`并将以下`import`语句添加到文件顶部。</span><span class="sxs-lookup"><span data-stu-id="b042f-114">Open `./src/App.js` and add the following `import` statement to the top of the file.</span></span>

```js
import Calendar from './Calendar';
```

<span data-ttu-id="b042f-115">然后, 将以下组件添加到现有`<Route>`的后面。</span><span class="sxs-lookup"><span data-stu-id="b042f-115">Then add the following component just after the existing `<Route>`.</span></span>

```JSX
<Route exact path="/calendar"
  render={(props) =>
    <Calendar {...props}
      showError={this.setErrorMessage.bind(this)} />
  } />
```

<span data-ttu-id="b042f-116">保存更改并重新启动该应用。</span><span class="sxs-lookup"><span data-stu-id="b042f-116">Save your changes and restart the app.</span></span> <span data-ttu-id="b042f-117">登录并单击导航栏中的 "**日历**" 链接。</span><span class="sxs-lookup"><span data-stu-id="b042f-117">Sign in and click the **Calendar** link in the nav bar.</span></span> <span data-ttu-id="b042f-118">如果一切正常, 应在用户的日历上看到一个 JSON 转储的事件。</span><span class="sxs-lookup"><span data-stu-id="b042f-118">If everything works, you should see a JSON dump of events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="b042f-119">显示结果</span><span class="sxs-lookup"><span data-stu-id="b042f-119">Display the results</span></span>

<span data-ttu-id="b042f-120">现在, 您可以更新`Calendar`组件以以更用户友好的方式显示事件。</span><span class="sxs-lookup"><span data-stu-id="b042f-120">Now you can update the `Calendar` component to display the events in a more user-friendly manner.</span></span> <span data-ttu-id="b042f-121">将中`./src/Calendar.js`的`render`现有函数替换为以下函数。</span><span class="sxs-lookup"><span data-stu-id="b042f-121">Replace the existing `render` function in `./src/Calendar.js` with the following function.</span></span>

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

<span data-ttu-id="b042f-122">这将遍历事件集合并为每个事件添加一个表行。</span><span class="sxs-lookup"><span data-stu-id="b042f-122">This loops through the collection of events and adds a table row for each one.</span></span> <span data-ttu-id="b042f-123">保存所做的更改, 然后重新启动应用程序。</span><span class="sxs-lookup"><span data-stu-id="b042f-123">Save the changes and restart the app.</span></span> <span data-ttu-id="b042f-124">单击 "**日历**" 链接, 应用现在应呈现一个事件表。</span><span class="sxs-lookup"><span data-stu-id="b042f-124">Click on the **Calendar** link and the app should now render a table of events.</span></span>

![事件表的屏幕截图](./images/add-msgraph-01.png)
