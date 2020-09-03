<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="7e359-101">在本练习中，你将使用 Azure Active Directory 管理中心创建新的 Azure AD web 应用程序注册。</span><span class="sxs-lookup"><span data-stu-id="7e359-101">In this exercise, you will create a new Azure AD web application registration using the Azure Active Directory admin center.</span></span>

1. <span data-ttu-id="7e359-102">打开浏览器，并转到 [Azure Active Directory 管理中心](https://aad.portal.azure.com)。</span><span class="sxs-lookup"><span data-stu-id="7e359-102">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="7e359-103">使用**个人帐户**（亦称为“Microsoft 帐户”）或**工作或学校帐户**登录。</span><span class="sxs-lookup"><span data-stu-id="7e359-103">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="7e359-104">选择左侧导航栏中的“**Azure Active Directory**”，再选择“**管理**”下的“**应用注册**”。</span><span class="sxs-lookup"><span data-stu-id="7e359-104">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="7e359-105">应用注册的屏幕截图</span><span class="sxs-lookup"><span data-stu-id="7e359-105">A screenshot of the App registrations</span></span> ](./images/aad-portal-app-registrations.png)

    > [!NOTE]
    > <span data-ttu-id="7e359-106">Azure AD B2C 用户只能查看 \*\* (旧版) 的应用注册 \*\*。</span><span class="sxs-lookup"><span data-stu-id="7e359-106">Azure AD B2C users may only see **App registrations (legacy)**.</span></span> <span data-ttu-id="7e359-107">在这种情况下，请直接转到 [https://aka.ms/appregistrations](https://aka.ms/appregistrations) 。</span><span class="sxs-lookup"><span data-stu-id="7e359-107">In this case, please go directly to [https://aka.ms/appregistrations](https://aka.ms/appregistrations).</span></span>

1. <span data-ttu-id="7e359-108">选择“新注册”\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="7e359-108">Select **New registration**.</span></span> <span data-ttu-id="7e359-109">在“注册应用”\*\*\*\* 页上，按如下方式设置值。</span><span class="sxs-lookup"><span data-stu-id="7e359-109">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="7e359-110">将“名称”\*\*\*\* 设置为“`React Graph Tutorial`”。</span><span class="sxs-lookup"><span data-stu-id="7e359-110">Set **Name** to `React Graph Tutorial`.</span></span>
    - <span data-ttu-id="7e359-111">将“受支持的帐户类型”\*\*\*\* 设置为“任何组织目录中的帐户和个人 Microsoft 帐户”\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="7e359-111">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="7e359-112">在“重定向 URI”\*\*\*\* 下，将第一个下拉列表设置为“`Single-page application (SPA)`”，并将值设置为“`http://localhost:3000`”。</span><span class="sxs-lookup"><span data-stu-id="7e359-112">Under **Redirect URI**, set the first drop-down to `Single-page application (SPA)` and set the value to `http://localhost:3000`.</span></span>

    !["注册应用程序" 页的屏幕截图](./images/aad-register-an-app.png)

1. <span data-ttu-id="7e359-114">选择 **“注册”**。</span><span class="sxs-lookup"><span data-stu-id="7e359-114">Choose **Register**.</span></span> <span data-ttu-id="7e359-115">在 " **响应图形教程** " 页上，将应用程序的值复制 \*\* (客户端) ID\*\* 并保存它，在下一步中将需要它。</span><span class="sxs-lookup"><span data-stu-id="7e359-115">On the **React Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![新应用注册的应用程序 ID 的屏幕截图](./images/aad-application-id.png)
