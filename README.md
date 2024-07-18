# Ansible 활용 개발환경 구축
## 환경설정
  - ssh key 생성

  ```
  $ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
  $ ssh-copy-id vagrant@192.168.33.12
  ```

  ```
  $ sudo vi /etc/ssh/ssh_config
  $ sudo systemctl restart sshd
  ```