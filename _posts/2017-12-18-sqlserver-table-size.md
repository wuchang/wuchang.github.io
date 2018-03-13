---
layout: post
title:  "SQL Server查看所有表大小，所占空间"
date:   2017-12-18 10:00:00
categories: sqlserver


#SQL Server查看所有表大小，所占空间

```sql
create table #Data(name varchar(100),row varchar(100),reserved varchar(100),data varchar(100),index_size varchar(100),unused varchar(100)) 
 
declare @name varchar(100) 
declare cur cursor  for 
    select name from sysobjects where xtype='u' order by name 
open cur 
fetch next from cur into @name 
while @@fetch_status=0 
begin 
    insert into #data 
    exec sp_spaceused   @name 
    print @name 
 
    fetch next from cur into @name 
end 
close cur 
deallocate cur 
 
create table #DataNew(name varchar(100),row int,reserved int,data int,index_size int,unused int) 
 
insert into #dataNew 
select name,convert(int,row) as row,convert(int,replace(reserved,'KB','')) as reserved,convert(int,replace(data,'KB','')) as data, 
convert(int,replace(index_size,'KB','')) as index_size,convert(int,replace(unused,'KB','')) as unused from #data  
 
select * from #dataNew order by data desc    
drop table #dataNew 
drop table #data
```
