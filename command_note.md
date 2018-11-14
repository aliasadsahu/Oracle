#     Comment check database on Oracle 

1. Check Capacity database
```
su - oracle 
echo $ORACLE_SID
If you want to check another database not default
export ORACLE_SID=devmradio 

SELECT ROUND (SUM (aaa), 2) || ' GB'
  FROM (SELECT SUM (bytes) / 1024 / 1024 / 1024 aaa FROM dba_data_files
        UNION ALL
        SELECT SUM (bytes) / 1024 / 1024 / 1024 aaa FROM dba_temp_files
        UNION ALL
        SELECT SUM (bytes * members) / 1024 / 1024 / 1024 aaa FROM v$log);  
```
2. Increase tablespace
```
ORA-39171: Job is experiencing a resumable wait.
ORA-01688: unable to extend table MRADIO.MYSUBREGISTERTRAN partition SYS_P701 by 128 in tablespace USERS

fix:
alter tablespace USERS add datafile '/data/users02.dbf' size 10m autoextend on;
```
3. Turn off datafile old 
```
select name from v$datafile;
alter database datafile '/u01/app/oracle/oradata/devmradio/users01.dbf' autoextend off;
Note: only users01.dbf 
SQL> select name from v$datafile;

NAME
--------------------------------------------------------------------------------
/u01/app/oracle/oradata/devmradio/system01.dbf
/u01/app/oracle/oradata/devmradio/sysaux01.dbf
/u01/app/oracle/oradata/devmradio/undotbs01.dbf
/u01/app/oracle/oradata/devmradio/users01.dbf

Don't turn off system01, sysaux01 and undotbs01 
```
4. Delete user 
```
DROP USER vasgate CASCADE;
```
5. Delete alert log
```
/opt/app/oracle/diag/tnslsnr/devmradio/listener/alert
```
6. Show user,DEFAULT_TABLESPACE,TEMPORARY_TABLESPACE
```
select USERNAME, DEFAULT_TABLESPACE, TEMPORARY_TABLESPACE 
	  from DBA_USERS
	  where USERNAME = 'MRADIO';
```
7.