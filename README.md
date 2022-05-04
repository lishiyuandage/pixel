# pixel

## 应哇塞请求开发一个图像马赛克程序

**效果如下:**

![preview](pixel.PNG)

**算法梗概：** 

- 每scale*scale方块里面的像素保留方块内第一个像素。

**算法逻辑：**

- 非scale列的像素复制上一列。
- 非scale行的像素复制上一行。
- 否则保留原像素。

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
