# webpack中的编译结果分析

## 1. 我的项目结构

```js
src/index.js
src/test.js

src文件下有两个js文件，index.js是入口文件

test.js文件内容如下

console.log('我是test模块');
module.exports = "caolei";

index.js文件内容如下

const test = require('./test.js');
console.log('我是入口文件');
console.log(test);
```



## 2. 自己对编译结果的分析（暂时只对Commonjs模块化进行分析）

```js
1. webpack先从入口文件出发，通过入口文件来逐个分析依赖，然后把它汇总到一个js文件

2. 我的入口文件是index.js, 我想把它输出到dist/main.js文件中

3. 考虑到变量全局污染，所以使用立即执行函数来运行

4. 我入口文件里面的内容是
执行第一句的时候，发现又依赖了test.js这个模块， 所以先去分析test.js这个模块里面的内容
const test = require('./test.js');

分析test.js模块的内容
console.log('我是test模块');
module.exports = "caolei";

分析完test.js里面的内容， 再分析下面的内容
console.log('我是入口文件');
console.log(test);


5. 我们知道引入模块只会执行一次， 再次引入返给你的是缓存结果， 所以肯定有一个缓存的机制

6. 分析的这些模块会以这种格式出现, 模块路径作为key值， 一个函数作为value值
{
    './src/index.js': function(module, exports, require){ 模块里面的代码 }
}

7. 如果不使用eval运行，在浏览器端报错的话他还是会指向原来的文件，不好改错
8. //# sourceURL=webpack:///./src/test1.js? 这个注释能把错误精确的指出来在哪个文件


/**
* 代码分析
**/
(function (modules){
    //缓存机制
    const installedModules = {};
    
    /**
    * path: 给我传入一个模块路径
    **/
    function require(path){
        //先判断缓存中有没有这个模块路径, 如果存在直接返回结果
        if(installedModules[path]){
            return installedModules[path].exports;
        }
        
        //如果没有这个模块
        const module = installedModules[path] = {
            id: path,          //模块唯一的标识
            isFinish: false,   //模块是否加载完成
            exports: {
                //返回的数据
            }
        }
        
        /**
        * call来改变this的指向
        **/
        modules[path].call(module.exports, module, module.exports, require);
        
        //把加载状态变成true
        module.isFinish = true;
        
        return module.exports;
    }
    
    
    //先从入口模块进行分析, 这个require是一个函数
    require('./src/index.js');
})(
    {
        './src/index.js': function(module, exports, require){
            const test = require('./test.js');
            console.log('我是入口文件');
            console.log(test);
            
            //webpack在运行这些语句的时候，是借助eval来运行的
            eval("const test = require(./test.js);\n console.log('我是入口文件'); \n console.log(test);sourceURL=webpack:///./src/index.js?")
        },
        
        './src/test.js': function(module, exports, require){
            console.log('我是test模块');
            module.exports = 'caolei'
            
            eval("console.log('我是test模快');\n module.exports='caolei';sourceURL=webpack:///./src/test.js?")
        }
    }
)
```



## 3.使用webpack的命令得到的编译结果

```js
这是执行webpack --mode=development, 得到的编译结果， dist/main.js

(function(modules) { // webpackBootstrap
 	// The module cache
 	var installedModules = {};

 	// The require function
 	function __webpack_require__(moduleId) {

 		// Check if module is in cache
 		if(installedModules[moduleId]) {
 			return installedModules[moduleId].exports;
 		}
 		// Create a new module (and put it into the cache)
 		var module = installedModules[moduleId] = {
 			i: moduleId,
 			l: false,
 			exports: {}
 		};

 		// Execute the module function
 		modules[moduleId].call(module.exports, module, module.exports, __webpack_require__);

 		// Flag the module as loaded
 		module.l = true;

 		// Return the exports of the module
 		return module.exports;
 	}


 	// expose the modules object (__webpack_modules__)
 	__webpack_require__.m = modules;

 	// expose the module cache
 	__webpack_require__.c = installedModules;

 	// define getter function for harmony exports
 	__webpack_require__.d = function(exports, name, getter) {
 		if(!__webpack_require__.o(exports, name)) {
 			Object.defineProperty(exports, name, { enumerable: true, get: getter });
 		}
 	};

 	// define __esModule on exports
 	__webpack_require__.r = function(exports) {
 		if(typeof Symbol !== 'undefined' && Symbol.toStringTag) {
 			Object.defineProperty(exports, Symbol.toStringTag, { value: 'Module' });
 		}
 		Object.defineProperty(exports, '__esModule', { value: true });
 	};

 	// create a fake namespace object
 	// mode & 1: value is a module id, require it
 	// mode & 2: merge all properties of value into the ns
 	// mode & 4: return value when already ns object
 	// mode & 8|1: behave like require
 	__webpack_require__.t = function(value, mode) {
 		if(mode & 1) value = __webpack_require__(value);
 		if(mode & 8) return value;
 		if((mode & 4) && typeof value === 'object' && value && value.__esModule) return value;
 		var ns = Object.create(null);
 		__webpack_require__.r(ns);
 		Object.defineProperty(ns, 'default', { enumerable: true, value: value });
 		if(mode & 2 && typeof value != 'string') for(var key in value) __webpack_require__.d(ns, key, function(key) { return value[key]; }.bind(null, key));
 		return ns;
 	};

 	// getDefaultExport function for compatibility with non-harmony modules
 	__webpack_require__.n = function(module) {
 		var getter = module && module.__esModule ?
 			function getDefault() { return module['default']; } :
 			function getModuleExports() { return module; };
 		__webpack_require__.d(getter, 'a', getter);
 		return getter;
 	};

 	// Object.prototype.hasOwnProperty.call
 	__webpack_require__.o = function(object, property) { return Object.prototype.hasOwnProperty.call(object, property); };

 	// __webpack_public_path__
 	__webpack_require__.p = "";


 	// Load entry module and return exports
 	return __webpack_require__(__webpack_require__.s = "./src/index.js");
})
/************************************************************************/
/******/ ({

/***/ "./src/index.js":
/*!**********************!*\
  !*** ./src/index.js ***!
  \**********************/
/*! no static exports found */
/***/ (function(module, exports, __webpack_require__) {

eval("const test1 = __webpack_require__(/*! ./test1.js */ \"./src/test1.js\");\r\n\r\nconsole.log(\"我是入口文件\");\r\n\r\nconsole.log(test1);\r\n\n\n//# sourceURL=webpack:///./src/index.js?");

/***/ }),

/***/ "./src/test1.js":
/*!**********************!*\
  !*** ./src/test1.js ***!
  \**********************/
/*! no static exports found */
/***/ (function(module, exports) {

eval("console.log(\"我是test1模块\");\r\n\r\nmodule.exports = \"caolei\";\r\n\n\n//# sourceURL=webpack:///./src/test1.js?");

/***/ })

/******/ });
```

