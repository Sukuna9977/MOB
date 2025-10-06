Vagrant.configure("2") do |config|
  # Basic configuration - CHANGED BOX
  config.vm.box = "ubuntu/focal64"

  # VirtualBox provider settings
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "4096"
    vb.cpus = 2
    vb.gui = true
  end

  # Boot timeout - ADD THIS LINE INSIDE THE BLOCK
  config.vm.boot_timeout = 600

  # Port forwarding - Change conflicting ports
   config.vm.network "forwarded_port", guest: 8080, host: 18083, host_ip: "127.0.0.1"  # Jenkins
   config.vm.network "forwarded_port", guest: 50000, host: 50000, host_ip: "127.0.0.1"  # Jenkins Agent
   config.vm.network "forwarded_port", guest: 9000, host: 19000, host_ip: "127.0.0.1"  # SonarQube
   config.vm.network "forwarded_port", guest: 3001, host: 13000, host_ip: "127.0.0.1"  # Grafana
   config.vm.network "forwarded_port", guest: 9090, host: 19090, host_ip: "127.0.0.1"  # Prometheus
   config.vm.network "forwarded_port", guest: 27017, host: 8270, host_ip: "127.0.0.1"  # MongoDB
  # Docker + Jenkins installation
    config.vm.provision "shell", inline: <<-SHELL
    #!/bin/bash
    set -e
    
    echo "=== Installing Docker & Docker Compose ==="
    
    # Update system
    apt-get update
    apt-get install -y apt-transport-https ca-certificates curl gnupg lsb-release

    # Add Docker's official GPG key
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

    # Add Docker repository
    echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null

    # Install Docker
    apt-get update
    apt-get install -y docker-ce docker-ce-cli containerd.io

    # Install Docker Compose
    curl -L "https://github.com/docker/compose/releases/download/v2.24.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
    chmod +x /usr/local/bin/docker-compose

    # Add vagrant user to docker group
    usermod -aG docker vagrant

    echo "=== Docker & Docker Compose Installation Complete ==="
    echo "Use: docker-compose -f monitoring.yml up -d"
  SHELL
end
# ← NOTHING should be after this line