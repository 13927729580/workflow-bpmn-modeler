# workflow-bpmn-modeler

🔥 本项目基于 `vue` 和 `bpmn.io@7.0` ，实现 flowable 的 modeler 流程设计器

## 预览

![20200818001755](https://cdn.jsdelivr.net/gh/goldsubmarine/cdn@master/blog/20200818001755.png)

## 在线 demo

👉 https://goldsubmarine.github.io/workflow-bpmn-modeler/demo/

## 安装

```bash
# 安装
yarn add workflow-bpmn-modeler
```

## 使用说明（最简 demo）

```vue
<template>
  <div>
    <bpmn-modeler
      ref="refNode"
      :xml="xml"
      :users="users"
      :groups="groups"
      :categorys="categorys"
    />
    <el-button type="primary" @click="save">保存</el-button>
  </div>
</template>

<script>
import bpmnModeler from "workflow-bpmn-modeler";

export default {
  components: {
    bpmnModeler,
  },
  data() {
    return {
      xml: "", // 后端查询到的xml
      users: [
        { name: "张三", id: "zhangsan" },
        { name: "李四", id: "lisi" },
        { name: "王五", id: "wangwu" },
      ],
      groups: [
        { name: "web组", id: "web" },
        { name: "java组", id: "java" },
        { name: "python组", id: "python" },
      ],
      categorys: [
        { name: "OA", id: "oa" },
        { name: "财务", id: "finance" },
      ],
    };
  },
  methods: {
    getModelDetail() {
      // 发送请求，获取xml
      // this.xml = response.xml
    },
    async save() {
      const processModel = this.$refs["refNode"].getProcess();
      const xml = await this.$refs["refNode"].saveXML();
      const svg = await this.$refs["refNode"].saveImg();
      console.log(processModel, xml, svg);
    },
  },
};
</script>
```

## iframe 部署

如果你的项目是 jquery 或 react 类项目，可以通过 iframe 的方式集成该流程设计器

本仓库通过 github pages 部署了静态页面，使用 jsdelivr 做 cdn ，国内访问也非常快速，所以你可以直接集成本仓库的页面，因为全部白嫖了 github 的资源，没有自己建服务器维护，所以不用担心资源失效问题。

当然你也可以在 `docs/lib` 文件夹下下载对应的版本，进行本地部署。

集成方式如下：

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <iframe
      src="https://goldsubmarine.github.io/workflow-bpmn-modeler/lib/0.2.0/"
      id="myFrame"
      frameborder="0"
      width="100%"
      height="800px">
    </iframe>
    <button onclick="myFrame.contentWindow.postMessage({type: 'get'}, '*')">获取流程的信息</button>

    <script>
      let myFrame = document.getElementById("myFrame");
      // 获取到流程详情
      window.addEventListener("message", (event) => {
        console.log(event.data); // { xml: 'xxx', img: 'xxx', process: {} }
      });
      myFrame.onload = () => {
        let postMsg = {
          type: "set",
          data: {
            xml: "", // 后端查询到的xml，新建则为空串
            users: [
              { name: "张三1", id: "zhangsan" },
              { name: "李四1", id: "lisi" },
              { name: "王五1", id: "wangwu" },
            ],
            groups: [
              { name: "web组1", id: "web" },
              { name: "java组1", id: "java" },
              { name: "python组1", id: "python" },
            ],
            categorys: [
              { name: "OA1", id: "oa" },
              { name: "财务1", id: "finance" },
            ],
          },
        }
        // 设置初始化值
        myFrame.contentWindow.postMessage(postMsg, "*")
      }
    </script>
  </body>
</html>
```

## License

[MIT](http://opensource.org/licenses/MIT)

Copyright (c) 2020-present, charles
