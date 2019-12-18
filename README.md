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
4. 方法传递默认参数及自定义参数
* 向方法`partionSizeChange`中传入默认参数`e`以及变量partion，方便在方法中进行处理：
```javaScript
 <Input
   suffix="m&sup2;"
   onChange={e => this.partionSizeChange(e, partion)}
   type="number"
 ></Input>
```
5. 表格禁止选择拥有某一属性的数据
```javaScript
 const rowSelection = {
    selectedRowKeys,
    onChange: this.onSelectChange,
    // 设置选择框的默认属性
    getCheckboxProps: record => ({
       disabled: record.isGroup,
    }),
 };
```
6. 表格分页数据
* state中page是当前页，从0开始，size是每页数据量，getList是查找列表数据的方法：
```javaScript
    handlePageChange = pagination => {
        this.setState({
            size: pagination.pageSize,
            page: pagination.current - 1,
        }, () => {
            this.getList();
        })
    }

    render() {
        const {
            businessData: { data, pageable },
        } = this.props.businessSpace;
        const pagination = {
            // 分页相关信息，一共多少条，每页多少条数据，当前页
            showTotal: () => (
                <span style={{ marginRight: 15, fontSize: '14px' }}>
                    共{<span style={{ color: '#0088FF' }}>{pageable.totalElements}</span>}条
              </span>
            ),
            total: pageable.totalElements,
            pageSize: pageable.size,
            current: pageable.number + 1,
            showSizeChanger: true,
            showQuickJumper: true,
        };
        return (
            <div>
                <Table 
                   columns={this.columns} 
                   dataSource={data} 
                   pagination={pagination} 
                   onChange={this.handlePageChange} 
                   rowKey="id"
                ></Table>
            </div>
        );
    }
```
7. 百度地图(待完善)
* 显示百度地图：
```javaScript
 setTimeout(() => {
    this.map = new BMap.Map("smallMap");
    let poi = new BMap.Point(108.952, 34.223);
    this.map.centerAndZoom(poi, 15);
    this.map.enableScrollWheelZoom();
    this.map.addEventListener("click", e => {
      let point = new BMap.Point(e.point.lng.toFixed(3), e.point.lat.toFixed(3)); 
      //获取当前地理名称
      let gc = new BMap.Geocoder();
      let attendanceAddress = "";
      gc.getLocation(point, rs => {
         attendanceAddress = rs.address;
         this.setState({
           latitude: e.point.lng,
           longitude: e.point.lat,
           attendanceAddress: attendanceAddress
         });
      });
    });
 }, 100);
```
* 地图搜索：
```javaScript
  searchRange = value => {
    const local = new BMap.LocalSearch(this.map, {
      renderOptions: { map: this.map }
    });
    local.search(value);
  };
```
8. 按需加载loading用法
```javaScript
  @connect(({businessSpace, loading }) => ({
    businessSpace,
    // 将loading在table等插件上引入即可
    loading: loading.effects['businessSpace/getInfoOne'] || loading.effects['businessSpace/getInfoTwo'],
  }))
```
9. 表格文字过长自动换行
```javaScript
  {
     title: '名字',
     dataIndex: 'name',
     align: 'left',
     render: text => <div style={{ wordWrap: 'break-word', wordBreak: 'break-all' }}>{text}</div>,
  },
```
10. 树形选择器显示每一级的名字
* 拼接树形数据方法：
```javaScript
    changeDataListToSelectData = (list, name) => {
        if (list.length > 0) {
            for (let i = 0; i < list.length; i += 1) {
                list[i].value = list[i].id;
                list[i].key = list[i].id;
                list[i].title = list[i].name || list[i].floorNumber;
                list[i].newValue = name !== '' ? `${name} / ${list[i].name || list[i].floorNumber}` : list[i].name || list[i].floorNumber;
                if (list[i].children && list[i].children.length > 0) {
                    this.changeDataListToSelectData(list[i].children, list[i].newValue);
                }
            }
        }
    }

    setTreeSelectData = (value, obj) => {
        this.setState({
            treeValue: obj.props.newValue,
        });
    }
```
* 树形组件中使用：
```javaScript
    <TreeSelect 
       // state中的值  
       value={treeValue} 
       // 树形组件数据   
       treeData={treeData} 
       // 配合showSearch按title进行搜索   
       treeNodeFilterProp="title" 
       treeDefaultExpandAll  
       showSearch 
       placeholder="请选择" 
       onSelect={this.setTreeSelectData} />
```
11. 对象转url中param的公共方法
* 下面方法会将除了undefined以外的任何值都拼接在param中：
```javaScript
function bodyToParam(body, head, oldParam) {
  let param = '';
  let newHead = '';
  if (head) {
    newHead = head;
  }
  if (oldParam) {
    param = oldParam;
  }
  if (undefined !== body) {
    let flag = 0;
    if (param) {
      flag = 1;
    }
    // eslint-disable-next-line no-restricted-syntax
    for (const i in body) {
      if (undefined !== body[i]) {
        if (flag === 0) {
          if (body[i] instanceof Object) {
            param = bodyToParam(body[i], `${newHead}${i}.`, param);
          } else {
            param = `?${newHead}${i}=${body[i]}`;
          }
          if (param) {
            flag = 1;
          }
        } else if (body[i] instanceof Object) {
            param = bodyToParam(body[i], `${newHead}${i}.`, param);
          } else {
            param = `${param}&${newHead}${i}=${body[i]}`;
          }
      }
    }
  }
  return param;
}
```
12. 文件导出工具类
```javaScript
/*
  文件导出公共方法
  name：文件名，带后缀
  oldurl：后端接口
  method：请求方式，默认get
*/
function exportAsset(name, oldurl, method = 'get', body) {
  const xhr = new XMLHttpRequest();
  const formData = new FormData();
  xhr.open(method, oldurl);
  //  添加认证信息
  xhr.setRequestHeader('authorization', sessionStorage.getItem('autnorization'));
  xhr.responseType = 'blob';
  xhr.onload = e => {
    if (e.target.status === 200) {
      const blob = e.target.response;
      // 这里的名字，可以按后端给的接口固定表单设置一下名字，如（费用单.xlsx,合同.doc等等）
      const filename = name;
      if (window.navigator.msSaveOrOpenBlob) {
        navigator.msSaveBlob(blob, filename);
      } else {
        const a = document.createElement('a');
        const url = createObjectURL(blob);
        a.href = url;
        a.download = filename;
        document.body.appendChild(a);
        a.click();
        window.URL.revokeObjectURL(url);
      }
    }
  };
  if (method === 'get') {
    xhr.send(formData);
  }
  if (method === 'post') {
    xhr.send(body);
  }
}

function createObjectURL(obj) {
  return window.URL ? window.URL.createObjectURL(obj) : window.webkitURL.createObjectURL(obj);
}
```