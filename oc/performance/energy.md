# 电量

1. 查看系统的总电量消耗

select 
datetime(timestamp, 'unixepoch', 'localtime') AS time, 
timestamp, 
Level, 
RawLevel, 
Voltage, 
InstantAmperage,
AppleRawCurrentCapacity
from PLBatteryAgent_EventBackward_Battery 
where timestamp >1604551890;

AppleRawCurrentCapacity: 电池当前剩余容量
InstantAmperage: 实时电流


2. 查看node Id:
- meeting
select * from PLAccountingOperator_EventNone_Nodes where name="com.tencent.meeting.db.haven";
结果：10204

- zoom 
select * from PLAccountingOperator_EventNone_Nodes where name="us.zoom.videomeetings.yunshipin";



3. 按模块汇总
SELECT 
datetime(timestamp, 'unixepoch', 'localtime') AS time , 
*,
(select Name From PLAccountingOperator_EventNone_Nodes where ID=PLAccountingOperator_Aggregate_RootNodeEnergy.NodeID) AS appname, 
(select Name From PLAccountingOperator_EventNone_Nodes where ID=PLAccountingOperator_Aggregate_RootNodeEnergy.RootNodeID) AS typename 
from PLAccountingOperator_Aggregate_RootNodeEnergy 
where NodeID=(select ID from PLAccountingOperator_EventNone_Nodes where Name ='com.tencent.meeting.db.haven') 
ORDER BY time

--
SELECT 
datetime(timestamp, 'unixepoch', 'localtime') AS time , 
*,
(select Name From PLAccountingOperator_EventNone_Nodes where ID=PLAccountingOperator_Aggregate_RootNodeEnergy.NodeID) AS appname, 
(select Name From PLAccountingOperator_EventNone_Nodes where ID=PLAccountingOperator_Aggregate_RootNodeEnergy.RootNodeID) AS typename 
from PLAccountingOperator_Aggregate_RootNodeEnergy 
where NodeID=(select ID from PLAccountingOperator_EventNone_Nodes where Name ='us.zoom.videomeetings.yunshipin') 
ORDER BY time

SELECT 
datetime(timestamp, 'unixepoch', 'localtime') AS time , 
*,
(select Name From PLAccountingOperator_EventNone_Nodes where ID=PLAccountingOperator_Aggregate_RootNodeEnergy.NodeID) AS appname, 
(select Name From PLAccountingOperator_EventNone_Nodes where ID=PLAccountingOperator_Aggregate_RootNodeEnergy.RootNodeID) AS typename 
from PLAccountingOperator_Aggregate_RootNodeEnergy 
where NodeID=(select ID from PLAccountingOperator_EventNone_Nodes where Name ='us.zoom.videomeetings') 
ORDER BY time



select
datetime(t1.timestamp, 'unixepoch', 'localtime') AS time,
*
from 
(
 select 
    *
    from 
    PLAccountingOperator_Aggregate_RootNodeEnergy
    where timestamp=1604484033 and RootNodeID=11
)as t1 inner join PLAccountingOperator_EventNone_Nodes as t2
on t1.NodeId=t2.ID



数据输出延迟一个小时，只能按小时统计
meeting: com.tencent.meeting.db.haven
zoom: us.zoom.videomeetings.yunshipin



# 测试方法
## 原始方法
1. 保持实验前置条件一致（手机当前电量）
2. 测试半个小时
3. 记录实验后的手机当前电量
优点：简单
缺点：需要人工参与，精准性不高, 无法按模块区分耗电量

##  instruments energy log
优点：可以看出各个模块的能耗消耗等级和时长
缺点： 模块耗电量未知


## 系统诊断-苹果电池诊断
- 日志开启
1. 下载profile(有效期7天)，并安装到ios设备
2. 同步日志信息到电脑（同步之前不要重启设备）
3. 复现问题，并记录问题出现的日期和时间
4. 再次同步信息到电脑
5. macos 日志位置~/Library/Logs/CrashReporter/MobileDevice/[Your_Device_Name]/
6. iOS:Go to: Settings.app > Privacy > Analytics > Analytics Data > (Locate the sysdiagnose file and AirDrop it to your Mac computer).

- 日志关闭
1. 移除profile
2. 重启设备

额定容量*额定电压=额定电量
单位的运算：mAH*V=mWH

Energy : mWH，不需要充电，可以连续测试

iphone6s 查看电池容量: 1715mAh

# 优化方向
1. 网络
2. 定位
3. 
