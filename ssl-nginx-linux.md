## Install/Renew SSL Certificate In Nginx on Linux 

#### To renew SSL Certificate in Nginx, you can follow the steps below.

#### check nginx status
~~~
systemctl status nginx
~~~

## Step 1: Generating a Private key and CSR.

#### Two ways can generate private key and csr.
1. Online
2. Command Line

#### 1: Generate CSR from Online:
#### From a browser search [ Generate csr from online ] we can see some online csr generated website. Then choose any trusted website and generate csr with private key. 
#### Bellow listing some trusted csr generated link.
1. [ssl.com](https://www.ssl.com/online-csr-and-key-generator/)

2. [csrgenerator.com](https://csrgenerator.com/)

3. [decoder.link](https://decoder.link/csr_generator)

Access any link and filup there form as your domain information. Then click generate button and download or copy paste csr & key file.

#### 2: Generate CSR from Command Line:
#### This way we need to access that server terminal and run a single line command for generate private key and csr.

#### Create a Directory, where store Private key and CSR file.
~~~
mkdir -p /etc/nginx/ssl/
~~~
#### Navigate the directory and run bellow command.
~~~
cd /etc/nginx/ssl/
~~~
~~~
openssl req -new -newkey rsa:2048 -nodes -keyout exampledomain.com.key -out exampledomain.com.csr
~~~
#### Enter the following CSR details when prompted:
<b>Common Name(CN):</b> CN is FQDN, www.paulco.xyz, cloud.paulco.xyz.

<b>Organization:</b> The full legal name of your organization.

<b>Organization Unit (OU):</b> Your department such as IT, Account, HR, etc.

<b>City or Locality:</b> Where your organization is legally incorporated.

<b>State or Province:</b> Where your organization is legally incorporated.

<b>Country:</b> Two uppercase letters only Your Country Code: BD,US, etc.

#### We can see two file in this diretory
~~~
ls -ln
~~~
> exampledomain.com.key
> exampledomain.com.csr

#### Now we need to the .csr file for baught paid SSL.

#### Create a backup the .key file as it will be required later when installing your SSL certificate in Nginx.


## Step 2: Download certificate file from ssl provider portal

#### upzip the .zip file in we got 3 files.
#### Note: The 3 certificates are.
1. TrustedRoot.crt
   
2. DigiCertCA.crt

3. Yourdomain.crt

#### Put this certificates file on /etc/nginx/ssl/ location.


#### Some Times need to that.
#### Concatenate the primary and intermediate certificates

#### cat Yourdomain.crt DigiCertCA.crt TrustedRoot.crt  >> sslbundle.crt


## Step 3: Edit the Nginx virtual hosts file

#### Find to .conf file for locate ssl configuration file.
~~~
sudo find nginx.conf
~~~
#### Default VirtualHost file is.
~~~
nano /etc/nginx/sites-available/default

server {
    listen 443 ssl;
    listen [::]:443;
    
    ssl on;
    server_name yourdomain.com;
    
    ssl_certificate /etc/nginx/ssl/certificate.crt; #(or bundle.crt, .pem).
    ssl_certificate_key /etc/nginx/ssl/your_domain_name.key;
    
    access_log /var/log/nginx/nginx.vhost.access.log;
    error_log /var/log/nginx/nginx.vhost.error.log;
    
    location / {
    root  /var/www/html;
    index  index.html;
    }
    }
 ~~~   

## Step 4: Systax Check and Restart


#### Test the Nginx configuration.
~~~
nginx -t
~~~
#### Restart the Nginx services.
~~~
nginx -s reload
~~~
OR
~~~
systemctl restart nginx
~~~
#### check Now The Website is Secure.

#### or check from online:
[ssllabs.com](www.ssllabs.com/ssltest/)

#
