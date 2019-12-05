MSSQL role
=========

Install MSSQL 2017 on an EC2 instance by default but is compatible with 2016

Requirements
------------

Needs access to an S3 bucket with the SQL ISO

Role Variables
--------------

This role has quite a few variables mostly to set the config.ini for SQL install.

***Required variables***
- `sa_password` - Specify the SA password. Normally done through Ansible vault or Tower so password does not live in VCS.

***Other variables***
- main directory
   - provision_vars.yml
      - `target_folder` - folder that is used for script execution and temp files
      - `sql_download_url` - URL to download SQL ISO. Defaults to SQL 2017 on S3 bucket
      - `iso_name` - temporary name for ISO
      - `win_features` - Windows features needed for this role
      - `sql_cu` - Dictionary with CU Information
   - sql_users.yml
     - `sql_admins` - List of users to added as sysadmins after SQL finishes installing
   - sqlserverconfig_vars.yml - This houses all the variables used in the SQL setup config.ini
     - Common variables

      ```yaml
      # SQL Agent settings
      mssql_config_agtsvcaccount: NT Service\SQLSERVERAGENT
      mssql_config_agtsvcstartup: Automatic
      # if below is defined then template will include key/value
      # mssql_config_agtsvcpassword:

      mssql_config_sqlsvcstartup: Automatic
      mssql_config_filestreamlevel: 0
      mssql_config_sqlsvcaccount: NT Service\MSSQLSERVER
      mssql_config_sqlsysadmins: DOMAIN\admin1
      mssql_config_securitymode: SQL

      mssql_config_sqldatadir: D:\Data
      mssql_config_sqlbackupdir: D:\Backup

      mssql_config_sqluserdbdir: D:\Data
      mssql_config_sqluserdblogdir: E:\Data
      # TempDB settings
      mssql_config_sqltempdbdir: F:\Data
      mssql_config_sqltempdblogdir: E:\Data
      mssql_config_sqltempdbcount: 8
      mssql_config_sqltempdb_filesize: 8192
      mssql_config_sqltempdb_filegrowth: 256
      mssql_config_sqltempdb_logfilesize: 2048
      mssql_config_sqltempdb_logfilegrowth: 64
      ```

Dependencies
------------

During server provisioning, this role should be executed after the windows role or when drives are provisioned.

Example Playbook
----------------


    - hosts: servers
      roles:
         - mssql


Author Information
------------------

GGSince88
