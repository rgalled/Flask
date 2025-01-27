Vagrant.configure("2") do |config|
  config.vm.box = "debian/bullseye64"
  config.ssh.insert_key = false
  config.vm.define "tomcat" do |c|
    c.vm.hostname = "tomcat"
    c.vm.network "private_network", ip: "192.168.57.10"
    c.vm.network "forwarded_port", guest: 8080, host: 8080
    c.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update && sudo apt-get install -y python3-pip
      pip3 install pipenv
      pip3 install python-dotenv
      apt install -y nginx
      mkdir -p /var/www/app
      chown -R $USER:www-data /var/www/app
      chmod -R 775 /var/www/app
      ln -s /etc/nginx/sites-available/app.conf /etc/nginx/sites-enabled/
      systemctl daemon-reload
      systemctl enable flask_app
      systemctl start nginx
      systemctl start flask_app
    SHELL
  end 
end