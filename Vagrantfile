require 'vagrant-aws'
require 'yaml'

Vagrant.configure('2') do |config|
    config.vm.box = 'aws-dummy'
      config.vm.provider 'aws' do |aws, override|
        config.vm.synced_folder ".", "/vagrant", disabled: true
        aws_config = YAML::load_file(File.join(Dir.home, ".aws_secrets"))
        aws.access_key_id = aws_config.fetch("access_key_id")
        aws.secret_access_key = aws_config.fetch("secret_access_key")
        aws.keypair_name = 'vagrantAWS-key'
        aws.instance_type = "t2.micro"
#       aws.keypair_name = aws_config.fetch("keypair_name")
        aws.region = 'us-east-1'
        aws.ami = 'ami-0394fe9914b475c53'
        aws.security_groups = 'sg-08ce8ddb34a878eaf'
#       aws.security_groups = 'vagrant'
#       aws.security_groups = ['default']
        aws.subnet_id = 'subnet-0708d9c83404b507c'
        aws.tags = {
          'Name'=> "vagrantAWS-Data"
        }

        aws.block_device_mapping = [{
          'DeviceName' => '/dev/sda1',
          'Ebs.ValumeSize' => 20,
          'Ebs.DeleteOnTermination' => true,
#         'Ebs.DeleteOnTermination' => false,
          'Ebs.VolumeType' => 'gp2'
#         'DeviceName' => AWS_DEVICE_NAME,
#         'Ebs.ValumeSize' => AWS_DEVICE_SIZE,
#         'Ebs.DeleteOnTermination' => false,
#         'Ebs.VolumeType' => AWS_DEVICE_VOL_TYPE,
        }]

        override.ssh.username = "ec2-user"
#       override.ssh.private_key_path = aws_config.fetch("keypair_path")
        override.ssh.private_key_path = '~/.ssh/vagrantAWS-key.pem'
      end
      config.vm.provision "ansible" do |ansible|
        ansible.playbook = "deploy_PythonRH7.yml"
        ansible.verbose = "true"
      end
end
