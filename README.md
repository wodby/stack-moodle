# Moodle application stack for Kubernetes on Wodby

Deploy Moodle LMS on Kubernetes with Wodby.

This repository defines the Wodby stack manifests and default service
composition for Moodle.

<!-- wodby:generated:start -->

## Stack contract

- [Browse Wodby application stacks](https://wodby.com/stacks)
- [Wodby stack documentation](https://wodby.com/docs/2.0/stacks/)
- [Stack manifest reference](https://wodby.com/docs/2.0/stacks/template/)

## Start from a boilerplate

Use one of the compatible boilerplates exposed by this stack's services to
start with Wodby CI build configuration:

- [Vanilla Moodle](https://github.com/moodle/moodle)

## Service definitions

- [PHP (Moodle) service](https://github.com/wodby/service-moodle)
- [Nginx (Moodle) service](https://github.com/wodby/service-moodle-nginx)
- [MariaDB service](https://github.com/wodby/service-mariadb)
- [NFS provisioner service](https://github.com/wodby/service-nfs-provisioner)
- [OpenSMTPD service](https://github.com/wodby/service-opensmtpd)
- [Valkey service](https://github.com/wodby/service-valkey)

## What's included

| Component / service | Default configuration |
| --- | --- |
| Moodle<br>`moodle` | required; enabled by default; derivatives: `cron`; volumes: `moodledata` 20 GB; links: `db` → `mariadb`, `moodledata` → `moodledata-nfs`, `sendmail` → `opensmtpd`, `redis` → `valkey` |
| Nginx<br>`nginx` | required; enabled by default; links: `backend` → `moodle` |
| MariaDB<br>`mariadb` | optional; enabled by default; versions: `12.3`, `11.8` (default), `11.4`; volumes: `data` 10 GB |
| Moodle data NFS storage<br>`moodledata-nfs` | optional; enabled by default; volumes: `data` 25 GB |
| OpenSMTPD<br>`opensmtpd` | optional; enabled by default |
| Valkey<br>`valkey` | optional; disabled by default |

Enabled optional services are selected by default but can be excluded when an
app is created. Disabled optional services are available but not selected by
default. Required services cannot be excluded.

## Validate the stack manifest

```bash
wodby stack validate-manifest stack.yml --org <org-id>
```

<!-- wodby:generated:end -->

## Deploy this stack

Start from [Vanilla Moodle](https://github.com/moodle/moodle), or connect your own Moodle 5.3 repository.

After the first deployment, open the site and finish Moodle's installer: accept the licence and create the
administrator account. Do this right away, because until then anyone who opens the site can do it.

Moodle's cron runs in the `cron` worker. Uploaded files are kept on the shared `moodledata` volume.
Enable Valkey to keep sessions out of the data volume.

## Maintain a custom version

1. Fork this repository.
2. Edit the stack manifest.
3. Import the repository as a [Git-backed stack](https://wodby.com/docs/2.0/stacks/create/#create-a-git-backed-stack).

When replacing or renaming a stack service, update every related link target
and derivative reference. Stack-local names and referenced service names are
distinct identifiers.
