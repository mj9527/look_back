# 创建
```
NSArray* arr2 = @[@"mj", @"zheng", @"ming"];
```

# 使用注意
- 数组以nil为结束
- 不可以使用@[]创建可变数组

# 数组的遍历
```
    // method 1
    for (int i=0; i< arr2.count; i++)
    {
        NSLog(@"index %d value %@", i, arr2[i]);
    }
    
    // method 2
    for (id obj in arr2)
    {
        NSLog(@"obj value %@", obj);
    }
    
    // method 3
    [arr2 enumerateObjectsUsingBlock:^(id  _Nonnull obj, NSUInteger idx, BOOL * _Nonnull stop) {
         NSLog(@"enum index %lu value %@", (unsigned long)idx , obj);
    }];
```

# 排序
```
NSArray* arr3 = [arr2 sortedArrayUsingSelector:@selector(compare:)];
```
