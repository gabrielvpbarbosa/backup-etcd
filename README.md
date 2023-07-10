# backup-etcd

Os passos abaixo foram realizados de acordo com a doc do kubernetes: https://kubernetes.io/docs/tasks/administer-cluster/configure-upgrade-etcd/

1. Install o etcd client
   sudo apt install etcd-client

2. Para obter informações do cacert, cert e key dar um describe no pod do etcd que ele contém todas as informações
   kubectl describe pod etcd-control-plane -n kube-system
   
4. Executar o comando para gerar o snapshot dos dados atuais.

ETCDCTL_API=3 etcdctl --cacert=/etc/kubernetes/pki/etcd/ca.crt --cert=/etc/kubernetes/pki/etcd/server.crt --key=/etc/kubernetes/pki/etcd/server.key  snapshot save backup.db

# restore-etcd
1. Executar o seguinte comando para realizar o restore do arquivo de backup realizado nos passos anteriores
   
   ETCDCTL_API=3 etcdctl snapshot restore --data-dir /var/lib/etcd-backup etcd.db

   A opção --data-dir informa qual será o novo caminho onde o dump será restaurado.
   
2. Alterar o manifesto de criação do pod do etcd em /etc/kubernetes/manifests/etcd.yaml, alterando o path do volume que será montado no pod para o novo local onde foi restaurado o abckup.
   
