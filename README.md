vagrant-for-mpr
===============

#####Steps:
1. run in bash "librarian-chef install" to install all require cookbooks;
2. put the sources into the src folder;
3. put backup of db into the data folder and call that file "physiorehab.backup";
3. run in bash "vagrant up";
4. copy media files into the media folder;
5. enter into the vagrant with "./vagrant-node-debug.sh" - this file will also forward node.js debug port. If you want to use remote debug, you should always run this file;
6. under vagrant ssh run the app: "sudo sh master.sh devel" from the /var/www/mpr/src folder;


> Notice!!! Everytime you've reloaded vm, you must start nginx, redis-server services again manually 
