# Database-Creation-Deletion-EOM

To create or delete a new pluggable database, we first open CMD and connect to an Oracle database using SQL*Plus as the SYS user with SYSDBA privileges.  
By Simply typing ‘sqlplus sys as sysdba;’or ‘sqlplus / as sysdba;’, as shown in a picture below.

 
![1](https://github.com/user-attachments/assets/b9574c0b-d99a-44aa-92a2-84f57b59553c)


## 1.CREATING A NEW PLUGGABLE DATABASES.
Before creating a new pluggable database(pdb), we fist need to know some important information as container databases name, pluggable database name, and the path for our container database, and other information by typing: show pdbs; (please always remember to add a semi coloron at the end of your query).

![2](https://github.com/user-attachments/assets/90db094f-141a-45f5-a2dd-885726da7731)

 
You may ask yourself that why do ORCLPDB is mounted? It means that pdb created is not yet open for use. To open it we use “alter pluggable database orclpdb open;” then you save the state that time it automatically opens the orclpdb is read and write in open mode as shown below.
 ![3](https://github.com/user-attachments/assets/e99357e9-4caf-4439-a4e6-9f8dbb555de4)

After this we need to display the path of our container database in order to create our new Pluggable database and we use pdbseed .
Pdbseed: this is the template we base one when creating our new pluggable database.
This query retrieves the tablespace names and file names of data 
files for the Pluggable Database (PDB).
. We use this query “SELECT CON_ID, TABLESPACE_NAME, FILE_NAME FROM
    CDB_DATA_FILES WHERE CON_ID = 3;” the result is shown below

![4](https://github.com/user-attachments/assets/288e9d1d-d762-42d7-96f2-2d036fb64af1)


At this point we can now create our pluggable database using pdbseed, we can successfully achieve this by using this query : create pluggable database plsql_class2024db
admin user sm_plsqlauca identified by 123
file_name_convert = ('C:\USERS\EDDY\DESKTOP\ORACLEAPP\ORADATA\ORCL\pdbseed\',
'C:\USERS\EDDY\DESKTOP\ORACLEAPP\ORADATA\ORCL\ORCLPDB\plsql_class2024db’);
![5](https://github.com/user-attachments/assets/6b71e592-0709-4b45-9df5-ff43340db727)

 
As you can see the pluggable database that we created has status new because it has not yet opened.
![6](https://github.com/user-attachments/assets/06b6e509-8b0f-44f5-b23a-f9488d8d7c50)

 
To open it we repeat the same thing like” alter pluggable database plsql_class2024db open;” then you save the state that time it automatically opens the plsql_class2024db is read and write in open mode as shown in above image.
## CHECKING THE CURRENT INSTANCE NAME
Checking the current instance name by using this query “alter session set container =PLSQL_CLASS2024DB;”, and we use the name of our pluggable database.
At this step we can create a new and later we grant (give) the created use all privileges,
To create a user, we use this query: (for example)
create user samillah identified by 123;
grant all privileges to samillah;
This enables us to be able to create new connection in sql developer by changing the service name. All have been shown in the picture below.
![7](https://github.com/user-attachments/assets/306e434c-66fe-4192-b985-1cbfa78743d1)

 
Connecting to Oracle PDB through Oracle SQL Developer for the Newly Created User
To check if our new pluggable databse can connect to Oracle PDB SQL Developer through PLSQL_CLASS2024DB, we first open SQL Developer >create a new connection > ..
On name : samillah , Authentication type is default , username : samillah (the user we created) , password :123 , then select service name and type PLSQL_CLASS2024DB then test it if the status is success, save it and connect.  
![7](https://github.com/user-attachments/assets/82e07677-1d64-48c3-8c48-a25903812917)

## 2.Delete the pluggable databases

I am going to create the pluggable database that I will delete because the pluggable database I have I can delete them because I will use them. To create pluggable database we repeat what we have done earlier but change the name of the pluggable database . I will name it sm_to_delete_pdb. As show below.
 ![9](https://github.com/user-attachments/assets/4cb02d48-2bc7-48a7-ab8b-258a5f60c4a0)

We all know what mounted mean. To open it, you can run the command ALTER PLUGGABLE DATABASE sm_to delete_pdb OPEN;  When you do this, the database will transition to an open state, allowing read and write access as shown below
 
![10](https://github.com/user-attachments/assets/b68efc47-ec94-4102-8f7f-7f3ce23c9c0b)

Before deleting or dropping a pluggable database , it needs to be closed by using this query:” alter pluggable database sm_to_delete_pdb close immediate;”
 
![11](https://github.com/user-attachments/assets/a54d26bd-58aa-4dc5-af59-e2254b595e88)

Then find or set the correct directory path for unplugging the sm_to delete_pdb using this query: select directory_name, directory_path from dba_directories;
 ![12](https://github.com/user-attachments/assets/d11389c9-371b-4c92-ae09-5c6e499b55ac)

Then use this path :'C:\Users\EDDY\Desktop\OracleApp\admin\orcl\dpdump\ ‘to unplug sm_to_delete_pdb including all datafiles. Using :”alter pluggable databse sm_to_delete_pdb unplug into :'C:\Users\EDDY\Desktop\OracleApp\admin\orcl\dpdump\sm_to_delete_pdb.xml;’”
 ![13](https://github.com/user-attachments/assets/40e8b9c3-23d6-4002-a03a-7ae1113df365)

Finally you can drop sm_to_delete_pdbs with all its datafiles using :”drop pluggable sm_to_delete_pdb including datafiles;”.
 ![14](https://github.com/user-attachments/assets/889c77b6-8d03-43b5-ad07-f57c139b65b9)

Lastly check if it is successfully dropped using :“select pdb_name, status from cdb_pdbs;” 
 ![15](https://github.com/user-attachments/assets/38687679-6400-4f28-89e2-8cb17ec2e12a)

As you see it is successfully dropped.
## 3. Configure Oracle Enterprise Manager
To check if I am connected to the root container we use “show con_name;” If not, we should use:
alter session set container = CDB$ROOT; and Check the current container with
select sys_context('userenv','con_name') as current_container from dual;
Then verify Configuration: After restarting, run the initial query again to
confirm the new port settings with
SELECT DBMS_XDB_CONFIG.GETHTTPPORT() AS HTTP_PORT,
DBMS_XDB_CONFIG.GETHTTPSPORT() AS HTTPS_PORT FROM dual;
![16](https://github.com/user-attachments/assets/57507acc-6015-4642-bd58-01e417255d11)

 
Since HTTP_PORT is returning 0, it confirms that the HTTP server is currently not enabled. Here’s how to proceed: Set New HTTP Port: Let's set the HTTP port to a specific value.
Choose ports that are not in use 8080 for with:
BEGIN
DBMS_XDB_CONFIG.SETHTTPPORT(8080);
END; /
After executing the above, check the port settings again:
SELECT DBMS_XDB_CONFIG.GETHTTPPORT() AS HTTP_PORT,
DBMS_XDB_CONFIG.GETHTTPSPORT() AS HTTPS_PORT
FROM dual;
 ![17](https://github.com/user-attachments/assets/b0f23820-27a6-4641-bb65-c5945d31617e)

If the ports were successfully set, restart the database to apply changes:
SHUTDOWN IMMEDIATE;
Then start it up again:
STARTUP;
Then this will display both port numbers in one result set. As shown in the above image.
select dbms_xdb_config.gethttpsport() from dual;
Later we access our ORACLE ENTREPRISE MANAGER, we can use any desired browser for us we are using chrome. Where we write in URL ‘https://localhost:5500/em/’. We click on advance and click on “continue to localhost(unsafe)”
 ![18](https://github.com/user-attachments/assets/de993d80-510b-419f-bbfc-3ac5c7bd5791)

We use Sys as username and leave container name empty and the password we use the one configured during installation.
 ![19](https://github.com/user-attachments/assets/62eae467-625f-47d7-a514-d06efa20669c)


