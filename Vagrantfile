# -*- mode: ruby -*-
# vi: set ft=ruby :


ipythonPort = 8888
ipythonHost = 8888
sparkUIPort = 4040


$script = <<SCRIPT
# APT-GET
sudo apt-get update
sudo apt-get upgrade

# PIP
apt-get -y install python-pip

# ANACONDA
anaconda=Anaconda-2.3.0-Linux-x86_64.sh
if [[ ! -f $anaconda ]]; then
	wget --quiet https://repo.anaconda.com/archive/$anaconda
  chmod +x $anaconda
  ./$anaconda -b -p /home/vagrant/anaconda
  cat >> /home/vagrant/.bashrc << END
  export PATH=/home/vagrant/anaconda/bin:$PATH
END
fi

# rm $anaconda

# JAVA
apt-get -y install openjdk-7-jdk

# APACHE SPARK
spark=spark-1.4.1-bin-hadoop2.6.tgz
if [[ ! -f $spark ]]; then
	wget --quiet https://archive.apache.org/dist/spark/spark-1.4.1/$spark
  tar xvf $spark
  # rm $spark
  mv spark-1.4.1-bin-hadoop2.6 spark
fi


# Install findspark & seaborn
/home/vagrant/anaconda/bin/pip install findspark==2.0.1 seaborn==0.8.0

echo 'SPARK_HOME=/home/vagrant/spark' >> /etc/environment
echo 'PYSPARK_PYTHON=/home/vagrant/anaconda/bin/python' >> /etc/environment

# Start ipython notebook
# sed -i "17i su vagrant -c 'cd /home/vagrant && /home/vagrant/anaconda/bin/ipython notebook --ip=\\"*\\"'" /etc/rc.local
cd /home/vagrant && /home/vagrant/anaconda/bin/ipython notebook --ip=0.0.0.0 --no-browser > jupyter.log 2>&1 &

SCRIPT


Vagrant.configure(2) do |config|
  config.vm.box = "trusty64"

  config.vm.network "forwarded_port", guest: ipythonPort, host: ipythonHost, host_ip: "127.0.0.1", guest_ip: "0.0.0.0"

  config.vm.network "forwarded_port", guest: sparkUIPort, host: sparkUIPort,
    auto_correct: true

  config.vm.provider :virtualbox do |v|
  	v.memory = 1024
  	v.cpus = 2
  end
  
  config.vm.provision :shell, inline: $script
  config.vm.synced_folder ".", "/home/vagrant/2015lab8"

end
