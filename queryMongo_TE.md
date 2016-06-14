
## Formula

$$
\text{zT}=\frac{S^2\sigma}{\kappa}
$$
$$
\text{pf}=S^2\sigma 
$$
$$
\text{thermal_conduct} = \kappa - \tau\cdot\text{pf}
$$
$$
\kappa=\kappa_L+\kappa_e
$$
## Constants  
```
tau			= 1E-14
kappal		= 1.0 (for MPWorks)
droping[0]	= 1e16
droping[1]	= 1e17
droping[2]	= 1e18
droping[3]	= 1e19
droping[4]	= 1e20
droping[5]	= 1e21
droping[6]	= 1e22
```
## Mongo shell
1. `proction` of `find()` only support dictionary.
2. `0` and `1` is the only support format of logical values

## Pymongo (>3.0)
1. **`.authenticate` is needed** for MongoDBs with **password**, no mater how you build up your connection.
2. both list or dictionary is accepted for `procjection` of `find()`, but **`_id` can be only blocked with  `{"_id":0} `(`{"_id":False} `)**.
3. use `$in` as `"$in"`
4. use `1`,`0` instead of `True` or `False`

## Structure of "vasp" collection
1. A snl owns specific `snlgroup_id`, `snl.snl_id`, but either of them equals `fw_id` of the first of workflow.
2. `snlgroup_key`=`formula`+`group_space`, which is not specific for structure.
3. `snl.formlua` will help classify

### collection of tasks

### collection of boltztrap

1. No `task_type`  
2. The key of temperature is string, others are int:  
> eg. ['cond_eigs']['p']['600'][5]['eigs'][2]    

3. eigs values has been re-sorted by values.
4. all values was calculated by ten

### collection of 
## Query
1. Query can be saved as json with out `_id`, for the value of `_id` \<object>... not supported by json.

## Entry
1. The type of entry is very like dictionary, in fact, it is a list. You should `import cPickle as pickle` to saved it as `.txt` or something else, if you'd prefer to save it as a file.
2. Only the potcar is support of input.