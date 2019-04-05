# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="eed6d-101">如何运行已完成的项目</span><span class="sxs-lookup"><span data-stu-id="eed6d-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="eed6d-102">先决条件</span><span class="sxs-lookup"><span data-stu-id="eed6d-102">Prerequisites</span></span>

<span data-ttu-id="eed6d-103">若要在此文件夹中运行已完成的项目, 您需要以下各项:</span><span class="sxs-lookup"><span data-stu-id="eed6d-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="eed6d-104">在开发计算机上安装的[Visual Studio](https://visualstudio.microsoft.com/vs/) 。</span><span class="sxs-lookup"><span data-stu-id="eed6d-104">[Visual Studio](https://visualstudio.microsoft.com/vs/) installed on your development machine.</span></span> <span data-ttu-id="eed6d-105">如果没有 Visual Studio, 请访问 "下载选项" 的上一个链接。</span><span class="sxs-lookup"><span data-stu-id="eed6d-105">If you do not have Visual Studio, visit the previous link for download options.</span></span> <span data-ttu-id="eed6d-106">(**注意:** 本教程是使用 Visual Studio 2017 版本15.81 编写的。</span><span class="sxs-lookup"><span data-stu-id="eed6d-106">(**Note:** This tutorial was written with Visual Studio 2017 version 15.81.</span></span> <span data-ttu-id="eed6d-107">本指南中的步骤可能适用于其他版本, 但尚未经过测试。</span><span class="sxs-lookup"><span data-stu-id="eed6d-107">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="eed6d-108">使用 Outlook.com 上的邮箱的个人 Microsoft 帐户, 或者是 microsoft 工作或学校帐户。</span><span class="sxs-lookup"><span data-stu-id="eed6d-108">Either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span>

<span data-ttu-id="eed6d-109">如果你没有 Microsoft 帐户, 可以使用以下几种方法获取免费帐户:</span><span class="sxs-lookup"><span data-stu-id="eed6d-109">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="eed6d-110">你可以[注册新的个人 Microsoft 帐户](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)。</span><span class="sxs-lookup"><span data-stu-id="eed6d-110">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="eed6d-111">你可以[注册 office 365 开发人员计划](https://developer.microsoft.com/office/dev-program)以获取免费的 office 365 订阅。</span><span class="sxs-lookup"><span data-stu-id="eed6d-111">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a><span data-ttu-id="eed6d-112">向 Azure Active Directory 管理中心注册 web 应用程序</span><span class="sxs-lookup"><span data-stu-id="eed6d-112">Register a web application with the Azure Active Directory admin center</span></span>

1. <span data-ttu-id="eed6d-113">打开浏览器并导航到[Azure Active Directory 管理中心](https://aad.portal.azure.com)。</span><span class="sxs-lookup"><span data-stu-id="eed6d-113">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="eed6d-114">使用**个人帐户**(亦称: Microsoft 帐户) 或**工作或学校帐户**登录。</span><span class="sxs-lookup"><span data-stu-id="eed6d-114">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="eed6d-115">在左侧导航栏中选择 " **Azure Active Directory** ", 然后选择 "**管理**" 下的 "**应用注册 (预览)** "。</span><span class="sxs-lookup"><span data-stu-id="eed6d-115">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations (Preview)** under **Manage**.</span></span>

    ![<span data-ttu-id="eed6d-116">应用注册的屏幕截图</span><span class="sxs-lookup"><span data-stu-id="eed6d-116">A screenshot of the App registrations</span></span> ](/tutorial/images/aad-portal-app-registrations.png)

1. <span data-ttu-id="eed6d-117">选择 "**新建注册**"。</span><span class="sxs-lookup"><span data-stu-id="eed6d-117">Select **New registration**.</span></span> <span data-ttu-id="eed6d-118">在 "**注册应用程序**" 页上, 按如下所示设置值。</span><span class="sxs-lookup"><span data-stu-id="eed6d-118">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="eed6d-119">将**名称**设置`React Graph Tutorial`为。</span><span class="sxs-lookup"><span data-stu-id="eed6d-119">Set **Name** to `React Graph Tutorial`.</span></span>
    - <span data-ttu-id="eed6d-120">将**支持的帐户类型**设置为**任何组织目录和个人 Microsoft 帐户中的帐户**。</span><span class="sxs-lookup"><span data-stu-id="eed6d-120">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="eed6d-121">在 "**重定向 URI**" 下, 将第一个`Web`下拉下拉箭头, 并`http://localhost:3000`将值设置为。</span><span class="sxs-lookup"><span data-stu-id="eed6d-121">Under **Redirect URI**, set the first drop-down to `Web` and set the value to `http://localhost:3000`.</span></span>

    !["注册应用程序" 页的屏幕截图](/tutorial/images/aad-register-an-app.png)

1. <span data-ttu-id="eed6d-123">选择 "**注册**"。</span><span class="sxs-lookup"><span data-stu-id="eed6d-123">Choose **Register**.</span></span> <span data-ttu-id="eed6d-124">在 "**角度图" 教程**页上, 复制**应用程序 (客户端) ID**的值并保存它, 下一步将需要它。</span><span class="sxs-lookup"><span data-stu-id="eed6d-124">On the **Angular Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![新应用注册的应用程序 ID 的屏幕截图](/tutorial/images/aad-application-id.png)

1. <span data-ttu-id="eed6d-126">选择 "**管理**" 下的 "**身份验证**"。</span><span class="sxs-lookup"><span data-stu-id="eed6d-126">Select **Authentication** under **Manage**.</span></span> <span data-ttu-id="eed6d-127">找到 "**隐式授予**" 部分并启用**访问令牌**和**ID 令牌**。</span><span class="sxs-lookup"><span data-stu-id="eed6d-127">Locate the **Implicit grant** section and enable **Access tokens** and **ID tokens**.</span></span> <span data-ttu-id="eed6d-128">选择“保存”\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="eed6d-128">Choose **Save**.</span></span>

    ![隐式 grant 部分的屏幕截图](/tutorial/images/aad-implicit-grant.png)

## <a name="configure-the-sample"></a><span data-ttu-id="eed6d-130">配置示例</span><span class="sxs-lookup"><span data-stu-id="eed6d-130">Configure the sample</span></span>

1. <span data-ttu-id="eed6d-131">将`./graph-tutorial/src/Config.js.example`文件重命名`./graph-tutorial/src/Config.js`为。</span><span class="sxs-lookup"><span data-stu-id="eed6d-131">Rename the `./graph-tutorial/src/Config.js.example` file to `./graph-tutorial/src/Config.js`.</span></span>
1. <span data-ttu-id="eed6d-132">编辑`./graph-tutorial/src/Config.js`文件并进行以下更改。</span><span class="sxs-lookup"><span data-stu-id="eed6d-132">Edit the `./graph-tutorial/src/Config.js` file and make the following changes.</span></span>
    1. <span data-ttu-id="eed6d-133">将`YOUR_APP_ID_HERE`替换为你从应用注册门户获取的**应用程序 Id** 。</span><span class="sxs-lookup"><span data-stu-id="eed6d-133">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>
1. <span data-ttu-id="eed6d-134">在命令行界面 (CLI) 中, 导航到`graph-tutorial`目录并运行以下命令以安装要求。</span><span class="sxs-lookup"><span data-stu-id="eed6d-134">In your command-line interface (CLI), navigate to the `graph-tutorial` directory and run the following command to install requirements.</span></span>

    ```Shell
    npm install
    ```

## <a name="run-the-sample"></a><span data-ttu-id="eed6d-135">运行示例</span><span class="sxs-lookup"><span data-stu-id="eed6d-135">Run the sample</span></span>

1. <span data-ttu-id="eed6d-136">在 CLI 中运行以下命令以启动应用程序。</span><span class="sxs-lookup"><span data-stu-id="eed6d-136">Run the following command in your CLI to start the application.</span></span>

    ```Shell
    npm start
    ```

1. <span data-ttu-id="eed6d-137">打开浏览器，然后转到 `http://localhost:3000`。</span><span class="sxs-lookup"><span data-stu-id="eed6d-137">Open a browser and browse to `http://localhost:3000`.</span></span>