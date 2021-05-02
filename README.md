# Ultimate-Pi-hole
The Ultimate Pi-hole configuration with home DoH and DoT to acces anywhere!

[This is not finised and still in the making]

--------------------------------------

# Install the Os

First we are going to download and install our Os.

We are going to use Raspberry Pi OS (Raspbian) for our Os, but you can also use Ubuntu.

We are going to install our Os without Gui (or a headless install):


For your Raspberry pi:
I recommend using the raspbery pi imager
https://www.raspberrypi.org/software/

Choose witch Os your using, grab a sd-card 8Go should be good, but i recommend having a 16Go one.

After this launch the Raspberry Pi Imager. Now we are going to choose our os for our sweet raspberry pi.

For this click on `choose os`, we are not going to choose the first option, but the second one `raspberry pi os (other)` and we are going to select `raspberry pi os lite`.
After this choose your sd-card in the `choose storage` button abd click `write` to flash your sd-card!

Now before we take our sd-card out we are going to add the `SSH` file in the boot section of our sd-card so we can have acces to our raspberry after it is hooked up to the network.

--------------------------------------

# Initial connection

After having powerd up your raspberry pi and have located it on your network we are going to connect to it via `ssh`.
On Windows your can either use the `CMD` with the `OpenSSH` feature (witch i reccomend) or `putty`(https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)

If your already on Linux it should already be built-in on your disto.

So how to ssh in our raspberry pi?
Simple, just type: `ssh pi@<the raspberry pi ip>`
`pi` is our user (i recommend to change it for security, but to keep it simple we are going to use the default user)

`I recommend changing your password for security, just type "passwd" to change it`

--------------------------------------

# Making sure everything is up-to-date

After you are succesfully connect to your raspberry pi we are going to make sure to everything is up-to-date!

For that we are just going to use the following command: 
`sudo apt-get update && sudo apt-get upgrade`

`sudo apt-get update` will update the local repo to know if there is update.
`sudo apt-get upgrade` will make the update happend (if there is some update)

If you have Os update just simply execute `sudo apt-get dist-upgrade`

We also going to install `ufw` as our firewall for a little bit of protection
`sudo apt-get install ufw`

And enable it with this command `sudo ufw enable`

--------------------------------------

# Install Pi-hole

Now we are finally going to install Pi-Hole!

For this we are going to use the One-Step Automated Install
`https://github.com/pi-hole/pi-hole/#one-step-automated-install`

--------------------------------------

# Dynamique Dns for our setup

Now just before making more stuff, we are going to create a DynDns so we can have acces to our Pi-hole web interface and dns from outside.

First install ddclient `sudo apt-get install ddclient`

Now go create an account at a Dynamique dns service. `I've recommend https://desec.io/ since it a secure service that protect privacy and it free, and that the premade config is made on it`

Create your custom domain name and make a token `this will be important for our ddclient`
In your ddclient config file, copy and past the premade one and change the values of `<token>` and `<yourdomain>` accordingly.

Now your done setting up your Dynamique Dns! Awesome!

--------------------------------------

# Nginx and Dns-over-Https setup

After this we are going to change lighttpd to nginx.
We are also going to follow the Optiona configuration part so we can have acces to our Dns and our interface from the outside.

But to do this we need to make a few changes in the configuration.
I will provide in this repo a full premade config and a config that you to your `nginx.conf` file if you want it that way.

You will just need to replace with your domain the `<certbot>` part.

Now we are going to add the support of Dns-over-Https to our nginx config.
Simply intall dnss (Dns-over-https)`sudo apt-get install dnss`
And start and enable the dnss and nginx services. No futher config is needed!

`sudo systemctl start nginx.service dnss.service`
`sudo systemctl enable nginx.servce dnss.service`

--------------------------------------

# Dns-over-Tls setup

Now for the Dns-over-Tls.
First we are going to allow the port `853` to `ufw` with the following
`sudo ufw allow 853/tcp`
Reload ufw `sudo ufw reload`

Now we install stunnel:
`sudo apt-get install stunnel`
And for our config just past the `stunnel.conf` of this repo and replace with your domain the `<certbot>` part.

--------------------------------------

# (Optional) Add a secure dns backend to our upstreams

Now for our backend we are going to use Unbound to have a secure way to our upstream dns.
First install it:
`sudo apt-get install unbound`

Now you can copy and past the `unbound.conf` of this repo.
It curently use Cloudflare as it upstream but i've recommend the followings dns for maxmuim privacy.

`https://github.com/Gontier-Julien/Recommended-Dns-services-for-privacy`

Additionally you can add this to your `/etc/hosts` so you can know on the Pi-hole interface that the queries come from Undound.
`127.0.0.2	unbound`

And now you can start and enable Undound!

`sudo systemctl start unbound.service`
`sudo systemctl enable unbound.service`

--------------------------------------

# Todos

-"Automated" install.

-Use unbound for Dns-over-Https for Having only exteranl dns acces without the interface.
