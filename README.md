# Welcome to GitHub Pages

You can use the [editor on GitHub](https://github.com/Steve82Smith/steve82smith.github.io/edit/main/README.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/Steve82Smith/steve82smith.github.io/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.


test1 <tips/test.html>

## MsSql Tips

#### Start SQL Server Agent when ‘Agent XPs disabled’

"Agent XPs" option in SQL Server is used to enable or disable the SQL Server Agent extended stored procedures. When you start the SQL Server Agent service from SQL Server Management Studio, this option is enabled automatically.
With "Agent XPs" disabled, the contents of the SQL Server Agent node is not visible in Management Studio Object Explorer, even if the SQL Server Agent service is started.

To see the state of "Agent XPs" option, run the folowing code:

    EXEC sp_configure 'show advanced options', 1
    GO
    RECONFIGURE
    GO
    EXEC sp_configure 'Agent XPs'

To enable the SQL Server Agent extended stored procedures:

    EXEC sp_configure 'show advanced options', 1
    GO
    RECONFIGURE
    GO
    EXEC sp_configure 'Agent XPs', 1
    GO
    RECONFIGURE
    GO

The setting takes effect without a server restart. To see the state of "Agent XPs" option again:

    EXEC sp_configure 'show advanced options', 1
    GO
    RECONFIGURE
    GO
    EXEC sp_configure 'Agent XPs'

#### How to query all databases sizes?

    select t1.name, t2.name, t2.physical_name,ROUND(t2.size/131072.0,3),t2.state_desc,t3.name, t3.physical_name,ROUND(t3.size/131072.0,3),t3.state_desc
    from 
    master.dbo.sysdatabases as t1 left join 
    (select database_id, name, physical_name, size, state_desc from sys.master_files where type_desc='ROWS') as t2 on t1.dbid=t2.database_id
    left join
    (select database_id, name, physical_name, size, state_desc from sys.master_files where type_desc='LOG') as t3 on t2.database_id=t3.database_id


## Exchange Tips

### Smtp Errors

#### 504 5.7.4 Unrecognized authentication type

Starting in Exchange 2010, the only authentication mechanism enabled is NTLM.

2 solutions:

a) tell your app to use the NTLM authentication scheme.

b) Enable AuthLogin authenticaton on Exchange:

In the Exchange console under server configuration:

Select hub transport.

Right click the client server and select properties.

Select the authentication tab.

Check the Basic Authentication checkbox.

Uncheck the Offer Basic only after TLS

May have to restart the Exchange services.

#### 550 5.7.1 Client does not have permission to send as this sender

This error happens because the FROM address, the customer was using, was different than the Exchange mailbox they were relaying through.
Like the error message implies, this is a permissions issue. Go to:

a)From an Exchange Command prompt, run the following command:

Add-AdPermission -Identity “Default Receive Connector” -User “NT AUTHORITY\Authenticated Users” -ExtendedRights ms-Exch-SMTP-Accept-Any-Sender

b) On the user account, in Active Directory, under Security, under the SELF account, select the Manage Send As Permission option.

## Oracle Tips

#### Readonly user

A user in an Oracle database only has the privileges you grant. So you can create a read-only user by simply not granting any other privileges.

When you create a user

    CREATE USER ro_user
    IDENTIFIED BY ro_user
    DEFAULT TABLESPACE users
    TEMPORARY TABLESPACE temp;

the user doesn't even have permission to log in to the database. You can grant that

    GRANT CREATE SESSION to ro_user

and then you can go about granting whatever read privileges you want. For example, if you want RO_USER to be able to query SCHEMA_NAME.TABLE_NAME, you would do something like

    GRANT SELECT ON schema_name.table_name TO ro_user

Generally, you're better off creating a role, however, and granting the object privileges to the role so that you can then grant the role to different users. Something like

Create the role

    CREATE ROLE ro_role;

Grant the role SELECT access on every table in a particular schema

    BEGIN
      FOR x IN (SELECT * FROM dba_tables WHERE owner='SCHEMA_NAME')
      LOOP
        EXECUTE IMMEDIATE 'GRANT SELECT ON schema_name.' || x.table_name || ' TO ro_role';
      END LOOP;
    END;

And then grant the role to the user  
    GRANT ro_role TO ro_user;

## Useful Links

Установка и настройка Zabbix 5.0 <https://serveradmin.ru/ustanovka-i-nastrojka-zabbix-5-0>

Install the Microsoft ODBC driver for SQL Server (Linux) <https://docs.microsoft.com/ru-ru/sql/connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server?view=sql-server-ver15#ubuntu17>

Configure Reverse Proxies Using Apache2 HTTP Server On Ubuntu 18.04 <https://websiteforstudents.com/configure-reverse-proxies-using-apache2-http-server-on-ubuntu-18-04/>

Настройка Reverse Proxy Apache (Debian 8) с автоматической выдачей Let's Encrypt <https://habr.com/ru/post/330670/>

Как преобразовать физический сервер Linux в виртуальную машину VMware <https://wiki.merionet.ru/servernye-resheniya/54/kak-preobrazovat-fizicheskij-server-linux-v-virtualnuyu-mashinu-vmware/>

Восстановление загрузчика Grub 2 в Linux Ubuntu <http://tmel.ru/vosstanovlenie-zagruzchika-grub-2-v-linux-ubuntu/>

Converting Linux server OpenSUSE 13.2 with kernel 3.16.7-29-desktop error: installGrub.sh failed with return code: 127, and message: FATAL: kernel too old <https://communities.vmware.com/message/2844710#2844710>

Настройка сервера Radius в Windows Server 2016 <https://vmblog.ru/nastrojka-servera-radius-v-windows-server/>

Настройка PPTP Сервера на MikroTik <https://mikrotiklab.ru/nastrojka/artga-pptp-servera.html>

Mikrotik (vpn server) + Windows server 2008r2 (ad, radius server) <https://habr.com/en/sandbox/58551/>

Access Control List - списки контроля доступа <https://help.ubuntu.ru/wiki/access_control_list>

How to Create MySQL Users Accounts and Grant Privileges <https://linuxize.com/post/how-to-create-mysql-user-accounts-and-grant-privileges/>

How to reset the MySQL root password <https://www.a2hosting.com/kb/developer-corner/mysql/reset-mysql-root-password>

Remote Desktop Services - Используем перенаправление профилей пользователей и перемещаемые папки <https://blog.it-kb.ru/2011/12/23/remote-desktop-services-rds-roaming-user-profiles-and-folder-redirection-gpo-settings/>

Windows Server 2012 R2 Remote Desktop Services - Настраиваем пользовательский интерфейс на серверах RD Session Host <https://blog.it-kb.ru/2014/09/02/windows-server-2012-r2-remote-desktop-services-hide-folders-music-video-in-my-computer-name-space-and-copy-and-lock-start-screen-settings-for-all-users-on-rd-session-host/>

Remote Desktop Services - Настраиваем пользовательский интерфейс на серверах RD Session Host <https://blog.it-kb.ru/2013/09/10/remote-desktop-rd-session-host-user-interface-settings-for-windows-explorer-profile-folders-control-panel-taskbar-modern-ui-start-screen-in-group-policy-preferences/#p11>

SQL Server Database Stuck in Restoring State <https://www.mssqltips.com/sqlservertip/5460/sql-server-database-stuck-in-restoring-state/>

ALTER DATABASE failed because a lock could not be placed on database ‘dbname’. Try again later. <https://sqlzealots.com/2018/07/20/alter-database-failed-because-a-lock-could-not-be-placed-on-database-dbname-try-again-later/>

Live monitoring queries to troubleshoot issues in sql server <https://sqlzealots.com/2015/02/24/live-monitoring-queries-to-troubleshoot-issues-in-sql-server/>

How to delete remote branches in Git <https://www.educative.io/edpresso/how-to-delete-remote-branches-in-git>
