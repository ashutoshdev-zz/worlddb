## Open World Database _alpha_ ##

This database has multi languages country names, region names, city names and they's latitude and longitude number and country's alpha2 code ..

|countries|regions|cities|
|:--------|:------|:-----|
|246 rows |4151 rows|103818 rows|


### TABLES ###
|locales|
|:------|
|countries|region |cities |
|countryNames|regionNames|cityNames|

#### locales ####
```
CREATE TABLE `locales` (
  `locale` char(5) NOT NULL,
  `name` varchar(50) NOT NULL,
  `fullname` varchar(50) NOT NULL,
  PRIMARY KEY  (`locale`),
  UNIQUE KEY `locale` (`locale`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```


#### countries ####
```
CREATE TABLE `countries` (
  `code` char(2) NOT NULL,
  `url` varchar(50) NOT NULL,
  `name` varchar(50) NOT NULL,
  `latitude` double NOT NULL,
  `longitude` double NOT NULL,
  `regions` int(2) NOT NULL,
  PRIMARY KEY  (`code`),
  UNIQUE KEY `name` USING BTREE (`name`),
  UNIQUE KEY `url` USING BTREE (`url`),
  UNIQUE KEY `code` (`code`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```


#### countryNames ####
```
CREATE TABLE `countryNames` (
  `code` char(2) NOT NULL,
  `locale` char(5) NOT NULL,
  `name` varchar(60) NOT NULL,
  `fullname` varchar(60) NOT NULL,
  UNIQUE KEY `code` (`code`,`locale`),
  KEY `locale` (`locale`),
  CONSTRAINT `countryNames_ibfk_1` FOREIGN KEY (`code`) REFERENCES `countries` (`code`) ON DELETE CASCADE ON UPDATE CASCADE,
  CONSTRAINT `countryNames_ibfk_2` FOREIGN KEY (`locale`) REFERENCES `locales` (`locale`) ON DELETE CASCADE ON UPDATE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```


#### regions ####
```
CREATE TABLE `regions` (
  `ID` int(4) unsigned NOT NULL auto_increment,
  `country` char(2) NOT NULL,
  `code` char(3) NOT NULL,
  `url` varchar(50) NOT NULL,
  `name` varchar(50) NOT NULL,
  `latitude` double NOT NULL,
  `longitude` double NOT NULL,
  `cities` int(4) unsigned NOT NULL,
  PRIMARY KEY  (`ID`),
  UNIQUE KEY `unique` USING BTREE (`country`,`code`),
  UNIQUE KEY `unique2` USING BTREE (`country`,`name`),
  UNIQUE KEY `unique3` USING BTREE (`country`,`url`),
  KEY `country` USING BTREE (`country`),
  KEY `code` (`code`),
  CONSTRAINT `regions_ibfk_1` FOREIGN KEY (`country`) REFERENCES `countries` (`code`) ON DELETE CASCADE ON UPDATE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

#### regionNames ####
```
CREATE TABLE `regionNames` (
  `ID` int(4) unsigned NOT NULL,
  `locale` char(5) NOT NULL,
  `name` varchar(60) NOT NULL,
  `fullname` varchar(60) NOT NULL,
  UNIQUE KEY `ID` USING BTREE (`ID`,`locale`),
  KEY `locale` (`locale`),
  CONSTRAINT `regionNames_ibfk_2` FOREIGN KEY (`locale`) REFERENCES `locales` (`locale`) ON DELETE CASCADE ON UPDATE CASCADE,
  CONSTRAINT `regionNames_ibfk_3` FOREIGN KEY (`ID`) REFERENCES `regions` (`ID`) ON DELETE CASCADE ON UPDATE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```


#### cities ####
```
CREATE TABLE `cities` (
  `ID` int(8) unsigned NOT NULL auto_increment,
  `country` char(2) NOT NULL,
  `region` char(3) NOT NULL,
  `url` varchar(50) NOT NULL,
  `name` varchar(50) NOT NULL,
  `latitude` double NOT NULL,
  `longitude` double NOT NULL,
  PRIMARY KEY  (`ID`),
  UNIQUE KEY `unique` USING BTREE (`country`,`region`,`name`),
  UNIQUE KEY `unique2` USING BTREE (`country`,`region`,`url`),
  KEY `country` USING BTREE (`region`,`country`),
  CONSTRAINT `cities_ibfk_1` FOREIGN KEY (`country`) REFERENCES `countries` (`code`) ON DELETE CASCADE ON UPDATE CASCADE,
  CONSTRAINT `cities_ibfk_2` FOREIGN KEY (`region`) REFERENCES `regions` (`code`) ON DELETE CASCADE ON UPDATE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```


#### cityNames ####
```
CREATE TABLE `cityNames` (
  `ID` int(8) unsigned NOT NULL,
  `locale` char(5) NOT NULL,
  `name` varchar(60) NOT NULL,
  `fullname` varchar(60) NOT NULL,
  UNIQUE KEY `ID` (`ID`,`locale`),
  KEY `locale` (`locale`),
  CONSTRAINT `cityNames_ibfk_3` FOREIGN KEY (`ID`) REFERENCES `cities` (`ID`) ON DELETE CASCADE ON UPDATE CASCADE,
  CONSTRAINT `cityNames_ibfk_2` FOREIGN KEY (`locale`) REFERENCES `locales` (`locale`) ON DELETE CASCADE ON UPDATE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```
