# Changing-Wordpress-Domain-from-old-to-new-manually

Open the PHPMyAdmin panel and log in.
Click the WordPress database.
For replacing the URL across all database tables, Click on SQL tab and in the panel type the below code:

    UPDATE wp_options SET option_value = replace(option_value, 'Existing URL', 'New URL') WHERE option_name = 'home' OR option_name = 'siteurl';

    UPDATE wp_posts SET post_content = replace(post_content, 'Existing URL', 'New URL');

    UPDATE wp_postmeta SET meta_value = replace(meta_value,'Existing URL','New URL');

    UPDATE wp_usermeta SET meta_value = replace(meta_value, 'Existing URL','New URL');

    UPDATE wp_links SET link_url = replace(link_url, 'Existing URL','New URL');

    UPDATE wp_comments SET comment_content = replace(comment_content , 'Existing URL','New URL');
    
    
    
    
 # 301 REDIRECT HTTP TO HTTPS AND NON-WWW TO WWW
<IfModule mod_rewrite.c>
RewriteEngine On
RewriteCond %{HTTPS} off [OR]
RewriteCond %{HTTP_HOST} !^www\. [NC]
RewriteCond %{HTTP_HOST} ^(?:www\.)?(.+)$ [NC]
RewriteRule ^ https://www.%1%{REQUEST_URI} [L,NE,R=301]
</IfModule>
