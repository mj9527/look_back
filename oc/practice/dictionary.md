# 创建
```
NSDictionary* dic = [NSDictionary dictionaryWithObjects:@[@"mj", @"jh", @"zx"] forKeys:@[@"1", @"2", @"3"]];
NSDictionary* dic1 = [NSDictionary dictionaryWithObjectsAndKeys:@"mj", @"1", @"jh", @"2", @"zx", @"3",nil];
NSDictionary* dic2 = @{@"1":@"mj", @"2":@"jh", @"3":@"zx"};
```

# 遍历
```
for (NSString* key in dic2)
{
    NSLog(@"%@-%@", key, dic1[key]);
}
```

