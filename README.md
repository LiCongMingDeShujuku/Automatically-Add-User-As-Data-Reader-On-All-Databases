![CLEVER DATA GIT REPO](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/0-clever-data-github.png "李聪明的数据库")


# 在所有数据库上自动添加用户作为数据读取器
### Automatically Add User As Data Reader On All Databases
**发布-日期:  2014年05月07日 (评论)**

![##############](images/image0012.png?raw=true "#######")

## Contents

- [中文](#中文)
- [English](#English)
- [SQL Logic](#Logic)
- [Build Quality](#Build-Info)
- [Author](#Author)
- [License](#License) 


## 中文
这是一个可以在所有数据库中使用只读（数据读取器）权限自动添加用户的快速脚本。另外，此脚本将作为可以从任何其他角色中删除用户（如果存在）的预防措施。


## English
Here’s a quick script you can use to automatically add a user across all databases with Read Only ( Data Reader ) rights. Additionally; this script will remove the user ( if it exists ) from any of the other roles just as a precaution.

---
## Logic
```SQL

use master;
set nocount on
declare @my_windows_user			varchar(50)
declare @change_access_to_read_only	varchar(max)
set 	@my_windows_user        	= 'MyDomain\MyUserName'
set 	@change_access_to_read_only = ''
select  @change_access_to_read_only = @change_access_to_read_only +
'use [' + name + '];'                                                                                               + char(10) + '
create user [' + @my_windows_user + '] for login [' + @my_windows_user + '];'                           + char(10) + '
exec master..sp_dropsrvrolemember @loginame = '''   + @my_windows_user + ''',   @rolename   = ''bulkadmin'';'   + char(10) + '
exec master..sp_dropsrvrolemember @loginame = '''   + @my_windows_user + ''',   @rolename   = ''dbcreator'';'   + char(10) + '
exec master..sp_dropsrvrolemember @loginame = '''   + @my_windows_user + ''',   @rolename   = ''diskadmin'';'   + char(10) + '
exec master..sp_dropsrvrolemember @loginame = '''   + @my_windows_user + ''',   @rolename   = ''processadmin'';'    + char(10) + '
exec master..sp_dropsrvrolemember @loginame = '''   + @my_windows_user + ''',   @rolename   = ''securityadmin'';'   + char(10) + '
exec master..sp_dropsrvrolemember @loginame = '''   + @my_windows_user + ''',   @rolename   = ''serveradmin'';' + char(10) + '
exec master..sp_dropsrvrolemember @loginame = '''   + @my_windows_user + ''',   @rolename   = ''setupadmin'';'      + char(10) + '
exec master..sp_dropsrvrolemember @loginame = '''   + @my_windows_user + ''',   @rolename   = ''sysadmin'';'    + char(10) + '
exec sp_addrolemember ''db_datareader'', '''        + @my_windows_user + ''';'                      + char(10) + '
exec sp_droprolemember ''db_owner'', '''        + @my_windows_user + ''';'                          + char(10) + char(10)                   
from
      sys.databases
where
      name not in ('master', 'model', 'msdb', 'tempdb')
 
exec  (@change_access_to_read_only) --for xml path(''), type

```

[![WorksEveryTime](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://shitday.de/)

## Build-Info

| Build Quality | Build History |
|--|--|
|<table><tr><td>[![Build-Status](https://ci.appveyor.com/api/projects/status/pjxh5g91jpbh7t84?svg?style=flat-square)](#)</td></tr><tr><td>[![Coverage](https://coveralls.io/repos/github/tygerbytes/ResourceFitness/badge.svg?style=flat-square)](#)</td></tr><tr><td>[![Nuget](https://img.shields.io/nuget/v/TW.Resfit.Core.svg?style=flat-square)](#)</td></tr></table>|<table><tr><td>[![Build history](https://buildstats.info/appveyor/chart/tygerbytes/resourcefitness)](#)</td></tr></table>|

## Author

- **李聪明的数据库 Lee's Clever Data**
- **Mike的数据库宝典 Mikes Database Collection**
- **李聪明的数据库** "Lee Songming"

[![Gist](https://img.shields.io/badge/Gist-李聪明的数据库-<COLOR>.svg)](https://gist.github.com/congmingshuju)
[![Twitter](https://img.shields.io/badge/Twitter-mike的数据库宝典-<COLOR>.svg)](https://twitter.com/mikesdatawork?lang=en)
[![Wordpress](https://img.shields.io/badge/Wordpress-mike的数据库宝典-<COLOR>.svg)](https://mikesdatawork.wordpress.com/)

---
## License
[![LicenseCCSA](https://img.shields.io/badge/License-CreativeCommonsSA-<COLOR>.svg)](https://creativecommons.org/share-your-work/licensing-types-examples/)

![Lee Songming](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/1-clever-data-github.png "李聪明的数据库")

