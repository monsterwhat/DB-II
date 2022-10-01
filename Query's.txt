=============System-Test==================
create user progra identified by progra
default tablespace users
temporary tablespace temp
quota unlimited on users;

grant create session, create table, create procedure to progra;
===========================================

Conexión sys:

username: sys as sysdba
password: test 
TNS: test 

alter user system identified by test;
alter user system account unlock;

===========================================

DECLARE
    x  NUMBER := 100;
BEGIN
    FOR i IN 1..10 LOOP
        IF MOD(i,2) = 0 THEN     -- i is even
            INSERT INTO temp VALUES (i, x, 'i is even');
        ELSE
            INSERT INTO temp VALUES (i, x, 'i is odd');
        END IF;

        x := x + 100;        
    END LOOP;
    COMMIT ;
END;


select *
from temp;

select *
from emp;

SELECT ename, empno, sal FROM emp
ORDER BY sal DESC; 

delete temp;

===========================================

DECLARE
    CURSOR c1 is
        SELECT ename, empno, sal FROM emp
        ORDER BY sal DESC;   -- start with highest-paid employee
    my_ename  CHAR(10);
    my_empno  NUMBER(4);
    my_sal    NUMBER(7,2);

BEGIN
    OPEN c1;

    FOR i IN 1..5 LOOP
        FETCH c1 INTO my_ename, my_empno, my_sal;
        EXIT WHEN c1%NOTFOUND;   /* in case the number requested is more *
                                  * than the total number of employees   */
        INSERT INTO temp VALUES (my_sal, my_empno, my_ename);
        COMMIT;
    END LOOP;

    CLOSE c1;
END;
/

select *
from temp;

===========================================


DECLARE
    x        NUMBER := 0;
    counter  NUMBER := 0;
BEGIN
    FOR i IN 1..4 LOOP
        x := x + 1000;
        counter := counter + 1;
        INSERT INTO temp VALUES (x, counter, 'in OUTER loop');

        /* start an inner block */
        DECLARE
            x  NUMBER := 0;   -- this is a local version of x
        BEGIN
            FOR i IN 1..4 LOOP
                x := x + 1;   -- this increments the local x
                counter := counter + 1;
                INSERT INTO temp VALUES (x, counter, 'inner loop');
            END LOOP;
        END;

    END LOOP;
    COMMIT;
END;

select *
from temp;


select *
from accounts;

select *
from action;


update accounts
set bal = 0  
where account_id = 20;


insert into accounts values (1,455);

===========================================

create or replace procedure ejemplo1 as
    x  NUMBER := 100;
BEGIN
    FOR i IN 1..10 LOOP
        IF MOD(i,2) = 0 THEN     -- i is even
            INSERT INTO temp VALUES (i, x, 'i is even');
        ELSE
            INSERT INTO temp VALUES (i, x, 'i is odd');
        END IF;

        x := x + 100;        
    END LOOP;
    COMMIT;
END;

execute ejemplo1

===============FROM SYS====================

grant  DEBUG CONNECT SESSION, DEBUG ANY PROCEDURE to progra;
grant execute on DBMS_DEBUG_JDWP to progra;

===========================================

create user sc_tablas identified by sc_tablas
default tablespace users
temporary tablespace temp
quota unlimited on users;

grant create session, create table to sc_tablas;

select username, default_tablespace, temporary_tablespace
from dba_users
where username like '%TABLAS';
    
# Revisar cuotas
select *
from dba_ts_quotas
where username like '%TABLAS';


# Revisar permisos de sistema
select *
from dba_sys_privs
where grantee like '%TABLAS';

===========================================
    
create user se_tablas identified by se_tablas
default tablespace users
temporary tablespace temp
quota unlimited on users;


grant create session, create table to se_tablas;

===========================================

create user sm_tablas identified by sm_tablas
default tablespace users
temporary tablespace temp
quota unlimited on users;

grant create session, create table to sm_tablas;

===========================================
