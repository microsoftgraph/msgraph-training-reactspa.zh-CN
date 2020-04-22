<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="c9e64-101">在本节中，你将创建新的响应应用程序。</span><span class="sxs-lookup"><span data-stu-id="c9e64-101">In this section you'll create a new React app.</span></span>

1. <span data-ttu-id="c9e64-102">打开命令行接口（CLI），导航到您有权在其中创建文件的目录，并运行以下命令以创建新的响应应用程序。</span><span class="sxs-lookup"><span data-stu-id="c9e64-102">Open your command-line interface (CLI), navigate to a directory where you have rights to create files, and run the following commands to create a new React app.</span></span>

    ```Shell
    npx create-react-app@3.4.0 graph-tutorial --template typescript
    ```

1. <span data-ttu-id="c9e64-103">命令完成后，转到 CLI 中`graph-tutorial`的目录，并运行以下命令以启动本地 web 服务器。</span><span class="sxs-lookup"><span data-stu-id="c9e64-103">Once the command finishes, change to the `graph-tutorial` directory in your CLI and run the following command to start a local web server.</span></span>

    ```Shell
    npm start
    ```

<span data-ttu-id="c9e64-104">默认浏览器将打开[https://localhost:3000/](https://localhost:3000)并使用默认的响应页面。</span><span class="sxs-lookup"><span data-stu-id="c9e64-104">Your default browser opens to [https://localhost:3000/](https://localhost:3000) with a default React page.</span></span> <span data-ttu-id="c9e64-105">如果你的浏览器未打开，请打开它[https://localhost:3000/](https://localhost:3000)并浏览以验证新应用是否正常工作。</span><span class="sxs-lookup"><span data-stu-id="c9e64-105">If your browser doesn't open, open it and browse to [https://localhost:3000/](https://localhost:3000) to verify that the new app works.</span></span>

## <a name="add-node-packages"></a><span data-ttu-id="c9e64-106">添加节点包</span><span class="sxs-lookup"><span data-stu-id="c9e64-106">Add Node packages</span></span>

<span data-ttu-id="c9e64-107">在继续操作之前，请先安装将使用的其他一些程序包：</span><span class="sxs-lookup"><span data-stu-id="c9e64-107">Before moving on, install some additional packages that you will use later:</span></span>

- <span data-ttu-id="c9e64-108">响应-在响应应用程序中进行声明性路由的[路由器-dom](https://github.com/ReactTraining/react-router) 。</span><span class="sxs-lookup"><span data-stu-id="c9e64-108">[react-router-dom](https://github.com/ReactTraining/react-router) for declarative routing inside the React app.</span></span>
- <span data-ttu-id="c9e64-109">用于样式和常用组件的[引导](https://github.com/twbs/bootstrap)。</span><span class="sxs-lookup"><span data-stu-id="c9e64-109">[bootstrap](https://github.com/twbs/bootstrap) for styling and common components.</span></span>
- <span data-ttu-id="c9e64-110">[reactstrap](https://github.com/reactstrap/reactstrap)基于引导组件做出响应的。</span><span class="sxs-lookup"><span data-stu-id="c9e64-110">[reactstrap](https://github.com/reactstrap/reactstrap) for React components based on Bootstrap.</span></span>
- <span data-ttu-id="c9e64-111">[fontawesome-](https://github.com/FortAwesome/Font-Awesome)图标的可用。</span><span class="sxs-lookup"><span data-stu-id="c9e64-111">[fontawesome-free](https://github.com/FortAwesome/Font-Awesome) for icons.</span></span>
- <span data-ttu-id="c9e64-112">日期和时间格式的[时刻](https://github.com/moment/moment)。</span><span class="sxs-lookup"><span data-stu-id="c9e64-112">[moment](https://github.com/moment/moment) for formatting dates and times.</span></span>
- <span data-ttu-id="c9e64-113">用于对 Azure Active Directory 进行身份验证和检索访问令牌的[msal](https://github.com/AzureAD/microsoft-authentication-library-for-js) 。</span><span class="sxs-lookup"><span data-stu-id="c9e64-113">[msal](https://github.com/AzureAD/microsoft-authentication-library-for-js) for authenticating to Azure Active Directory and retrieving access tokens.</span></span>
- <span data-ttu-id="c9e64-114">microsoft graph-用于调用 Microsoft Graph 的[客户端](https://github.com/microsoftgraph/msgraph-sdk-javascript)。</span><span class="sxs-lookup"><span data-stu-id="c9e64-114">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) for making calls to Microsoft Graph.</span></span>

<span data-ttu-id="c9e64-115">在 CLI 中运行以下命令。</span><span class="sxs-lookup"><span data-stu-id="c9e64-115">Run the following command in your CLI.</span></span>

```Shell
npm install react-router-dom@5.1.2 @types/react-router-dom@5.1.3 bootstrap@4.4.1 reactstrap@8.4.1 @types/reactstrap@8.4.2
npm install @fortawesome/fontawesome-free@5.12.1 moment@2.24.0 msal@1.2.1 @microsoft/microsoft-graph-client@2.0.0 @types/microsoft-graph@1.12.0
```

## <a name="design-the-app"></a><span data-ttu-id="c9e64-116">设计应用程序</span><span class="sxs-lookup"><span data-stu-id="c9e64-116">Design the app</span></span>

<span data-ttu-id="c9e64-117">首先，创建应用的导航栏。</span><span class="sxs-lookup"><span data-stu-id="c9e64-117">Start by creating a navbar for the app.</span></span>

1. <span data-ttu-id="c9e64-118">在名为`./src` `NavBar.tsx`的目录中创建一个新文件，并添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="c9e64-118">Create a new file in the `./src` directory named `NavBar.tsx` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/NavBar.tsx" id="NavBarSnippet":::

1. <span data-ttu-id="c9e64-119">为该应用程序创建一个主页。</span><span class="sxs-lookup"><span data-stu-id="c9e64-119">Create a home page for the app.</span></span> <span data-ttu-id="c9e64-120">在名为`./src` `Welcome.tsx`的目录中创建一个新文件，并添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="c9e64-120">Create a new file in the `./src` directory named `Welcome.tsx` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/Welcome.tsx" id="WelcomeSnippet":::

1. <span data-ttu-id="c9e64-121">创建错误消息显示，以向用户显示消息。</span><span class="sxs-lookup"><span data-stu-id="c9e64-121">Create an error message display to display messages to the user.</span></span> <span data-ttu-id="c9e64-122">在名为`./src` `ErrorMessage.tsx`的目录中创建一个新文件，并添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="c9e64-122">Create a new file in the `./src` directory named `ErrorMessage.tsx` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/ErrorMessage.tsx" id="ErrorMessageSnippet":::

1. <span data-ttu-id="c9e64-123">打开 `./src/index.css` 文件，并将其全部内容替换为以下内容。</span><span class="sxs-lookup"><span data-stu-id="c9e64-123">Open the `./src/index.css` file and replace its entire contents with the following.</span></span>

    :::code language="css" source="../demo/graph-tutorial/src/index.css":::

1. <span data-ttu-id="c9e64-124">打开`./src/App.tsx`并将其全部内容替换为以下内容。</span><span class="sxs-lookup"><span data-stu-id="c9e64-124">Open `./src/App.tsx` and replace its entire contents with the following.</span></span>

    ```typescript
    import React, { Component } from 'react';
    import { BrowserRouter as Router, Route, Redirect } from 'react-router-dom';
    import { Container } from 'reactstrap';
    import NavBar from './NavBar';
    import ErrorMessage from './ErrorMessage';
    import Welcome from './Welcome';
    import 'bootstrap/dist/css/bootstrap.css';

    class App extends Component<any> {
      render() {
        let error = null;
        if (this.props.error) {
          error = <ErrorMessage
            message={this.props.error.message}
            debug={this.props.error.debug} />;
        }

        return (
          <Router>
            <div>
              <NavBar
                isAuthenticated={this.props.isAuthenticated}
                authButtonMethod={this.props.isAuthenticated ? this.props.logout : this.props.login}
                user={this.props.user}/>
              <Container>
                {error}
                <Route exact path="/"
                  render={(props) =>
                    <Welcome {...props}
                      isAuthenticated={this.props.isAuthenticated}
                      user={this.props.user}
                      authButtonMethod={this.props.login} />
                  } />
              </Container>
            </div>
          </Router>
        );
      }
    }

    export default App;
    ```

1. <span data-ttu-id="c9e64-125">保存所有更改并刷新页面。</span><span class="sxs-lookup"><span data-stu-id="c9e64-125">Save all of your changes and refresh the page.</span></span> <span data-ttu-id="c9e64-126">现在，应用程序看起来应非常不同。</span><span class="sxs-lookup"><span data-stu-id="c9e64-126">Now, the app should look very different.</span></span>

    ![重新设计的主页的屏幕截图](images/create-app-01.png)
