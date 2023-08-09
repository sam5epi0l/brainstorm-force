# Brainstorm Force

Modern and secure WordPress (LEMP) development and deployment workflow. (See [features](https://roots.io/trellis/docs/installation/#why-use-trellis) and [back story](#back-story-))

## Prerequisites

Before you start, You should have:

- Python3 and Git.
- Ansible - You can install it with `pip install ansible`. Make sure Ansible is in `$PATH` or run `export PATH=$PATH:~/.local/bin`
- Trellis-CLI - You can install it with `curl -sL https://roots.io/trellis/cli/get | sudo bash`
- A **DigitalOcean** token - You can create one from the [DigitalOcean dashboard](https://cloud.digitalocean.com/account/api/tokens?i=838c5d).
- An SSH key pair (`~/.ssh/id_rsa` and `~/.ssh/id_rsa.pub`) - You can generate one with `ssh-keygen -t rsa -b 4096`.
- A domain or subdomain pointing at the droplet/server IP (which we will provision in the next step)

## Getting Started

To get started, follow these steps:

1. Clone this project with `git clone https://github.com/sam5epi0l/brainstorm-force.git`
2. Get vault password from the project owner. Add the vault password in `trellis/.vault_pass` file.
3. Use `trellis init` to [initialize the project](https://roots.io/trellis/docs/python/#trellis-cli-and-virtualenv) and create a virtual environment for Python and Ansible dependencies.

### Provision DigitalOcean Server

1. Run `trellis droplet create production` to [provision](https://roots.io/trellis/docs/deploy-to-digitalocean/) (to Digitalocean) a Ubuntu server. You will be prompted to enter your DigitalOcean token, droplet name, type, and location.
2. If your code repository is private, you can either use [SSH agent forwarding](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/using-ssh-agent-forwarding) or manually add `~/.ssh/id_rsa` (GitHub key) to the `web` user on the server (optional).
3. Run `gh secret set ANSIBLE_VAULT_PASSWORD -b $(cat trellis/.vault_pass)` to set the [Vault password](https://roots.io/trellis/docs/vault/#vault) as a GitHub secret.
4. Run `trellis key generate` to generate a deploy key for GitHub and add it to your repository automatically. This will also create two new repository secrets: TRELLIS_DEPLOY_SSH_KNOWN_HOSTS and TRELLIS_DEPLOY_SSH_PRIVATE_KEY.
5. Run `trellis deploy production` to deploy your code to the server.

## Deployment Options

You can deploy your code to the server in two ways:

- Manually, by running `trellis deploy production` from your local machine.
- Automatically, by using GitHub Actions to trigger a deployment on every code push. You can configure this by following the [instructions in this guide](https://roots.io/trellis/docs/deploy-with-github-actions/#deploying-trellis-wordpress-sites-with-github-actions).

## Resources 🔗

For more information the workflow and technologies used, please refer to these official documentation:

- [Trellis-CLI Docs](https://roots.io/trellis/docs/cli/)
- [Features](https://roots.io/trellis/docs/installation/)
- [Community discussions](https://discourse.roots.io/) and help:
	- Issue: [SSH issue](https://discourse.roots.io/t/trellis-ssh-unreachable-changed-false-unreachable-true-error/9682/4)
	- Issue: [Subtree path error](https://discourse.roots.io/t/deploying-remote-server-fails-with-repo-subtree-path-error/7061/2)
- [Trellis Wordpress docs](https://roots.io/trellis/docs/wordpress-sites/)


## Back-Story 🎬

Before I decided to use Trellis, I tried starting from scratch but it was a vast project. So, I tried some options for provisioning and deploying the application website. Here is what I did and why I finally chose Trellis:

- [AnsiPress](https://github.com/AnsiPress/AnsiPress/) and [WordPress Ansible Playbook](https://github.com/nerrad/wordpress-ansible-playbook/)
- I then discovered two projects by [rtCamp](http://rtcamp.com/), WordPress-Skeleton and Action-Deploy-WordPress, that used best practices and standards for WordPress development and deployment.
- Finally, I found Trellis. It has Trellis-CLI, a command-line interface that made it easy to provision and deploy servers. It also have many features, such as Nginx, PHP-FPM, MariaDB, Redis, Let’s Encrypt, Fail2ban, Composer, WP-CLI, MailHog, and more. With great documentation and community support.

Trellis is the most complete, robust and community powered solution available.
