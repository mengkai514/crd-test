# 1.首先创建一个新的项目
mkdir -p $HOME/projects/memcached-operator
cd $HOME/projects/memcached-operator
operator-sdk init --domain example.com --repo github.com/example/memcached-operator

# 2.创建一个crd
operator-sdk create api --group cache --version v1alpha1 --kind Memcached --resource --controller

# 3.添加一些属性

# 4.生成CRD YMAL文件
make generate
make manifests

# 5.镜像编译和推送
make docker-build IMG=memcached-operator:latest
make docker-push IMG=memcached-operator:latest

# 6.生成k8s资源
make build-installer

# 7.将CRD安装到k8s
make install

# 8.控制器部署到k8s
make deploy

# 9.查看部署
kubectl get deployment -n memcached-operator-system