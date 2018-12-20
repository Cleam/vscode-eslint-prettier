# VsCode 开发环境配置规范

VsCode 前端代码配置规范

## 目录

- [一、下载安装 vscode 和插件](#一、下载安装-vscode-和插件)
- [二、配置 vscode](#二、配置-vscode)
- [三、配置 editorconfig](#三、配置-editorconfig)
- [四、项目配置](#四、项目配置)
  - [自定义项目配置](#自定义项目配置)
  - [vue 项目配置](#vue-项目配置)
  - [react 项目配置](#react-项目配置)
- [五、代码提交前自动格式化配置](#五、代码提交前自动格式化配置)
- [六、问题反馈](#六、问题反馈)
- [七、主题插件推荐](#七、主题插件推荐)

## 一、下载安装 vscode 和插件

[点击下载 vscode](https://code.visualstudio.com/)

安装插件

- [eslint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)：js 代码检查工具
- [prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)：代码格式化工具
- [editorconfig](https://marketplace.visualstudio.com/items?itemName=EditorConfig.EditorConfig)：帮助开发人员在不同的编辑器和 IDE 之间定义和维护一致的编码样式
- [stylelint](https://marketplace.visualstudio.com/items?itemName=shinnn.stylelint)：css 代码检查工具

## 二、配置 vscode

`code -> 首选项 -> 设置 -> 用户设置` 添加以下配置项

```js
{
  // 使用2个空格代替一个tab操作（必选）
  "editor.tabSize": 2,
  // js中使用单引号（必选）
  "prettier.singleQuote": true,
  // 配置打印宽度为120，超过120代码自动换行。（必选）
  "prettier.printWidth": 120,
  // 保存时自动修复（必选）
  "eslint.autoFixOnSave": true,
  // 添加对vue文件的检查（开发vue项目必选）
  "eslint.validate": ["javascript", "javascriptreact", { "autoFix": true, "language": "vue" }],

  // 保存自动格式化（可选，如果不喜欢工具自动格式化，可以用手动使用快捷键格式化之后再手动保存）
  "editor.formatOnSave": true,
  // 排除js文件的自动格式化（避免与eslint自动修复冲突）（与上一配置配合使用）
  "[javascript]": {
    "editor.formatOnSave": false
  },
  // markdownlint配置（可选）
  "markdownlint.config": {
    "MD029": false  // fix this: https://github.com/DavidAnson/markdownlint/issues/45
  }
}
```

## 三、配置 editorconfig

项目根目录添加`.editorconfig`文件，内容如下：

```bash
# https://editorconfig.org
root = true

# 格式规范，请直接复制使用，勿修改。
[*]
charset = utf-8
end_of_line = lf
indent_size = 2
indent_style = space
insert_final_newline = true
max_line_length = 120
tab_width = 2
trim_trailing_whitespace = true
```

## 四、项目配置

### 自定义项目配置

> 完整示例项目地址：[https://github.com/Cleam/vscode-eslint-prettier-demo](https://github.com/Cleam/vscode-eslint-prettier-demo)

- 安装 npm 包

`eslint`、`babel-eslint`、`prettier`、`eslint-config-prettier`、`eslint-plugin-prettier`

```bash
# 生成package.json
npm init -y

# 安装依赖
npm i -D eslint babel-eslint eslint-config-prettier eslint-plugin-prettier
npm i -D -E prettier
```

- 配置 prettier（.prettierrc.js）

```javascript
module.exports = {
  singleQuote: true,
  printWidth: 120,
  tabWidth: 2,
  semi: true
};
```

- 配置 eslint（.eslintrc.js）

```javascript
module.exports = {
  root: true,
  // 环境配置（根据实际情况做取舍）
  env: {
    browser: true,
    node: true,
    commonjs: true,
    es6: true,
    jquery: true
  },
  // 添加项目使用到的全局变量，按需添加。
  globals: {
    SLAPP: true,
    SL: true
  },
  // 继承eslint的默认配置和prettier的配置。
  extends: ['eslint:recommended', 'plugin:prettier/recommended'],
  // 支持babel的使用
  parser: 'babel-eslint',
  parserOptions: {
    // 支持es6模块语法
    sourceType: 'module'
  },
  // 自定义规则（优先级最高）
  rules: {
    'prettier/prettier': [
      'error',
      {
        singleQuote: true,
        printWidth: 120,
        tabWidth: 2,
        semi: true
      }
    ],
    'no-console': 'off'
  }
};
```

- 配置 eslint 检查忽略文件（.eslintignore）

```bash
# eslint默认不会检查node_modules里面的代码，所以这里添加不检查打包后dist下的代码和src下已经压缩的库文件代码
/dist
/src/**/*.min.js
```

- 配置 gitignore

项目根目录添加`.gitignore`文件，内容如下：

```bash
# Created by https://www.gitignore.io/api/node,macos,sublimetext,visualstudiocode
# Edit at https://www.gitignore.io/?templates=node,macos,sublimetext,visualstudiocode

### macOS ###
# General
.DS_Store
.AppleDouble
.LSOverride

# Icon must end with two \r
Icon

# Thumbnails
._*

# Files that might appear in the root of a volume
.DocumentRevisions-V100
.fseventsd
.Spotlight-V100
.TemporaryItems
.Trashes
.VolumeIcon.icns
.com.apple.timemachine.donotpresent

# Directories potentially created on remote AFP share
.AppleDB
.AppleDesktop
Network Trash Folder
Temporary Items
.apdisk

### Node ###
# Logs
logs
*.log
npm-debug.log*
yarn-debug.log*
yarn-error.log*

# Runtime data
pids
*.pid
*.seed
*.pid.lock

# Directory for instrumented libs generated by jscoverage/JSCover
lib-cov

# Coverage directory used by tools like istanbul
coverage

# nyc test coverage
.nyc_output

# Grunt intermediate storage (https://gruntjs.com/creating-plugins#storing-task-files)
.grunt

# Bower dependency directory (https://bower.io/)
bower_components

# node-waf configuration
.lock-wscript

# Compiled binary addons (https://nodejs.org/api/addons.html)
build/Release

# Dependency directories
node_modules/
jspm_packages/

# TypeScript v1 declaration files
typings/

# Optional npm cache directory
.npm

# Optional eslint cache
.eslintcache

# Optional REPL history
.node_repl_history

# Output of 'npm pack'
*.tgz

# Yarn Integrity file
.yarn-integrity

# dotenv environment variables file
.env

# parcel-bundler cache (https://parceljs.org/)
.cache

# next.js build output
.next

# nuxt.js build output
.nuxt

# vuepress build output
.vuepress/dist

# Serverless directories
.serverless/

# FuseBox cache
.fusebox/

#DynamoDB Local files
.dynamodb/

### SublimeText ###
# Cache files for Sublime Text
*.tmlanguage.cache
*.tmPreferences.cache
*.stTheme.cache

# Workspace files are user-specific
*.sublime-workspace

# Project files should be checked into the repository, unless a significant
# proportion of contributors will probably not be using Sublime Text
# *.sublime-project

# SFTP configuration file
sftp-config.json

# Package control specific files
Package Control.last-run
Package Control.ca-list
Package Control.ca-bundle
Package Control.system-ca-bundle
Package Control.cache/
Package Control.ca-certs/
Package Control.merged-ca-bundle
Package Control.user-ca-bundle
oscrypto-ca-bundle.crt
bh_unicode_properties.cache

# Sublime-github package stores a github token in this file
# https://packagecontrol.io/packages/sublime-github
GitHub.sublime-settings

### VisualStudioCode ###
.vscode/*
!.vscode/settings.json
!.vscode/tasks.json
!.vscode/launch.json
!.vscode/extensions.json

### VisualStudioCode Patch ###
# Ignore all local history of files
.history

# End of https://www.gitignore.io/api/node,macos,sublimetext,visualstudiocode
```

### vue 项目配置

1. 安装依赖包

```bash
npm i -D eslint babel-eslint eslint-config-prettier eslint-plugin-prettier
npm i -D -E prettier
```

2. 项目根目录添加文件`.prettierrc.js`

```javascript
module.exports = {
  singleQuote: true,
  printWidth: 120,
  tabWidth: 2,
  semi: true
};
```

3. 配置 eslint

- 如果是使用 [Vue CLI 3](https://cli.vuejs.org/) 创建的项目，修改`package.json`中的配置项`eslintConfig`

```js
{
  // ...
  "eslintConfig": {
    "root": true,
    "env": {
      "node": true,
      "browser": true
    },
    "extends": ["eslint:recommended", "plugin:vue/essential", "plugin:prettier/recommended"],
    "rules": {
      "prettier/prettier": [
        "error",
        {
          "singleQuote": true,
          "printWidth": 120,
          "tabWidth": 2,
          "semi": true
        }
      ],
      "no-console": "off"
    },
    "parserOptions": {
      "parser": "babel-eslint"
    }
  }
  // ...
}
```

- 如果使用 [Vue CLI 2](https://github.com/vuejs/vue-cli/tree/v2#vue-cli--) 创建的项目，修改`.eslintrc.js`

```javascript
module.exports = {
  root: true,
  parserOptions: {
    parser: 'babel-eslint'
  },
  env: {
    node: true,
    browser: true
  },
  extends: ['eslint:recommended', 'plugin:vue/essential', 'plugin:prettier/recommended'],
  plugins: ['vue'],
  rules: {
    'prettier/prettier': [
      'error',
      {
        singleQuote: true,
        printWidth: 120,
        tabWidth: 2,
        semi: true
      }
    ],
    'no-console': 'off'
  }
};
```

> 点击查看示例：[Vue CLI 3](https://github.com/Cleam/vscode-eslint-prettier-demo/tree/vue)、[Vue CLI 2](https://github.com/Cleam/vscode-eslint-prettier-demo/tree/vue2)

### react 项目配置

如果是使用[create-react-app](https://github.com/facebook/create-react-app)创建的项目，做如下调整：

1. 安装依赖包

```bash
# 使用npm
npm i -D eslint-config-prettier eslint-plugin-prettier
npm i -D -E prettier
# 使用yarn
yarn add -D eslint-config-prettier eslint-plugin-prettier
yarn add -D -E prettier
```

2. 项目根目录添加文件`.prettierrc.js`

```javascript
module.exports = {
  singleQuote: true,
  printWidth: 120,
  tabWidth: 2,
  semi: true
};
```

3. 修改`package.json`中的配置项`eslintConfig`

```js
{
  // ...
  "eslintConfig": {
    "extends": ["eslint:recommended", "react-app", "plugin:prettier/recommended"],
    "rules": {
      "prettier/prettier": [
        "error",
        {
          "singleQuote": true,
          "printWidth": 120,
          "tabWidth": 2,
          "semi": true
        }
      ],
      "no-console": "off"
    }
  }
  // ...
}
```

> 点击查看示例：[react](https://github.com/Cleam/vscode-eslint-prettier-demo/tree/react)

## 五、代码提交前自动格式化配置

安装依赖包

```bash
npm i -D husky lint-staged
```

修改`package.json`配置

```js
{
  // ...,
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  },
  "lint-staged": {
    "linters": {
      "src/**/*.{js,jsx,ts,tsx,json,css,scss,md}": [
        "prettier --single-quote --semi --print-width 120 --tab-width 2 --write",
        "git add"
      ]
    },
    "ignore": ["src/**/*.min.js"]
  }
  // ...
}
```

配好后提交代码时会自动将代码格式化

## 六、问题反馈

如发现问题，欢迎提工单（issue）或者提合并请求（PR）。

## 七、主题插件推荐

[vscode 主题插件推荐](./vscode_ext.md)
