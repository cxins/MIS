# MIS

建表：<br/>
DROP TABLE IF EXISTS `consume`;<br/>
CREATE TABLE `consume` (<br/>
  `reid` int(11) NOT NULL COMMENT '记录id',<br/>
  `materialid` int(11) NOT NULL COMMENT '材料名称',<br/>
  `number` varchar(45) DEFAULT NULL COMMENT '数量',<br/>
  PRIMARY KEY (`reid`),<br/>
  KEY `materialid_idx` (`materialid`),<br/>
  CONSTRAINT `materialid` FOREIGN KEY (`materialid`) REFERENCES `material` (`materialid`) ON DELETE NO ACTION ON UPDATE NO ACTION,<br/>
  CONSTRAINT `recordidd` FOREIGN KEY (`reid`) REFERENCES `record` (`recordid`) ON DELETE NO ACTION ON UPDATE NO ACTION<br/>
) ENGINE=InnoDB DEFAULT CHARSET=utf8;<br/>
INSERT INTO `consume` VALUES ('1', '3', '11');<br/>
INSERT INTO `consume` VALUES ('2', '2', '10');<br/>
INSERT INTO `consume` VALUES ('3', '1', '8');<br/>

<br/>
DROP TABLE IF EXISTS `equipment`;<br/>
CREATE TABLE `equipment` (<br/>
  `equipmentid` int(11) NOT NULL AUTO_INCREMENT COMMENT '设备id',<br/>
  `equipmentname` varchar(225) NOT NULL COMMENT '设备名称',<br/>
  `timeid` int(11) DEFAULT NULL COMMENT '检查时间id',<br/>
  PRIMARY KEY (`equipmentid`),<br/>
  KEY `time_idx` (`timeid`),<br/>
  CONSTRAINT `time` FOREIGN KEY (`timeid`) REFERENCES `time` (`timeid`) ON DELETE NO ACTION ON UPDATE NO ACTION<br/>
) ENGINE=InnoDB AUTO_INCREMENT=6 DEFAULT CHARSET=utf8;<br/>
INSERT INTO `equipment` VALUES ('1', '6000V及以上电机', '1');<br/>
INSERT INTO `equipment` VALUES ('2', '6000V以下不带振动电机', '1');<br/>
INSERT INTO `equipment` VALUES ('3', '6000V以下带振动电机', '2');<br/>
INSERT INTO `equipment` VALUES ('4', 'CST', '4');<br/>
INSERT INTO `equipment` VALUES ('5', 'PLC', '4');<br/>
DROP TABLE IF EXISTS `maintain`;<br/>
CREATE TABLE `maintain` (<br/>
  `maintainid` int(11) NOT NULL AUTO_INCREMENT COMMENT '维修项目id',<br/>
  `maintainname` varchar(445) DEFAULT NULL COMMENT '维修项目内容',<br/>
  `eqid` int(11) DEFAULT NULL COMMENT '设备id',<br/>
  `ok` varchar(45) DEFAULT NULL COMMENT '是否完成',<br/>
  `other` varchar(445) DEFAULT NULL,<br/>
  PRIMARY KEY (`maintainid`),<br/>
  KEY `shebeiid_idx` (`eqid`),<br/>
  CONSTRAINT `shebeiid` FOREIGN KEY (`eqid`) REFERENCES `equipment` (`equipmentid`) ON DELETE NO ACTION ON UPDATE NO ACTION<br/>
) ENGINE=InnoDB AUTO_INCREMENT=6 DEFAULT CHARSET=utf8;<br/>
INSERT INTO `maintain` VALUES ('1', '接线盒开盖检查', '3', '完成', null);<br/>
INSERT INTO `maintain` VALUES ('2', '电机电缆头固定、磨损情况', '3', '完成', null);<br/>
INSERT INTO `maintain` VALUES ('3', '电机接线盒的密封情况', '3', '完成', null);<br/>
INSERT INTO `maintain` VALUES ('4', '检查电磁阀的好坏', '4', '完成', null);<br/>
<br/>
DROP TABLE IF EXISTS `material`;<br/>
CREATE TABLE `material` (<br/>
  `materialid` int(11) NOT NULL AUTO_INCREMENT COMMENT '材料id',<br/>
  `materialname` varchar(225) DEFAULT NULL COMMENT '材料名称',<br/>
  PRIMARY KEY (`materialid`)<br/>
) ENGINE=InnoDB AUTO_INCREMENT=4 DEFAULT CHARSET=utf8;<br/>
INSERT INTO `material` VALUES ('1', '电磁阀');<br/>
INSERT INTO `material` VALUES ('2', '接线盒');<br/>
INSERT INTO `material` VALUES ('3', '电缆头');<br/>

<br/>
<br/>
DROP TABLE IF EXISTS `record`;<br/>
CREATE TABLE `record` (<br/>
  `recordid` int(11) NOT NULL AUTO_INCREMENT COMMENT '保养记录id',<br/>
  `people` varchar(225) DEFAULT NULL COMMENT '保养人',<br/>
  `date1` date DEFAULT NULL COMMENT '保养日期',<br/>
  `eqid` int(11) DEFAULT NULL COMMENT '设备id',<br/>
  `maintainid` int(11) DEFAULT NULL COMMENT '保养内容id',<br/>
  `group` varchar(45) DEFAULT NULL,<br/>
  PRIMARY KEY (`recordid`),<br/>
  KEY `eeqid_idx` (`eqid`),<br/>
  KEY `mainid_idx` (`maintainid`),<br/>
  CONSTRAINT `eeqid` FOREIGN KEY (`eqid`) REFERENCES `equipment` (`equipmentid`) ON DELETE NO ACTION ON UPDATE NO ACTION,<br/>
  CONSTRAINT `mainid` FOREIGN KEY (`maintainid`) REFERENCES `maintain` (`maintainid`) ON DELETE NO ACTION ON UPDATE NO ACTION<br/>
) ENGINE=InnoDB AUTO_INCREMENT=4 DEFAULT CHARSET=utf8;<br/>
INSERT INTO `record` VALUES ('1', '王小二', '2016-10-04', '3', '2', '1');<br/>
INSERT INTO `record` VALUES ('2', '王小二', '2016-03-02', '3', '3', '1');<br/>
INSERT INTO `record` VALUES ('3', '张三', '2015-01-03', '4', '4', '2');<br/>

DROP TABLE IF EXISTS `time`;<br/>
CREATE TABLE `time` (<br/>
  `timeid` int(11) NOT NULL AUTO_INCREMENT,<br/>
  `timename` varchar(45) DEFAULT NULL,<br/>
  PRIMARY KEY (`timeid`)<br/>
) ENGINE=InnoDB AUTO_INCREMENT=6 DEFAULT CHARSET=utf8;
<br/>
INSERT INTO `time` VALUES ('1', '年检');<br/>
INSERT INTO `time` VALUES ('2', '半年检');<br/>
INSERT INTO `time` VALUES ('3', '双月检');<br/>
INSERT INTO `time` VALUES ('4', '月检');<br/>
INSERT INTO `time` VALUES ('5', '周检');<br/>
<br/>

查询：<br/>
use system;<br/>
select * from record<br/>
where maintainid=(select maintainid from maintain where eqid='4');<br/>
![1](https://github.com/cxins/MIS/blob/master/%E5%9B%BE%E7%89%871.png)<br/>
use system;<br/>
select * from equipment<br/>
where equipmentid=’2’;<br/>
![2](https://github.com/cxins/MIS/blob/master/%E5%9B%BE%E7%89%872.png)<br/><br/>
use system;<br/>
select * from maintain<br/>
where eqid=’3’;<br/>
![3](https://github.com/cxins/MIS/blob/master/%E5%9B%BE%E7%89%873.png)
<br/>
<br/>
use system;<br/>
select * from equipment<br/>
where timeid=(select timeid from time where timename=’月检’);<br/>
![4](https://github.com/cxins/MIS/blob/master/%E5%9B%BE%E7%89%874.png)<br/>
use system;<br/>
select eqid from record<br/>
where 365-datediff(now(),(select date1 from record))<10; <br/>
![5](https://github.com/cxins/MIS/blob/master/%E5%9B%BE%E7%89%875.png)<br/>
E-R图：<br/>
![5](https://github.com/cxins/MIS/blob/master/ER.PNG)
<br/>
Axure:http://i03ntx.axshare.com<br/>
附件：http://pan.baidu.com/s/1i45vxQX<br/>

