# Vagrant 配置

- ref: https://pan.baidu.com/s/1c4RFaA#list/path=%2FK8S

  tar zxvf k8s.1-11-2.tar.gz -C /Users/twocucao/Codes/ReposPublic/kubeasz/ # your
  repos here
  tar zxvf basic_images_kubeasz_0.3.tar.gz -C /Users/twocucao/Codes/ReposPublic/kubeasz/down/ # your repos here

  cd /Users/twocucao/Codes/ReposPublic/kubeasz
  vagrant up
  cd
  ansible-playbook 90.setup.yml -i example/hosts.vagrant.s-master.example --become-user=root -e "base_dir=`pwd`" -u root
