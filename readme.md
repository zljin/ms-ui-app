# ms-ui-app Getting Started
> front-service to https://github.com/zljin/ms-app

## Ready do it
### step1 Technical selection
1. we choose uni-app for my front-end project Architecture ,because it's easy learn and muti-platform,suitable for rapid development.
2. we alse choose uni-ui(是基于uni-app的、全端兼容的、高性能UI框架) as a daily development template.
3. we use HBuilderX for dev tool
4. you can refer this guide to learn it:
```
https://m.imooc.com/wiki/uniapp-introframework
https://ext.dcloud.net.cn/search?q=uni-ui
https://zh.uniapp.dcloud.io/
谷歌自带颜色选择器:  https://www.google.com/search?q=%23F7F7F7
```

### step2 用法
1. 组件文档：https://uniapp.dcloud.net.cn/component/uniui/uni-ui.html#
2. 在pages.json里面配置路由访问,可以新建page自动生成到pages.json里面,访问路径：http://localhost:3000/#/pages/login/login
3. ctrl+k 代码格式化
4. 网络请求的写法
https://m.imooc.com/wiki/uniapp-networkrequest
	header: {
		//data 会 JSON 序列化
	    'Content-Type': 'application/json;charset=UTF-8',
		//data 会被转为 query string
	    'Content-Type': 'application/x-www-form-urlencoded',
	},


### 