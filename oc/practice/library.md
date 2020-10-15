# 静态库.a制作
1. xcode 创建Cocoa Touch Static Library
2. 导入需要打包的资源文件包含（.h, .m, .mm）
3. 修改项目配置把.h 文件放进[New Headers Phase]
4. [Copy Files] 添加需要导出的头文件
5. [Build Configuration] 修改为release 
6. [Build Active Architecture Only] Release 选项设置为NO
7. 真机和模拟器静态库合并为一个静态库 lipo -create 第一个.a文件的绝对路径 第二个.a文件的绝对路径 -output 最终的.a文件路径

# 静态库.framework制作
1. xcode 创建Cocoa Touch Framework
2. 导入需要打包的资源文件包含（.h, .m, .mm）
3. 修改项目配置把需要暴露的头文件放到[Headers]的public 目录下
4. [Build Configuration] 修改为release 
5. [Build Active Architecture Only] Release 选项设置为NO
6. 修改[Mach-O Type] 为Static Library
7. 真机和模拟器静态库合并为一个静态库 lipo -create 第一个.a文件的绝对路径 第二个.a文件的绝对路径 -output 最终的.a文件路径
8. 合并文件为framework里面的静态库文件


# 动态库制作

