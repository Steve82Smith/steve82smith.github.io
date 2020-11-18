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
