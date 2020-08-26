# VsCode 开发环境配置规范

一个前端团队开发规范的最佳实践。

## 目录

- [一、下载安装 vscode 和插件](#一下载安装-vscode-和插件)
- [二、配置 vscode](#二配置-vscode)
- [三、配置 editorconfig](#三配置-editorconfig)
- [四、配置 prettier](#四配置-prettier)
- [五、项目配置](#五项目配置)
- [六、代码提交前自动格式化配置](#六代码提交前自动格式化配置)
- [七、问题反馈](#七问题反馈)
- [八、主题插件推荐](#八主题插件推荐)

## 一、下载安装 vscode 和插件

[点击下载 vscode](https://code.visualstudio.com/)

安装插件

- [eslint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)：js 代码检查工具
- [prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)：代码格式化工具
- [editorconfig](https://marketplace.visualstudio.com/items?itemName=EditorConfig.EditorConfig)：帮助开发人员在不同的编辑器和 IDE 之间定义和维护一致的编码样式

## 二、配置 vscode

给vscode添加以下配置项（Mac 位置：`code -> 首选项 -> 设置`，Windows 位置：欢迎补充）

```js
{
  // jsx单引号规则和taro规范保持一致，react默认是双引号。如果没有taro项目，可以不配置。
  "prettier.jsxSingleQuote": true,
  // js单引号规范
  "prettier.singleQuote": true,
  // 单行字符控制在100，超过换行。
  "prettier.printWidth": 100,
  // vscode配置，保存自动eslint fix
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  // js、json、vue默认格式化规则选择prettier。
  "[javascript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[json]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[vue]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
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
max_line_length = 100
tab_width = 2
trim_trailing_whitespace = true
```

## 四、配置 prettier

项目根目录添加文件`.prettierrc.js`

```javascript
module.exports = {
  singleQuote: true,
  printWidth: 100,
  tabWidth: 2,
  semi: true,
};
```

## 五、项目配置

- eslint 配置

按照[eslint-plugin-xmfe](https://www.npmjs.com/package/eslint-plugin-xmfe)的规则配置，有问题及时反馈。

- .gitignore 配置

项目根目录添加`.gitignore`文件，[点击查看内容](./.gitignore)

## 六、代码提交前自动格式化配置

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
        "prettier --single-quote --semi --print-width 100 --tab-width 2 --write",
        "git add"
      ]
    },
    "ignore": ["src/**/*.min.js"]
  }
  // ...
}
```

配好后提交代码时会自动将代码格式化

## 七、问题反馈

由于`vscode`工具、 react 脚手架`create-react-app` 和 vue 脚手架`vue-cli`的不断升级，在配置过程中可能会遇到一些问题，如有问题可以直接找我反馈，也欢迎提工单（issue）或者提合并请求（PR）。

## 八、主题插件推荐

[vscode 主题插件推荐](./vscode_ext.md)
