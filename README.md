# apache-conf
Instructions and sample Apache .conf file

# Where is my Apache configuration file?

    locate apache | grep apache2\.conf

It's probably something like `/etc/apache2/apache2.conf`

# Edit the file

Use `nano /etc/apache2/apache2.conf` or `vi /etc/apache2/apache2.conf` to edit the file.

Make sure you have the following (replace the IP, domain, and port (80) as necessary):

    Include sites-enabled/
    
    NameVirtualHost *:80
    ServerName 8.8.8.8
    ServerName example.com
    
    AddType application/x-httpd-php .html

# Edit your site-specific configuration

I have several different websites (with different domains) on my site. For each, I have a file called `/etc/apache2/sites-available/website.com` that looks something like:

    <VirtualHost *:80>
            ServerName paulkaefer.com
            ServerAlias www.paulkaefer.com
            ServerAdmin webmaster@localhost
    
            ErrorDocument 404 /404.html
    
            Redirect permanent /useful/ http://paulkaefer.com/useful.txt
    
            DocumentRoot /var/www
            <Directory />
                    Options FollowSymLinks
                    AllowOverride All
    
                    Options +Includes
                    AddType text/html .shtml
                    AddOutputFilter INCLUDES .shtml
    
            </Directory>
            <Directory /var/www/>
                    Options Indexes FollowSymLinks MultiViews
                    AllowOverride All
                    Order allow,deny
                    allow from all
            </Directory>
    </VirtualHost>
    
    <VirtualHost *:80>
            DocumentRoot /var/www/haiku
            ServerName haiku.paulkaefer.com
    </VirtualHost>

As you can imagine, there are different <VirtualHost>s for each subdomain (e.g., dice.paulkaefer.com or ip.paulkaefer.com).

# Now what?

Next, add the site(s) that have configuration files with:

    sudo a2ensite /etc/apache2/sites-available/website.com

Now, make sure to reload the Apache configuration and restart the server:

    sudo service apache2 reload
    sudo service apache2 restart


