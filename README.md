# cloudflare-origin-ips

[![Update Cloudflare IPs](https://github.com/kugland/cloudflare-origin-ips/actions/workflows/update.yml/badge.svg)](https://github.com/kugland/cloudflare-origin-ips/actions/workflows/update.yml)

A mirror of the IP ranges Cloudflare uses to connect to origin servers, kept up to date automatically.

## Files

| File         | Contents                       |
| ------------ | ------------------------------ |
| `ips-v4.txt` | IPv4 ranges, one CIDR per line |
| `ips-v6.txt` | IPv6 ranges, one CIDR per line |

The data comes from <https://www.cloudflare.com/ips-v4> and <https://www.cloudflare.com/ips-v6>. Lines are sorted, so diffs stay small and easy to read.

## Why

When your site sits behind Cloudflare, every request reaches your origin from one of these ranges. Typical uses:

- restricting your origin's firewall to Cloudflare traffic only
- telling your web server which proxies to trust for the real client IP (e.g. nginx `set_real_ip_from`)

Pulling the lists from this repo gives you a git history of every change, so you can pin a commit or see when a range was added or removed.

## Usage

Fetch the raw files directly:

```sh
curl -sSL https://raw.githubusercontent.com/kugland/cloudflare-origin-ips/master/ips-v4.txt
curl -sSL https://raw.githubusercontent.com/kugland/cloudflare-origin-ips/master/ips-v6.txt
```

Or pull the repo into your own project as a submodule:

```sh
git submodule add https://github.com/kugland/cloudflare-origin-ips.git cloudflare-ips
git submodule update --remote cloudflare-ips   # update later
```

Or as a subtree:

```sh
git subtree add --prefix=cloudflare-ips https://github.com/kugland/cloudflare-origin-ips.git master --squash
git subtree pull --prefix=cloudflare-ips https://github.com/kugland/cloudflare-origin-ips.git master --squash   # update later
```

## How it updates

A [GitHub Actions workflow](.github/workflows/update.yml) runs daily at 00:00 UTC, on every push to `master`, and on manual dispatch. It downloads both lists, sorts them, and makes a GPG-signed commit only if something changed. [Renovate](renovate.json) keeps the workflow's actions up to date.
