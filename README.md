### vue-quill-editor图片上传七牛云使用方法

#### 1.安装依赖

- npm install

#### 2.在相应的页面引入tichTextEditor.vue组件

 - 父组件data里定义如下对象：

   ```javascript
   qiniuObj: {
   	url: '', //七牛域名,如：https://upload-z2.qiniup.com/
   	domain: '', //服务器子域名,如：https://xxx.xxxx.com
   	token:'' //七牛token
   }
   ```

   

   ###### 效果图
   
   ![效果图1](/src/assets/1.gif)
   
   

![效果图2](/src/assets/2.png)