## 1、react总结

#### 为什么学习react

1、和angular相比，React设计很优秀，一切基于js并且实现了组件化开发的思想

2、开发团队实力强悍，不必担心断更的情况

3、社区强大，很多问题都能找到对应的解决方案

4、提供了无缝转到ReactNative上的开发体验，让我们技术能力得到了扩展，增强了核心竞争力

5、很多企业，前端项目的技术选型采用的是react.js

#### Diff算法

Tree Diff      component diff       element diff

#### 创建基本的webpack4.x项目

1、初始化一个项目配置文件

```javascript
npm init -y
```

2、在项目根目录创建`src`源代码目录和`dist`产品目录

3、在src目录下创建index.html

4、安装webpack

5、npm安装淘宝镜像

```javascript
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

​全局运行`npm i cnpm -g`              

#### 在项目中使用react

1. 运行`npm i react react-dom -S`安装包
   - react  专门用于创建组件和虚拟DOM的，同时组件的生命周期都在这个包中
   - react-dom     专门进行DOM操作的，最主要的应用场景 就是 ReactDOM.render()

