# domjs
DOM Enlightenment中的微型dom库


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
