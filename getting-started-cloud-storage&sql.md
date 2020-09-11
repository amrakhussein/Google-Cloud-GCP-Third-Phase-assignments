
# Getting Started with Cloud Storage and Cloud SQL

# Objectives
In this lab, you learn how to perform the following tasks:

Create a Cloud Storage bucket and place an image into it.

Create a Cloud SQL instance and configure it.

Connect to the Cloud SQL instance from a web server.

Use the image in the Cloud Storage bucket on a web page.

Deploy a web server VM instance
#Create a virtual machine using the gcloud command line
gcloud compute instances create <instance_name> --zone --project [1] [2] [2] [ 3 ]
To display a list of all the zones in the region to which Qwiklabs assigned you, enter this partial command gcloud compute zones list | grep followed by the region that Qwiklabs or your instructor assigned you to.

Your completed command will look like this:

`gcloud compute zones list | grep us-central1`

Choose a zone from that list other than the zone to which Qwiklabs assigned you. For example, if Qwiklabs assigned you to region us-central1 and zone us-central1-a you might choose zone us-central1-b.

To set your default zone to the one you just chose, enter this partial command gcloud config set compute/zone followed by the zone you chose.

Your completed command will look like this:

`gcloud config set compute/zone us-central1-b`

To create a VM instance called my-vm-2 in that zone, execute this command:
```
--machine-type "n1-standard-1" \
--image-project "debian-cloud" \
--image "debian-9-stretch-v20190213" \
--subnet "default"
```

Enter the following script as the value for Startup script:
```
apt-get update
apt-get install apache2 php php-mysql -y
service apache2 restart
```
Be sure to supply that script as the value of the Startup script field. If you accidentally put it into another field, it won't be executed when the VM instance starts.

Leave the remaining settings as their defaults, and click Create.
Instance can take about two minutes to launch and be fully available for use.

On the VM instances page, copy the bloghost VM instance's internal and external IP addresses to a text editor for use later in this lab.
Create a Cloud Storage bucket using the gsutil command line
All Cloud Storage bucket names must be globally unique. To ensure that your bucket name is unique, these instructions will guide you to give your bucket the same name as your Cloud Platform project ID, which is also globally unique.

Cloud Storage buckets can be associated with either a region or a multi-region location: US, EU, or ASIA. In this activity, you associate your bucket with the multi-region closest to the region and zone that Qwiklabs or your instructor assigned you to.


In Cloud Shell, the DEVSHELL_PROJECT_ID environment variable contains your project ID. Enter this command to make a bucket named after your project ID:

`gsutil mb -l $LOCATION gs://$DEVSHELL_PROJECT_ID`

Retrieve a banner image from a publicly accessible Cloud Storage location:

`gsutil cp gs://cloud-training/gcpfci/my-excellent-blog.png my-excellent-blog.png`

Copy the banner image to your newly created Cloud Storage bucket:

`gsutil cp my-excellent-blog.png gs://$DEVSHELL_PROJECT_ID/my-excellent-blog.png`

Modify the Access Control List of the object you just created so that it is readable by everyone:

`gsutil acl ch -u allUsers:R gs://$DEVSHELL_PROJECT_ID/my-excellent-blog.png`

# Create the Cloud SQL instance
`gcloud sql instances create prod-instance --database-version=MYSQL_5_7 --tier=db-n1-standard-1 --region=us-central1 --root-password=password123`


# Run the following gcloud command to find the CLOUD_SQL_PUBLIC_IP_ADDR:
`gcloud sql instances list`

# Install the mysql client on the Compute Engine instance, if it is not already installed.
```
sudo apt-get update
sudo apt-get install mysql-server
```
# Install the Cloud SQL Proxy on the Compute Engine instance.
`wget https://dl.google.com/cloudsql/cloud_sql_proxy.linux.amd64 -O cloud_sql_proxy`
# Make the proxy executable
`chmod +x cloud_sql_proxy`

# Start the proxy
`./cloud_sql_proxy -instances=<INSTANCE_CONNECTION_NAME>=tcp:3306`
# Start the mysql session
mysql -u <USERNAME> -p --host 127.0.0.1
Restart the web server:

sudo service apache2 restart

Return to the web browser tab in which you opened your bloghost VM instance's external IP address. When you load the page, the following message appears:

Database connection succeeded.
