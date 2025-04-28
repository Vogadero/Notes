#  npm、cnpm、yarn、pnpm、npx、nvm、nrm、nodemon 的区别

> *我个人倾向于大项目使用 yarn，其余的小项目全部使用 npm，我也在尝试着使用 pnpm 了。*

npm、cnpm、yarn、pnpm、npx、nvm、nrm、nodemon 是与 JavaScript 开发相关的不同工具，它们各自具有独特的功能和用途。以下是对这些工具的区别进行的详细解释：

### 1. npm（Node Package Manager）

* **定义**：npm 是 Node.js 的默认包管理器，用于安装、发布和管理 JavaScript 包。它是一个命令行工具，可以在终端中使用。npm 有一个全球的包仓库，可以从中下载和安装各种 JavaScript 包。

* **功能**：管理项目的依赖关系，通过 `package.json` 和 `package-lock.json` 文件来记录这些依赖。

* **特点**：拥有庞大的 JavaScript 包生态，支持丰富的第三方模块。通过命令行工具提供广泛的功能，如版本管理、脚本执行等。

* **命令**：

  - 配置镜像站：原来的 [https://registry.npm.taobao.org](https://link.zhihu.com/?target=http%3A//registry.npm.taobao.org) 已替换为 [https://registry.npmmirror.com](https://link.zhihu.com/?target=http%3A//registry.npmmirror.com) 

    ```shell
    npm config set registry https://registry.npmmirror.com
    ```

  - 查看当前npm的下载源设置：

    ```shell
    npm config get registry
    ```

### 2. cnpm（China npm）

* **定义**：cnpm 是 npm 的一个镜像，旨在为中国用户提供更快的下载速度。
* **功能**：由于 npm 的服务器位于国外，中国用户在使用 npm 时可能会遇到下载速度慢的问题。通过将 npm 的包镜像到国内服务器，解决中国用户在使用 npm 时可能遇到的下载速度慢的问题。
* **特点**：与 npm 完全兼容，但默认使用淘宝的镜像源。

### 3. yarn

* **定义**：yarn 是由 Facebook 开发的另一个 JavaScript 包管理器。与 npm 不同，yarn 具有更快的下载速度和更稳定的依赖管理。它还引入了一些新的功能，如离线模式、并行安装等。yarn 使用与 npm 相同的包仓库，可以直接使用 npm 的包。
* **功能**：旨在解决 npm 的一些性能和可靠性问题，提供更快的下载速度和更稳定的依赖管理。
* **特点**：使用并行下载的机制，更快地安装依赖项。使用锁文件（`yarn.lock`）来确保依赖项的版本一致性。

### 4. pnpm

* **定义**：pnpm 是另一个 JavaScript 包管理器，是一个快速、磁盘空间友好的包管理器。
* **功能**：通过硬链接和符号链接来共享依赖项，从而节省磁盘空间。pnpm 还具有更快的安装速度和更低的网络流量消耗。它也可以使用 npm 的包仓库。
* **特点**：避免了重复的依赖安装，即使在多项目环境下也能保持高效。使用 `pnpm-lock.yaml` 文件来锁定依赖版本，支持并行安装依赖。

### 5. npx

* **定义**：npx 是 npm 5.2.0 版本引入的一个命令行工具。
* **功能**：允许你在不全局安装包的情况下运行命令行工具，是一个 Node 包执行器，该 Node 包可以是本地也可以是远程的。允许开发者在无需安装的情况下执行任意 Node 包。
* **特点**：避免了全局安装带来的版本冲突问题，减轻了对系统全局环境的污染。可以直接运行安装在项目中的依赖包，而不需要手动设置环境变量或全局安装。
* **原理**：
  - 执行本地 Node 包时，NPX 会到node_modules/.bin路径和环境变量 $PATH 里面，检查命令是否存在。
  - 执行远程 Node 包时，NPX 会将 Node 包下载到一个临时目录中，使用以后再删除。

### 6. nvm（Node Version Manager）

* **定义**：nvm 是一个用于管理多个 Node.js 版本的工具。

* **功能**：允许你在同一台机器上安装和切换不同的 Node.js 版本。

* **特点**：帮助开发人员在不同的项目中使用不同的 Node.js 版本，以适应项目的需求。

* **常用命令**：

  - 查看可在线安装的NodeJS版本

    ```shell
    nvm list available
    ```

  - 安装指定版本的 Node.js。

    ```shell
    nvm install <version>
    ```

  - 切换到指定版本的 Node.js。

    ```shell
    nvm use <version>
    ```

  - 列出已安装的所有 Node.js 版本。

    ```shell
    nvm ls 或 nvm list
    ```

  - 显示当前正在使用的 Node.js 版本。

    ```shell
    nvm current
    ```

  - 为指定的版本创建别名。

    ```shell
    nvm alias <name> <version>
    ```

  - 删除指定版本的别名。

    ```shell
    nvm unalias <name>
    ```

  - 卸载指定的 Node.js 版本。

    ```shell
    nvm uninstall <version>
    ```

  - 重新安装指定版本的 Node.js，并将全局包重新安装到新版本中。

    ```shell
    nvm reinstall-packages <version>
    ```

  - 在指定版本的 Node.js 环境下执行特定命令。

    ```shell
    nvm exec <version> <command>
    ```

  - 显示 NVM 的版本信息。

    ```shell
    nvm --version
    ```

### 7. nrm（npm registry manager）

* **定义**：npm的镜像管理工具，有时候国外的资源太慢，使用这个就可以快速地在npm源间切换。

* **功能**：用于快速切换 npm 的镜像源，以便从不同地区的镜像源下载依赖包，提高下载速度。

* **特点**：简化了镜像源切换的过程，使得在不同环境下开发更加便捷。

* **命令**：

  - 安装nrm

    在命令行执行命令，`npm install -g nrm`，全局安装nrm。

  - 使用

    一般来说，在终端直接运行基础命令，可以看到该命令的解析，使用方法。

  - 查看当前源

    执行命令`nrm ls`查看可选的源。其中带*号的是当前使用的源，下图表明当前源为rh。

    或者直接使用`nrm current`命令，也可以查看当前源。


  - 切换

    如果要切换到taobao源，执行命令`nrm use taobao`。

  - 增加

    你可以增加定制的源，特别适用于添加企业内部的私有源，执行命令`nrm add <registry> <url>`，其中registry为源名，url为源的路径。

  - 删除

    执行命令`nrm del <registry>`删除对应的源。

  - 测试速度

    你还可以通过`nrm test <registry>`测试响应源的响应时间。

### 8. nodemon

* **定义**：nodemon 是一个实用工具，用于在开发过程中自动重启 Node.js 应用程序。
* **功能**：当检测到代码更改时，nodemon 会自动重启 Node.js 应用程序，无需手动重启。
* **特点**：提高了开发效率，减少了因手动重启应用程序而浪费的时间。

### 9. 包管理工具指令整理

| 常用操作           | npm                                                    | cnpm                                                     | yarn                                   | pnpm                               |
| ------------------ | ------------------------------------------------------ | -------------------------------------------------------- | -------------------------------------- | ---------------------------------- |
| 安装包             | npm install [package-name]` / `npm i [package-name]    | cnpm install [package-name]` / `cnpm i [package-name]    | yarn add [package-name]                | pnpm add [package-name]            |
| 全局安装包         | `npm install -g [package-name]`                        | `cnpm install -g [package-name]`                         | `yarn global add [package-name]`       | `pnpm add -g [package-name]`       |
| 安装开发依赖       | npm install [package-name] --save-dev                  | cnpm install [package-name] --save-dev                   | yarn add [package-name] --dev          | pnpm add [package-name] --save-dev |
| 卸载包             | npm uninstall [package-name]` / `npm rm [package-name] | cnpm uninstall [package-name]` / `cnpm rm [package-name] | yarn remove [package-name]             | pnpm remove [package-name]         |
| 查看已安装的全局包 | `npm list -g`                                          | `cnpm list -g`                                           | `yarn list -g`                         | `pnpm list -g`                     |
| 查看特定包的版本   | `npm list -g [package-name]`                           | `cnpm list -g [package-name]`                            | `yarn list -g [package-name]`          | `pnpm list -g [package-name]`      |
| 更新包             | `npm update -g [package-name]`                         | `cnpm update -g [package-name]`                          | `yarn upgrade -g [package-name]`       | `pnpm update -g [package-name]`    |
| 初始化项目         | `npm init`                                             | `cnpm init`                                              | `yarn init`                            | `pnpm init`                        |
| 查看可更新的包     | `npm -g outdated`                                      | `cnpm -g outdated`                                       | `yarn upgrade-interactive --latest -g` | `pnpm -g outdated`                 |



### 总结

这些工具虽然都有包管理的核心功能，但在速度、依赖管理策略、缓存机制、以及特定场景的优化方面各有特色。开发者可以根据项目需求和个人偏好选择合适的工具。例如，对于需要快速下载和稳定依赖管理的项目，yarn 可能是一个不错的选择；而对于需要管理多个 Node.js 版本的项目，nvm 则非常有用。cnpm 主要解决了在中国地区使用 npm 时下载速度慢的问题；pnpm 则通过共享依赖来节省磁盘空间和提高安装速度；npx 则为开发者提供了一种方便的方式来运行项目中的依赖包命令，而无需全局安装。