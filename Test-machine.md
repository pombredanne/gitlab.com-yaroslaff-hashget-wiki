For testing purpose, we recommend to make test machine this way:

  host# lxc-create -n mydebvm -t download -- --dist=debian --release=stretch --arch=amd64
  host# lxc-attach -n mydebvm
  mydebvm# apt install wget apache2 mysql-server vim
