# WP-Cron Limitations and Best Practices

WP-Cron is well suited to many scheduled tasks, but it behaves differently from a system cron. Understanding those differences will help you write plugin callbacks that behave predictably on every site.

For how scheduling and timing work, see [Cron](https://developer.wordpress.org/plugins/cron/) and [Understanding WP-Cron Scheduling](https://developer.wordpress.org/plugins/cron/understanding-wp-cron-scheduling/).

## Performance on High-Traffic Sites

Cron tasks do not run inside the visitor's request. WordPress spawns a separate loopback request to `wp-cron.php`, and since WordPress 6.9 it does so on the `shutdown` action, after the response has already been sent to the browser. A `doing_cron` lock also prevents a new spawn more often than once every `WP_CRON_LOCK_TIMEOUT` seconds, which defaults to 60.

The cost on a busy site is therefore server load from the extra concurrent PHP processes and from the work itself, not added latency for whichever visitor happened to trigger the spawn.

To keep that load down:

- Keep scheduled task callbacks lightweight.
- Break large jobs into smaller batches across several runs rather than doing everything in one callback.
- Consider disabling the default WP-Cron behaviour and using a system cron instead, which gives you more control over when and how often tasks execute. See [Hooking WP-Cron Into the System Task Scheduler](https://developer.wordpress.org/plugins/cron/hooking-wp-cron-into-the-system-task-scheduler/).

## Writing Callbacks That Can Run More Than Once

WordPress takes two precautions against the same event running twice. A `doing_cron` transient stops a second `wp-cron.php` request from processing the queue while one is already in progress, and each event is unscheduled before its hook fires, so a later request does not see it again.

Neither precaution makes execution exactly once. The `doing_cron` lock is released after `WP_CRON_LOCK_TIMEOUT` seconds, which defaults to 60, so a callback that runs for longer no longer holds it. Events can also be triggered directly, for example with WP-CLI, while a scheduled run is in flight.

Write callbacks that are safe to repeat rather than relying on them firing once. Record what has already been processed and skip it on the next run, so a repeated call does no harm.

[info]Making a callback safe to repeat is simpler and more reliable than locking it.[/info]

If you do need a lock, claim it atomically. A `get_transient()` check followed by `set_transient()` is not safe, because two processes can pass the check before either one writes. WordPress core faces the same problem in `WP_Upgrader::create_lock()`, which relies on the unique index on `option_name` so that only one process can create the lock row. Use that as a reference if you implement your own, and make sure the lock is released even when the callback throws.

## WP-Cron on Managed Hosting

Some managed WordPress hosts handle cron execution at the server level. These hosts typically:

- Disable the default WP-Cron page-load trigger automatically.
- Run `wp-cron.php` on a fixed interval using a system cron, commonly every minute or every five minutes.

Where this is configured, cron tasks generally execute more reliably than on a standard shared host. You should not assume any particular server configuration, however. Write your cron callbacks so they work correctly regardless of how or when they are triggered, and check your hosting provider's documentation for the details of their setup.

## Debugging Cron Callbacks

For general testing methods, including WP-CLI commands and inspection functions, see [Testing of WP-Cron](https://developer.wordpress.org/plugins/cron/simple-testing/). The following covers what to do when an event is scheduled correctly but the callback misbehaves.

If a task runs but produces unexpected results, enable debug logging in `wp-config.php`:

```php
define( 'WP_DEBUG', true );
define( 'WP_DEBUG_LOG', true );
define( 'WP_DEBUG_DISPLAY', false );
```

Errors and warnings raised inside your callback are then written to `wp-content/debug.log`. Because cron runs in a separate loopback request, these errors are never displayed in the browser, so the log is often the only place they appear.

[info]A `200` response from `wp-cron.php` confirms only that the file is reachable. It returns `200` when nothing is due, when another process holds the lock, and when it exits early, so it is not a check that any task ran.[/info]

## Common Issues

| Symptom | Likely cause | Solution |
|---------|-------------|----------|
| Task never runs | `DISABLE_WP_CRON` is `true` with no system cron configured | Set up a system cron or remove the constant |
| Task runs late | Low site traffic delays execution | Use a [system cron](https://developer.wordpress.org/plugins/cron/hooking-wp-cron-into-the-system-task-scheduler/) to trigger `wp-cron.php` on a fixed interval |
| Task runs multiple times | Duplicate scheduling, or a callback that is not safe to repeat | Check `wp_next_scheduled()` before calling `wp_schedule_event()`, and make the callback safe to repeat |
| Task disappears after an update | Activation hooks do not fire when a plugin is updated, so a lost event is never restored | Do not rely on `register_activation_hook()` alone. Check `wp_next_scheduled()` on `init` and reschedule if the event is missing |

## When WP-Cron Is Not Enough

WP-Cron stores all scheduled events in a single autoloaded option, and it processes them in one pass. That is a good fit for a handful of periodic tasks, and a poor fit for a queue of thousands of jobs.

If your plugin needs guaranteed retries, task chaining, per-job status history, or high throughput, WP-Cron alone will not provide it. In that case:

- Trigger `wp-cron.php` from the system task scheduler so execution no longer depends on site traffic.
- Break work into small batches and record progress, so a run that stops partway can resume.
- Implement your own retry handling. A one-off event is unscheduled before its hook fires, so WP-Cron will not re-attempt it if the callback fails.

Several job queue libraries for WordPress provide these features. If you use one, check that it is actively maintained and that it works on the hosting environments you support.
