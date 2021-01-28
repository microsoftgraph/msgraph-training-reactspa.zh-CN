# <a name="getting-started-with-create-react-app"></a><span data-ttu-id="0edd2-101">创建 React 应用入门</span><span class="sxs-lookup"><span data-stu-id="0edd2-101">Getting Started with Create React App</span></span>

<span data-ttu-id="0edd2-102">此项目已启动创建 React [应用](https://github.com/facebook/create-react-app)。</span><span class="sxs-lookup"><span data-stu-id="0edd2-102">This project was bootstrapped with [Create React App](https://github.com/facebook/create-react-app).</span></span>

## <a name="available-scripts"></a><span data-ttu-id="0edd2-103">可用脚本</span><span class="sxs-lookup"><span data-stu-id="0edd2-103">Available Scripts</span></span>

<span data-ttu-id="0edd2-104">在项目目录中，可以运行：</span><span class="sxs-lookup"><span data-stu-id="0edd2-104">In the project directory, you can run:</span></span>

### `yarn start`

<span data-ttu-id="0edd2-105">在开发模式下运行应用。</span><span class="sxs-lookup"><span data-stu-id="0edd2-105">Runs the app in the development mode.</span></span>\
<span data-ttu-id="0edd2-106">打开 [http://localhost:3000](http://localhost:3000) 以在浏览器中查看。</span><span class="sxs-lookup"><span data-stu-id="0edd2-106">Open [http://localhost:3000](http://localhost:3000) to view it in the browser.</span></span>

<span data-ttu-id="0edd2-107">如果进行编辑，页面将重新加载。</span><span class="sxs-lookup"><span data-stu-id="0edd2-107">The page will reload if you make edits.</span></span>\
<span data-ttu-id="0edd2-108">你还将在控制台中看到任何 lint 错误。</span><span class="sxs-lookup"><span data-stu-id="0edd2-108">You will also see any lint errors in the console.</span></span>

### `yarn test`

<span data-ttu-id="0edd2-109">在交互式监视模式下启动测试运行程序。</span><span class="sxs-lookup"><span data-stu-id="0edd2-109">Launches the test runner in the interactive watch mode.</span></span>\
<span data-ttu-id="0edd2-110">有关详细信息，请参阅有关 [运行测试](https://facebook.github.io/create-react-app/docs/running-tests) 的部分。</span><span class="sxs-lookup"><span data-stu-id="0edd2-110">See the section about [running tests](https://facebook.github.io/create-react-app/docs/running-tests) for more information.</span></span>

### `yarn build`

<span data-ttu-id="0edd2-111">将生产应用生成到 `build` 文件夹。</span><span class="sxs-lookup"><span data-stu-id="0edd2-111">Builds the app for production to the `build` folder.</span></span>\
<span data-ttu-id="0edd2-112">它在生产模式下正确捆绑 React，并优化内部版本以获得最佳性能。</span><span class="sxs-lookup"><span data-stu-id="0edd2-112">It correctly bundles React in production mode and optimizes the build for the best performance.</span></span>

<span data-ttu-id="0edd2-113">生成缩小，文件名包括哈希。</span><span class="sxs-lookup"><span data-stu-id="0edd2-113">The build is minified and the filenames include the hashes.</span></span>\
<span data-ttu-id="0edd2-114">你的应用已准备好部署！</span><span class="sxs-lookup"><span data-stu-id="0edd2-114">Your app is ready to be deployed!</span></span>

<span data-ttu-id="0edd2-115">有关详细信息，请参阅 [有关部署](https://facebook.github.io/create-react-app/docs/deployment) 的部分。</span><span class="sxs-lookup"><span data-stu-id="0edd2-115">See the section about [deployment](https://facebook.github.io/create-react-app/docs/deployment) for more information.</span></span>

### `yarn eject`

<span data-ttu-id="0edd2-116">**注意：这是单向操作。一 `eject` 旦完成，你无法返回！**</span><span class="sxs-lookup"><span data-stu-id="0edd2-116">**Note: this is a one-way operation. Once you `eject`, you can’t go back!**</span></span>

<span data-ttu-id="0edd2-117">如果你对生成工具和配置选择不满意，你随时 `eject` 都可以。</span><span class="sxs-lookup"><span data-stu-id="0edd2-117">If you aren’t satisfied with the build tool and configuration choices, you can `eject` at any time.</span></span> <span data-ttu-id="0edd2-118">此命令将删除项目中的单一生成依赖项。</span><span class="sxs-lookup"><span data-stu-id="0edd2-118">This command will remove the single build dependency from your project.</span></span>

<span data-ttu-id="0edd2-119">相反，它会将 webpack、 (、ESLint 等) 配置文件和可传递依赖项复制到你的项目中，以便你可以完全控制它们。</span><span class="sxs-lookup"><span data-stu-id="0edd2-119">Instead, it will copy all the configuration files and the transitive dependencies (webpack, Babel, ESLint, etc) right into your project so you have full control over them.</span></span> <span data-ttu-id="0edd2-120">除了这些命令之外，所有命令仍可以正常工作，但它们将指向复制 `eject` 的脚本，以便你可以调整它们。</span><span class="sxs-lookup"><span data-stu-id="0edd2-120">All of the commands except `eject` will still work, but they will point to the copied scripts so you can tweak them.</span></span> <span data-ttu-id="0edd2-121">此时，你已自己了。</span><span class="sxs-lookup"><span data-stu-id="0edd2-121">At this point you’re on your own.</span></span>

<span data-ttu-id="0edd2-122">你不必再使用 `eject` 。</span><span class="sxs-lookup"><span data-stu-id="0edd2-122">You don’t have to ever use `eject`.</span></span> <span data-ttu-id="0edd2-123">特展功能集适用于小型和中间部署，你不应觉得必须使用此功能。</span><span class="sxs-lookup"><span data-stu-id="0edd2-123">The curated feature set is suitable for small and middle deployments, and you shouldn’t feel obligated to use this feature.</span></span> <span data-ttu-id="0edd2-124">但是，我们知道，如果你无法自定义它（当你准备好它时）此工具将没有用。</span><span class="sxs-lookup"><span data-stu-id="0edd2-124">However we understand that this tool wouldn’t be useful if you couldn’t customize it when you are ready for it.</span></span>

## <a name="learn-more"></a><span data-ttu-id="0edd2-125">了解详细信息</span><span class="sxs-lookup"><span data-stu-id="0edd2-125">Learn More</span></span>

<span data-ttu-id="0edd2-126">可以在 Create React [App 文档中了解更多信息](https://facebook.github.io/create-react-app/docs/getting-started)。</span><span class="sxs-lookup"><span data-stu-id="0edd2-126">You can learn more in the [Create React App documentation](https://facebook.github.io/create-react-app/docs/getting-started).</span></span>

<span data-ttu-id="0edd2-127">若要了解 React，请查看 [React 文档](https://reactjs.org/)。</span><span class="sxs-lookup"><span data-stu-id="0edd2-127">To learn React, check out the [React documentation](https://reactjs.org/).</span></span>
