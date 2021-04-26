<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="0d49d-101">本教程指导你如何构建一React单页应用，该应用使用 Microsoft Graph API 检索用户的日历信息。</span><span class="sxs-lookup"><span data-stu-id="0d49d-101">This tutorial teaches you how to build a React single-page app that uses the Microsoft Graph API to retrieve calendar information for a user.</span></span>

> [!TIP]
> <span data-ttu-id="0d49d-102">如果只想下载已完成的教程，可以下载或克隆GitHub[存储库](https://github.com/microsoftgraph/msgraph-training-reactspa)。</span><span class="sxs-lookup"><span data-stu-id="0d49d-102">If you prefer to just download the completed tutorial, you can download or clone the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-reactspa).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0d49d-103">必备条件</span><span class="sxs-lookup"><span data-stu-id="0d49d-103">Prerequisites</span></span>

<span data-ttu-id="0d49d-104">在开始本教程之前，应该在[开发计算机上安装Node.js](https://nodejs.org)[和一](https://classic.yarnpkg.com/)线线。</span><span class="sxs-lookup"><span data-stu-id="0d49d-104">Before you start this tutorial, you should have [Node.js](https://nodejs.org) and [Yarn](https://classic.yarnpkg.com/) installed on your development machine.</span></span> <span data-ttu-id="0d49d-105">如果没有设置或Node.js，请访问前面的链接，查看下载选项。</span><span class="sxs-lookup"><span data-stu-id="0d49d-105">If you do not have Node.js or Yarn, visit the previous links for download options.</span></span>

<span data-ttu-id="0d49d-106">您还应该有一个在 Outlook.com 上拥有邮箱的个人 Microsoft 帐户，或者一个 Microsoft 工作或学校帐户。</span><span class="sxs-lookup"><span data-stu-id="0d49d-106">You should also have either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span> <span data-ttu-id="0d49d-107">如果你没有 Microsoft 帐户，则有几个选项可以获取免费帐户：</span><span class="sxs-lookup"><span data-stu-id="0d49d-107">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="0d49d-108">你可以 [注册新的个人 Microsoft 帐户](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)。</span><span class="sxs-lookup"><span data-stu-id="0d49d-108">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="0d49d-109">你可以[注册开发人员计划Office 365](https://developer.microsoft.com/office/dev-program)免费订阅Office 365订阅。</span><span class="sxs-lookup"><span data-stu-id="0d49d-109">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="0d49d-110">本教程使用 Node 版本 14.15.0 和一线 1.22.10 编写。</span><span class="sxs-lookup"><span data-stu-id="0d49d-110">This tutorial was written with Node version 14.15.0 and Yarn version 1.22.10.</span></span> <span data-ttu-id="0d49d-111">本指南中的步骤可能与其他版本一起运行，但尚未经过测试。</span><span class="sxs-lookup"><span data-stu-id="0d49d-111">The steps in this guide may work with other versions, but that has not been tested.</span></span>

## <a name="feedback"></a><span data-ttu-id="0d49d-112">反馈</span><span class="sxs-lookup"><span data-stu-id="0d49d-112">Feedback</span></span>

<span data-ttu-id="0d49d-113">Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-reactspa).</span><span class="sxs-lookup"><span data-stu-id="0d49d-113">Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-reactspa).</span></span>
