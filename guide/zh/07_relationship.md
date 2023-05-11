# 对象关系
<!-- relationship -->

在`ThingJS`引擎中，对象和对象之间最常用的是父子关系（或叫属于关系），还可以通过`Relationship`建立各种自定义关系。

## 父子关系
场景中的每个对象都具有父子关系，一般是由加载场景式自动建立的，可以通过对象的`obj.parent`和`obj.children`进行访问：

```javascript
// 子对象数组
obj.children

// 父对象
obj.parent

// 父对象数组（包括父亲和父亲的父亲）
obj.parents

// 兄弟对象数组（有共同父亲的对象）
obj.brothers
```

使用`add`、`remove`接口来添加或移除父子关系
```javascript
obj.add(childObj);
obj.remove(childObj);
```

父子返回的`parents`和`children`都是选择集类型，当需要寻找单个对象时，可以使用`find`接口进行搜索，找到立即返回，比`query`接口更高效：
```javascript
obj.children.find('#001');
obj.parents.find('.Floor');
```

## 自定义关系
可以通过`new THING.Relationship`来创建自定义关系，需要制定关系类型`type`、关系的源对象`source`和目标对象`target`（源和目标可以是多个对象），关系还可指定标签`tags`和自定义属性`userData`，下面是几个创建关系的例子：
```javascript
// 创建关系
let rls = new THING.Relationship({
    type: "control",
    name: "control01",
    source: obj1,
    target: obj2,
});

// 创建关系，更多参数
let rls2 = new THING.Relationship({
    type: "control",
    name: "control02",
    source: obj1,
    target: [obj2, obj3]
    tags: ["xxx"],
    userData: {}
    // queryDirection: THING.RelationshipDirection.Out // 默认Out
});

// 创建多对象关系
let rls3 = new THING.Relationship({
    type: "group",
    source: [obj1, obj2, obj3]
});
```

创建关系后，可以通过`obj.relationships`来访问到对象关系：
```javascript
let rls = obj.relationships[0];
console.log(rls.type+", "+rls.source+", "+rls.target);
```

可以通过`app.queryRelationships()`在所有对象的关系中进行查询：
```javascript
let rls = app.queryRelationships({
    "type": "control"
});
```
或者通过`obj.relationship.query()`在这个对象的关系范围中进行查询：
```javascript
let objs = obj.relationship.query({
    "type": "control"
});

let objs = [];
objs = obj.relationship.queryByType("control");
objs = obj.relationship.queryByName("control01");
```

通过关系的`destroy`接口可以删除关系，当关系的相关对象被销毁时，也会同步删除相关的关系：
```javascript
let rls = app.queryRelationships({
    "type": "control"
});
rls.destroy();
```

```javascript
？关系为数组时候，是否可以：
obj.source;
obj.sources;
obj.target;
obj.targets;
```
