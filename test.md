## 测试

components 在对应的组件文件夹内创建__test__目录，再创建测试文件*.test.js

pages 在对应的组件文件夹内创建__test__目录，再创建测试文件*.test.js

其他 在/src同级创建__test__目录，目录内与/src内的文件结构一致


## 常见错误及解决

#### import语法不对

创建 或者 检查 .babelrc 
```json
{
  "env": {
    "test": {
      "presets": ["env", "stage-2"] 
    }
  }
}
```


#### refs测试子组件找不到如何解决?

Refs are null in Jest snapshot tests with react-test-renderer

https://github.com/facebook/jest/issues/42

解决: 测试之前需要mock一些假的refs代码
```javascript
let instance = wrapper.instance();  
instance.refs = {timerButton: { state: {counting: false}}};
    instance._requestSMSCode();

    // 使用真正的定时器, 否则异步执行会立即返回, 导致状态判断出错
    jest.useRealTimers();

    setTimeout(function() {
        // run your expectation
        expect(instance.state.verifyText).toEqual('需要图形码44');
        console.log("instance.state", instance.state);
        console.log("instance.refs", instance.refs);
        done();
    }, 1000);
```


#### 代码中import了json资源但是提示访问不到?

https://github.com/facebook/jest/issues/238
```javascript
import xxx from '../data/myjson';// myjson.json
```
出错信息: Cannot find module '../data/myjson' from 'xxxTest.js'

解决: 修改package.json中的jest配置, 加入json扩展名
```javascript
"moduleFileExtensions": [
       "js", "jsx", "json" 
 ]
 ```
 
 
#### mock代码被当做测试执行报错?

https://github.com/facebook/jest/tree/master/examples/manual-mocks

参考这里的目录结构, __tests__和 __mocks__分开存放test.js文件和mock的类.
