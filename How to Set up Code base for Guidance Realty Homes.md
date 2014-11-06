# Guidance Realty Homes Development #
## 1)	Installs and downloads ##
a.	Download and install VirtualBox

b.	Download and install Vagrant

c.	Register for an account with github.com

d.	Register for an account with bitbucket.com

e.	Send your bitbucket username to Daniel so we can add your account to the repositories

f.	Create a directory on your computer for this project (example: “c:\Projects\Guidance\Vagrant\grealtydev\”)

g.	Create a “vhosts” subdirectory in the new project folder

h.	Download guidance vagrant file and save in the new project directory (not in the vhosts directory, the vhosts directory will be used to store the development website files)

##2)	Init and Startup the development box##
a.	Go into the new project folder created above

b.	Run command “vagrant init” to initiate the vagrant box

c.	Run command “vagrant up” to start up box

d.	Run command “vagrant ssh” to log in to the box 

e.	Create SSH key (ssh-keygen –t rsa

f.	Log in to bitbucket and add the new ssh key to your bitbucket user’s preferences. 

               i.	The new ssh key should be located in ~/.ssh/id_rsa.pub

               ii.	Just type “cat ~/.ssh/id_rsa.pub” and copy+paste the output into bitbucket

##3)	Setting up the guidance realty homes website for development##

a.	Cd to “/var/www/vhosts”

b.	Make a “guidancerealtyhomes.dev” folder and go into it

c.	Clone the “guidance-realty” repo into a “grealty” folder (/var/www/vhosts/ guidancerealtyhomes.dev/grealty)        (use git clone <git-repo> grealty)

d.	Go into the new grealty folder

e.	Create the mysql database

                             i.	Type “mysql” to open a mysql prompt
                             ii.	Type “create database grealtyhomesdev_db1” to create the database
                            iii.	Type “quit” to exit the mysql prompt

f.	Cd to “app/config/”

g.	Copy the development folder (including all files in it) to a new folder called “local”

                             i.	Go into the new local folder and open each of the settings files.
                            ii.	Edit any settings that need to be updated for your local environment (for example make sure the database name matches the above database you created) and save the files

h.	Cd back to the main “grealty” folder

i.	Edit “app/routes.php” and comment out all the lines in the file.

j.	Type “composer install” (DO NOT RUN “composer update”)

k.	Type “artisan migrate” to create the database tables

l.	Seed the database using the mysql import script

m.	Edit “app/routes.php” again and undo your above comments so the file is back to normal.

##4)	Setup apache to server the development website##

a.	Copy the example apache conf file

                       i.	Cp /etc/httpd/sites-available/example.conf /etc/httpd/sites-available/guidancerealtyhomes.dev.conf
                      ii.	Edit the new “/etc/httpd/sites-available/guidancerealtyhomes.dev.conf” file and make the necessary changes so it points to the “(/var/www/vhosts/ guidancerealtyhomes.dev/grealty”) directory

b.	Run command: “a2ensite guidancerealtyhomes.dev” to activate the website

c.	Restart apache (service httpd restart)

##5)	Update your computer to be able to access the new website##

a.	On your computer (not inside the vagrant vm), edit your hosts file and add the following two lines without the quotes:

                       i.	“192.168.10.10	guidancerealtyhomes.dev”
                      ii.	“192.168.10.10	www.guidancerealtyhomes.dev”

b.	Save your hosts file

##6)	Test##
a.	Open a browser on your computer and go to www.guidancerealtyhomes.dev

##7)	The website files will be located in the vhosts folder you created in step 1.g, so you can setup whatever IDE you want to edit those files.##

##8)	Your git commands should be run from within the vagrant box in the “/var/www/vhosts/guidancerealtyhomes.dev/grealty” folder##