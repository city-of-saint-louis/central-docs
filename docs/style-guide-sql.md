# Style Guides : SQL

## General SQL

``` sql title="Avoid the use of SELECT * where possible"
-- this is discouraged unless you really need all the columns 
-- (and even then it is better to be explicit)
select 
    *
from azteca.CA_OBJECT

-- specify only the columns you actually need
select
     ca_object_id
    ,ca_fee_id
    ,factor
    ,rate
    ,quantity
from azteca.CA_OBJECT
```


``` sql title="Include ORDER BY when using TOP"
-- BAD:
-- using TOP without ORDER BY can produce unexpected results
select top 5
    ca_object_id
from azteca.CA_OBJECT

-- GOOD:
-- including ORDER BY guarantees consistent results are returned
select top 5
     ca_object_id
from azteca.CA_OBJECT
order by ca_object_id desc
```