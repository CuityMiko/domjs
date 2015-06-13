# domjs
DOM Enlightenment中的微型dom库

[https://github.com/codylindley/domjs][1]


## 创建唯一作用域

在立即调用函数表达式中设置window和document的引用可以加速访问

    (function (win) {

      var global = win;
      var doc = win.document;

    })(window);



## 创建dom()与GetOrMakeDom(),全局暴露dom()与GetOrMakeDom.prototype

我们将创建一个函数,根据传入参数返回一个可链式调用的,封装好的含有DOM节点的集合.


    (function (win) {

      var global = win;
      var doc = win.document;

      var dom = function (params, context) {
        return new GetOrMakeDom(params, context);
      };

      var GetOrMakeDom = function (params, context) {

      }

      // 暴露dom到全局作用域
      global.dom = dom;

      // prototype 提供
      dom.fn = GetOrMakeDom.prototype;

    })(window);

## 创建传给dom()的可选上下文参数

(function (win) {

  var global = win;
  var doc = global.document;
  var dom = function (params, context) {
    return new GetOrMakeDom(params, context);
  };

  var GetOrMakeDom = function (params, context) {
    var currentContext = doc;

    if (context) {
      if (context.nodeType) {
        currentContext = context;
      }
      else {
        currentContext = doc.querySelector(context);
      }
    }
  };

  global.dom = dom;

  dom.fn = GetOrMakeDom.prototype;
})(window);

## 依据params产生一个持有DOM节点引用的对象并返回

传递给dom()的参数可以有以下几种:

- CSS选择器
- HTML字符串
- Element节点
- 元素节点数组
- NodeList
- HTMLCollection
- dom()对象

(fucntion (win) {

  var global = win;
  var doc = global.document;
  var dom = function (params, context) {
    return new GetOrMakeDom(params, context);
  };

  var regXContainsTag = /^\s*<(\w+|!)[^>]*>/;

  var GetOrMakeDom = function (params, context) {

    var currentContext = doc;
    if (context) {
      if (context.nodeType) {
        currentContext = context;
      }
      else {
        currentContext = doc.querySelector(context);
      }
    }

    if (!params || params === '' ||
        typeof params === 'string' && params.trim() === '') {

      this.length = 0;
      return this;
    }

    if (typeof params === 'string' && regXContainsTag.text(params)) {

      var divElm = currentContext.createElement('div');
      divElem.className = 'hippo-doc-frag-wrapper';
      var docFrag = doc.createDocumentFragment();
      docFrag.appendChild(divElm);
      var queryDiv = docFrag.querySelector('div');
      queryDiv.innerHTML = params;
      var numberOfChildren = queryDiv.children.length;

      for (var z = 0; z < numberOfChildren; ++z) {
        this[z] = queryDiv.children[z];
      }

      this.length = numberOfChildren;

      return this;
    }


    if (typeof params === 'object' && params.nodeName) {
      this.length = 1;
      this[0] = params;
      return this;
    }

    var nodes;
    if (typeof params !== 'string') {
      nodes = params;
    }
    else {
      nodes = currentContext.querySelectorAll(params.trim());
    }

    var nodeLength = nodes.length;
    for (var i = 0; i < nodeLength; ++i) {
      this[i] = nodes[i];
    }

    this.length = nodeLength;

    return this;
  };

  global.dom = dom;

  dom.fn = GetOrMakeDom.prototype;

})(window);



[1]: https://github.com/codylindley/domjs
