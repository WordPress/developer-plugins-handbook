# Hooking WP-Cron Into the System Task Scheduler

As previously mentioned, WP-Cron does not run continuously, which can be an issue if there are critical tasks that must run on time. There is an easy solution for this. Simply set up your system's task scheduler to run on the intervals you desire (or at the specific time needed). The easiest solution is to use a tool to make a web request to the `wp-cron.php` file.

After scheduling the task on your system, there is one more step to complete. WordPress will continue to run WP-Cron on each page load. This is no longer necessary and will contribute to extra resource usage on your server. WP-Cron can be disabled in the `wp-config.php` file. Open the `wp-config.php` file for editing and add the following line:

```
define( 'DISABLE_WP_CRON', true );
```
## Windows

Windows calls their time based scheduling system the Task Scheduler. It can be accessed via the **Administrative Tools** in the control panel.

How you setup the task varies with server setup. One method is to use PowerShell and a Basic Task. After creating a Basic Task the following command can be used to call the WordPress Cron script.

```
powershell "Invoke-WebRequest http://YOUR_SITE_URL/wp-cron.php"
```

## MacOS and Linux

Mac OS X and Linux both use cron as their time based scheduling system. It is typically access from the terminal with the `crontab -e` command. It should be noted that tasks will be run as a regular user or as root depending on the system user running the command.

Cron has a specific syntax that needs to be followed and contains the following parts:

- Minute
- Hour
- Day of month
- Month
- Day of week
- Command to execute

![Cron Scheduling](https://i3.wp.com/developer.wordpress.org/files/2014/10/plugin-wp-cron-cron-scheduling.png)

If a command should be run regardless of one of the time sections an asterisk (`*`) should be used. For example if you wanted to run a command every 15 minutes regardless of the hour, day, or month it would look like:

Many servers have `wget` installed and this is an easy tool to call the WordPress Cron script.

```
wget --delete-after https://example.com/wp-cron.php
```

[info]Without –delete-after option, wget would save the output of the HTTP GET request.[/info]

A daily call to your site's WordPress Cron that triggers at midnight every night could look similar to:

```
0 0 * * * wget --delete-after https://example.com/wp-cron.php
```