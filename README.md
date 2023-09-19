# Gitlab Install and Upgrade Step by Step

## Install and configure the necessary dependencies
`sudo apt-get update`  
`sudo apt-get install -y curl openssh-server ca-certificates tzdata perl`  

## Add the GitLab package repository and install the package
`curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.deb.sh | sudo bash`  

## Install Gitlab 15.0.0-ce.0
`sudo apt-get install gitlab-ce=15.0.0-ee.0 -y`  

## Configure gitlab backup paths
`sudo vim /etc/gitlab/gitlab.rb`  
`/gitlab_rails['backup_path']` (find this line and uncomment)  
`sudo gitlab-ctl reconfigure` (reload gitlab config)  

## Backup /etc/gitlab folder
`sudo cp -R /etc/gitlab ./gitlab_bk`

## Backup command
- ref: https://docs.gitlab.com/ee/administration/backup_restore/backup_gitlab.html#backup-command  
`sudo gitlab-backup create`

## Downgrade steps
- ref: https://docs.gitlab.com/ee/update/package/downgrade.html  
`sudo gitlab-ctl stop puma`  
`sudo gitlab-ctl stop sidekiq`  
`sudo dpkg -r gitlab-ee`  
`sudo apt install gitlab-ee=15.0.0-ee.0`  
`sudo gitlab-ctl reconfigure`  

- Restore steps  
`sudo gitlab-ctl stop puma`  
`sudo gitlab-ctl stop sidekiq`  
`sudo gitlab-ctl status`  
`sudo gitlab-backup restore BACKUP=1695088971_2023_09_19_15.0.0`  
`1695092635_2023_09_19_15.0.0`  

- Restart for checking Gitlab step  
`sudo gitlab-ctl restart`  
`sudo gitlab-rake gitlab:check SANITIZE=true`  
`sudo gitlab-rake gitlab:doctor:secrets`  


## Upgrade Gitlab
- ref: https://gitlab-com.gitlab.io/support/toolbox/upgrade-path/?current=15.1.0&edition=ce  
`sudo apt-get install gitlab-ee=15.4.6-ee.0 -y`  
`sudo apt-get install gitlab-ee=15.11.13-ee.0 -y`  
`sudo apt-get install gitlab-ee=16.3.3-ee.0 -y`  



## Backup path
`/var/opt/gitlab/backups`
