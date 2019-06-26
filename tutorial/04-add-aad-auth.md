<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="682a7-101">在本练习中, 你将扩展上一练习中的应用程序, 以支持 Azure AD 的身份验证。</span><span class="sxs-lookup"><span data-stu-id="682a7-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="682a7-102">若要获取所需的 OAuth 访问令牌以调用 Microsoft Graph, 这是必需的。</span><span class="sxs-lookup"><span data-stu-id="682a7-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="682a7-103">在此步骤中, 将[Microsoft 身份验证库](https://github.com/AzureAD/microsoft-authentication-library-for-js)库集成到应用程序中。</span><span class="sxs-lookup"><span data-stu-id="682a7-103">In this step you will integrate the [Microsoft Authentication Library](https://github.com/AzureAD/microsoft-authentication-library-for-js) library into the application.</span></span>

<span data-ttu-id="682a7-104">在名为`./src` `Config.js`的目录中创建一个新文件, 并添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="682a7-104">Create a new file in the `./src` directory named `Config.js` and add the following code.</span></span>

```js
module.exports = {
  appId: 'YOUR_APP_ID_HERE',
  scopes: [
    "user.read",
    "calendars.read"
  ]
};
```

<span data-ttu-id="682a7-105">将`YOUR_APP_ID_HERE`替换为应用程序注册门户中的应用程序 ID。</span><span class="sxs-lookup"><span data-stu-id="682a7-105">Replace `YOUR_APP_ID_HERE` with the application ID from the Application Registration Portal.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="682a7-106">如果您使用的是源代码管理 (如 git), 现在可以从源代码管理中排除`Config.js`该文件, 以避免无意中泄漏您的应用程序 ID。</span><span class="sxs-lookup"><span data-stu-id="682a7-106">If you're using source control such as git, now would be a good time to exclude the `Config.js` file from source control to avoid inadvertently leaking your app ID.</span></span>

<span data-ttu-id="682a7-107">打开`./src/App.js`并将以下`import`语句添加到文件顶部。</span><span class="sxs-lookup"><span data-stu-id="682a7-107">Open `./src/App.js` and add the following `import` statements to the top of the file.</span></span>

```js
import config from './Config';
import { UserAgentApplication } from 'msal';
```

## <a name="implement-sign-in"></a><span data-ttu-id="682a7-108">实施登录</span><span class="sxs-lookup"><span data-stu-id="682a7-108">Implement sign-in</span></span>

<span data-ttu-id="682a7-109">`constructor`首先更新`App`类的, 以创建`UserAgentApplication`类的实例, 并调用`getUser`以查看是否已有登录用户。</span><span class="sxs-lookup"><span data-stu-id="682a7-109">Start by updating the `constructor` for the `App` class to create an instance of the `UserAgentApplication` class and call `getUser` to see if there's already a logged-in user.</span></span> <span data-ttu-id="682a7-110">将现有`constructor`替换为以下项。</span><span class="sxs-lookup"><span data-stu-id="682a7-110">Replace the existing `constructor` with the following.</span></span>

```js
constructor(props) {
  super(props);

  this.userAgentApplication = new UserAgentApplication({
        auth: {
            clientId: config.appId
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

<span data-ttu-id="682a7-111">此代码使用您`UserAgentApplication`的应用程序 ID 初始化类, 并检查是否存在用户。</span><span class="sxs-lookup"><span data-stu-id="682a7-111">This code initializes the `UserAgentApplication` class with your application ID and checks for the presence of a user.</span></span> <span data-ttu-id="682a7-112">如果有用户, 则设置`isAuthenticated`为 true。</span><span class="sxs-lookup"><span data-stu-id="682a7-112">If there is a user, it sets `isAuthenticated` to true.</span></span> <span data-ttu-id="682a7-113">尚未`getUserProfile`实现此方法。</span><span class="sxs-lookup"><span data-stu-id="682a7-113">The `getUserProfile` method isn't implemented yet.</span></span> <span data-ttu-id="682a7-114">稍后将对此进行此实现。</span><span class="sxs-lookup"><span data-stu-id="682a7-114">You will implement this a bit later.</span></span>

<span data-ttu-id="682a7-115">接下来, 向`App`类添加函数以执行登录。</span><span class="sxs-lookup"><span data-stu-id="682a7-115">Next, add a function to the `App` class to do the login.</span></span> <span data-ttu-id="682a7-116">将以下函数添加到 `App` 类。</span><span class="sxs-lookup"><span data-stu-id="682a7-116">Add the following function to the `App` class.</span></span>

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
    var errParts = err.split('|');
    this.setState({
      isAuthenticated: false,
      user: {},
      error: { message: errParts[1], debug: errParts[0] }
    });
  }
}
```

<span data-ttu-id="682a7-117">此方法调用`loginPopup`函数以执行登录, 然后调用`getUserProfile`函数。</span><span class="sxs-lookup"><span data-stu-id="682a7-117">This method calls the `loginPopup` function to do the login, then calls the `getUserProfile` function.</span></span>

<span data-ttu-id="682a7-118">现在, 添加要注销的函数。</span><span class="sxs-lookup"><span data-stu-id="682a7-118">Now add a function to logout.</span></span> <span data-ttu-id="682a7-119">将以下函数添加到 `App` 类。</span><span class="sxs-lookup"><span data-stu-id="682a7-119">Add the following function to the `App` class.</span></span>

```js
logout() {
  this.userAgentApplication.logout();
}
```

<span data-ttu-id="682a7-120">现在`login` `logout`和方法已实现, 更新了`NavBar` `Welcome` `render` `App`类的方法中的和元素。</span><span class="sxs-lookup"><span data-stu-id="682a7-120">Now that the `login` and `logout` methods are implemented, update the `NavBar` and `Welcome` elements in the `render` method of the `App` class.</span></span> <span data-ttu-id="682a7-121">将现有`NavBar`元素替换为以下项。</span><span class="sxs-lookup"><span data-stu-id="682a7-121">Replace the existing `NavBar` element with the following.</span></span>

```JSX
<NavBar
  isAuthenticated={this.state.isAuthenticated}
  authButtonMethod={this.state.isAuthenticated ? this.logout.bind(this) : this.login.bind(this)}
  user={this.state.user}/>
```

<span data-ttu-id="682a7-122">将现有`Welcome`元素替换为以下项。</span><span class="sxs-lookup"><span data-stu-id="682a7-122">Replace the existing `Welcome` element with the following.</span></span>

```JSX
<Welcome {...props}
  isAuthenticated={this.state.isAuthenticated}
  user={this.state.user}
  authButtonMethod={this.login.bind(this)} />
```

<span data-ttu-id="682a7-123">最后, 实现该`getUserProfile`函数。</span><span class="sxs-lookup"><span data-stu-id="682a7-123">Finally, implement the `getUserProfile` function.</span></span> <span data-ttu-id="682a7-124">将以下函数添加到 `App` 类。</span><span class="sxs-lookup"><span data-stu-id="682a7-124">Add the following function to the `App` class.</span></span>

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

<span data-ttu-id="682a7-125">此代码调用`acquireTokenSilent`获取访问令牌, 然后只是将令牌输出为错误。</span><span class="sxs-lookup"><span data-stu-id="682a7-125">This code calls `acquireTokenSilent` to get an access token, then just outputs the token as an error.</span></span>

<span data-ttu-id="682a7-126">保存所做的更改并刷新浏览器。</span><span class="sxs-lookup"><span data-stu-id="682a7-126">Save your changes and refresh the browser.</span></span> <span data-ttu-id="682a7-127">单击 "登录" 按钮, 您应会被重定向`https://login.microsoftonline.com`到。</span><span class="sxs-lookup"><span data-stu-id="682a7-127">Click the sign-in button and you should be redirected to `https://login.microsoftonline.com`.</span></span> <span data-ttu-id="682a7-128">使用你的 Microsoft 帐户登录, 并同意请求的权限。</span><span class="sxs-lookup"><span data-stu-id="682a7-128">Login with your Microsoft account and consent to the requested permissions.</span></span> <span data-ttu-id="682a7-129">应用程序页面应刷新, 并显示令牌。</span><span class="sxs-lookup"><span data-stu-id="682a7-129">The app page should refresh, showing the token.</span></span>

### <a name="get-user-details"></a><span data-ttu-id="682a7-130">获取用户详细信息</span><span class="sxs-lookup"><span data-stu-id="682a7-130">Get user details</span></span>

<span data-ttu-id="682a7-131">首先, 创建一个新文件来保存所有 Microsoft Graph 调用。</span><span class="sxs-lookup"><span data-stu-id="682a7-131">Start by creating a new file to hold all of your Microsoft Graph calls.</span></span> <span data-ttu-id="682a7-132">在`./src`目录中创建一个名`GraphService.js`为的新文件, 并添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="682a7-132">Create a new file in the `./src` directory called `GraphService.js` and add the following code.</span></span>

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

<span data-ttu-id="682a7-133">这将`getUserDetails`实现使用 MICROSOFT Graph SDK 调用`/me`终结点并返回结果的函数。</span><span class="sxs-lookup"><span data-stu-id="682a7-133">This implements the `getUserDetails` function, which uses the Microsoft Graph SDK to call the `/me` endpoint and return the result.</span></span>

<span data-ttu-id="682a7-134">更新中`getUserProfile` `./src/App.js`的方法以调用此函数。</span><span class="sxs-lookup"><span data-stu-id="682a7-134">Update the `getUserProfile` method in `./src/App.js` to call this function.</span></span> <span data-ttu-id="682a7-135">首先, 将以下`import`语句添加到文件顶部。</span><span class="sxs-lookup"><span data-stu-id="682a7-135">First, add the following `import` statement to the top of the file.</span></span>

```js
import { getUserDetails } from './GraphService';
```

<span data-ttu-id="682a7-136">将现有的 `getUserProfile` 函数替换成以下代码。</span><span class="sxs-lookup"><span data-stu-id="682a7-136">Replace the existing `getUserProfile` function with the following code.</span></span>

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

<span data-ttu-id="682a7-137">现在, 如果您保存更改并启动应用程序, 登录后应返回到主页, 但 UI 应更改以指示您已登录。</span><span class="sxs-lookup"><span data-stu-id="682a7-137">Now if you save your changes and start the app, after sign-in you should end up back on the home page, but the UI should change to indicate that you are signed-in.</span></span>

![登录后主页的屏幕截图](./images/add-aad-auth-01.png)

<span data-ttu-id="682a7-139">单击右上角的用户头像以访问 "**注销**" 链接。</span><span class="sxs-lookup"><span data-stu-id="682a7-139">Click the user avatar in the top right corner to access the **Sign Out** link.</span></span> <span data-ttu-id="682a7-140">单击 "**注销**" 重置会话并返回到主页。</span><span class="sxs-lookup"><span data-stu-id="682a7-140">Clicking **Sign Out** resets the session and returns you to the home page.</span></span>

![带有 "注销" 链接的下拉菜单的屏幕截图](./images/add-aad-auth-02.png)

## <a name="storing-and-refreshing-tokens"></a><span data-ttu-id="682a7-142">存储和刷新令牌</span><span class="sxs-lookup"><span data-stu-id="682a7-142">Storing and refreshing tokens</span></span>

<span data-ttu-id="682a7-143">此时, 您的应用程序具有访问令牌, 该令牌是在 API `Authorization`调用的标头中发送的。</span><span class="sxs-lookup"><span data-stu-id="682a7-143">At this point your application has an access token, which is sent in the `Authorization` header of API calls.</span></span> <span data-ttu-id="682a7-144">这是允许应用代表用户访问 Microsoft Graph 的令牌。</span><span class="sxs-lookup"><span data-stu-id="682a7-144">This is the token that allows the app to access the Microsoft Graph on the user's behalf.</span></span>

<span data-ttu-id="682a7-145">但是, 此令牌的生存期较短。</span><span class="sxs-lookup"><span data-stu-id="682a7-145">However, this token is short-lived.</span></span> <span data-ttu-id="682a7-146">令牌在发出后会过期一小时。</span><span class="sxs-lookup"><span data-stu-id="682a7-146">The token expires an hour after it is issued.</span></span> <span data-ttu-id="682a7-147">这就是刷新令牌变得有用的地方。</span><span class="sxs-lookup"><span data-stu-id="682a7-147">This is where the refresh token becomes useful.</span></span> <span data-ttu-id="682a7-148">刷新令牌允许应用在不要求用户重新登录的情况下请求新的访问令牌。</span><span class="sxs-lookup"><span data-stu-id="682a7-148">The refresh token allows the app to request a new access token without requiring the user to sign in again.</span></span>

<span data-ttu-id="682a7-149">由于应用程序使用的是 MSAL 库, 因此您无需实现任何令牌存储或刷新逻辑。</span><span class="sxs-lookup"><span data-stu-id="682a7-149">Because the app is using the MSAL library, you do not have to implement any token storage or refresh logic.</span></span> <span data-ttu-id="682a7-150">该`UserAgentApplication`方法在浏览器会话中缓存标记。</span><span class="sxs-lookup"><span data-stu-id="682a7-150">The `UserAgentApplication` method caches the token in the browser session.</span></span> <span data-ttu-id="682a7-151">该`acquireTokenSilent`方法首先检查缓存的标记, 如果它未过期, 它将返回。</span><span class="sxs-lookup"><span data-stu-id="682a7-151">The `acquireTokenSilent` method first checks the cached token, and if it is not expired, it returns it.</span></span> <span data-ttu-id="682a7-152">如果它已过期, 则使用缓存的刷新令牌获取新的。</span><span class="sxs-lookup"><span data-stu-id="682a7-152">If it is expired, it uses the cached refresh token to obtain a new one.</span></span> <span data-ttu-id="682a7-153">您将在以下模块中更多地使用此方法。</span><span class="sxs-lookup"><span data-stu-id="682a7-153">You'll use this method more in the following module.</span></span>
