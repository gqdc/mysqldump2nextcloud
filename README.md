# mysqldump2nextcloud
A bash script which do mysqldump(s) and upload it to a nextcloud server.

1. Install prerequisites : mysqldump, nextcloudcmd, gzip :
`apt install mariadb-client gzip nextcloud-desktop-cmd`
2. clone this repository inside /usr/local/share :
```
cd /usr/local/share
git clone https://github.com/gqdc/mysqldump2nextcloud.git
```
3. Open /usr/local/share/mysqldump2nextcloud file, and fill in ALL the empty variables.
4. Make the script executable :
`chmod +X /usr/local/share/mysqdump2nextcloud/mysqldump2nextcloud`
6. Run the script and correct any errors.
7. Add new line in your crontab to automate the script :
`*/30 * * * * /usr/local/share/mysqdump2nextcloud/mysqldump2nextcloud`
  
You can have a second task, to be able to have a "daily" backup above the first ones, by adding the "daily" parameter to the script :
`01 0 * * * /usr/local/share/mysqldump2nextcloud/mysqldump2nextcloud daily`
  
With these 2 lines, you'll have a new "classic" dump every 30 minutes, and, at 00h01 a new "daily" backup.
The daily backups will be deleted every $EXPIRATIONDAILY days, while "classic" dumps will be deleted every $EXPIRATION minutes.
  
That means with default values for $EXPIRATIONDAILY and $EXPIRATION, after a week of execution, your backup folder will contains :
    - 5 "classics" dumps M+0, M+30, M+60, M+90, M+120
    - 4 "daily" dumps : D+3, D+2, D+1, D+0

## Advices
nextcloudcmd has actullay a speed problem when it uses classis user/password auth method (oc_authtoken table will increase, and connection time will increase too).
To avoid that, use an "application password".
