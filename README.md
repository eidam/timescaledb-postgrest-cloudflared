# timescaledb-postgrest-cloudflared

## Create an VM
- login to Vultr/DigitalOcean and create a VM, make sure you add your public key so you can ssh there
- once started, make sure you can SSH to your VM
- add VM IP address to `ansible/inventory/hosts`

## Adjust ansible configs
- edit cloudflared config `./ansible/roles/cloudflared/templates/cloudflared.yml`

## Install/Run Ansible
- `ansible-playbook -i ansible/inventory/hosts ansible/timecaledb.yml`

## Create and configure Cloudflare tunnel
- follow https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/create-tunnel
- copy generated secret file to your VM, to `/root/.cloudflared/tunnel-credentials.json`

## Restart/reboot
- restart `cloudflared`, `postgrest` systemd services or simply reboot the VM 

## Login to PSQL and create your tables
- you can just run `psql` from root user, should not require a password

## Set cloudflared subdomain and firewall rules 
- before you set CNAME to your cloudflared tunnel, make sure you have correct firewall rules in place so you can query your postgrest instance only from your Workers (the best/easiest way is still being worked on https://twitter.com/adam_janis/status/1392145978262790144)

## Couple of notes
- In production, I would suggest using block storage and change postgres data directory once mounted.
- This is not HA setup by any means, to keep it simple, its kept as a single postgres server.
