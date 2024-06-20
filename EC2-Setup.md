# EC2 setup JAVA, MySQL, NGINX:

first install java by running these command

	sudo yum install java-11-amazon-corretto

check the path

	sudo alternatives --config java

add path to JAVA_HOME variable

	export JAVA_HOME=/usr/lib/jvm/java-11-amazon-corretto.x86_64/

add to class path

	export PATH=$PATH:$JAVA_HOME

check by echo if properly done

	echo $JAVA_HOME

This will install java 11 version on ec2


## RUN THE SPRINGBOOT JAR WITH THIS COMMAND

	nohup java -jar CommunityApp-0.0.1-SNAPSHOT.jar > logfile.log 2>&1 &

ex:-	nohup java -jar CommunityApp-0.0.1-SNAPSHOT.jar 2>&1 >> logfile.log &

this will run the jar in the background and logs will be saved in logfile.log

to check running jar

	ps -ef | grep java

to kill running process(jar)

	kill {number}

## After that for installing MySQL Server:

download files

	sudo yum install -y https://dev.mysql.com/get/mysql80-community-release-el7-3.noarch.rpm

add GPG key

	sudo rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2023

install mysql server

	sudo yum install -y mysql-community-server

start the server

	sudo systemctl start mysqld

check the status of server

	sudo systemctl status mysqld

enable mysql server to start automatically once the ec2 starts

	sudo systemctl enable mysqld

get temporary password to login and setup new credentials

	sudo grep 'temporary password' /var/log/mysqld.log

login 

	mysql -u root -p

at next dialogue enter the temp password


after logging in 
first set password for the root user by

	ALTER USER 'root'@'localhost' IDENTIFIED BY 'Admin@123';

Create new user with following commands:

	CREATE USER 'admin'@'localhost' IDENTIFIED BY 'Admin@123';

	GRANT ALL PRIVILEGES ON *.* TO 'admin'@'localhost' WITH GRANT OPTION;

	CREATE USER 'admin'@'%' IDENTIFIED BY 'Admin@123';

	GRANT ALL PRIVILEGES ON *.* TO 'admin'@'%' WITH GRANT OPTION;

	FLUSH PRIVILEGES;

Now to connnect to the server via Workbench 
first edit the In-bound rules and allow MySQL port (3306) to be publicly acessible

After that set up new connection in Workbench and connect to the Mysql server

## For Installing and setting up NGINX:

Install NGINX

	sudo amazon-linux-extras install nginx1

Start 

	sudo systemctl start nginx

Check Status

	sudo systemctl status nginx

Open nginx.conf file to edit configuration

	sudo vim /etc/nginx/nginx.conf

Edit the conf file : 
	
1. press i to start insert mode

2. Add this in server{} after "server_name" line
 
		location / {
			root /home/ec2-user/project_404;
			try_files $uri $uri/ /index.html;
		}


3. Add this in "error_page 500"
 
		location = /50x.html {
			root /home/ec2-user/project_404;
        	}	

4. Press ESC and then write ':wq!' and press enter to save changes


then upload the dist's folder

after that see the priviledges by following command

	namei -om /home/ec2-user/project_404

Allow read write priviledges by

	sudo chmod 755 /home/ec2-user

again check 

	namei -om /home/ec2-user/project_404


Reload NGINX

	sudo systemctl reload nginx

Restart NGINX

	sudo systemctl restart nginx


## To Add SSL Certificate to Nginx

A site to generate ssl certificates for free :  

https://www.sslforfree.com


Upload the certificate files to your desired directory in your ec2 instance (files - private.key, certificate.crt, ca_bundle.crt)

after uploading, you need to merge the certificate and ca_bundle to one file

	cat certificate.crt ca_bundle.crt >> server.crt

Then edit your nginx.conf file by the vim command

	sudo vim /etc/nginx/nginx.conf

 press i to start editing

 At the bottom uncomment the server{} component which will have the ssl porperties
 after uncommenting edit properties

 the server component should look like this

 	server {
        listen       443 ssl http2;
        listen       [::]:443 ssl http2;
        server_name  _;
        location / {
           root /home/ec2-user/project_404;
           try_files $uri $uri/ /index.html;
        }

	location /api/ {
                #backend
                proxy_pass http://my-backend-url;
        }
	
        ssl_certificate "/home/ec2-user/ssl/server.crt";
        ssl_certificate_key "/home/ec2-user/ssl/private.key";
        ssl_session_cache shared:SSL:1m;
        ssl_session_timeout  10m;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_prefer_server_ciphers on;

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;

        error_page 404 /404.html;
            location = /40x.html {
        }

        error_page 500 502 503 504 /50x.html;
            location = /50x.html {
	    root /home/ec2-user/project_404;
        }
    }


 remember to change the line after the `server_name` from `root` to the `location` component
 
 and also remember to remove the property `ssl_ciphers PROFILE=SYSTEM;` and instead add `ssl_protocols TLSv1 TLSv1.1 TLSv1.2;` instead.

 also in the angular frontend change the backend path from the regular `http://server-url`  -->  `https://server-url/api` .
