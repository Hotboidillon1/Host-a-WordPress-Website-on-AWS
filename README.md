Hosting a WordPress website on AWS involves deploying WordPress on an EC2 instance with a database, configuring networking, and ensuring security. Below is a step-by-step guide to set up WordPress on AWS.

Step 1: Choose Your Deployment Method
You can deploy WordPress on AWS in multiple ways:

Manual Installation – Set up EC2, RDS, and install WordPress manually.
Amazon Lightsail – Simplified deployment with pre-configured instances.
AWS Marketplace AMI – Use pre-configured Amazon Machine Images (AMIs).
Elastic Beanstalk – Automated deployment with scalability.
For this guide, we will use manual installation on an EC2 instance.

Step 2: Launch an EC2 Instance
Go to AWS Console → EC2 → Launch Instance.
Choose AMI: Select Amazon Linux 2 or Ubuntu.
Instance Type: Choose t2.micro (Free Tier) or t3.small for better performance.
Key Pair: Create or select an existing key pair.
Security Group:
Allow HTTP (port 80) & HTTPS (port 443) for web traffic.
Allow SSH (port 22) from your IP for secure access.
Launch the Instance and connect via SSH.
Step 3: Install LAMP Stack (Linux, Apache, MySQL, PHP)
1. Update the system
bash
Copy
Edit
sudo yum update -y   # Amazon Linux
sudo apt update -y   # Ubuntu
2. Install Apache
bash
Copy
Edit
sudo yum install -y httpd   # Amazon Linux
sudo apt install -y apache2 # Ubuntu
Start and enable Apache:

bash
Copy
Edit
sudo systemctl start httpd
sudo systemctl enable httpd
3. Install MySQL (MariaDB)
bash
Copy
Edit
sudo yum install -y mariadb-server   # Amazon Linux
sudo apt install -y mariadb-server   # Ubuntu
Start and enable MariaDB:

bash
Copy
Edit
sudo systemctl start mariadb
sudo systemctl enable mariadb
Secure MySQL:

bash
Copy
Edit
sudo mysql_secure_installation
Follow the prompts to set a root password and remove anonymous users.

4. Install PHP
bash
Copy
Edit
sudo yum install -y php php-mysqlnd php-fpm php-json php-mbstring   # Amazon Linux
sudo apt install -y php php-mysql php-fpm php-json php-mbstring     # Ubuntu
Restart Apache:

bash
Copy
Edit
sudo systemctl restart httpd
Step 4: Download & Configure WordPress
Navigate to Apache root directory:
bash
Copy
Edit
cd /var/www/html
Download WordPress:
bash
Copy
Edit
sudo wget https://wordpress.org/latest.tar.gz
sudo tar -xzf latest.tar.gz
sudo mv wordpress/* .
sudo rm -rf wordpress latest.tar.gz
Set permissions:
bash
Copy
Edit
sudo chown -R apache:apache /var/www/html
sudo chmod -R 755 /var/www/html
Step 5: Configure MySQL for WordPress
Log into MySQL:
bash
Copy
Edit
sudo mysql -u root -p
Create a WordPress database:
sql
Copy
Edit
CREATE DATABASE wordpress;
CREATE USER 'wp_user'@'localhost' IDENTIFIED BY 'your_password';
GRANT ALL PRIVILEGES ON wordpress.* TO 'wp_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;
Step 6: Configure WordPress
Rename the sample configuration file:
bash
Copy
Edit
sudo cp wp-config-sample.php wp-config.php
Edit the wp-config.php file:
bash
Copy
Edit
sudo nano wp-config.php
Modify the database settings:

php
Copy
Edit
define('DB_NAME', 'wordpress');
define('DB_USER', 'wp_user');
define('DB_PASSWORD', 'your_password');
define('DB_HOST', 'localhost');
Save and exit.

Step 7: Restart Apache & Complete Installation
bash
Copy
Edit
sudo systemctl restart httpd
Now, go to your EC2 instance’s Public IP in a browser:

arduino
Copy
Edit
http://your-public-ip
Follow the WordPress installation wizard.

Step 8: Secure Your WordPress Site
Enable HTTPS using an SSL certificate from Let’s Encrypt or AWS Certificate Manager.
Use a Firewall (AWS Security Groups) to allow only necessary traffic.
Set Up Backups using Amazon RDS or EC2 snapshot
