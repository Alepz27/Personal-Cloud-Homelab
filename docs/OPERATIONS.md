# Operations Runbook

This runbook is a verification guide. Commands must be run from the private deployment directory containing the real, uncommitted `.env` and compatible Compose file.

## Pre-flight

- Confirm Windows, WSL2 and Docker Desktop are installed and patched.
- Confirm the media, database and model-cache paths exist and have sufficient capacity.
- Confirm the real `.env` is ignored by Git and contains no placeholder values.
- Confirm a backup exists before upgrading or changing database storage.

## Start and Inspect

```powershell
docker compose config
docker compose pull
docker compose up -d
docker compose ps
```

Inspect only the affected service when possible:

```powershell
docker compose logs --tail 100 immich-server
docker compose logs --tail 100 database
docker compose logs --tail 100 redis
docker compose logs --tail 100 immich-machine-learning
```

Before sharing output, remove private addresses, account names, paths and secrets.

## Controlled Stop and Restart

```powershell
docker compose stop
docker compose start
```

Use `docker compose down` only when deliberately removing the running containers/network. Do not add `--volumes` during normal maintenance; that option can remove managed persistent data.

## Recovery Verification

For each test, record the date, versions, action, expected state, observed state, elapsed recovery time and any filesystem/database checks.

1. Restart only the Immich server container.
2. Restart Docker Desktop.
3. Restart Windows normally.
4. Test sudden power loss only with a safe procedure and recoverable test data.
5. Confirm the iPhone can view an existing test asset and upload a new non-sensitive test asset.
6. Confirm database and media backups can be restored into an isolated test location.

Do not claim automatic recovery until these observations exist.

## Update Workflow

1. Read the release notes and migration requirements for the target Immich version.
2. Back up media, PostgreSQL and private configuration.
3. Record the current image versions.
4. Pull and start the compatible service set.
5. Check logs, web access, mobile upload and background processing.
6. Keep a tested rollback/restore path.

## Capacity and Backup Checks

- Record free space for media and database locations.
- Verify the newest backup age and size.
- Periodically compare representative file hashes or use an appropriate integrity tool.
- Perform an isolated restore; a successful backup command alone does not prove recoverability.
