## prerequisite

- Python3
- Ansible - `pip install ansible`.
    Make sure ansible is in $PATH or run `export PATH=$PATH:~/.local/bin`
- Trellis-CLI - `curl -sL https://roots.io/trellis/cli/get | bash`
- Clone project - `git clone https://github.com/sam5epi0l/brainstorm-force.git`
- Add vault pass to `trellis/.vault_pass` file.
- DigitalOcean token
- SSH key pair (`~/.ssh/id_rsa` and `~/.ssh/id_rsa.pub`)
- A domain or subdomain pointing at droplet/server IP (which we will provision in next step)

## [Provision Digitalocean Server](https://roots.io/trellis/docs/deploy-to-digitalocean/)

Run:
```bash
trellis droplet create production
```
- Provide digitalocean token
- Droplet name, type, location

## Deploy Code on server

If code repository is private. Use ssh agent forwarding or manually add `~/.ssh/id_rsa` (github key) in `web` user (optional)


```bash
gh secret set ANSIBLE_VAULT_PASSWORD -b $(cat trellis/.vault_pass)
```
then run
```bash
trellis key generate
```
After running this command you'll have:

    A new file in trellis/public_keys — make sure to commit this addition
    A deploy key added to your repo automatically (Settings > Deploy keys)
    Two new repository secrets added to your repo automatically: TRELLIS_DEPLOY_SSH_KNOWN_HOSTS and TRELLIS_DEPLOY_SSH_PRIVATE_KEY


### Deploy with trellis-cli

```bash
trellis deploy production
```


### [Deploy on code push with github-action](https://roots.io/trellis/docs/deploy-with-github-actions/)
Just push something


## Related

https://roots.io/trellis/docs/cli/
https://roots.io/trellis/docs/deploy-to-digitalocean/
https://discourse.roots.io/t/trellis-ssh-unreachable-changed-false-unreachable-true-error/9682/4
https://discourse.roots.io/t/deploying-remote-server-fails-with-repo-subtree-path-error/7061/2

## Back Story

First I started Provisioning manually

https://github.com/AnsiPress/AnsiPress/tree/develop
https://github.com/nerrad/wordpress-ansible-playbook/tree/master

then I found these use best practices
https://github.com/rtCamp/wordpress-skeleton
https://github.com/rtCamp/action-deploy-wordpress

