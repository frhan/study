Ubuntu 18.04
-------------

1. Run update command
`sudo apt update`

2. install java [`openjd 8`]
`sudo apt install openjdk-8-jdk`

3. Run `java -version`

4. 


## setup public and private key with SCM
1. from jenkins server (as root user or add `sudo` if not root user)
 `su jenkins`
2. generate `ssh`
`ssh keygen`
3.  run and copy the output to SCM access keys
`cat /var/lib/jenkins/.ssh/id_rsa.pub`
4. run and copy
`cat  /var/lib/jenkins/.ssh/id_rsa`

go to the `jenkins` -> `Credentials` -> `System` -> `Global Credential` 
`Add Credntial` -> change kind to `SSh username with password`
set `username` as `jenkins`   


# setup webhooks for github

#

### Ref:
* [How To Install Java with `apt` on Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-install-java-with-apt-on-ubuntu-18-04)

* [How To Install Jenkins on Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-install-jenkins-on-ubuntu-18-04)

* [How To Configure Jenkins with SSL Using an Nginx Reverse Proxy on Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-configure-jenkins-with-ssl-using-an-nginx-reverse-proxy-on-ubuntu-18-04)