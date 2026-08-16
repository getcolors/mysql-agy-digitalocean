# mysql-agy-digitalocean

Deployment of `mysql-agy` on DigitalOcean (`ams3`) with Cloudflare DNS and Cloudflare R2 backup.

## Endpoints

- **Cluster Host**: `mysql-agy.bigconfig.online` (port 3306)
- **Nodes**: 3x `s-2vcpu-4gb` in `ams3`

## Usage

```sh
./green build
./green create --dry-run
./green create
./green health
./green delete
```
