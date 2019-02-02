# 相对路径的简单解释

* / 根目录
* ./ 当前文件所在目录
* ../ 当前文件所在目录的父目录

## 例子

```javascript
.
└── /src/
    ├── /target2.html
    └── /tools/
        ├── index.html
        └── target1.html
```

* 在 index.html 访问 target1.html

  ./target1.html

* 在 index.html 访问 target2.html

  ../target2.html
