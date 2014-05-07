mod_auth_form sample
==========
A sample page for how to use mod_auth_form in Apache 2.4+ under Ubuntu.
It uses some javascript to detect if a login has failed to display a friendly message to the user.

### Installation 
1. Enable the required mods in Apache 
```
sudo a2enmod session
sudo a2enmod session_cookie
sudo a2enmod auth_form
```
2. Create a folder ```/var/www/login``` and copy index.html and css to that folder
3. Create a password-file with ```htpasswd -c /etc/apache2/passwords testuser```
4. Modify the Directory-tag for the DocumentRoot to contain the follow in your apache-config (```/etc/apache2/sites-enabled/000-default.conf```) 
```
<Directory /var/www/>
        AuthFormProvider file
        AuthName "authenticationform"
        AuthType form
        AuthUserFile /etc/apache2/passwords
        ErrorDocument 401 /login/index.html
        Session On
        SessionCookieName session path=/
</Directory>
```
5. Add a tag for each URL that needs to be protected by the login to the same file
```
<Directory /var/www/protected_folder>
        Require valid-user
</Directory>
```
6. Restart Apache with ```sudo /etc/init.d/apache2 restart```
