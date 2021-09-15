# Change vb.name, config.vm.hostname to set Virtual Machine Name and Hostname
# Change ip: to update ipaddress
# Creating multiple machines (update Vagrantfile with name, hostname and IP)

Vagrant.configure("2") do |config|

  config.vm.define "ubuntu" do |cfg|
    cfg.vm.box = "hashicorp/bionic64"
    cfg.vm.network "private_network", ip: "192.168.38.200"
    cfg.vm.provider "virtualbox" do |vb, override|
      vb.gui = true
      vb.name = "ubuntu200"    
      vb.customize ["modifyvm", :id, "--memory", 2048]
      vb.customize ["modifyvm", :id, "--cpus", 2]
      vb.customize ["modifyvm", :id, "--vram", "32"]
      vb.customize ["modifyvm", :id, "--nicpromisc2", "allow-all"]
      vb.customize ["modifyvm", :id, "--clipboard", "bidirectional"]
      vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      vb.customize ["setextradata", "global", "GUI/SuppressMessages", "all" ]
    end
  end
  config.vm.hostname = "ubuntu200"

  # OSQuery config files are copied
  # Fleetdm server cert is pulled from 192.168.38.105 using curl and default creds
  config.vm.provision "file", source: "flagfile.txt", destination: "~/flagfile.txt"
  config.vm.provision "file", source: "secret.txt", destination: "~/secret.txt"
  config.vm.provision "file", source: "ossec.conf", destination: "~/ossec.conf"
  config.vm.provision "shell", inline: <<-SHELL
     export OSQUERY_KEY=1484120AC4E9F8A1A577AEEE97A80C63C9D8B80B
     apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys $OSQUERY_KEY
     add-apt-repository 'deb [arch=amd64] https://pkg.osquery.io/deb deb main'
     wget -O -  https://www.atomicorp.com/RPM-GPG-KEY.atomicorp.txt | apt-key add -
     echo "deb https://updates.atomicorp.com/channels/atomic/ubuntu bionic main" > /etc/apt/sources.list.d/atomic.list
     apt-get update
     apt-get install -y osquery ossec-hids-server sshpass

     JWT=$(curl --insecure 'https://192.168.38.105:8412/api/v1/fleet/login' -H 'User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:90.0) Gecko/20100101 Firefox/90.0' -H 'Accept: application/json' -H 'Accept-Language: en-US,en;q=0.5' --compressed -H 'Referer: https://192.168.38.105:8412/login' -H 'Content-Type: application/json' -H 'Origin: https://192.168.38.105:8412' -H 'Connection: keep-alive' -H 'Sec-Fetch-Dest: empty' -H 'Sec-Fetch-Mode: cors' -H 'Sec-Fetch-Site: same-origin' -H 'Pragma: no-cache' -H 'Cache-Control: no-cache' -H 'TE: trailers' --data-raw '{"username":"admin","password":"admin123#"}' | python3 -c "import sys, json; print(json.load(sys.stdin)['token'])")

     curl --insecure 'https://192.168.38.105:8412/api/v1/fleet/config/certificate' -H 'User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:90.0) Gecko/20100101 Firefox/90.0' -H 'Accept: application/json' -H 'Accept-Language: en-US,en;q=0.5' --compressed -H 'Content-Type: application/json' -H "Authorization: Bearer $JWT" -H 'Connection: keep-alive' -H 'Sec-Fetch-Dest: empty' -H 'Sec-Fetch-Mode: cors' -H 'Sec-Fetch-Site: same-origin' -H 'Pragma: no-cache' -H 'Cache-Control: no-cache' | python3 -c "import sys, json, base64; f=open('/tmp/fleet.pem', 'wb'); f.write(base64.b64decode(json.load(sys.stdin)['certificate_chain']))"

     mv /tmp/fleet.pem /etc/osquery/
     mv flagfile.txt /etc/osquery/osquery.flags
     mv secret.txt /etc/osquery/

     # Match SSL Cert Hostname (fleet)
     echo "192.168.38.105    fleet" >> /etc/hosts

     # Add UDP syslog input to splunk and syslog_output to ossec.conf
     # sshpass allows input of password on the command line
     echo "enable splunk udp"
     sshpass -p "vagrant" ssh -o StrictHostKeyChecking=no vagrant@192.168.38.105 "sudo /opt/splunk/bin/splunk add udp 514 -sourcetype syslog -index syslog -auth admin:changeme"
     mv ossec.conf /var/ossec/etc/
     echo "enable syslog on ossec"
     /var/ossec/bin/ossec-control enable client-syslog
     echo "restart ossec and osqueryd"
     systemctl restart ossec
     systemctl restart osqueryd
  SHELL
end
