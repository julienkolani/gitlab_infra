# gitlab_infra
My custom Vagrantfile sets up a complete GitLab infrastructure with 3 runner replicas effortlessly.
Just like this : 

╭─julien@zbookcreate in ~/Downloads took 2ms
╰─λ git clone https://github.com/julienkolani/gitlab_infra.git
Cloning into 'gitlab_infra'...
remote: Enumerating objects: 7, done.
remote: Counting objects: 100% (7/7), done.
remote: Compressing objects: 100% (7/7), done.
remote: Total 7 (delta 1), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (7/7), 15.50 KiB | 131.00 KiB/s, done.
Resolving deltas: 100% (1/1), done.

╭─julien@zbookcreate in ~/Downloads took 1s
╰─λ cd gitlab_infra/

╭─julien@zbookcreate in repo: gitlab_infra on  main via ⍱ v2.4.1
╰─λ mkdir gitlab_data

╭─julien@zbookcreate in repo: gitlab_infra on  main via ⍱ v2.4.1 took 3ms
╰─λ ls
drwxr-xr-x    - julien 27 Jun 17:28  .git
drwxr-xr-x    - julien 27 Jun 17:29  gitlab_data
.rw-r--r--  35k julien 27 Jun 17:28  LICENSE
.rw-r--r--  115 julien 27 Jun 17:28  README.md
.rw-r--r-- 4.2k julien 27 Jun 17:28 ⍱ Vagrantfile

╭─julien@zbookcreate in repo: gitlab_infra on  main via ⍱ v2.4.1 took 5ms
╰─λ vagrant up

╭─julien@zbookcreate in repo: gitlab_infra on  main [?] via ⍱ v2.4.1 took 7m55s
╰─λ vagrant ssh
Welcome to Ubuntu 22.04.4 LTS (GNU/Linux 5.15.0-113-generic x86_64)

* Documentation:  https://help.ubuntu.com
* Management:     https://landscape.canonical.com
* Support:        https://ubuntu.com/pro

System information as of Thu Jun 27 17:30:17 UTC 2024

System load:  0.82              Processes:               112
Usage of /:   3.6% of 38.70GB   Users logged in:         0
Memory usage: 5%                IPv4 address for enp0s3: 10.0.2.15
Swap usage:   0%


Expanded Security Maintenance for Applications is not enabled.

5 updates can be applied immediately.
5 of these updates are standard security updates.
To see these additional updates run: apt list --upgradable

Enable ESM Apps to receive additional future security updates.
See https://ubuntu.com/esm or run: sudo pro status

vagrant@gitlab:~$ ls
vagrant@gitlab:~$ cd /
bin/          dev/          home/         lib32/        libx32/       media/        opt/          root/         sbin/         srv/          tmp/          vagrant/      var/
boot/         etc/          lib/          lib64/        lost+found/   mnt/          proc/         run/          snap/         sys/          usr/          vagrant_data/
vagrant@gitlab:~$ cd /vagrant_data/
vagrant@gitlab:/vagrant_data$ ls
docker-compose.yml
vagrant@gitlab:/vagrant_data$ sudo docker compose ps
WARN[0000] /vagrant_data/docker-compose.yml: `version` is obsolete
NAME            IMAGE                         COMMAND                  SERVICE         CREATED              STATUS                                 PORTS
gitlab          gitlab/gitlab-ce:latest       "/assets/wrapper"        gitlab          About a minute ago   Up About a minute (health: starting)   0.0.0.0:80->80/tcp, :::80->80/tcp, 0.0.0.0:443->443/tcp, :::443->443/tcp, 0.0.0.0:5050->5050/tcp, :::5050->5050/tcp, 0.0.0.0:2222->22/tcp, :::2222->22/tcp
gitlab-runner   gitlab/gitlab-runner:ubuntu   "/usr/bin/dumb-init …"   gitlab-runner   About a minute ago   Up About a minute
vagrant@gitlab:/vagrant_data$ sudo docker compose exec gitlab bash
WARN[0000] /vagrant_data/docker-compose.yml: `version` is obsolete
root@gitlab-ce:/#
