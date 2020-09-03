<!-- markdownlint-disable MD002 MD041 -->

在本节中，你将创建新的响应应用程序。

1. 打开命令行界面 (CLI) ，导航到您有权在其中创建文件的目录，并运行以下命令以创建新的响应应用程序。

    ```Shell
    npx create-react-app@3.4.1 graph-tutorial --template typescript
    ```

1. 命令完成后，转到 `graph-tutorial` CLI 中的目录，并运行以下命令以启动本地 web 服务器。

    ```Shell
    npm start
    ```

默认浏览器将打开 [https://localhost:3000/](https://localhost:3000) 并使用默认的响应页面。 如果你的浏览器未打开，请打开它并浏览以 [https://localhost:3000/](https://localhost:3000) 验证新应用是否正常工作。

## <a name="add-node-packages"></a>添加节点包

在继续操作之前，请先安装将使用的其他一些程序包：

- 响应-在响应应用程序中进行声明性路由的[路由器-dom](https://github.com/ReactTraining/react-router) 。
- 用于样式和常用组件的[引导](https://github.com/twbs/bootstrap)。
- [reactstrap](https://github.com/reactstrap/reactstrap) 基于引导组件做出响应的。
- [fontawesome-](https://github.com/FortAwesome/Font-Awesome) 图标的可用。
- 日期和时间格式的[时刻](https://github.com/moment/moment)。
- 用于将 Windows 时间区域转换为 IANA 格式的[windows-iana](https://github.com/rubenillodo/windows-iana) 。
- [msal-](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-browser) 用于对 Azure Active Directory 进行身份验证和检索访问令牌的浏览器。
- microsoft graph-用于调用 Microsoft Graph 的[客户端](https://github.com/microsoftgraph/msgraph-sdk-javascript)。

在 CLI 中运行以下命令。

```Shell
npm install react-router-dom@5.2.0 @types/react-router-dom@5.1.5 bootstrap@4.5.2 reactstrap@8.5.1 @types/reactstrap@8.5.1 @fortawesome/fontawesome-free@5.14.0
npm install moment@2.27.0 moment-timezone@0.5.31 windows-iana@4.2.1 @azure/msal-browser@2.1.0 @microsoft/microsoft-graph-client@2.0.0 @types/microsoft-graph@1.18.0
```

## <a name="design-the-app"></a>设计应用程序

首先，创建应用的导航栏。

1. 在名为的目录中创建一个新文件 `./src` `NavBar.tsx` ，并添加以下代码。

    :::code language="typescript" source="../demo/graph-tutorial/src/NavBar.tsx" id="NavBarSnippet":::

1. 为该应用程序创建一个主页。 在名为的目录中创建一个新文件 `./src` `Welcome.tsx` ，并添加以下代码。

    :::code language="typescript" source="../demo/graph-tutorial/src/Welcome.tsx" id="WelcomeSnippet":::

1. 创建错误消息显示，以向用户显示消息。 在名为的目录中创建一个新文件 `./src` `ErrorMessage.tsx` ，并添加以下代码。

    :::code language="typescript" source="../demo/graph-tutorial/src/ErrorMessage.tsx" id="ErrorMessageSnippet":::

1. 打开 `./src/index.css` 文件，并将其全部内容替换为以下内容。

    :::code language="css" source="../demo/graph-tutorial/src/index.css":::

1. 打开 `./src/App.tsx` 并将其全部内容替换为以下内容。

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

1. 保存所有更改并刷新页面。 现在，应用程序看起来应非常不同。

    ![重新设计的主页的屏幕截图](images/create-app-01.png)
