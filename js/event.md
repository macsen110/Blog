# js 事件系统
1.对于dom中原生支持的时间系统,调用时直接eg: element.click()
2.对于dom中的事件扩展,使用new CustomEvent，，使用对象的dispatchEvent方法来触发eg:

```
var obj = document;
// add an appropriate event listener
obj.addEventListener("cat", function(e) { alert(e.detail) });
// create and dispatch the event
var event = new CustomEvent("cat", {
  detail: {
    hazcheeseburger: true
  }
});
obj.dispatchEvent(event);

```
2.dispath and emit

每个library中设计的事件系统也都是不一样的, 以下是ng中的事件分析,与Vuejs 最大的区别在于emit的方法,ng中会触发当前作用域以及父作用域直至root，Vue中直接会stop propagation,Vue中子作用域要传递到父作用域以及root要使用$dispatch

$rootScope.$emit only lets other $rootScope listeners catch it. This is good when you don't want every $scope to get it. Mostly a high level communication. Think of it as adults talking to each other in a room so the kids can't hear them.

$rootScope.$broadcast is a method that lets pretty much everything hear it. This would be the equivalent of parents yelling that dinner is ready so everyone in the house hears it.

$scope.$emit is when you want that $scope and all its parents and $rootScope to hear the event. This is a child whining to their parents at home (but not at a grocery store where other kids can hear).

$scope.$broadcast is for the $scope itself and its children. This is a child whispering to its stuffed animals so their parents can't hear.