# 背单词

> 简单的可自定义的背单词程序

效果图

![效果图](.\img\效果图.gif)

### 特性

* 可自定义设置单词库
* 单词可朗读

## 部署环境

你需要以下的安装环境

* [Node.js](https://nodejs.org/en/download/)
* [Git](https://git-scm.com/download/win)

1. 克隆本仓库`git clone https://github.com/liuqianpan2008/beidanci `

2. 然后在项目目录下执行`npm install`
3. 最后运行它`npm run serve`

### 配置单词库
打开`./scr/data.js` 文件

```tsx
const data=[
    {
        world:"第一个单词",
        hanyi:"第一个单词描述",
    },{
        world:"第二个单词",
        hanyi:"第二个单词描述"
    }
]
export default data
```

### 未来和计划

* [ ] 暗色主题  
* [ ] 单词批量导入





