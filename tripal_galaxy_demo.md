# Galaxy

## Galaxy instance

We need a Galaxy instance running for this demo. The Staton lab has created a docker galaxy image with all necessary tools
installed. The docker image is built upon [this](https://github.com/bgruening/docker-galaxy-stable) image. 

* Launch a Jetstream VM with image https://use.jetstream-cloud.org/application/images/107

* Login to Jetstream VM `ssh your-jetstream-id@jetstream-VM-ip-address` and launch a Docker tripal container

```
sudo docker run -it --rm -p 80:80 -p 8021:21 -p 8022:22  \
    	-e "ENABLE_TTS_INSTALL=True" \
    	-e "GALAXY_CONFIG_ADMIN_USERS=example@gmail.com" \
    	mingchen0919/docker-galaxy-for-tripal-galaxy-demo /bin/bash
    	
# start galaxy
startup
```

## Galaxy account

The Galaxy instance should be available at URL: `your-jetstream-VM-ip-address`.

Go to **Users->Register** and use the email `example@gmail.com` to register an account. This email has been registered
as an admin user email (see the command that we used to launch the Galaxy instance).

After you register an account with the email and login with the account, you can go to **Users->Preferences->Manage API key->Create a new key** 
to create an API key. If an API key already exists, you can use the existing one. If not, you will need to create one. 
This API key together with the account user name will be used by Tripal Galaxy module to connect to this Galaxy instance.

## Import Galaxy workflows

We will use two workflows to demonstrate how to use the Tripal Galaxy module. To import workflows into Galaxy, go to
the Galaxy instance and click **Workflow->Upload or import workflow** and copy & paste a workflow URL to the input box 
and then click **Import**. Below are URLs of two workflows we are going to use for this demo. 

* hisat2 alignment for paired end reads: https://raw.githubusercontent.com/statonlab/dibbs/master/bdss-hisat2-alignment-pe(with-quality-control).ga
* wgcna: https://raw.githubusercontent.com/statonlab/dibbs/master/wgcna-analysis.ga

# Launch Tripal 3 site on Jetstream 

* Launch a Jetstream VM with image https://use.jetstream-cloud.org/application/images/107

* Login to Jetstream VM `ssh your-jetstream-id@jetstream-VM-ip-address` and launch a Docker tripal container

```unix
sudo docker run -it --rm -p 80:80 mingchen0919/docker-tripal-v3 /bin/bash
```

## Tripal Galaxy module

* Install Tripal Galaxy module

``` 
## git clone tripal_galaxy
cd /var/www/html/sites/all/modules
mkdir tripal_extensions
cd tripal_extensions
git clone https://github.com/tripal/tripal_galaxy.git
```

* Install `blend4php` library

The `blend4php` library is a dependency of the Tripal Galaxy module. It needs to be installed within Drupal's **libraries** directory.

```
## install blend4php
cd /var/www/html/sites/all/libraries
git clone https://github.com/galaxyproject/blend4php.git
```

* Enable the module

```
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

First, download the [demo datasets](https://github.com/statonlab/tripal_galaxy_demo/tree/master/demo-data) to your local computer.

(Maybe we need to demo how to edit the form title and labels for workflows here)

## wgcna-analysis

* step 1: no input, click **Next Page**
* step 2: expression data
    + click **Upload Files** and upload the file `demo-data/expression_data.csv`
    + click **Next Page**
* step 3: trait data
    + click **Upload Files** and upload the file `demo-data/trait_data.csv`
    + click **Next Page**
* step 4: use default, Click **Next Page**
* step 5: use default, Click **Next Page**
* step 6: use default, Click **Next Page**
* step preview
* submit

## hisat2-alignment

* step 1: no input, click **Next Page**
* step 2: reference genom, click **Upload Files** to upload the dataset `demo-data/GCF_000005845.2_ASM584v2_genomic.fna.fasta`
* step 3:
    + **SRR/DRR/ERR accessions of single end SRA**: input `ERR1748507`
    + **SRR/DRR/ERR accessions of paired end SRA**: input `ERR1748507`
    + all other fields use default. Click **Next Page**
* step 4: use default, Click **Next Page**
* step 5: use default, Click **Next Page**
* step 6: use default, Click **Next Page**
* step 7: use default, Click **Next Page**
* step 8: 
    + **Specify strand information**: select `Forward (FR)`
    + all other fields use default, Click **Next Page**
* step 9: use default, Click **Next Page**
* step preview
* submit


