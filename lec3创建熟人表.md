##创建熟人表

declare acquaintance table (f1 char(1),f2 char(2),class varchar(10));
insert into acquaintance
 select 'a','b','reading' union all select 'a','c','reading' union all
 select 'a','b','soccer' union all select 'c','d','soccer' union all
 select 'd','e','soccer' union all select 'e','f','soccer' union all
 select 'b','a','drinking' union all select 'c','e','drinking' union all
 select 'd','e','drinking';
  
-- 1
;with t as (
select f1 m from acquaintance
union 
select f2 from acquaintance
)
select t1.m m1, t2.m m2
from t t1, t t2
where t1.m<>t2.m
and not exists (select * from acquaintance where (f1=t1.m and f2=t2.m) or (f2=t1.m and f1=t2.m));
 
-- 2
;with t as (
select f1 m,class from acquaintance
union 
select f2,class from acquaintance
)
select m from t t1 where not exists(select * from t where m=t1.m and class<>t1.class);
 
-- 3
select f1,f2,class from acquaintance t1
where not exists (select * from acquaintance t2
where not exists(select * from acquaintance where class=t2.class 
and ((f1=t1.f1 and f2=t1.f2) or (f1=t1.f2 and f2=t1.f1))));
