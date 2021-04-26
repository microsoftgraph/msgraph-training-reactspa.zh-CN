<!-- markdownlint-disable MD002 MD041 -->

本教程指导你如何构建一React单页应用，该应用使用 Microsoft Graph API 检索用户的日历信息。

> [!TIP]
> 如果只想下载已完成的教程，可以下载或克隆GitHub[存储库](https://github.com/microsoftgraph/msgraph-training-reactspa)。

## <a name="prerequisites"></a>必备条件

在开始本教程之前，应该在[开发计算机上安装Node.js](https://nodejs.org)[和一](https://classic.yarnpkg.com/)线线。 如果没有设置或Node.js，请访问前面的链接，查看下载选项。

您还应该有一个在 Outlook.com 上拥有邮箱的个人 Microsoft 帐户，或者一个 Microsoft 工作或学校帐户。 如果你没有 Microsoft 帐户，则有几个选项可以获取免费帐户：

- 你可以 [注册新的个人 Microsoft 帐户](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)。
- 你可以[注册开发人员计划Office 365](https://developer.microsoft.com/office/dev-program)免费订阅Office 365订阅。

> [!NOTE]
> 本教程使用 Node 版本 14.15.0 和一线 1.22.10 编写。 本指南中的步骤可能与其他版本一起运行，但尚未经过测试。

## <a name="feedback"></a>反馈

Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-reactspa).
