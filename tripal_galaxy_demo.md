# Galaxy

## Galaxy instance

We need a Galaxy instance running for this demo. The Staton lab has created a docker galaxy image with all necessary tools
installed. The docker image is built upon [this](https://github.com/bgruening/docker-galaxy-stable) image. 

* Launch a Jetstream VM with image https://use.jetstream-cloud.org/application/images/107

* Login to Jetstream VM `ssh your-jetstream-id@jetstream-VM-ip-address` and launch a Docker Galaxy container within Docker image `mingchen0919/docker-galaxy-for-tripal-galaxy-demo`

```
sudo docker run -it --rm -p 80:80 -p 8021:21 -p 8022:22  \
    	-e "ENABLE_TTS_INSTALL=True" \
    	-e "GALAXY_CONFIG_ADMIN_USERS=example@gmail.com" \
    	mingchen0919/docker-galaxy-for-tripal-galaxy-demo /bin/bash
    	
# start galaxy
startup
```

## Galaxy account

We need an Galaxy account to connect the Tripal site to the Galaxy instance.

The Galaxy instance should be available at URL: `your-jetstream-VM-ip-address`.

Go to **Users->Register** and use the email `example@gmail.com` to register an account. This email has been registered
as an admin user email.

After you register an account with the email and login with the account, you can go to **Users->Preferences->Manage API key->Create a new key** 
to create an API key. This API key will be used by Tripal Galaxy module to connect to this Galaxy instance.

## Import Galaxy workflows

We will use two workflows to demonstrate how to use the Tripal Galaxy module:

* hisat2 alignment for paired end reads: https://raw.githubusercontent.com/statonlab/dibbs/master/bdss-hisat2-alignment-pe(with-quality-control).ga
* wgcna: https://raw.githubusercontent.com/statonlab/dibbs/master/wgcna-analysis.ga

# Launch Tripal 3 site on Jetstream 

* Launch a Jetstream VM with image https://use.jetstream-cloud.org/application/images/107

* Login to Jetstream VM `ssh your-jetstream-id@jetstream-VM-ip-address` and launch a Docker tripal container

```unix
sudo docker run -it --rm -p 80:80 mingchen0919/docker-tripal-v3 /bin/bash
```

* Install Tripal Galaxy module

``` 
## git clone tripal_galaxy
cd /var/www/html/sites/all/modules
mkdir tripal_extensions
cd tripal_extensions
git clone https://github.com/tripal/tripal_galaxy.git

## install blend4php
cd /var/www/html/sites/all/libraries
git clone https://github.com/galaxyproject/blend4php.git

## enable tripal galaxy
drush en tripal_galaxy -y
```

# Using Tripal Galaxy module

Go to your tripal site, which should be available at the URL **your-jetstream-ip-address** 

* Add Galaxy instance

    + Go to **Tripal->Extensions->Galaxy->Add Galaxy Instance**, fill the form and submit.
    
* quota settings

    + Go to **Tripal->Extensions->Galaxy->Quata** to setup a system-wide user quota.
    
* Add Workflows

    + Go to **Tripal->Extensions->Galaxy->Workflows->Add workflows**. Select workflows that you want to expose to Tripal
    site users and submit.
    

# Testing Workflows


* wgcna-analysis
* hisat2-alignment
