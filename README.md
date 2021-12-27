# replicaset

replicaset 도커이미지 만들기 이미지 만드는데 두가지 중요하다
하나는 ip 주소 그리고 docker key 입니다.

> 키생성방법

```sh
openssl rand -base64 756 > <path-to-keyfile> && chmod 400 <path-to-keyfile>
```

키가 생성됩니다.
키는 생성되지만 실제 도커 이미지 실행할때 파일 잘못됬다는 에러 문구가 발생합니다. 해결 방법은
파일소유자를 systemd-coredump 으로 설정하면 해결됩니다.

```
chown systemd-coredump keyfile/mongo-cluster-ke
```

> mongodb-init

이미지에 ip 주소를 세팅해야합니다.
외부에서 접속이 가능한 주소를 넣어야 오류가 발생하지 않습니다.
테스트가 부족하여 더 완벅한 좋은 방법은 찾지 못했습니다.

```base
rs.initiate(
  { _id: "rs0", members: [
    {_id:1,host:"192.168.80.244:27017"},
    {_id:2,host:"192.168.80.244:27018"},
    {_id:3,host:"192.168.80.244:27019"}
  ]})
```
