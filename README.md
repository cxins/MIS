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
<br/>
DROP TABLE IF EXISTS `material`;<br/>
CREATE TABLE `material` (<br/>
  `materialid` int(11) NOT NULL AUTO_INCREMENT COMMENT '材料id',<br/>
  `materialname` varchar(225) DEFAULT NULL COMMENT '材料名称',<br/>
  PRIMARY KEY (`materialid`)<br/>
) ENGINE=InnoDB AUTO_INCREMENT=4 DEFAULT CHARSET=utf8;<br/>
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
DROP TABLE IF EXISTS `time`;<br/>
CREATE TABLE `time` (<br/>
  `timeid` int(11) NOT NULL AUTO_INCREMENT,<br/>
  `timename` varchar(45) DEFAULT NULL,<br/>
  PRIMARY KEY (`timeid`)<br/>
) ENGINE=InnoDB AUTO_INCREMENT=6 DEFAULT CHARSET=utf8;
<br/>
查询：<br/>
use system;<br/>
select * from record<br/>
where maintainid=(select maintainid from maintain where eqid='4');<br/>
![1](https://github.com/cxins/MIS/blob/master/%E5%9B%BE%E7%89%871.png)<br/>
use system;<br/>
select * from equipment<br/>
where equipmentid=’2’;<br/>
<br/>
use system;<br/>
select * from maintain<br/>
where eqid=’3’;<br/>
<br/>
<br/>
use system;<br/>
select * from equipment<br/>
where timeid=(select timeid from time where timename=’月检’);<br/>
<br/>
use system;<br/>
select eqid from record<br/>
where 365-datediff(now(),(select date1 from record))<10;<br/>
E-R图：<br/>
<br/>
Axure:http://i03ntx.axshare.com<br/>
附件：http://pan.baidu.com/s/1i45vxQX<br/>

