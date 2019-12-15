# reactLearning
##  I. 基础知识：html ,css, js, es6
1. html  
1.1 <a href="http://www.w3school.com.cn/html/index.asp" target="_blank">http://www.w3school.com.cn/html/index.asp</a>   
2. css  
2.1 <a href="https://www.w3cschool.cn/css/css-tutorial.html" target="_blank">https://www.w3cschool.cn/css/css-tutorial.html</a>   
3. js  
3.1 <a href="https://wangdoc.com/javascript/operators/index.html" target="_blank">https://wangdoc.com/javascript/operators/index.html</a>   
3.2 <a href="http://www.w3school.com.cn/js/index.asp" target="_blank">http://www.w3school.com.cn/js/index.asp</a>   
4. es6  
4.1 <a href="http://es6.ruanyifeng.com" target="_blank">http://es6.ruanyifeng.com</a>  
5. echartsjs  
5.1 <a href="https://www.echartsjs.com/zh/index.html" target="_blank">https://www.echartsjs.com/zh/index.html</a>  
## II. 前端框架及脚手架
1. react  
1.1 react框架入门 <a href="https://zh-hans.reactjs.org/tutorial/tutorial.html" target="_blank">https://zh-hans.reactjs.org/tutorial/tutorial.html</a>  
1.2 antd-pro脚手架文档 <a href="https://pro.ant.design/docs/getting-started-cn" target="_blank">https://pro.ant.design/docs/getting-started-cn</a>  
1.3 antd文档 <a href="https://ant.design/docs/react/introduce-cn" target="_blank">https://ant.design/docs/react/introduce-cn</a>  
1.4 antd实战教程 <a href="https://www.yuque.com/ant-design/course/intro" target="_blank">https://www.yuque.com/ant-design/course/intro</a>  
2. vue  
2.1 vue框架入门 <a href="https://cn.vuejs.org/v2/guide" target="_blank">https://cn.vuejs.org/v2/guide</a>  
2.2 ElementUi文档 <a href="https://element.eleme.cn/#/zh-CN/component/installation" target="_blank">https://element.eleme.cn/#/zh-CN/component/installation</a>  
2.3 vuex <a href="https://vuex.vuejs.org/zh" target="_blank">https://vuex.vuejs.org/zh</a>  
## III.antd常见问题及解决方法
1. 表单自定义校验   
* FormItem写法示例:  
```javaScript
     <FormItem label="编号">
         {getFieldDecorator('no', {
            rules: [
            {
              required: true,
              message: '请输入编号',
            }, {
                // 自定义校验
                validator: this.validateStatus,
            }],
            // 校验触发时机
            validateTrigger: 'onBlur',
         })(<Input placeholder="请输入编号"/>)}
    </FormItem>
```  
* 校验方法中`callback()`必须每个条件分支都有返回结果，否则可能会导致校验在提交表单时失效：
```javaScript
validateStatus = (rule, value, callback) => {
    const { dispatch } = this.props;
    const newData = {};
    if (value) {
        newData.code = value;
        dispatch({
            type: 'checkSpace/checkCodeIfExist',
            payload: newData,
            callback: response => {
                if (response.data) {
                     callback('该编号已被占用');
                } else {
                     callback();
                }
            },
        });
    } else {
        callback();
    }
}
```
2. 根据屏幕大小动态改变样式
* `:global`改变全局样式,`@media`可以针对不同的屏幕尺寸设置不同的样式:
```javaScript
.workModal {
    :global(.ant-modal-content) {
      width: 200%;
      margin-left: -30%;
    }
   @media screen and (max-width: 1253px) {
     :global(.ant-modal-content) {
       width: 180%;
       margin-left: -30%;
     }
   }
   @media screen and (max-width: 1060px) {
     :global(.ant-modal-content) {
       width: 150%;
       margin-left: -20%;
     }
   }
}
```
3. 使用变量动态计算样式
* 下面是根据数值计算div的宽度：
```javaScript
 const dataString = ((area - total) / area) * 100;
 const width = { width: `${dataString}%` };
 const newDiv = <div style={{ float: 'left', ...width }}></div>;
```
