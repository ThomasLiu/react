## 测试

components 在对应的组件文件夹内创建__test__目录，再创建测试文件*.test.js

pages 在对应的组件文件夹内创建__test__目录，再创建测试文件*.test.js

其他 在/src同级创建__test__目录，目录内与/src内的文件结构一致


## 常见错误及解决

#### import语法不对

https://stackoverflow.com/questions/43514455/react-nativejestsyntaxerror-unexpected-token-import

[Test suite failed to run Invariant Violation: Native module cannot be null.]

​ at invariant (node_modules/fbjs/lib/invariant.js:44:7)

​ at new NativeEventEmitter (node_modules/react-native/Libraries/EventEmitter/NativeEventEmitter.js:32:1)

​ at Object.<anonymous style="-webkit-font-smoothing: antialiased; -webkit-tap-highlight-color: rgba(0, 0, 0, 0); text-size-adjust: none; box-sizing: border-box;"> (node_modules/react-native/Libraries/Network/NetInfo.js:20:25)</anonymous>

​ at Object.get NetInfo [as NetInfo] (node_modules/react-native/Libraries/react-native/react-native.js:93:22)

​ at new NetInfoSingleton (app/util/NetInfoSingleton.js:24:13)

​ at Object.<anonymous style="-webkit-font-smoothing: antialiased; -webkit-tap-highlight-color: rgba(0, 0, 0, 0); text-size-adjust: none; box-sizing: border-box;"> (app/util/NetInfoSingleton.js:48:25)</anonymous>

参考: https://github.com/facebook/jest/issues/2208

需要mock native的一些行为. 这个导致RN测试比较难做.  


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
