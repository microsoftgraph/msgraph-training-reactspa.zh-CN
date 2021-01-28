<!-- markdownlint-disable MD002 MD041 -->

在此部分中，你将创建新的 React 应用。

1. 打开命令行界面 (CLI) ，导航到您具有创建文件权限的目录，然后运行以下命令以创建新的 React 应用。

    ```Shell
    npx create-react-app@4.0.1 graph-tutorial --template typescript
    ```

1. 命令完成后，在 CLI 中更改为目录并运行 `graph-tutorial` 以下命令以启动本地 Web 服务器。

    ```Shell
    yarn start
    ```

    > [!NOTE]
    > 如果未安装 ["一](https://yarnpkg.com/) 线"，可以 `npm start` 改为使用。

默认浏览器将打开 [https://localhost:3000/](https://localhost:3000) 至默认 React 页面。 如果浏览器未打开，请打开它并浏览 [https://localhost:3000/](https://localhost:3000) 以验证新应用是否正常工作。

## <a name="add-node-packages"></a>添加节点包

在继续之前，请安装一些稍后将使用的附加程序包：

- [react-router-dom，](https://github.com/ReactTraining/react-router) 用于 React 应用内的声明性路由。
- [用于样式](https://github.com/twbs/bootstrap) 设置和常见组件的引导。
- [基于 Bootstrap](https://github.com/reactstrap/reactstrap) 的 React 组件的 reactstrap。
- [无 fontwintesome](https://github.com/FortAwesome/Font-Awesome) 的图标。
- [设置](https://github.com/moment/moment) 日期和时间格式的时间。
- 用于将 Windows 时区转换为 IANA 格式的[windows-iana。](https://github.com/rubenillodo/windows-iana)
- [用于对](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-browser) Azure Active Directory 进行身份验证和检索访问令牌的 msal 浏览器。
- [用于调用 Microsoft](https://github.com/microsoftgraph/msgraph-sdk-javascript) Graph 的 microsoft-graph-client。

在 CLI 中运行以下命令。

```Shell
yarn add react-router-dom@5.2.0 @types/react-router-dom@5.1.7 bootstrap@4.6.0 reactstrap@8.9.0 @types/reactstrap@8.7.2 @fortawesome/fontawesome-free@5.15.2
yarn add moment@2.29.1 moment-timezone@0.5.32 windows-iana@4.2.1 @azure/msal-browser@2.10.0 @microsoft/microsoft-graph-client@2.2.1 @types/microsoft-graph@1.28.0
```

## <a name="design-the-app"></a>设计应用

首先为应用创建导航栏。

1. 在名为的目录中创建新文件 `./src` 并 `NavBar.tsx` 添加以下代码。

    :::code language="typescript" source="../demo/graph-tutorial/src/NavBar.tsx" id="NavBarSnippet":::

1. 为应用创建主页。 在名为的目录中创建新文件 `./src` 并 `Welcome.tsx` 添加以下代码。

    :::code language="typescript" source="../demo/graph-tutorial/src/Welcome.tsx" id="WelcomeSnippet":::

1. 创建错误消息显示以向用户显示消息。 在名为的目录中创建新文件 `./src` 并 `ErrorMessage.tsx` 添加以下代码。

    :::code language="typescript" source="../demo/graph-tutorial/src/ErrorMessage.tsx" id="ErrorMessageSnippet":::

1. 打开 `./src/index.css` 文件，并将其全部内容替换为以下内容。

    :::code language="css" source="../demo/graph-tutorial/src/index.css":::

1. 打开 `./src/App.tsx` 并替换其全部内容，并替换为以下内容。

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

1. 保存全部更改并重新启动该应用程序。 现在，应用看起来应该非常不同。

    ![重新设计主页的屏幕截图](images/create-app-01.png)
