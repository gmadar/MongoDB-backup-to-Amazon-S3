gmadar / MongoDB-backup-to-Amazon-S3
=================================

this script is based on [woxxy / MySQL-backup-to-Amazon-S3](https://github.com/woxxy/MySQL-backup-to-Amazon-S3). Thanks woxxy!

This script is based on the [s3cmd](http://s3tools.org/s3cmd)

Setup
-----
1. Register for Amazon AWS 
2. Install s3cmd (following commands are for debian/ubuntu, but you can find how-to for other Linux distributions on [s3tools.org/repositories](http://s3tools.org/repositories))

		wget -O- -q http://s3tools.org/repo/deb-all/stable/s3tools.key | sudo apt-key add -
		sudo wget -O/etc/apt/sources.list.d/s3tools.list http://s3tools.org/repo/deb-all/stable/s3tools.list
		sudo apt-get update && sudo apt-get install s3cmd
	
3. Get your key and secret key at this [link](https://aws-portal.amazon.com/gp/aws/developer/account/index.html?ie=UTF8&action=access-key)
4. Configure s3cmd to work with your account

		s3cmd --configure

5. Make a bucket (must be an original name, s3cmd will tell you if it's already used)

		s3cmd mb s3://my-database-backups
	
6. Put the mongoToS3.sh file somewhere in your server, like `/home/youruser`
7. run with sudo OR change the 'MONGODUMPPATH' to a folder with write permitions and give the script file 755 permissions `chmod 755 /home/youruser/mongoToS3.shh` or via FTP
8. Edit the variables near the top of the mongoToS3.sh file to match your bucket name

Now we're set. You can use it manually:

	#set a new daily backup, and store the previous day as "previous_day"
	sh /home/youruser/mysqltos3.sh
	
	#set a new weekly backup, and store previous week as "previous_week"
	/home/youruser/mysqltos3.sh week
	
	#set a new weekly backup, and store previous month as "previous_month"
	/home/youruser/mysqltos3.sh month
	
But, we don't want to think about it until something breaks! So enter `crontab -e` and insert the following after editing the folders

	# daily MySQL backup to S3 (not on first day of month or sundays)
	0 3 2-31 * 1-6 sh /home/youruser/mysqltos3.sh day
	# weekly MySQL backup to S3 (on sundays, but not the first day of the month)
	0 3 2-31 * 0 sh /home/youruser/mysqltos3.sh week
	# monthly MySQL backup to S3
	0 3 1 * * sh /home/youruser/mysqltos3.sh month

Or, if you'd prefer to have the script determine the current date and day of the week, insert the following after editing the folders

	# automatic daily / weekly / monthly backup to S3.
	0 3 * * * sh /home/youruser/mysqltos3.sh auto

And you're set.


Troubleshooting
---------------

None yet.

