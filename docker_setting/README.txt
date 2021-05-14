0. 본 압축파일 구성

DCPRG_assign_2 : 본 과제에 사용된 서버 소스코드 (스프링부트 프로젝트)

dockerfile : 본 과제에 사용된 서버 이미지 제작 docker file

Docker-compose.yml : 서버와 db 컨테이너를 연결하기 위해 사용된 yml 파일

2016104122.txt : 서버/DB 이미지가 업로드된 docker hub URL / 서버 소스코드 GitHub URL

README.txt : 본 과제물을 실행시키기 위한 정보를 담은 설명파일


1. 개요

스프링 부트를 활용하여 간단한 게시판 서비스를 제작했으며, 이를 알파인 리눅스에 올린 서버 이미지를 제작했습니다.
dockerfile을 사용한 부분은, alpine 리눅스 위에 java11을 설치한 이미지를 제작하는 과정입니다.
이렇게 이미지를 제작한 뒤에, 직접 컨테이너에 접속하여 github에 push 해놓은 스프링 부트 프로젝트를 clone 했습니다.
또한, 직접 프로젝트의 빌드를 완료해놓은 상태에서 docker commit 과 docker tag / docker push 를 활용해 이미지를 업데이트하고 docker hub에 업로드했습니다. 최종적으로 docker-compose.yml 에 사용된 이미지는 이렇게 업데이트를 마친 최종버전의 이미지입니다.

데이터베이스 이미지는 docker hub의 mysql latest version을 그대로 받은 뒤에, 직접 mysql에 접속하여 계정과 권한설정을 해주고 테스트에 사용할 DB를 만들었습니다. 그 후에 docker commit / tag/ push를 활용해 현상태의 컨테이너를 이미지화 하여 docker hub에 업로드했습니다. 최종적으로 docker-compose.yml에 사용된 DB 이미지는 이 최종 이미지입니다. 이와같은 이유로 데이터베이스 이미지를 만들기 위해 작성된 dockerfile은 없습니다.


2. 실행방법

- docker compose 파일을 이용해 컨테이너 환경을 구성합니다. (Docker-compose up -d)

- docker exec -it database bash 명령어를 통해 database 컨테이너에 접속합니다.

- mysql -u root -p 명령어를 통해 mysql 서비스에 접속합니다. (패스워드 : 123)

- use boooardgame 명령어를 통해 db를 전환합니다.

- create table posts (id bigint not null auto_increment, created_data datetime(6), modified_data datetime(6), author varchar(255), content TEXT not null, title varchar(500) not null, primary key (id)) engine=InnoDB;  명령어를 실행하여 테이블을 생성합니다.

- local 콘솔로 돌아와서 docker exec -it server /bin/sh 명령어를 통해 server 컨테이너에 접속합니다.

- cd /DCPRG_assign_2/build/libs 명령어를 통해 경로를 전환합니다.

- java -jar BoooardGame-0.0.1-SNAPSHOT.jar 명령어를 통해 프로젝트를 빌드합니다.

- 로컬의 브라우저로 localhost:10000 에 접속하여 게시글을 작성하고, 불러와지는것을 확인합니다.



3. 유의사항

원래 본 프로젝트를 구성하기 위해 모든 환경설정을 한번에 해결하고, docker-compose 시킨뒤에 스프링부트 프로젝트만 빌드하면 되도록 하려 했습니다.

하지만, 데이터베이스 이미지를 제작하는 과정에서 생성된 DB는 docker image로 백업이 되나, table은 image로 백업이 안되는 현상이 발생했습니다.

이에 의해 본 프로젝트를 실행하기 위해 직접 데이터베이스 컨테이너에 접속하여 posts 테이블을 생성해주어야 하는 번거로움이 있습니다.

이는 차후 개선하여, 테이블까지 백업할 수 있도록 할 예정입니다.

감사합니다.
