# Data Transfer

Choose the source and target using confirmed resource IDs and effective runtime connections. Keep the initial rehearsal and final copy distinguishable. Rehearse with the same restore path, provider versions, extensions and data shape that the cutover will use. Inspect foreign keys, sequence or auto-increment state, special column types and jobs before accepting a restore on counts alone.

## PostgreSQL

- Use a compatible `pg_dump` client and a custom-format dump. Check the source and destination versions, extensions such as pgvector, extension schemas and target role privileges. Use a supported direct connection for the restore when the runtime uses a pooler.
- For a database local to Forge, a fast path is to run `pg_dump` on Forge and run `pg_restore` from Forge to the verified Cloud endpoint over TLS. This keeps the source database private and avoids relaying the expanded restore through a laptop. Use an SSH tunnel or another authenticated private transfer path when direct access is unavailable.
- Use a private archive and a protected client credential source. Keep credentials out of command arguments, shell history and deployment logs. Exclude source ownership and ACLs when Cloud manages roles. Check for app-specific grants that must be recreated intentionally. For an appropriate-size archive, `pg_restore --no-owner --no-privileges --single-transaction --exit-on-error` rolls back a failed restore.
- Restore into an empty or explicitly disposable destination. Use `--clean --if-exists` only when the exact destination is verified as an isolated rehearsal copy that can be replaced. Keep unrelated environments, schemas and roles out of the operation. For larger datasets where a single full restore would exceed the agreed window, rehearse a staged copy and measured catch-up before committing to that window.

## MySQL

- Confirm engine versions, storage engines, character sets and collations, SQL modes, and managed-server privileges. Use a consistent logical export and include app-owned views, triggers, routines and events where used. `--single-transaction` provides a snapshot for transactional tables. Nontransactional tables need a separate consistency plan.
- Rehearse import into a dedicated database. Review definer accounts, GTID statements and event scheduler behavior before import. MySQL DDL can commit implicitly, so recover a partial import by rebuilding the disposable destination or restoring a known checkpoint. Check [Laravel's Forge migration guide](https://laravel.com/cloud/migrate-forge-cloud) for the current service-specific import options.

## Redis and Valkey

- Identify which connections hold queues, sessions, locks and disposable cache. Preserve the operational state the app needs. Drain running jobs first, and account for waiting, delayed and reserved jobs. Keep just one set of consumers active. Use the [Valkey migration guide](https://laravel.com/cloud/docs/knowledge-base/migrate-upstash-to-valkey.md) for transfer and validation options.
- Verify the DB indices and key prefixes on both ends. Cloud may map source keys from multiple DB indices into one destination DB. Detect collisions before importing. If environments share Valkey, verify isolation for keys, queue consumers and cache operations. Preserve key types and expiry times, check binary restore compatibility, and skip stale lock keys or Horizon's live process registrations unless their migration is intentional.
- Use a supported private import path or run the import from the destination environment. Keep Valkey private. Prove the transfer with isolated keys or a disposable environment before copying live sessions or jobs. Confirm the actual queue depth and session behavior after cutover.

## Files and Verification

- Inspect the shared storage paths and symlink targets on Forge. Move durable uploads to persistent storage or retain the existing object store as appropriate. Cloud deployment filesystems are ephemeral. Check counts, size or checksums and file visibility, and repeat the sync after source writes stop if files were mutable.
- At the write freeze, capture source counts and representative integrity checks for the scoped data. After restore, compare against that frozen baseline before allowing destination writes. Check app-visible data, encrypted fields and a representative query for required database extensions. Report what was sampled or excluded. Remove temporary endpoints or database access you enabled for transfer once the app uses its intended connection.
