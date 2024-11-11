# -*- mode: ruby -*-
# vi: set ft=ruby : 

Vagrant.configure("2") do |config|
  # Configuración del Servidor Apache 1
  config.vm.define "web1" do |web1|
    web1.vm.box = "ubuntu/bionic64"
    web1.vm.network "private_network", ip: "192.168.33.2"
    web1.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
      vb.cpus = 1
    end
    web1.vm.synced_folder "C:/Users/juanManuel/OneDrive/Desktop/examenfinal/mipagina1", "/var/www/html"  # Carpeta compartida
    web1.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y apache2
      systemctl enable apache2
      echo "Servidor Web 1" > /var/www/html/index.html
    SHELL
  end

  # Configuración del Servidor Apache 2
  config.vm.define "web2" do |web2|
    web2.vm.box = "ubuntu/bionic64"
    web2.vm.network "private_network", ip: "192.168.33.5"
    web2.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
      vb.cpus = 1
    end
    web2.vm.synced_folder "C:/Users/juanManuel/OneDrive/Desktop/examenfinal/mipagina2", "/var/www/html"  # Carpeta compartida
    web2.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y apache2
      systemctl enable apache2
      echo "Servidor Web 2" > /var/www/html/index.html
    SHELL
  end

  # Configuración del Servidor Nginx para balanceo de carga
  config.vm.define "nginx" do |nginx|
    nginx.vm.box = "ubuntu/bionic64"
    nginx.vm.network "private_network", ip: "192.168.33.12"
    nginx.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
      vb.cpus = 1
    end
    nginx.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y nginx
      # Configuración de Nginx para balanceo de carga
      cat <<EOL > /etc/nginx/conf.d/load_balancer.conf
upstream backend {
    server 192.168.33.10;
    server 192.168.33.11;
}
server {
    listen 80;
    location / {
        proxy_pass http://backend;
    }
}
EOL
      systemctl restart nginx
    SHELL
  end
end
