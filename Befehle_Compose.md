export DOCKER_COMPOSE_VERSION=`git ls-remote --tags git://github.com/docker/compose.git | awk '{print $2}' |grep -v "docs\|rc" |awk -F'/' '{print $3}' |sort -V |tail -n1`

cd

curl -L https://github.com/docker/compose/releases/download/$DOCKER_COMPOSE_VERSION/docker-compose-`uname -s`-`uname -m` > docker-compose

sudo mv docker-compose /opt/bin/

chmod +x /opt/bin/docker-compose
    ls
    ls
ls

