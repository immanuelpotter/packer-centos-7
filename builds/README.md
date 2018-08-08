This directory will contain built Vagrant box files. To get them up and running:

Run: BOX_NAME="your_box_name"
     vagrant box add ${BOX_NAME}.box --name ${BOX_NAME}
     vagrant init
     sed -i "s/  config\.vm\.box \= \"base\"/  config\.vm\.box \= \"$BOX_NAME\"/" Vagrantfile
     vagrant up
