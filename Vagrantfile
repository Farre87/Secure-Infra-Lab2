# -*- mode: ruby -*-
# vi: set ft=ruby :

# ─── IP-adresser ────────────────────────────────────────────
CONTROL_IP  = "192.168.56.20"
NGINX_IP    = "192.168.56.21"
WEB1_IP     = "192.168.56.22"
WEB2_IP     = "192.168.56.23"
DATABASE_IP = "192.168.56.24"
MONITOR_IP  = "192.168.56.25"

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/jammy64"

  # ─── control ──────────────────────────────────────────────
  config.vm.define "control" do |control|
    control.vm.hostname = "control"
    control.vm.network "private_network", ip: CONTROL_IP
    control.vm.provider "virtualbox" do |vb|
      vb.name   = "control"
      vb.memory = 2048
      vb.cpus   = 1
    end
  end

  # ─── nginx ────────────────────────────────────────────────
  config.vm.define "nginx" do |nginx|
    nginx.vm.hostname = "nginx"
    nginx.vm.network "private_network", ip: NGINX_IP
    nginx.vm.network "forwarded_port", guest: 80, host: 8080
    nginx.vm.provider "virtualbox" do |vb|
      vb.name   = "nginx"
      vb.memory = 1024
      vb.cpus   = 1
    end
  end

  # ─── web1 ─────────────────────────────────────────────────
  config.vm.define "web1" do |web1|
    web1.vm.hostname = "web1"
    web1.vm.network "private_network", ip: WEB1_IP
    web1.vm.provider "virtualbox" do |vb|
      vb.name   = "web1"
      vb.memory = 1024
      vb.cpus   = 1
    end
  end

  # ─── web2 ─────────────────────────────────────────────────
  config.vm.define "web2" do |web2|
    web2.vm.hostname = "web2"
    web2.vm.network "private_network", ip: WEB2_IP
    web2.vm.provider "virtualbox" do |vb|
      vb.name   = "web2"
      vb.memory = 1024
      vb.cpus   = 1
    end
  end

  # ─── database ─────────────────────────────────────────────
  config.vm.define "database" do |database|
    database.vm.hostname = "database"
    database.vm.network "private_network", ip: DATABASE_IP
    database.vm.provider "virtualbox" do |vb|
      vb.name   = "database"
      vb.memory = 2048
      vb.cpus   = 1
    end
  end

  # ─── monitor ──────────────────────────────────────────────
  config.vm.define "monitor" do |monitor|
    monitor.vm.hostname = "monitor"
    monitor.vm.network "private_network", ip: MONITOR_IP
    monitor.vm.provider "virtualbox" do |vb|
      vb.name   = "monitor"
      vb.memory = 3072
      vb.cpus   = 2
    end
  end
end