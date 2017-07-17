# Mysql触发器
- 触发器简介 : 触发器(Trigger)是一个特殊的存储过程，它的执行不是由程序调用，也不是手工启动，而是由事件触发。
触发器经常用于加强数据的完整性约束和业务规则等。
- 本次新建触发器背景：有两个数据表 A|B，A是在另外的系统中同步过来的，表中数据每3分钟会在更新一次。B是在本系统内使用的，由前端调用接口将A表中某一条数据导入到B表中。那么，在此时刻之后，A表中如数据有更新，AB两表数据便不一致了。数据以A表为准，所以，使用触发器是一个方案。下面展示如何新建这一个触发器。

---

- **触发器语法**
```
-- 1. 新建触发器
CREATE TRIGGER 触发器名称 BEFORE|AFTER 触发事件
  ON 表名 FOR EACH ROW
  BEGIN
    触发器程序体
  END

-- {BEFORE|AFTER} 触发器触发的时机
-- {INSERT|UPDATE|DELETE} 触发器事件
-- FOR EACH ROW 子句通知触发器每隔一行执行一次动作，而不是对整个表执行一次。
--   (针对insert和delete语句，每一行都触发)

--2. 查看触发器
show triggers\G

-- 3. 删除
drop trigger 触发器名称

```

- 触发器中 new && old

```
针对update操作，new表示的是更新后的值，old表示的是原来的数据。
针对insert操作，new表示的是插入的值。
针对delete操作，old表示的是删除后的值。
```
---

- 示例
-
```
DELIMITER ;;
CREATE TRIGGER `case_update_to_case2` AFTER UPDATE ON `jz_case_detail` FOR EACH ROW BEGIN
      UPDATE case2_baseinfo SET lasj = new.SL_LRSJ ,belongTo = new.DEPARTMENTCODE, caseName= new.AJMC,caseClass=new.AJLX,case_status = new.AJSTATE,masterSN = new.AJZBRY,masterName=new.RESERVATION01,assist_masterSN=new.AJXBRY,assist_masterName=new.RESERVATION02,introduction=new.ZYAQ where caseID = new.AJBH;
END
;;
DELIMITER ;
```

