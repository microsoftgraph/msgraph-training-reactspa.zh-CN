<!-- markdownlint-disable MD002 MD041 -->

在此部分中，你将创建一个新的React应用。

1. 使用 CLI (打开命令行接口) 导航到你拥有创建文件的权限的目录，然后运行以下命令以创建新的 React 应用。

    ```Shell
    yarn create react-app graph-tutorial --template typescript
    ```

1. 命令完成后，在 CLI 中更改为 目录并运行 `graph-tutorial` 以下命令以启动本地 Web 服务器。

    ```Shell
    yarn start
    ```

    > [!NOTE]
    > 如果没有安装 [一个更新](https://yarnpkg.com/) 程序，可以 `npm start` 改为使用。

默认浏览器将打开， [https://localhost:3000/](https://localhost:3000) 并打开默认React页面。 如果浏览器未打开，请打开它并浏览 [https://localhost:3000/](https://localhost:3000) 以验证新应用是否正常工作。

## <a name="add-node-packages"></a>添加节点包

在继续之前，请安装一些你稍后将使用的其他程序包：

- [react-router-dom，](https://github.com/ReactTraining/react-router)用于应用程序内的声明React路由。
- [用于样式](https://github.com/twbs/bootstrap) 设置和常见组件的引导。
- [基于 Bootstrap](https://github.com/reactstrap/reactstrap) React组件的 reactstrap。
- [对于图标，无](https://github.com/FortAwesome/Font-Awesome) font操作。
- [设置](https://github.com/moment/moment) 日期和时间格式的时间。
- 用于将时区Windows IANA 格式的[windows-iana。](https://github.com/rubenillodo/windows-iana)
- [msal-browser，](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-browser)用于验证Azure Active Directory和检索访问令牌。
- [用于调用](https://github.com/microsoftgraph/msgraph-sdk-javascript)Microsoft Graph 的 microsoft-graph-client。

在 CLI 中运行以下命令。

```Shell
yarn add react-router-dom@5.2.0 bootstrap@4.6.0 reactstrap@8.9.0 @fortawesome/fontawesome-free@5.15.3
yarn add moment-timezone@0.5.33 windows-iana@5.0.2 @azure/msal-browser@2.14.1 @microsoft/microsoft-graph-client@2.2.1
yarn add -D @types/react-router-dom@5.1.7 @types/microsoft-graph@1.36.0
```

## <a name="design-the-app"></a>设计应用

首先为应用创建导航栏。

1. 在目录中新建一个名为 `./src` 的文件 `NavBar.tsx` 并添加以下代码。

    :::code language="typescript" source="../demo/graph-tutorial/src/NavBar.tsx" id="NavBarSnippet":::

1. 为应用创建主页。 在目录中新建一个名为 `./src` 的文件 `Welcome.tsx` 并添加以下代码。

    :::code language="typescript" source="../demo/graph-tutorial/src/Welcome.tsx" id="WelcomeSnippet":::

1. 创建错误消息显示，向用户显示消息。 在目录中新建一个名为 `./src` 的文件 `ErrorMessage.tsx` 并添加以下代码。

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

    ![重新设计的主页的屏幕截图](images/create-app-01.png)
