<!-- markdownlint-disable MD002 MD041 -->

在本练习中，你将扩展上一练习中的应用程序，以支持 Azure AD 的身份验证。 若要获取所需的 OAuth 访问令牌以调用 Microsoft Graph，这是必需的。 在此步骤中，将[Microsoft 身份验证库](https://github.com/AzureAD/microsoft-authentication-library-for-js)库集成到应用程序中。

在名为`./src` `Config.js`的目录中创建一个新文件，并添加以下代码。

```js
module.exports = {
  appId: 'YOUR_APP_ID_HERE',
  redirectUri: 'http://localhost:3000',
  scopes: [
    'user.read',
    'calendars.read'
  ]
};
```

将`YOUR_APP_ID_HERE`替换为应用程序注册门户中的应用程序 ID。

> [!IMPORTANT]
> 如果您使用的是源代码管理（如 git），现在可以从源代码管理中排除`Config.js`该文件，以避免无意中泄漏您的应用程序 ID。

打开`./src/App.js`并将以下`import`语句添加到文件顶部。

```js
import config from './Config';
import { UserAgentApplication } from 'msal';
```

## <a name="implement-sign-in"></a>实施登录

`constructor`首先更新`App`类的，以创建`UserAgentApplication`类的实例，并调用`getUser`以查看是否已有登录用户。 将现有`constructor`替换为以下项。

```js
constructor(props) {
  super(props);

  this.userAgentApplication = new UserAgentApplication({
        auth: {
            clientId: config.appId,
            redirectUri: config.redirectUri
        },
        cache: {
            cacheLocation: "localStorage",
            storeAuthStateInCookie: true
        }
    });

  var user = this.userAgentApplication.getAccount();

  this.state = {
    isAuthenticated: (user !== null),
    user: {},
    error: null
  };

  if (user) {
    // Enhance user object with data from Graph
    this.getUserProfile();
  }
}
```

此代码使用您`UserAgentApplication`的应用程序 ID 初始化类，并检查是否存在用户。 如果有用户，则设置`isAuthenticated`为 true。 尚未`getUserProfile`实现此方法。 稍后将对此进行此实现。

接下来，向`App`类添加函数以执行登录。 将以下函数添加到 `App` 类。

```js
async login() {
  try {
    await this.userAgentApplication.loginPopup(
        {
          scopes: config.scopes,
          prompt: "select_account"
      });
    await this.getUserProfile();
  }
  catch(err) {
    var error = {};

    if (typeof(err) === 'string') {
      var errParts = err.split('|');
      error = errParts.length > 1 ?
        { message: errParts[1], debug: errParts[0] } :
        { message: err };
    } else {
      error = {
        message: err.message,
        debug: JSON.stringify(err)
      };
    }

    this.setState({
      isAuthenticated: false,
      user: {},
      error: error
    });
  }
}
```

此方法调用`loginPopup`函数以执行登录，然后调用`getUserProfile`函数。

现在，添加要注销的函数。 将以下函数添加到 `App` 类。

```js
logout() {
  this.userAgentApplication.logout();
}
```

现在`login` `logout`和方法已实现，更新了`NavBar` `Welcome` `render` `App`类的方法中的和元素。 将现有`NavBar`元素替换为以下项。

```JSX
<NavBar
  isAuthenticated={this.state.isAuthenticated}
  authButtonMethod={this.state.isAuthenticated ? this.logout.bind(this) : this.login.bind(this)}
  user={this.state.user}/>
```

将现有`Welcome`元素替换为以下项。

```JSX
<Welcome {...props}
  isAuthenticated={this.state.isAuthenticated}
  user={this.state.user}
  authButtonMethod={this.login.bind(this)} />
```

最后，实现该`getUserProfile`函数。 将以下函数添加到 `App` 类。

```js
async getUserProfile() {
  try {
    // Get the access token silently
    // If the cache contains a non-expired token, this function
    // will just return the cached token. Otherwise, it will
    // make a request to the Azure OAuth endpoint to get a token

    var accessToken = await this.userAgentApplication.acquireTokenSilent({
        scopes: config.scopes
      });

    if (accessToken) {
      // TEMPORARY: Display the token in the error flash
      this.setState({
        isAuthenticated: true,
        error: { message: "Access token:", debug: accessToken.accessToken }
      });
    }
  }
  catch(err) {
    var errParts = err.split('|');
    this.setState({
      isAuthenticated: false,
      user: {},
      error: { message: errParts[1], debug: errParts[0] }
    });
  }
}
```

此代码调用`acquireTokenSilent`获取访问令牌，然后只是将令牌输出为错误。

保存所做的更改并刷新浏览器。 单击 "登录" 按钮，您应会被重定向`https://login.microsoftonline.com`到。 使用你的 Microsoft 帐户登录，并同意请求的权限。 应用程序页面应刷新，并显示令牌。

### <a name="get-user-details"></a>获取用户详细信息

首先，创建一个新文件来保存所有 Microsoft Graph 调用。 在`./src`目录中创建一个名`GraphService.js`为的新文件，并添加以下代码。

```js
var graph = require('@microsoft/microsoft-graph-client');

function getAuthenticatedClient(accessToken) {
  // Initialize Graph client
  const client = graph.Client.init({
    // Use the provided access token to authenticate
    // requests
    authProvider: (done) => {
      done(null, accessToken.accessToken);
    }
  });

  return client;
}

export async function getUserDetails(accessToken) {
  const client = getAuthenticatedClient(accessToken);

  const user = await client.api('/me').get();
  return user;
}
```

这将`getUserDetails`实现使用 MICROSOFT Graph SDK 调用`/me`终结点并返回结果的函数。

更新中`getUserProfile` `./src/App.js`的方法以调用此函数。 首先，将以下`import`语句添加到文件顶部。

```js
import { getUserDetails } from './GraphService';
```

将现有的 `getUserProfile` 函数替换成以下代码。

```js
async getUserProfile() {
  try {
    // Get the access token silently
    // If the cache contains a non-expired token, this function
    // will just return the cached token. Otherwise, it will
    // make a request to the Azure OAuth endpoint to get a token

    var accessToken = await this.userAgentApplication.acquireTokenSilent({
      scopes: config.scopes
    });

    if (accessToken) {
      // Get the user's profile from Graph
      var user = await getUserDetails(accessToken);
      this.setState({
        isAuthenticated: true,
        user: {
          displayName: user.displayName,
          email: user.mail || user.userPrincipalName
        },
        error: null
      });
    }
  }
  catch(err) {
    var error = {};
    if (typeof(err) === 'string') {
      var errParts = err.split('|');
      error = errParts.length > 1 ?
        { message: errParts[1], debug: errParts[0] } :
        { message: err };
    } else {
      error = {
        message: err.message,
        debug: JSON.stringify(err)
      };
    }

    this.setState({
      isAuthenticated: false,
      user: {},
      error: error
    });
  }
}
```

现在，如果您保存更改并启动应用程序，登录后应返回到主页，但 UI 应更改以指示您已登录。

![登录后主页的屏幕截图](./images/add-aad-auth-01.png)

单击右上角的用户头像以访问 "**注销**" 链接。 单击 "**注销**" 重置会话并返回到主页。

![带有 "注销" 链接的下拉菜单的屏幕截图](./images/add-aad-auth-02.png)

## <a name="storing-and-refreshing-tokens"></a>存储和刷新令牌

此时，您的应用程序具有访问令牌，该令牌是在 API `Authorization`调用的标头中发送的。 这是允许应用代表用户访问 Microsoft Graph 的令牌。

但是，此令牌的生存期较短。 令牌在发出后会过期一小时。 这就是刷新令牌变得有用的地方。 刷新令牌允许应用在不要求用户重新登录的情况下请求新的访问令牌。

由于应用程序使用的是 MSAL 库，因此您无需实现任何令牌存储或刷新逻辑。 该`UserAgentApplication`方法在浏览器会话中缓存标记。 该`acquireTokenSilent`方法首先检查缓存的标记，如果它未过期，它将返回。 如果它已过期，则使用缓存的刷新令牌获取新的。 您将在以下模块中更多地使用此方法。
