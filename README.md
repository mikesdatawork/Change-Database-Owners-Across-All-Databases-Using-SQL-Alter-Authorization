![MIKES DATA WORK GIT REPO](https://raw.githubusercontent.com/mikesdatawork/images/master/git_mikes_data_work_banner_01.png "Mikes Data Work")        

# Change Database Owners Across All Databases Using SQL Alter Authorization
**Post Date: May 5, 2014**        



## Contents    
- [About Process](##About-Process)  
- [SQL Logic](#SQL-Logic)  
- [Build Info](#Build-Info)  
- [Author](#Author)  
- [License](#License)       

## About-Process

<p>You noticed that all the database owners are set to the wrong account or person, and you need a simple way to check all databases for their database owners. Here's alittle script you can run to get all database owners across all databases.</p>      


## SQL-Logic
```SQL
select
'owner' = suser_sname(owner_sid)
, 'database name' = name
from
sys.databases
where
name not in ('master', 'model', 'msdb', 'tempdb') order by database_id asc
```
<p>So now you can see all the owners across the databases. Ok; so if you're like most enterprises you'll notice there's some inconsistencies probably. Next you're thinking; What's the best, and most current way to change all the database owners in one go? Here's where you can run the 'alter authorization' statement, but… you don't want to do it for each database one at a time. Why not ALL databases? Excluding the system databases of course.
Here's some logic to make that happen followed by the owner query above to verify. I threw in a 5 second delay between statements just for kicks.</p>      


## SQL-Logic
```SQL
use master;
set nocount on
declare @change_database_owner varchar(max)
set @change_database_owner = ''
select @change_database_owner = @change_database_owner +
'alter authorization on database::[' + replace(name, '''', '''') + '] to MyNewDBowner;' + char(10) from sys.databases where name not in ('master', 'model', 'msdb', 'tempdb') exec (@change_database_owner) --for xml path(''), type
 
waitfor delay '00:00:03'
 
select
'owner' = suser_sname(owner_sid)
, 'database name' = name
from
sys.databases
where
name not in ('master', 'model', 'msdb', 'tempdb') order by database_id asc
```


[![WorksEveryTime](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://shitday.de/)

## Build-Info

| Build Quality | Build History |
|--|--|
|<table><tr><td>[![Build-Status](https://ci.appveyor.com/api/projects/status/pjxh5g91jpbh7t84?svg?style=flat-square)](#)</td></tr><tr><td>[![Coverage](https://coveralls.io/repos/github/tygerbytes/ResourceFitness/badge.svg?style=flat-square)](#)</td></tr><tr><td>[![Nuget](https://img.shields.io/nuget/v/TW.Resfit.Core.svg?style=flat-square)](#)</td></tr></table>|<table><tr><td>[![Build history](https://buildstats.info/appveyor/chart/tygerbytes/resourcefitness)](#)</td></tr></table>|

## Author

[![Gist](https://img.shields.io/badge/Gist-MikesDataWork-<COLOR>.svg)](https://gist.github.com/mikesdatawork)
[![Twitter](https://img.shields.io/badge/Twitter-MikesDataWork-<COLOR>.svg)](https://twitter.com/mikesdatawork)
[![Wordpress](https://img.shields.io/badge/Wordpress-MikesDataWork-<COLOR>.svg)](https://mikesdatawork.wordpress.com/)

    
## License
[![LicenseCCSA](https://img.shields.io/badge/License-CreativeCommonsSA-<COLOR>.svg)](https://creativecommons.org/share-your-work/licensing-types-examples/)

![Mikes Data Work](https://raw.githubusercontent.com/mikesdatawork/images/master/git_mikes_data_work_banner_02.png "Mikes Data Work")

