# htaccess
Htaccess (HyperText Access) is a simple configuration file that allows designers, developers and programmers alike to alter the configuration of the Apache Web Server in order to provide additional functionality. Such functionality can include redirecting users, URL re-writes and providing password-protected directories; but it can do so much more.

## Using .htaccess
It is important to remember that an .htaccess file will affect the directory it is placed in and all resulting sub-directories. Therefore, if you add your ‘.htaccess’ file to the ‘web site root’ then it will affect all subsequent folders like so:

```
http://www.yourdomain.com/
| -- directory1
| -- directory2
| -- directory3
|    | -- directory3/childdirectory1
|    | -- directory3/childdirectory2
| -- .htaccess
| -- index.html
```

However, if you place the ‘.htaccess’ file in http://www.yourdomain.com/directory1 then the features of the ‘.htaccess’ will be restricted to that folder and all child folders only. For example:

```
http://www.yourdomain.com/
| -- directory1
|    | -- directory1/childdirectory1
|    | -- directory1/childdirectory2
|    | -- directory1/childdirectory3
|    |    | -- directory1/childdirectory3/newdirectory1
|    |    | -- directory1/childdirectory3/newdirectory2
|    | -- .htaccess
|    | -- index.html
```

After editing your .htaccess file on multiple occassions it may look a little complicated so I would recommend implementing comments. To do this, simply place the hash symbol at the beginning of every line like so:

```
# comment here
# another comment here
```

## Useful Snippets

And to get you started, it’s snippet time …
(although one or two of them are strictly directives for Apache)

### Directory Index

You can change a default index file of directory with:

```
DirectoryIndex welcome.html welcome.php
```

### Custom Error Pages

You can redirect your users to an error page with:

```
ErrorDocument 404 error.html
```

And you can extend this like so:

```
ErrorDocument 400 /400.html
ErrorDocument 401 /401.html
ErrorDocument 403 /403.html
ErrorDocument 404 /404.html
ErrorDocument 500 /500.html
ErrorDocument 502 /502.html
ErrorDocument 504 /504.html
```

But remember to create your error pages!

### Remove the Need for www in Your URL

Keep your site consistent by removing the need for ‘www’ by using:

```
RewriteEngine On
RewriteBase /
RewriteCond %{HTTP_HOST} ^www.yourdomain.com [NC]
RewriteRule ^(.*)$ http://yourdomain.com/$1 [L,R=301]
```

### Set the Time Zone for Your Server

```
SetEnv TZ Europe/London
```

### Control Access to Files

Most people will remember that .htaccess is most often used to restrict or deny access to individual files and folders and you can do this like so:

```
deny from all
```

However, if you would like to be more specific and ban a specific IP address then you could use:

```
order allow,deny
deny from XXX.XXX.XXX.XXX
allow from all
```

or alternatively for several IP addresses, you could use:

```
allow from all
deny from 145.186.14.122
deny from 124.15
```

### 301 Permanent Redirects

Worried about those old links? Then try:

```
Redirect 301 /olddirectory/file.html http://www.domainname.com/newdirectory/file.html
```

Set the Email Address for the Server Administrator

By using the following code you can specify the default email address for the server administrator:

```
ServerSignature EMail
SetEnv SERVER_ADMIN webmaster@domain.com
```

### Detecting Tablets and Redirecting

If you would like to redirect tablet-based users to a particular web page or directory, try:

```
RewriteCond %{HTTP_USER_AGENT} ^.*iPad.*$
RewriteRule ^(.*)$ http://yourdomain.com/folderfortablets [R=301]
RewriteCond %{HTTP_USER_AGENT} ^.*Android.*$
RewriteRule ^(.*)$ http://yourdomain.com/folderfortablets [R=301]
```

### Link Protection

Concerned about hotlinking or simply want to reduce your bandwidth usage? Try experimenting with:

```
Options +FollowSymlinks
RewriteEngine On
RewriteCond %{HTTP_REFERER} !^$
RewriteCond %{HTTP_REFERER} !^http://(www.)?domainname.com/ [nc]
RewriteRule .*.(gif|jpg|png)$ http://domainname.com/img/hotlink_f_o.png [nc]
```

### Force “File Save As”

If you would like force users to download files rather than view them in the browser you could use:

```
AddType application/octet-stream .csv
AddType application/octet-stream .xls
AddType application/octet-stream .doc
AddType application/octet-stream .avi
AddType application/octet-stream .mpg
AddType application/octet-stream .mov
AddType application/octet-stream .pdf
```

or you simplify this as:

```
AddType application/octet-stream .avi .mpg .mov .pdf .xls .mp4
```

### Rewrite URLs

If you would like to make your URLs a little easier to read (ie changing content.php?id=92 to content-92.html) you could implement the following ‘rewrite’ rules:

```
RewriteEngine on
RewriteRule ^content-([0-9]+).html$ content.php?id=$1
```

### Redirect Browser to https

This is always useful for those who have just installed an SSL certificate:

```
RewriteEngine On
RewriteCond %{HTTPS} !on
RewriteRule (.*) https://%{HTTP_HOST}%{REQUEST_URI}
```

### Activate SSI

If you want to activate SSI for HTML and or SHTML file types, try:

```
AddType text/html .html
AddType text/html .shtml
AddHandler server-parsed .html
AddHandler server-parsed .shtml
AddHandler server-parsed .htm
```

### Disable or Enable Directory browsing

```
# disable directory browsing
Options All -Indexes
# enable directory browsing
Options All +Indexes
```

### Change the Charset and Language headers

For those who want to change the current character set and language for a specific directory use:

```
AddDefaultCharset UTF-8
DefaultLanguage en-GB
```

### Block Unwanted Referrals

If you want to block unwanted visitors from a particular website or range of websites you could use:

```
<IfModule mod_rewrite.c>
 RewriteEngine on
 RewriteCond %{HTTP_REFERER} website1.com [NC,OR]
 RewriteCond %{HTTP_REFERER} website2.com [NC,OR]
 RewriteRule .* - [F]
</ifModule>
```

### Block Unwanted User Agents

With the following method, you could save your bandwidth by blocking certain bots or spiders from trawling your website:

```
<IfModule mod_rewrite.c>
SetEnvIfNoCase ^User-Agent$ .*(bot1|bot2|bot3|bot4|bot5|bot6|) HTTP_SAFE_BADBOT
SetEnvIfNoCase ^User-Agent$ .*(bot1|bot2|bot3|bot4|bot5|bot6|) HTTP_SAFE_BADBOT
Deny from env=HTTP_SAFE_BADBOT
</ifModule>
```

### Block Access to a Comprehensive Range of Files

If you want to protect particular files, or even block access to the .htaccess file, try customising the following code:

```
<Files privatefile.jpg>
 order allow,deny
 deny from all
</Files>

<FilesMatch ".(htaccess|htpasswd|ini|phps|fla|psd|log|sh)$">
 Order Allow,Deny
 Deny from all
</FilesMatch>
```

### And Lastly …

For reasons of security alone, I think the chance to rename the .htaccess file is very useful:

```
AccessFileName ht.access
```
