# LAB: Google Cloud Fundementals: Getting Started with Compute Engine

# Objectives
In this lab, you learn how to launch a solution using Cloud Marketplace.

 Create a Compute Engine virtual machine using the Google Cloud Platform (GCP) Console.
 Create a Compute Engine virtual machine using the gcloud command-line interface.
 Connect between the two instances.

# Create a virtual machine using the gcloud command line

- gcloud compute instances create <instance_name> --zone <zone> --project <id>
  [1]    [2]      [2]    [          3         ]


To display a list of all the zones in the region to which Qwiklabs assigned you, enter this partial command gcloud compute zones list | grep followed by the region that Qwiklabs or your instructor assigned you to.

Your completed command will look like this:

``` gcloud compute zones list | grep us-central1```

Choose a zone from that list other than the zone to which Qwiklabs assigned you. For example, if Qwiklabs assigned you to region us-central1 and zone us-central1-a you might choose zone us-central1-b.

To set your default zone to the one you just chose, enter this partial command gcloud config set compute/zone followed by the zone you chose.

Your completed command will look like this:

``` gcloud config set compute/zone us-central1-b ```

To create a VM instance called my-vm-2 in that zone, execute this command:

``` gcloud compute instances create "my-vm-2" \
--machine-type "n1-standard-1" \
--image-project "debian-cloud" \
--image "debian-9-stretch-v20190213" \
--subnet "default"
```

To close the Cloud Shell, execute the following command:

`exit`


# Connect between VM instances

 To open a command prompt on the my-vm-2 instance, click SSH in its row in the VM instances list.

 Use the ping command to confirm that my-vm-2 can reach my-vm-1 over the network:

`ping my-vm-1`

 Notice that the output of the ping command reveals that the complete hostname of my-vm-1 is my-vm-1.c.PROJECT_ID.internal, where PROJECT_ID is the name of your Google Cloud Platform project. GCP automatically supplies Domain Name Service (DNS) resolution for the internal IP addresses of VM instances.

 Press Ctrl+C to abort the ping command.

 Use the ssh command to open a command prompt on my-vm-1:

`ssh my-vm-1`

 If you are prompted about whether you want to continue connecting to a host with unknown authenticity, enter yes to confirm that you do.

At the command prompt on my-vm-1, install the Nginx web server:

`sudo apt-get install nginx-light -y`

Use the nano text editor to add a custom message to the home page of the web server:

`sudo nano /var/www/html/index.nginx-debian.html`

Use the arrow keys to move the cursor to the line just below the h1 header. Add text like this, and replace YOUR_NAME with your name:

Hi from YOUR_NAME

Press Ctrl+O and then press Enter to save your edited file, and then press Ctrl+X to exit the nano text editor.

Confirm that the web server is serving your new page. At the command prompt on my-vm-1, execute this command:

`curl http://localhost/`

The response will be the HTML source of the web server's home page, including your line of custom text.

To exit the command prompt on my-vm-1, execute this command:

`exit`

You will return to the command prompt on my-vm-2

To confirm that my-vm-2 can reach the web server on my-vm-1, at the command prompt on my-vm-2, execute this command:

`curl http://my-vm-1/`



