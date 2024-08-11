# RHEL 8 
## Install OS
  - Vagrant & Virtual Box 를 활용 설치
  ```bash
  $ vagrant init generi/rhel8
  $ vagrant up
  ```

## Set up Remote SSH
  - SSH 접속을 Remote에서 패스워드로 접속 가능하게 설정
  ```bash
  $ sudo vi /etc/ssh/sshd_config
   PasswordAuthentication yes  // 주석해제
  $ sudo systemctl restart sshd
  ```

## Add DNF Repository
  - Redhat에서 제공하는 epel 저장소는 패키지가 없음
  - 'CodeReady Builder' 저장소는 Redhat 구독자만 사용 가능
  - 대안으로 Rockylinux 저장소를 등록하여 활용
  ```bash
  $ sudo vi /etc/yum.repos.d/rockylinux.repo
  ```
  ```
  [rocky-baseos]
  name=Rocky Linux 8 - BaseOS
  baseurl=https://download.rockylinux.org/pub/rocky/8/BaseOS/x86_64/os/
  enabled=1
  gpgcheck=1
  gpgkey=https://dl.rockylinux.org/pub/rocky/RPM-GPG-KEY-rockyofficial

  [rocky-appstream]
  name=Rocky Linux 8 - AppStream
  baseurl=https://download.rockylinux.org/pub/rocky/8/AppStream/x86_64/os/
  enabled=1
  gpgcheck=1
  gpgkey=https://dl.rockylinux.org/pub/rocky/RPM-GPG-KEY-rockyofficial
  ```

### Clear DNF Cache and Rebuild
   ```bash
   $ sudo dnf clean all
   $ sudo dnf makecache
   ```

### Install Test
   - git 설치해 보기

   ```bash
   $ sudo dnf install git ansible -y
   $ git --version
   $ ansible --version
   ```

### Set up DNF Repository
  - Local DNF Repository 만들기
  ```bash
  $ sudo mkdir /var/www/html/dnfrepo
  $ sudo dnf download --resolve --downloaddir /var/www/html/dnfrepo git ansible createrepo httpd
  $ sudo createrepo /var/www/html/dnfrepo/
  $ sudo systemctl enable httpd
  $ sudo systemctl start httpd
  ```
  - Open httpd port
  ```bash
  $ sudo firewall-cmd --list-all
  $ sudo firewall-cmd --add-service=http --permanent
  $ sudo firewall-cmd --reload
  ```

  - Set up client server
  ```bash
  $ sudo vi /etc/yum.repo.d/rocky.repo
  ```
  ```
  [rockyrepo]
  name=Custom dnf Repository
  baseurl=http://192.168.33.10/dnfrepo/
  enabled=0
  gpgcheck=0
  ```
  ```bash
  $ sudo yum-config-manager --disable \*
  $ dnf repolist
  $ sudo yum-config-manager --enable rockyrepo
  $ dnf repolist
  ```



