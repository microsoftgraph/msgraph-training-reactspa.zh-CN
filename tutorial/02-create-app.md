<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="7c17d-101">打开命令行接口 (CLI), 导航到您有权在其中创建文件的目录, 并运行以下命令来安装 "[创建响应-应用](https://www.npmjs.com/package/create-react-app)" 工具并创建新的响应应用程序。</span><span class="sxs-lookup"><span data-stu-id="7c17d-101">Open your command-line interface (CLI), navigate to a directory where you have rights to create files, and run the following commands to install the [create-react-app](https://www.npmjs.com/package/create-react-app) tool and create a new React app.</span></span>

```Shell
npm install create-react-app@3.0.1 -g
create-react-app graph-tutorial
```

<span data-ttu-id="7c17d-102">命令完成后, 转到 CLI 中`graph-tutorial`的目录, 并运行以下命令以启动本地 web 服务器。</span><span class="sxs-lookup"><span data-stu-id="7c17d-102">Once the command finishes, change to the `graph-tutorial` directory in your CLI and run the following command to start a local web server.</span></span>

```Shell
npm start
```

<span data-ttu-id="7c17d-103">默认浏览器将打开[https://localhost:3000/](https://localhost:3000)并使用默认的响应页面。</span><span class="sxs-lookup"><span data-stu-id="7c17d-103">Your default browser opens to [https://localhost:3000/](https://localhost:3000) with a default React page.</span></span> <span data-ttu-id="7c17d-104">如果你的浏览器未打开, 请打开它[https://localhost:3000/](https://localhost:3000)并浏览以验证新应用是否正常工作。</span><span class="sxs-lookup"><span data-stu-id="7c17d-104">If your browser doesn't open, open it and browse to [https://localhost:3000/](https://localhost:3000) to verify that the new app works.</span></span>

<span data-ttu-id="7c17d-105">在继续操作之前, 请先安装将使用的其他一些程序包:</span><span class="sxs-lookup"><span data-stu-id="7c17d-105">Before moving on, install some additional packages that you will use later:</span></span>

- <span data-ttu-id="7c17d-106">响应-在响应应用程序中进行声明性路由的[路由器-dom](https://github.com/ReactTraining/react-router) 。</span><span class="sxs-lookup"><span data-stu-id="7c17d-106">[react-router-dom](https://github.com/ReactTraining/react-router) for declarative routing inside the React app.</span></span>
- <span data-ttu-id="7c17d-107">用于样式和常用组件的[引导](https://github.com/twbs/bootstrap)。</span><span class="sxs-lookup"><span data-stu-id="7c17d-107">[bootstrap](https://github.com/twbs/bootstrap) for styling and common components.</span></span>
- <span data-ttu-id="7c17d-108">[reactstrap](https://github.com/reactstrap/reactstrap)基于引导组件做出响应的。</span><span class="sxs-lookup"><span data-stu-id="7c17d-108">[reactstrap](https://github.com/reactstrap/reactstrap) for React components based on Bootstrap.</span></span>
- <span data-ttu-id="7c17d-109">[fontawesome-](https://github.com/FortAwesome/Font-Awesome)图标的可用。</span><span class="sxs-lookup"><span data-stu-id="7c17d-109">[fontawesome-free](https://github.com/FortAwesome/Font-Awesome) for icons.</span></span>
- <span data-ttu-id="7c17d-110">日期和时间格式的[时刻](https://github.com/moment/moment)。</span><span class="sxs-lookup"><span data-stu-id="7c17d-110">[moment](https://github.com/moment/moment) for formatting dates and times.</span></span>
- <span data-ttu-id="7c17d-111">用于对 Azure Active Directory 进行身份验证和检索访问令牌的[msal](https://github.com/AzureAD/microsoft-authentication-library-for-js) 。</span><span class="sxs-lookup"><span data-stu-id="7c17d-111">[msal](https://github.com/AzureAD/microsoft-authentication-library-for-js) for authenticating to Azure Active Directory and retrieving access tokens.</span></span>
- <span data-ttu-id="7c17d-112">microsoft graph-用于调用 Microsoft Graph 的[客户端](https://github.com/microsoftgraph/msgraph-sdk-javascript)。</span><span class="sxs-lookup"><span data-stu-id="7c17d-112">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) for making calls to Microsoft Graph.</span></span>

<span data-ttu-id="7c17d-113">在 CLI 中运行以下命令。</span><span class="sxs-lookup"><span data-stu-id="7c17d-113">Run the following command in your CLI.</span></span>

```Shell
npm install react-router-dom@5.0.0 bootstrap@4.3.1 reactstrap@8.0.0 @fortawesome/fontawesome-free@5.8.2
npm install moment@2.24.0 msal@1.0.0 @microsoft/microsoft-graph-client@1.6.0
```

## <a name="design-the-app"></a><span data-ttu-id="7c17d-114">设计应用程序</span><span class="sxs-lookup"><span data-stu-id="7c17d-114">Design the app</span></span>

<span data-ttu-id="7c17d-115">首先, 创建应用的导航栏。</span><span class="sxs-lookup"><span data-stu-id="7c17d-115">Start by creating a navbar for the app.</span></span> <span data-ttu-id="7c17d-116">在名为`./src` `Navbar.js`的目录中创建一个新文件, 并添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="7c17d-116">Create a new file in the `./src` directory named `Navbar.js` and add the following code.</span></span>

```JSX
import React from 'react';
import { NavLink as RouterNavLink } from 'react-router-dom';
import {
  Collapse,
  Container,
  Navbar,
  NavbarToggler,
  NavbarBrand,
  Nav,
  NavItem,
  NavLink,
  UncontrolledDropdown,
  DropdownToggle,
  DropdownMenu,
  DropdownItem } from 'reactstrap';
import '@fortawesome/fontawesome-free/css/all.css';

function UserAvatar(props) {
  // If a user avatar is available, return an img tag with the pic
  if (props.user.avatar) {
    return <img
            src={props.user.avatar} alt="user"
            className="rounded-circle align-self-center mr-2"
            style={{width: '32px'}}></img>;
  }

  // No avatar available, return a default icon
  return <i
          className="far fa-user-circle fa-lg rounded-circle align-self-center mr-2"
          style={{width: '32px'}}></i>;
}

function AuthNavItem(props) {
  // If authenticated, return a dropdown with the user's info and a
  // sign out button
  if (props.isAuthenticated) {
    return (
      <UncontrolledDropdown>
        <DropdownToggle nav caret>
          <UserAvatar user={props.user}/>
        </DropdownToggle>
        <DropdownMenu right>
          <h5 className="dropdown-item-text mb-0">{props.user.displayName}</h5>
          <p className="dropdown-item-text text-muted mb-0">{props.user.email}</p>
          <DropdownItem divider />
          <DropdownItem onClick={props.authButtonMethod}>Sign Out</DropdownItem>
        </DropdownMenu>
      </UncontrolledDropdown>
    );
  }

  // Not authenticated, return a sign in link
  return (
    <NavItem>
      <NavLink onClick={props.authButtonMethod}>Sign In</NavLink>
    </NavItem>
  );
}

export default class NavBar extends React.Component {
  constructor(props) {
    super(props);

    this.toggle = this.toggle.bind(this);
    this.state = {
      isOpen: false
    };
  }

  toggle() {
    this.setState({
      isOpen: !this.state.isOpen
    });
  }

  render() {
    // Only show calendar nav item if logged in
    let calendarLink = null;
    if (this.props.isAuthenticated) {
      calendarLink = (
        <NavItem>
          <RouterNavLink to="/calendar" className="nav-link" exact>Calendar</RouterNavLink>
        </NavItem>
      );
    }

    return (
      <div>
        <Navbar color="dark" dark expand="md" fixed="top">
          <Container>
            <NavbarBrand href="/">React Graph Tutorial</NavbarBrand>
            <NavbarToggler onClick={this.toggle} />
            <Collapse isOpen={this.state.isOpen} navbar>
              <Nav className="mr-auto" navbar>
                <NavItem>
                  <RouterNavLink to="/" className="nav-link" exact>Home</RouterNavLink>
                </NavItem>
                {calendarLink}
              </Nav>
              <Nav className="justify-content-end" navbar>
                <NavItem>
                  <NavLink href="https://developer.microsoft.com/graph/docs/concepts/overview" target="_blank">
                    <i className="fas fa-external-link-alt mr-1"></i>
                    Docs
                  </NavLink>
                </NavItem>
                <AuthNavItem
                  isAuthenticated={this.props.isAuthenticated}
                  authButtonMethod={this.props.authButtonMethod}
                  user={this.props.user} />
              </Nav>
            </Collapse>
          </Container>
        </Navbar>
      </div>
    );
  }
}
```

<span data-ttu-id="7c17d-117">接下来, 为该应用程序创建一个主页。</span><span class="sxs-lookup"><span data-stu-id="7c17d-117">Next, create a home page for the app.</span></span> <span data-ttu-id="7c17d-118">在名为`./src` `Welcome.js`的目录中创建一个新文件, 并添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="7c17d-118">Create a new file in the `./src` directory named `Welcome.js` and add the following code.</span></span>

```JSX
import React from 'react';
import {
  Button,
  Jumbotron } from 'reactstrap';

function WelcomeContent(props) {
  // If authenticated, greet the user
  if (props.isAuthenticated) {
    return (
      <div>
        <h4>Welcome {props.user.displayName}!</h4>
        <p>Use the navigation bar at the top of the page to get started.</p>
      </div>
    );
  }

  // Not authenticated, present a sign in button
  return <Button color="primary" onClick={props.authButtonMethod}>Click here to sign in</Button>;
}

export default class Welcome extends React.Component {
  render() {
    return (
      <Jumbotron>
        <h1>React Graph Tutorial</h1>
        <p className="lead">
            This sample app shows how to use the Microsoft Graph API to access Outlook and OneDrive data from React
        </p>
        <WelcomeContent
          isAuthenticated={this.props.isAuthenticated}
          user={this.props.user}
          authButtonMethod={this.props.authButtonMethod} />
      </Jumbotron>
    );
  }
}
```

<span data-ttu-id="7c17d-119">现在, 创建错误消息显示, 以向用户显示消息。</span><span class="sxs-lookup"><span data-stu-id="7c17d-119">Now create an error message display to display messages to the user.</span></span> <span data-ttu-id="7c17d-120">在名为`./src` `ErrorMessage.js`的目录中创建一个新文件, 并添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="7c17d-120">Create a new file in the `./src` directory named `ErrorMessage.js` and add the following code.</span></span>

```JSX
import React from 'react';
import { Alert } from 'reactstrap';

export default class ErrorMessage extends React.Component {
  render() {
    let debug = null;
    if (this.props.debug) {
      debug = <pre className="alert-pre border bg-light p-2"><code>{this.props.debug}</code></pre>;
    }
    return (
      <Alert color="danger">
        <p className="mb-3">{this.props.message}</p>
        {debug}
      </Alert>
    );
  }
}
```

<span data-ttu-id="7c17d-121">现在, 定义了这些基本组件, 更新应用程序以使用它们。</span><span class="sxs-lookup"><span data-stu-id="7c17d-121">Now with those basic components defined, update the app to use them.</span></span> <span data-ttu-id="7c17d-122">首先, 打开`./src/index.css`文件, 并将其全部内容替换为以下内容。</span><span class="sxs-lookup"><span data-stu-id="7c17d-122">First, open the `./src/index.css` file and replace its entire contents with the following.</span></span>

```css
body {
  padding-top: 4.5rem;
}

.alert-pre {
  word-wrap: break-word;
  word-break: break-all;
  white-space: pre-wrap;
}
```

<span data-ttu-id="7c17d-123">接下来, `./src/App.js`打开并将其全部内容替换为以下内容。</span><span class="sxs-lookup"><span data-stu-id="7c17d-123">Next, open `./src/App.js` and replace its entire contents with the following.</span></span>

```JSX
import React, { Component } from 'react';
import { BrowserRouter as Router, Route } from 'react-router-dom';
import { Container } from 'reactstrap';
import NavBar from './NavBar';
import ErrorMessage from './ErrorMessage';
import Welcome from './Welcome';
import 'bootstrap/dist/css/bootstrap.css';

class App extends Component {
  constructor(props) {
    super(props);

    this.state = {
      isAuthenticated: false,
      user: {},
      error: null
    };
  }

  render() {
    let error = null;
    if (this.state.error) {
      error = <ErrorMessage message={this.state.error.message} debug={this.state.error.debug} />;
    }

    return (
      <Router>
        <div>
          <NavBar
            isAuthenticated={this.state.isAuthenticated}
            authButtonMethod={null}
            user={this.state.user}/>
          <Container>
            {error}
            <Route exact path="/"
              render={(props) =>
                <Welcome {...props}
                  isAuthenticated={this.state.isAuthenticated}
                  user={this.state.user}
                  authButtonMethod={null} />
              } />
          </Container>
        </div>
      </Router>
    );
  }

  setErrorMessage(message, debug) {
    this.setState({
      error: {message: message, debug: debug}
    });
  }
}

export default App;
```

<span data-ttu-id="7c17d-124">保存所有更改并刷新页面。</span><span class="sxs-lookup"><span data-stu-id="7c17d-124">Save all of your changes and refresh the page.</span></span> <span data-ttu-id="7c17d-125">现在, 应用程序看起来应非常不同。</span><span class="sxs-lookup"><span data-stu-id="7c17d-125">Now, the app should look very different.</span></span>

![重新设计的主页的屏幕截图](images/create-app-01.png)
