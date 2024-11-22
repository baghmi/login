Vagrant.configure("2") do |config|
  config.vm.define "ismael" do |ismael|
    ismael.vm.box = "debian/bookworm64"
    ismael.vm.network "private_network", ip: "192.168.57.102"
    ismael.vm.provision "shell", inline: <<-SHELL
      apt update
      apt install -y nginx git openssl ufw

      #Creando primera web git
      mkdir -p /var/www/ismael/html
      git clone https://github.com/cloudacademy/static-website-example /var/www/ismael/html
      chown -R www-data:www-data /var/www/ismael/html
      chmod -R 755 /var/www/ismael
      cp -v /vagrant/ismael-sites /etc/nginx/sites-available/ismael
      ln -s /etc/nginx/sites-available/ismael /etc/nginx/sites-enabled/

     



      #web con login

      mkdir -p /var/www/login/
      cp -r /vagrant/html /var/www/login/
      chown -R www-data:www-data /var/www/login/html
      chmod -R 755 /var/www/login
      cp -v /vagrant/login-sites /etc/nginx/sites-available/login
      ln -s /etc/nginx/sites-available/login /etc/nginx/sites-enabled/


      sh -c "echo -n 'ismael:' >> /etc/nginx/.htpasswd"
      sh -c "openssl passwd -apr1 'vagrant'>> /etc/nginx/.htpasswd"
      sh -c "echo -n 'baghdadi:' >> /etc/nginx/.htpasswd"
      sh -c "openssl passwd -apr1 '12345'>> /etc/nginx/.htpasswd"



      

      systemctl restart vsftpd
      systemctl restart nginx
      systemctl status nginx
    SHELL
  end
end
