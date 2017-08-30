# Convert Wordpress Posts to Kirby CMS 

This is a script based on @greywillfade's repo to convert a Wordpress WPXML / RSS file to a flat file YML Structure for Kirby CMS (http://getkirby.com).

## Upgrades

I added featured images to the articles export (index.php) and the event export (index-events.php). **Note** You will need to modify your Wordpress core files to include the featured image meta data in the WPXML/RSS file.

I also made an "event" specific export for the Tribes Event Calendar plugin that's commonly used for Wordpress. It also required the core file change (same file, no need for two changes).

## How to use

-Modify your Wordpress core files (see below)
-Create a Wordpress export file of your posts (or events)
-Add your export directory and XML file to the variables up top.
-Create the export directory on your server and CHMOD it 777 to ensure writeability (localhost/wpxml-to-kirby/your-dir)
-Upload scripts to same folder as export directory (localhost/wpxml-to-kirby)
-Run the script!


## Modify Core Files

Open up **wp-admin/includes/export.php**

Find this bit of code around line 542-544:

`<wp:post_type><?php echo wxr_cdata( $post->post_type ); ?></wp:post_type>`
`<wp:post_password><?php echo wxr_cdata( $post->post_password ); ?></wp:post_password>`
`<wp:is_sticky><?php echo intval( $is_sticky ); ?></wp:is_sticky>`

Add this below

`<?php	if ( has_post_thumbnail($post->ID) ) : ?>`
`<?php $image =  wp_get_attachment_image_src(get_post_thumbnail_id($post->ID), 'full') ?>`
`<wp:attachment_url><?php echo wxr_cdata( $image[0] ); ?></wp:attachment_url>`
`<?php 	endif; ?>`

After that all those WPXML/RSS files will have a link to your full-sized featured image available to either download or link remotely. You can obviously modify the get_post_thumbnail_id function to get any size you desire, and add more XML objects for different sizes.

Here's my standard "get-it-done" disclaimer: This was quick and dirty, I'm sure there are more elegant ways to handle this. If you're migrating off Wordpress, I'm sure core file modifications probably won't matter. *But don't take it as good practice.*
