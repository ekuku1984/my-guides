# 安装环境

~~~
npm install -g @vue/cli
~~~

**查看 vue cli 版本**
~~~
vue -V
~~~

**创建一个新工程**

~~~
vue create projectName
~~~

**运行工程**

~~~
cd projectName
npm run serve
~~~

**修改运行的端口**

打开 package.json 文件，在 scripts 节点下面， 修改 serve 的运行属性：
~~~javascript
scripts: {
  "serve": "vue-cli-service serve --port 8090"
}
~~~
