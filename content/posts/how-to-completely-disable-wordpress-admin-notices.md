---
title: "How to Completely Disable WordPress Admin Notices"
date: 2025-01-07
---

Are you tired of seeing endless notifications cluttering your WordPress dashboard? Those pesky admin notices from plugins and themes can be distracting and sometimes even overwhelming. Today, I'll show you a practical solution to remove all WordPress admin notices permanently while following best practices.

## The Problem with WordPress Admin Notices

WordPress admin notices serve an important purpose – they keep us informed about updates, warnings, and important messages. However, when you're managing multiple websites or using several plugins, these notifications can quickly get out of hand. They can:

- Distract you from important tasks
    
- Make it harder to focus on essential dashboard elements
    
- Create a cluttered and messy admin interface
    
- Slow down your workflow significantly
    

The worst part? Some plugins bypass WordPress's standard notification system, making it challenging to manage them effectively.

## The Solution: Disable WordPress Admin Notices

I've developed a simple yet powerful code snippet that completely removes all admin notices from your WordPress dashboard. This solution is different from others because it:

- Removes ALL types of admin notices
    
- Prevents plugins from bypassing the removal
    
- Uses multiple approaches to ensure complete removal
    
- Follows WordPress coding standards
    
- Works with the latest WordPress version
    

## The Final Code

```
<?php
/**
 * Disable Admin Notices WordPress
 * Description: Completely removes all admin notices from the WordPress dashboard,
 * including core WordPress notices and those added by plugins and themes.
 * @author Faisal Ahammad <me@faisalahammad.com>
 */

/**
 * Remove all notice actions
 */
function disable_all_admin_notices() {
    remove_all_actions('admin_notices');
    remove_all_actions('all_admin_notices');
    remove_all_actions('user_admin_notices');
    remove_all_actions('network_admin_notices');
}
add_action('admin_init', 'disable_all_admin_notices', 1);

/**
 * Add CSS to hide notice elements
 */
function hide_admin_notices_css() {
    ?>
    <style>
        .notice, 
        .notice-error, 
        .notice-warning, 
        .notice-success, 
        .notice-info, 
        .updated, 
        .error, 
        .update-nag {
            display: none !important;
        }
    </style>
    <?php
}
add_action('admin_head', 'hide_admin_notices_css', 1);

/**
 * Disable notice output
 */
function return_false() {
    return false;
}
add_action('admin_notices', 'return_false', 1);
add_action('all_admin_notices', 'return_false', 1);
add_action('user_admin_notices', 'return_false', 1);
add_action('network_admin_notices', 'return_false', 1);

/**
 * Remove update nags
 */
function remove_core_update_notices() {
    remove_action('admin_notices', 'update_nag', 3);
    remove_action('admin_notices', 'maintenance_nag', 10);
}
add_action('admin_init', 'remove_core_update_notices', 1);
```

### The Code Explained

Let's break down the key components of our solution:

#### 1\. Notice Action Removal

```
function disable_all_admin_notices() {
    remove_all_actions('admin_notices');
    remove_all_actions('all_admin_notices');
    remove_all_actions('user_admin_notices');
    remove_all_actions('network_admin_notices');
}
```

This function removes all action hooks related to admin notices, preventing them from being displayed in the first place.

#### 2\. CSS-based Notice Hiding

The snippet includes CSS rules to hide any notices that might slip through:  

```
function hide_admin_notices_css() {
    ?>
    <style>
        .notice, 
        .notice-error, 
        .notice-warning, 
        .notice-success, 
        .notice-info, 
        .updated, 
        .error, 
        .update-nag {
            display: none !important;
        }
    </style>
    <?php
}
```

#### 3\. Update Nag Removal

```
function remove_core_update_notices() {
    remove_action('admin_notices', 'update_nag', 3);
    remove_action('admin_notices', 'maintenance_nag', 10);
}
```

This specifically targets and removes WordPress core update notifications.

## How to Implement the Solution

You have several options to implement this code:

### Method 1: Using Code Snippets Plugin (Recommended)

1. Install and activate the Code Snippets plugin
    
2. Navigate to **_Snippets → Add New_**
    
3. Copy the complete code
    
4. Enable "**Only run in administration area**"
    
5. Save and activate
    

![Code Snippets](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fwww.faisalahammad.com%2Fwp-content%2Fuploads%2F2025%2F01%2FCode-Snippets-scaled.jpeg "Code Snippets")

### Method 2: Via `functions.php`

You can add this code to your theme's `functions.php` file, but remember that it will stop working if you change themes.

## Performance Impact

The good news is that this solution has minimal impact on your website's performance. It only runs in the admin area and uses efficient hooks and methods to remove notices. The CSS rules are also lightweight and only loaded in the dashboard.

## Frequently Asked Questions

#### Will this remove important security notifications as well?

Yes, this will remove all notifications, including security ones. If you need to keep security notices, you'll need to modify the code to exclude specific notice types.

#### Is it safe to remove all admin notices?

While it's generally safe, you should ensure you have alternative ways to stay updated about important changes and updates on your WordPress site.

#### Will this affect my website's front end?

No, this code only affects the admin dashboard. Your website's front end remains completely unchanged.

## Conclusion

This solution provides a clean, efficient way to declutter your WordPress dashboard by removing all admin notices. While it's important to stay informed about your website's status, having a clean, distraction-free admin interface can significantly improve your workflow efficiency.

Remember to regularly check your site's updates and maintenance needs through other means if you implement this solution, as you won't receive the standard WordPress notifications anymore.

The Post previously published on my blog here: How to Completely Disable WordPress Admin Notices

Go to Source
