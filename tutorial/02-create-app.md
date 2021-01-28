<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="3ec4f-101">在此部分中，你将创建新的 React 应用。</span><span class="sxs-lookup"><span data-stu-id="3ec4f-101">In this section you'll create a new React app.</span></span>

1. <span data-ttu-id="3ec4f-102">打开命令行界面 (CLI) ，导航到您具有创建文件权限的目录，然后运行以下命令以创建新的 React 应用。</span><span class="sxs-lookup"><span data-stu-id="3ec4f-102">Open your command-line interface (CLI), navigate to a directory where you have rights to create files, and run the following commands to create a new React app.</span></span>

    ```Shell
    npx create-react-app@4.0.1 graph-tutorial --template typescript
    ```

1. <span data-ttu-id="3ec4f-103">命令完成后，在 CLI 中更改为目录并运行 `graph-tutorial` 以下命令以启动本地 Web 服务器。</span><span class="sxs-lookup"><span data-stu-id="3ec4f-103">Once the command finishes, change to the `graph-tutorial` directory in your CLI and run the following command to start a local web server.</span></span>

    ```Shell
    yarn start
    ```

    > [!NOTE]
    > <span data-ttu-id="3ec4f-104">如果未安装 ["一](https://yarnpkg.com/) 线"，可以 `npm start` 改为使用。</span><span class="sxs-lookup"><span data-stu-id="3ec4f-104">If you do not have [Yarn](https://yarnpkg.com/) installed, you can use `npm start` instead.</span></span>

<span data-ttu-id="3ec4f-105">默认浏览器将打开 [https://localhost:3000/](https://localhost:3000) 至默认 React 页面。</span><span class="sxs-lookup"><span data-stu-id="3ec4f-105">Your default browser opens to [https://localhost:3000/](https://localhost:3000) with a default React page.</span></span> <span data-ttu-id="3ec4f-106">如果浏览器未打开，请打开它并浏览 [https://localhost:3000/](https://localhost:3000) 以验证新应用是否正常工作。</span><span class="sxs-lookup"><span data-stu-id="3ec4f-106">If your browser doesn't open, open it and browse to [https://localhost:3000/](https://localhost:3000) to verify that the new app works.</span></span>

## <a name="add-node-packages"></a><span data-ttu-id="3ec4f-107">添加节点包</span><span class="sxs-lookup"><span data-stu-id="3ec4f-107">Add Node packages</span></span>

<span data-ttu-id="3ec4f-108">在继续之前，请安装一些稍后将使用的附加程序包：</span><span class="sxs-lookup"><span data-stu-id="3ec4f-108">Before moving on, install some additional packages that you will use later:</span></span>

- <span data-ttu-id="3ec4f-109">[react-router-dom，](https://github.com/ReactTraining/react-router) 用于 React 应用内的声明性路由。</span><span class="sxs-lookup"><span data-stu-id="3ec4f-109">[react-router-dom](https://github.com/ReactTraining/react-router) for declarative routing inside the React app.</span></span>
- <span data-ttu-id="3ec4f-110">[用于样式](https://github.com/twbs/bootstrap) 设置和常见组件的引导。</span><span class="sxs-lookup"><span data-stu-id="3ec4f-110">[bootstrap](https://github.com/twbs/bootstrap) for styling and common components.</span></span>
- <span data-ttu-id="3ec4f-111">[基于 Bootstrap](https://github.com/reactstrap/reactstrap) 的 React 组件的 reactstrap。</span><span class="sxs-lookup"><span data-stu-id="3ec4f-111">[reactstrap](https://github.com/reactstrap/reactstrap) for React components based on Bootstrap.</span></span>
- <span data-ttu-id="3ec4f-112">[无 fontwintesome](https://github.com/FortAwesome/Font-Awesome) 的图标。</span><span class="sxs-lookup"><span data-stu-id="3ec4f-112">[fontawesome-free](https://github.com/FortAwesome/Font-Awesome) for icons.</span></span>
- <span data-ttu-id="3ec4f-113">[设置](https://github.com/moment/moment) 日期和时间格式的时间。</span><span class="sxs-lookup"><span data-stu-id="3ec4f-113">[moment](https://github.com/moment/moment) for formatting dates and times.</span></span>
- <span data-ttu-id="3ec4f-114">用于将 Windows 时区转换为 IANA 格式的[windows-iana。](https://github.com/rubenillodo/windows-iana)</span><span class="sxs-lookup"><span data-stu-id="3ec4f-114">[windows-iana](https://github.com/rubenillodo/windows-iana) for translating Windows time zones to IANA format.</span></span>
- <span data-ttu-id="3ec4f-115">[用于对](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-browser) Azure Active Directory 进行身份验证和检索访问令牌的 msal 浏览器。</span><span class="sxs-lookup"><span data-stu-id="3ec4f-115">[msal-browser](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-browser) for authenticating to Azure Active Directory and retrieving access tokens.</span></span>
- <span data-ttu-id="3ec4f-116">[用于调用 Microsoft](https://github.com/microsoftgraph/msgraph-sdk-javascript) Graph 的 microsoft-graph-client。</span><span class="sxs-lookup"><span data-stu-id="3ec4f-116">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) for making calls to Microsoft Graph.</span></span>

<span data-ttu-id="3ec4f-117">在 CLI 中运行以下命令。</span><span class="sxs-lookup"><span data-stu-id="3ec4f-117">Run the following command in your CLI.</span></span>

```Shell
yarn add react-router-dom@5.2.0 @types/react-router-dom@5.1.7 bootstrap@4.6.0 reactstrap@8.9.0 @types/reactstrap@8.7.2 @fortawesome/fontawesome-free@5.15.2
yarn add moment@2.29.1 moment-timezone@0.5.32 windows-iana@4.2.1 @azure/msal-browser@2.10.0 @microsoft/microsoft-graph-client@2.2.1 @types/microsoft-graph@1.28.0
```

## <a name="design-the-app"></a><span data-ttu-id="3ec4f-118">设计应用</span><span class="sxs-lookup"><span data-stu-id="3ec4f-118">Design the app</span></span>

<span data-ttu-id="3ec4f-119">首先为应用创建导航栏。</span><span class="sxs-lookup"><span data-stu-id="3ec4f-119">Start by creating a navbar for the app.</span></span>

1. <span data-ttu-id="3ec4f-120">在名为的目录中创建新文件 `./src` 并 `NavBar.tsx` 添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="3ec4f-120">Create a new file in the `./src` directory named `NavBar.tsx` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/NavBar.tsx" id="NavBarSnippet":::

1. <span data-ttu-id="3ec4f-121">为应用创建主页。</span><span class="sxs-lookup"><span data-stu-id="3ec4f-121">Create a home page for the app.</span></span> <span data-ttu-id="3ec4f-122">在名为的目录中创建新文件 `./src` 并 `Welcome.tsx` 添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="3ec4f-122">Create a new file in the `./src` directory named `Welcome.tsx` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/Welcome.tsx" id="WelcomeSnippet":::

1. <span data-ttu-id="3ec4f-123">创建错误消息显示以向用户显示消息。</span><span class="sxs-lookup"><span data-stu-id="3ec4f-123">Create an error message display to display messages to the user.</span></span> <span data-ttu-id="3ec4f-124">在名为的目录中创建新文件 `./src` 并 `ErrorMessage.tsx` 添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="3ec4f-124">Create a new file in the `./src` directory named `ErrorMessage.tsx` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/ErrorMessage.tsx" id="ErrorMessageSnippet":::

1. <span data-ttu-id="3ec4f-125">打开 `./src/index.css` 文件，并将其全部内容替换为以下内容。</span><span class="sxs-lookup"><span data-stu-id="3ec4f-125">Open the `./src/index.css` file and replace its entire contents with the following.</span></span>

    :::code language="css" source="../demo/graph-tutorial/src/index.css":::

1. <span data-ttu-id="3ec4f-126">打开 `./src/App.tsx` 并替换其全部内容，并替换为以下内容。</span><span class="sxs-lookup"><span data-stu-id="3ec4f-126">Open `./src/App.tsx` and replace its entire contents with the following.</span></span>

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
                user={this.props.user} />
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

1. <span data-ttu-id="3ec4f-127">保存全部更改并重新启动该应用程序。</span><span class="sxs-lookup"><span data-stu-id="3ec4f-127">Save all of your changes and restart the app.</span></span> <span data-ttu-id="3ec4f-128">现在，应用看起来应该非常不同。</span><span class="sxs-lookup"><span data-stu-id="3ec4f-128">Now, the app should look very different.</span></span>

    ![重新设计主页的屏幕截图](images/create-app-01.png)
