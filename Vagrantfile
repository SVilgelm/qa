$script = <<SCRIPT
sudo apt update
sudo apt install -y git bridge-utils crudini
git clone https://git.openstack.org/openstack-dev/devstack
cd devstack
cat << 'EOF' > local.conf
[[local|localrc]]
ADMIN_PASSWORD=secret
DATABASE_PASSWORD=$ADMIN_PASSWORD
RABBIT_PASSWORD=$ADMIN_PASSWORD
SERVICE_PASSWORD=$ADMIN_PASSWORD

LOGFILE=/opt/stack/logs/stack.sh.log
VERBOSE=True
LOG_COLOR=True
SCREEN_LOGDIR=/opt/stack/logs

enable_service neutron-trunk
enable_service neutron-segments
enable_service neutron-trunk
enable_service neutron-qos

disable_service tempest

EOF

IP=$(ip -o -4 a | awk '/3:/ {print $4}' | cut -d/ -f1)
echo "HOST_IP=${IP}" >> local.conf

crudini --set "/vagrant/tests/etc/tempest.conf" identity uri_v3 "http://${IP}/identity/v3/"

./stack.sh

echo "source ~/devstack/openrc admin admin" >> ~/.bashrc

for p in cinder keystone neutron nova; do

DIR="/vagrant/tests/etc/$p"
mkdir -p $DIR
oslopolicy-policy-generator --namespace $p --output-file $DIR/policy.json 2>/dev/null

done

mkdir -p /vagrant/tests/etc/glance/
cp /etc/glance/policy.json /vagrant/tests/etc/glance/

SCRIPT


Vagrant.configure("2") do |cfg|
  cfg.vm.define :devstack, primary: true do |c|
    c.vm.box = "ubuntu/bionic64"
    c.vm.provision :shell, privileged: false, inline: $script
    c.vm.network "public_network", bridge: "en0: Wi-Fi (AirPort)"
    c.vm.provider :virtualbox do |v|
      v.memory = 7168
      v.cpus = 4
    end
  end
end
