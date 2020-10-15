# Foundation和Core Foundation的区别
- Foundation 使用OC实现，Core Foundation 使用C实现
- Foundation 和Core Foundation 对象间的转换: 俗称桥接
- Foundation 类以NS开头, Core Foundation以CF开头

## OC(Foundation)strOC ->C(Core Foundation) strC
- __bridge 没有转移对象所有权，依然由OC管理
- __bridge_retianed 转移对象所有权，strC 必须自己手动释放内存

## C(Core Foundation) strC -> OC(Foudation) strOC
- __bridge 没有转移对象所有权，依然由C管理
- __bridge_transfer 转移对象所有权，由OC管理

