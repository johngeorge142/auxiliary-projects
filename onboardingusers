# Shell Scripting.
# This script will read a CSV file that contains 20 Linux users.
# This script will create each user on the sever and add to an existing group called 'Developers'.
# This script will first check for the existence of the user on the system, before it will attempt to create
# The user that is being created also must have a default home folder
# Each user should have a .ssh folder within its HOME folder. if it does not exist, then it will be created. 
# For each user's SSH configuration, We will create an authorized_keys file and add the below public key.


#!/usr/bin/bash
userfile=$(cat names.csv)
PASSWORD=password 

# To ensure the user running this script has sudo privilege
    if [  $(id -u) -eq 0  ]; then 

# Reading the CSV file

        for user in $userfile; 
        do 
            echo $user
        if id "$user" &>/dev/null  
        then 
            echo "User Exist"
        else

# This will create a new user
        useradd -m -d /home/$user -s /bin/bash -g developers $user 
        echo "New User Created"
        echo


# This will create a ssh folder in the user home folder
        su - -c "mkdir ~/.ssh" $user
        echo ".ssh directory created for new user"
        echo


# We need to set the user permission for the ssh dir 
        su - -c "chmod 700 ~/.ssh" $user
        echo "user permission for .ssh directory set"
        echo


# This will create an authorized_key file
        su - -c "touch ~/.ssh/authorized_keys"  $user 
        echo "Authorized key file created"
        echo


# We need to set permission for the key file
        su - -c "chmod 600 ~/.ssh/authorized_keys" $user
        echo "user permission for the authorized key file set"
        echo 

# We need to create and set public key for users in the server
        cp -R "/root/onboarding_users/id_rsa.pub" "/home/$user/.ssh/authorized_keys"
        echo "Copyied the Public Key to New User Account on the server"
        echo 
        echo

        echo "USER CREATED"


# Generate a password.
sudo echo -e "$PASSWORD\n$PASSWORD" | sudo passwd "$user"
sudo passwd -x 5 $user
            fi 
        done 
    else
    echo "Only Admin can onboard A user"
    fi
