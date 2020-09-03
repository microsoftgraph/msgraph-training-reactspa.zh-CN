# <a name="how-to-run-the-completed-project"></a>如何运行已完成的项目

## <a name="prerequisites"></a>先决条件

若要在此文件夹中运行已完成的项目，您需要以下各项：

- 在开发计算机上安装[Node.js](https://nodejs.org) 。 如果您没有 Node.js，请访问 "下载选项" 的上一个链接。  (**注意：** 本教程是使用节点版本12.16.1 编写的。 本指南中的步骤可能适用于其他版本，但尚未经过测试。 ) 
- 使用 Outlook.com 上的邮箱的个人 Microsoft 帐户，或者是 Microsoft 工作或学校帐户。

如果你没有 Microsoft 帐户，可以使用以下几种方法获取免费帐户：

- 你可以 [注册新的个人 Microsoft 帐户](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)。
- 你可以 [注册 office 365 开发人员计划](https://developer.microsoft.com/office/dev-program) 以获取免费的 office 365 订阅。

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a>向 Azure Active Directory 管理中心注册 web 应用程序

1. 打开浏览器，并转到 [Azure Active Directory 管理中心](https://aad.portal.azure.com)。 使用**个人帐户**（亦称为“Microsoft 帐户”）或**工作或学校帐户**登录。

1. 选择左侧导航栏中的“**Azure Active Directory**”，再选择“**管理**”下的“**应用注册**”。

    ![应用注册的屏幕截图 ](/tutorial/images/aad-portal-app-registrations.png)

    > **注意：** Azure AD B2C 用户只能查看 ** (旧版) 的应用注册 **。 在这种情况下，请直接转到 [https://aka.ms/appregistrations](https://aka.ms/appregistrations) 。

1. 选择“新注册”****。 在“注册应用”**** 页上，按如下方式设置值。

    - 将“名称”**** 设置为“`React Graph Tutorial`”。
    - 将“受支持的帐户类型”**** 设置为“任何组织目录中的帐户和个人 Microsoft 帐户”****。
    - 在“重定向 URI”**** 下，将第一个下拉列表设置为“`Single-page application (SPA)`”，并将值设置为“`http://localhost:3000`”。

    !["注册应用程序" 页的屏幕截图](/tutorial/images/aad-register-an-app.png)

1. 选择 **“注册”**。 在 " **角度图" 教程** 页面上，将应用程序的值复制 ** (客户端) ID** 并保存它，在下一步中将需要它。

    ![新应用注册的应用程序 ID 的屏幕截图](/tutorial/images/aad-application-id.png)

## <a name="configure-the-sample"></a>配置示例

1. `./graph-tutorial/src/Config.example.ts`将文件重命名为 `./graph-tutorial/src/Config.ts` 。
1. 编辑 `./graph-tutorial/src/Config.ts` 文件并进行以下更改。
    1. `YOUR_APP_ID_HERE`将替换为你从应用注册门户获取的**应用程序 Id** 。
1. 在命令行界面中 (CLI) ，导航到 `graph-tutorial` 目录并运行以下命令以安装要求。

    ```Shell
    npm install
    ```

## <a name="run-the-sample"></a>运行示例

1. 在 CLI 中运行以下命令以启动应用程序。

    ```Shell
    npm start
    ```

1. 打开浏览器，然后转到 `http://localhost:3000`。
