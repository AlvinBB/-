# JavaScript代码规范总结

## JS/JSX代码规范

> 谷歌/Airbnb 对于代码规范的要求基本可以算业内规范了，直接参考即可
  [Airbnb JavaScript Style Guide](http://airbnb.io/javascript/)，
  [Airbnb React/JSX Style Guide](http://airbnb.io/javascript/react/)，
  [Google JavaScript Style Guide](http://es6.ruanyifeng.com/#docs/symbol)
  
## ESLint

> 项目中引入ESlint可直接检查代码是否符合规范
  [ESLint Documentation](https://eslint.org/docs/user-guide/getting-started)

### 步骤

1. 全局安装ESlint
``` bash
$ npm install -g eslint
```

2.配置.eslint文件放置在项目目录下
```javascript
{
  "extends": "eslint:recommended",
  "env":{
    "jquery": true,
    "browser": true,
    "node": true,
    "es6": true
  },
  "parserOptions": {
    "ecmaVersion": 6,
    "sourceType": "module",
    "ecmaFeatures": {
      "modules": true,
      "jsx": true
    }
  },
  "rules": {
    "semi": ["error", "always"],
    "quotes": "off",
    "no-console":"off",
    "no-unused-vars":"off",
    "no-redeclare":"warn",
    "no-var": 0,
    "comma-spacing": [1, {
      "before": false,
      "after": true
    }]
  }
}
```

3.VScode中打开js文件即可看到不符合规范的位置已标红

4.使用 VScode 快捷键 Shift + Alt + F ，自动格式化代码
