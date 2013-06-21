Vagrant.configure("2") do |config|                                                                                                                                                        
  config.vm.box = "precise-server-cloudimg-amd64-vagrant-20130603"
  config.vm.box_url = "http://cloud-images.ubuntu.com/vagrant/precise/20130603/precise-server-cloudimg-amd64-vagrant-disk1.box"

  config.vm.network :forwarded_port, guest: 8080, host: 4567

  config.vm.provider :virtualbox do |v|
    v.customize ["modifyvm", :id, "--memory", "1024"]
  end

  config.vm.provider :aws do |aws, override|
    aws.region = 'us-east-1'
    aws.ami = 'ami-d0f89fb9' # aka ubuntu-12.04-64
    aws.instance_type = 't1.micro'
    aws.keypair_name = "#{ENV['USER']}-default"
    aws.security_groups = [ 'vagrant', 'logstash-default' ]
    aws.tags = {
      'Name' => "#{ENV['USER']}-#{File.basename(Dir.getwd)}",
    }

    override.ssh.username = 'ubuntu'
    override.ssh.private_key_path = '~/.ssh/cityindex-aws-ec2-default.pem'
  end

  config.vm.provision :shell, :path => ".build/dev_server/provision.sh"
  config.vm.synced_folder ".", "/app/app"
end

