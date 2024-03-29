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

#Fixing WordPress indexes, foreign keys and auto_increment fields

I recently migrated a number of WordPress websites and a custom PHP website from AWS RDS to MySQL on an EC2 instance using the AWS Database Migration Service. I didn’t know beforehand that secondary indexes, foreign keys, and auto_increment fields aren’t migrated – this made a heck of a mess, with me unable to post, upload images, or do much else. This led to me fixing WordPress indexes, foreign keys and auto_increment fields.

https://www.photographerstechsupport.com/tutorials/fixing-wordpress-indexes-foreign-keys-and-auto_increment-fields/

    ALTER TABLE wp_termmeta MODIFY COLUMN meta_id bigint(20) unsigned NOT NULL auto_increment;
    ALTER TABLE wp_terms MODIFY COLUMN term_id bigint(20) unsigned NOT NULL auto_increment;
    ALTER TABLE wp_term_taxonomy MODIFY COLUMN term_taxonomy_id bigint(20) unsigned NOT NULL auto_increment;
    ALTER TABLE wp_commentmeta MODIFY COLUMN meta_id bigint(20) unsigned NOT NULL auto_increment;
    ALTER TABLE wp_comments MODIFY COLUMN comment_ID bigint(20) unsigned NOT NULL auto_increment;
    ALTER TABLE wp_links MODIFY COLUMN link_id bigint(20) unsigned NOT NULL auto_increment;
    ALTER TABLE wp_options MODIFY COLUMN option_id bigint(20) unsigned NOT NULL auto_increment;
    ALTER TABLE wp_postmeta MODIFY COLUMN meta_id bigint(20) unsigned NOT NULL auto_increment;
    ALTER TABLE wp_users MODIFY COLUMN ID bigint(20) unsigned NOT NULL auto_increment;
    ALTER TABLE wp_posts MODIFY COLUMN ID bigint(20) unsigned NOT NULL auto_increment;
    ALTER TABLE wp_usermeta MODIFY COLUMN umeta_id bigint(20) unsigned NOT NULL auto_increment;

    CREATE INDEX term_id on wp_termmeta (term_id);
    CREATE INDEX meta_key on wp_termmeta (meta_key(191));
    CREATE INDEX slug on wp_terms (slug(191));
    CREATE INDEX name on wp_terms (name(191));
    CREATE UNIQUE INDEX term_id_taxonomy on wp_term_taxonomy (term_id, taxonomy);
    CREATE INDEX taxonomy on wp_term_taxonomy (taxonomy );
    CREATE INDEX comment_id on wp_commentmeta (comment_id);
    CREATE INDEX meta_key on wp_commentmeta (meta_key(191));
    CREATE INDEX comment_post_ID on wp_comments (comment_post_ID);
    CREATE INDEX comment_approved_date_gmt on wp_comments (comment_approved,comment_date_gmt);
    CREATE INDEX comment_date_gmt on wp_comments (comment_date_gmt);
    CREATE INDEX comment_parent on wp_comments (comment_parent);
    CREATE INDEX comment_author_email on wp_comments (comment_author_email(10));
    CREATE INDEX link_visible on wp_links (link_visible);
    CREATE UNIQUE INDEX option_name on wp_options (option_name);
    CREATE INDEX post_id on wp_postmeta (post_id);
    CREATE INDEX meta_key on wp_postmeta (meta_key);
    CREATE INDEX post_name on wp_posts (post_name(191));
    CREATE INDEX type_status_date on wp_posts (post_type,post_status,post_date,ID);
    CREATE INDEX post_parent on wp_posts (post_parent);
    CREATE INDEX post_author on wp_posts (post_author);
    CREATE INDEX user_login_key on wp_users (user_login);
    CREATE INDEX user_nicename on wp_users (user_nicename);
    CREATE INDEX user_email on wp_users (user_email);
    CREATE INDEX user_id on wp_usermeta (user_id);
    CREATE INDEX meta_key on wp_usermeta (meta_key(191));

    ALTER TABLE wp_terms AUTO_INCREMENT = 10000;
    ALTER TABLE wp_term_taxonomy AUTO_INCREMENT = 10000;
    ALTER TABLE wp_commentmeta AUTO_INCREMENT = 10000;
    ALTER TABLE wp_comments AUTO_INCREMENT = 10000;
    ALTER TABLE wp_links AUTO_INCREMENT = 10000;
    ALTER TABLE wp_options AUTO_INCREMENT = 10000;
    ALTER TABLE wp_postmeta AUTO_INCREMENT = 10000;
    ALTER TABLE wp_users AUTO_INCREMENT = 10000;
    ALTER TABLE wp_posts AUTO_INCREMENT = 10000;
    ALTER TABLE wp_usermeta AUTO_INCREMENT = 10000;
