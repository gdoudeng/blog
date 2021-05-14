---
title: _property vs self.property
date: 2021-03-06 18:03:49
tags: iOS
excerpt: 什么时候用property，什么时候用self.property
categories: 移动端
---

`SomeClass.h`

```
@interface SomeClass : NSObject

@property NSString *property;

@end
```

实际使用中我们是可以有两种方式使用的：
- `_property`

- `self.property` 

那么究竟什么时候用第一种什么时候用第二种？

一般来说你可能会在getter/setter/init/dealloc方法种用到`_property`，其他任何情况都应该是用`self.property`

## 为什么？

因为调用`self.property`实际上是去访问了这个属性的getter方法，如果是`self.property = @"something"`则是去调用这个属性的setter方法。

而`_property`则是直接访问这个变量。

```
- (Type)property{
    return 2*_property;
}

// AND/OR

- (void)setProperty:(Type)property
{
    _property = 2*property;
}
```

所以如果有复写这个属性的getter方法和setter方法，那么`_property`和`self.property`就会变得不同。
