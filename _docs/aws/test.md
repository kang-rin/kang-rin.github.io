---
title: aws ec2에서 apache + modi_wsgi + django 웹서버 설치
category: aws
order: 1
---

============================================
 * 구성
 
 ```
 Amazon Linux2 (contos 기반)
 apache 2.4
 python 3.7
 django 2.2.3
 mod_wsgi 4.
 
 ```
주의)필요한 인프라를 다운받아 직업 소스 컴파일해서 설치 하는 밥법도 있으나 버그 및 설치 미스에 따른 취약점이 발생 직접 대응 해야하는 문제도 생기기때문에 패키지 설치권장

#  aws생성
<ul>
	<li>
    ec2 Amazon Linux 2 AMI 생성 필요한 패키지는 직접 설치해야함
	</li>
  <li>
    결제알림 ,elastic ip 설정
	</li>
  <li>
    yum 설치되어있음 update
  </li>
  <li>
   아파치 설치후 가상호스트설정
  </li> 
<li>
   로컬에서 작업한 venv는 적용되지 않음 서버에서 로컬에서 사용한 모듈을 
  </li>	
</ul>
    
   
## yum update
 * 모든 소프트웨어 패키지가 최신 상태로 업데이트되어 있는지 확인하기 위해, 인스턴스에서 퀵 소프트웨어 업데이트를 실행합니다. 이 업데이트 과정은 몇 분 정도 시간이 소요될 수 있지만, 최신 보안 업데이트와 버그 수정을 위해 수행할 필요가 있습니다.

-y 옵션을 사용하면 확인 여부를 묻지 않고 업데이트를 설치합니다. 설치 전에 업데이트 정보를 확인하려면 이 옵션을 생략합니다
 ```
 $ sudo yum update -y
 ```

* 의존성 설치
```
$ yum install -y wget
$ yum install -y net-tools
$ yum install -y gcc
$ yum install -y gcc-c++
$ yum install -y make
$ yum install -y apr
$ yum install -y apr-util
$ yum install -y expat-devel
```

## apche 설치
* 참고
<http://progtrend.blogspot.com/2018/06/amazon-linux-2-apache-web-server.html>

* 1.패키지로 설치 하기
<https://pypi.org/project/mod-wsgi/>

```
If you are running Debian or Ubuntu Linux with Apache 2.4 system packages, regardless of which Apache MPM is being used, you would need both:
If you are running RHEL, CentOS or Fedora, you would need both:

$ sudo yum install httpd
$ sudo yum install httpd-devel 
$ systemctl stop httpd.service 
$ systemctl restart httpd.service
$ systemctl start httpd.service
$ systemctl start vsftpd@vsftpd.service
$ systemctl status httpd

* 2.소스 컴파일

 아파치설치 파일 <https://httpd.apache.org/download.cgi#apache24><br>
 설치파일  <https://apr.apache.org/download.cgi><br>
 APR 참고  <https://victorydntmd.tistory.com/220><br>
 
 
* 2.1 PCRE 설치
$ wget https://sourceforge.net/projects/pcre/files/pcre/8.36/pcre-8.36.tar.gz/download
$ tar xvfz download 
$ cd pcre-8.36
$ ./configure --prefix=/usr/local
$ make
$ make install

* 2.2 apache 설치
$ cd ..
$ wget http://mirror.navercorp.com/apache//httpd/httpd-2.4.39.tar.gz
$ tar xvfz httpd-2.4.39.tar.gz

* 2.3  apr 설치
$ wget http://mirror.navercorp.com/apache//apr/apr-1.7.0.tar.gz
$ tar xvfz apr-1.7.0.tar.gz

* 2.4 apr-util 설치
$ http://mirror.navercorp.com/apache//apr/apr-util-1.6.1.tar.gz
$ tar xvfz apr-util-1.6.1.tar.gz

* 2.5 apr과 apr-util을 아파치의 srclib 디렉터리 안으로 이동
$ mv apr-1.7.0 httpd-2.4.39/srclib/apr
$ mv apr-util-1.6.1 httpd-2.4.39/srclib/apr-util

* 2.6 아파치 configure
$ cd httpd-2.4.39
$ ./configure \
$ --prefix=/usr/local/apache2.4 \
$ --with-included-apr \
$ -with-pcre=/usr/local/bin/pcre-config
$ make
$ make install

* 2.7에러시
$ yum install -y expat-devel
$ make clean
$ make

root@ip-172-31-46-69 httpd-2.4.39]# /usr/local/apache2.4/bin/apachectl start
[root@ip-172-31-46-69 httpd-2.4.39]# ps -aux | grep http
root      8342  0.0  0.4  74992  4316 ?        Ss   01:53   0:00 /usr/local/apache2.4/bin/httpd -k start
daemon    8343  0.0  0.3 822712  4016 ?        Sl   01:53   0:00 /usr/local/apache2.4/bin/httpd -k start
daemon    8344  0.0  0.3 822712  4016 ?        Sl   01:53   0:00 /usr/local/apache2.4/bin/httpd -k start
daemon    8345  0.0  0.3 822712  4016 ?        Sl   01:53   0:00 /usr/local/apache2.4/bin/httpd -k start
root      8428  0.0  0.2 123632  2468 pts/1    S+   01:53   0:00 grep --color=auto http
[root@ip-172-31-46-69 httpd-2.4.39]#
```




## 호스트설정  
```
1 상위에 ec2-ser:apache www 생성
2 /etc/httpd/conf.d/vhost.conf 생성
3 도메인 연결전까지 (http.conf의 DocumentRoot를 수정할필요없이 vhost에서 포트별 DocumentRoot)

<VirtualHost *:80>
DocumentRoot "/www/test"
       <Directory "/www/test">
                AllowOverride all
                Require all granted
        </Directory>
</VirtualHost>
```

## 파이썬 설치
```
$ sudo yum install python3
현재기준으로 3.3.7
/lib/python3.7/
```

## mod_wsgi
* 1.mod_wsgi 패키시 설치

```
mod_wsgi를 설치하여 아파치로 파이썬을 띄울수있게
$ pip3 install mod_wsgi
에러 날경우
$ yum install python3-devel
```
* 2.mod_wsgi 소스컴파일설치<br>
설치참고 <https://taetaetae.github.io/2018/06/29/simple-web-server-flask-apache/><br>
공식가이드 <https://modwsgi.readthedocs.io/en/develop/user-guides/quick-installation-guide.html><br>

```
* 2.1 mod_wsgi다운로드
$ wget https://github.com/GrahamDumpleton/mod_wsgi/archive/4.6.5.tar.gz
$ tar xvfz 4.6.5.tar.gz
$ cd mod_wsgi-4.6.5/
$ ./configure --with-apxs=/usr/bin/apxs \
  --with-python=/bin/python3.7
  
  헤더가 없다는 에러일경우
  $ yum install python3-devel
  
  find -name 'apxs' -print
  모듈추가
 LoadModule wsgi_module modules/mod_wsgi.so
```


* host설정
	- 참고
<http://www.fun25.co.kr/blog/python-django-apache-mod-wsgi-install>
<https://modwsgi.readthedocs.io/en/develop/user-guides/virtual-environments.html>

```
<VirtualHost *:80>
	WSGIScriptAlias / /www/myapp/config/wsgi.py
	WSGIDaemonProcess myapp.io python-path=/lib/python3.7/site-packages
	WSGIProcessGroup myapp.io
   <Directory "/www/myapp/config">
       <Files wsgi.py>
		    AllowOverride None
		    Require all granted
		</Files>
    </Directory>
</VirtualHost>
```


## 파이썬 디폴드 변경
<https://codechacha.com/ko/change-python-version/>
```
[root@ip-122-00-00-00 bin]# python -V
Python 2.7.16
[root@ip-122-00-00-00 bin]# update-alternatives --install /bin/python python /bin/python2
python2           python2-config    python2.7         python2.7-config  
[root@ip-122-00-00-00 bin]# update-alternatives --install /bin/python python /bin/python2.7 1
[root@ip-122-00-00-00 bin]# update-alternatives --install /bin/python python /bin/python3 1
python3     python3.7   python3.7m  
[root@ip-122-00-00-00 bin]# update-alternatives --install /bin/python python /bin/python3 1
python3     python3.7   python3.7m  
[root@ip-122-00-00-00 bin]# update-alternatives --install /bin/python python /bin/python3 1
python3     python3.7   python3.7m  
[root@ip-122-00-00-00 bin]# update-alternatives --install /bin/python python /bin/python3.7 2
[root@ip-122-00-00-00 bin]# update-alternatives --config python


2 개의 프로그램이 'python'를 제공합니다.

  선택    명령
-----------------------------------------------
   1           /bin/python2.7
*+ 2           /bin/python3.7

현재 선택[+]을 유지하려면 엔터키를 누르고, 아니면 선택 번호를 입력하십시오:2
[root@ip-122-00-00-00 bin]# python -V
Python 3.7.3

```


## 기타에러
  * aws 기존의 키페어와 IP로 새로운 인스턴스 접속시
  ```
  콘솔에서 아래의 커멘드 실행
  ssh-keygen -R 접속hostIP
  ```
  
  * 퍼미션 에러일경우
  ```
  chcon -R -t httpd_sys_content_t www/
  ```
  


## 종합 설치
```
* 1. 아파치설치
 sudo yum install httpd
 sudo yum install httpd-devel
* 2. 파이썬
 sudo yum install python3
* 3.mod_wsgi설치
  pip3 install mod_wsgi 
* 4.mod_wsgi 등록 (왜 그런지 모르겠지만 pip로 설치하면 mod_wsgi.po가 모듈하위디렉토리에 생기지않음)
  cp -ai mod_wsgi-py37.cpython-37m-x86_64-linux-gnu.so /etc/httpd/modules/
  vim 01-cgi.conf
  LoadModule wsgi_module modules/mod_wsgi-py37.cpython-37m-x86_64-linux-gnu.so
 * 장고설치
 pip3 install django==2.2.3
 
```
