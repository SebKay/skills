# Cutover and Retirement

## DNS Preparation

- Read Cloud's exact record names, types, targets and proxy mode for the custom hostname. Keep the old routing values for rollback. Use pre-verification where supported and coordinate ownership or certificate records in advance. Some already-proxied Cloudflare setups require real-time verification. Budget for it in the maintenance window. [Laravel Cloud domains](https://laravel.com/cloud/docs/domains.md)
- If the user owns DNS, give them the exact record changes and when each should be applied. Replace conflicting records for the same hostname. Certificate delegation records often need DNS Only. Keep a required delegation record in place for renewals.
- Avoid assuming a proxy from Forge to a Cloud test URL will preserve the public hostname. Verify routing, TLS, login and signed URLs before relying on a proxy during propagation.

## Production Cutover

1. Confirm the agreed window, the DNS operator's availability, usable access to both environments and a rollback decision point. Confirm the active deployments and destination state again if anyone has made changes since rehearsal.
2. Fence every writer to the scoped data on Forge, including web traffic, scheduled jobs and direct database or storage clients. Let in-flight writes and jobs finish, then stop the scoped workers and account for any remaining queue entries. Preserve other sites on the server. Maintenance mode may allow health checks and explicitly configured commands through, so verify the write-capable ingress paths, including webhooks, and inspect the actual scheduler and worker state.
3. Transfer the final database, durable files and remaining job or session state for this site. Keep destination writers fenced during restore and baseline comparison. Run any pending deploy migrations in the rehearsed order and verify the resulting schema, state and runtime connections before enabling workers and the scheduler.
4. Once the final copy and destination checks pass, tell the DNS operator that the site is ready for the DNS switch. Hand over the confirmed routing records or apply them if authorized. Keep existing callback URLs when the public hostname is preserved. Where supported, use a read-only probe while writes are fenced to verify public routing and TLS, then enable Cloud writes and side-effecting jobs. If the handoff requires Cloud to accept writes sooner, treat that as the point after which a simple rollback is unsafe. Keep Forge fenced and run one set of consumers and scheduled jobs.
5. Probe the production hostname over HTTPS with a fresh request. Confirm it reached Cloud through access logs or equivalent origin evidence. Check login, assets and the relevant app flows, then verify a scheduler run and healthy queue processing. Account for side effects when testing integrations. A Cloudflare proxy can hide the configured origin from public DNS queries. Check the provider record when necessary. If Cloudflare reports 1014 or 1016, inspect its configured target and Cloud's hostname and certificate status before recommending a change to the proxy mode.

If cutover fails before Cloud accepts production writes or emits external effects, keep Cloud fenced and use the tested rollback path within the agreed window, including coordinating any DNS reversal. Once Cloud has new production writes or external effects, prefer fixing Cloud in place. If rollback is necessary, fence both sides and checkpoint Cloud's data. Verify the Forge release can run with the reconciled schema. Bring across new database writes, files and pending jobs, and account for external effects before restoring Forge as the sole writer and reversing DNS. Keep the user informed if the handoff exceeds the window.

## Source Retirement

Verify current conditions before saying that the Forge server can be powered off:

- Confirm the public DNS target and TLS verification, recent requests on Cloud, and working Cloud database, cache, workers and scheduler. Check old aliases, application connection settings, webhook or callback hosts, and any still-routed nonproduction names.
- Inventory every Forge site, database, worker, scheduler or cron entry, monitor and backup configuration on the server. Inspect listening services, active clients and available recent logs for remaining dependencies. A point-in-time connection check cannot rule out infrequent consumers. Say when access logs are unavailable.
- Identify data that exists only on Forge, such as a staging database or local files outside the original migration scope. Verify Cloud backup or recovery settings and record what needs an archive before permanent deletion. Keep the Forge database available during the agreed recovery period.
- Distinguish a reversible power-off from server deletion or billing cancellation. Perform retirement actions only when authorized for the exact server.
