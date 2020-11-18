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
