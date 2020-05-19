---
template: BlogPost
path: /database/mongo/MongoDB-Atlas
date: 2020-05-19T13:38:53.222Z
title: Đưa dữ liệu lên đỉnh với MongoDB Atlas
metaDescription: "Với những tín đồ của NoSQL thì MongoDB là một sự lựa chọn tuyệt vời, và trong thời đại mà ai ai cũng đòi đi đua đưa thì chẳng lý do gì mà chúng ta không thử get high đưa dữ liệu lên đỉnh \U0001F917\n"
thumbnail: /assets/dbatlas1.png
---
# Tổng quan về MongoDB Atlas (MongoDB Atlas Overview)

MongoDB Atlas cung cấp một cách dễ dàng để lưu trữ và quản lý dữ liệu của bạn trên đám mây.  Hướng dẫn này giúp bạn tạo 1 Atlas cluster, kết nối với nó, chèn dữ liệu và truy vấn dữ liệu.

MongoDB Atlas là cloud database của MongoDB được ra mắt vào năm 2016 chạy trên AWS, Microsoft Azure và Google Cloud Platform.

Dữ liệu trong mỗi Cluster ở Atlas được lưu trữ theo cơ chế Replication, với 3 nodes: 1 master (primary) và 2 slaves (secondary)

## Bước 1: Tạo một tài khoản Atlas (Create an Atlas Account )

Bạn có thể tạo tài khoản bằng tài khoản Google hoặc địa chỉ email và đăng nhập bằng link dưới đây

```
https://account.mongodb.com/account/login
```



## Bước 2: Tạo 1 cluster miễn phí (Deploy a Free Tier Cluster )

Cluster miễn phí cung cấp một môi trường phát triển quy mô nhỏ để lưu trữ dữ liệu của bạn, không bao giờ hết hạn và cung cấp quyền truy cập giói hạn một số chức năng. Muốn nhiều tính năng hơn thì bạn phải trả tiền 

Làm theo các bước link bên dưới và tạo 1 Cluster

```
https://docs.atlas.mongodb.com/tutorial/deploy-free-tier-cluster
```

## Bước 3: Tạo 1 danh sách hoặc 1 địa chỉ IP đề kết nối với Cluster (Whitelist Your Connection IP Address )

Vào link bên dưới sau khi đăng nhập chọn mục Network Access và tạo 1 IP address

```
https://cloud.mongodb.com
```

Có các tuỳ chọn bạn có thể vào link dưới để xem kĩ hơn

```
https://docs.atlas.mongodb.com/security-whitelist
```

Sau khi đã thêm địa chỉ IP là bạn có thể tạo 1 database trên Cluster rồi đó

## Part 4: Create a Database User for Your Cluster (Tạo account quản lý cho Cluster của bạn )

```
https://docs.atlas.mongodb.com/tutorial/create-mongodb-user-for-cluster
```

Về cơ bản thì chỉ cần làm theo y hệt trên docs là các bạn đã có thể kết nối được rồi nhưng mình sẽ tóm tắt ngắn gọn các bước như sau

1. Từ thanh công cụ bên trái ấn vào mục Clusters sau đó ấn vào Connect để kết nối tới cluster
2. Hệ thống sẽ bắt bạn tạo 1 user để kết nối tới cluster, bạn cứ chọn quyền admin nhé. Sau đó điền username, password
3. Sau khi setup xong trên hệ thống web Atlas thì bạn cần cài đặt trên máy tính cá nhân của mình

## Phần 5: Kết nối tới Cluster của bạn (Connect to Your Cluster )

Mình sử dụng macOs nên sẽ dùng Terminal để cài

```
brew install mongodb/brew/mongodb-community-shell
```

```
brew link mongodb-community-shell
```

1. Cài đặt mongo sell, mongodb theo hướng dẫn trên trang chủ. Sau khi cài xong test bằng lệnh

```
mongo --version
```

Hiện lên như này là bạn đã cài đặt thành công và sẵn sàng kết nối rồi đó

```
MongoDB shell version v4.0.7
git version: 7c13a75b928ace3f65c9553352689dc0a6d0ca83
allocator: system
modules: none
build environment:
    distarch: x86_64
    target_arch: x86_64
```

2. Tiếp theo là bước kết nối

```
mongo "mongodb+srv://cluster0-81nju.azure.mongodb.net/test"  --username <tên user bạn vừa tạo ở bước 4>
```

Sau đó điền password. Nhớ là bạn phải làm ở trên máy mà bạn đăng ký địa chỉ IP ở bước trên không thì sẽ không vào được đâu. Điền password xong hiện lên như dưới là bạn đã có thể thao tác như trên mongo ở máy bạn rồi đó

```
MongoDB Enterprise GettingStarted-shard-0:PRIMARY>
```

## Part 6: Insert and View Data in Your Cluster (Tạo thử data trên cluster của bạn )

1. Tạo 1 database demo 

```
use DEMO
```

Dòng lệnh trên sẽ tạo 1 database tên là DEMO

2. Tạo thử dữ liệu trên DEMO

```
db.people.insert({
  name: { first: "Quang", last: "Bùi" },
  birth: new Date('July 29, 1999')
})
```

Lệnh này tạo một collection mới trong cơ sở dữ liệu DEMO của bạn được gọi là peole và chèn một data vào collection đó. Khi thêm dữ liệu thành công sẽ hiện ra thông báo

```
WriteResult({ "nInserted" : 1 })
```

3. Bây giờ ta có thể thêm, sửa, xoá và tìm kiếm. Mình sẽ thử tìm kiếm dữ liệu mình vừa tạo bên trên như sau

```
db.people.find({ "name.last": "Bùi" })
```

Data sẽ hiện ra như sau

```
{
  "_id" : ObjectId("5c9bca3c5345268c98ac7abc"),
  "name" : {
    "first" : "Quang",
    "last" : "Bùi"
  },
  "birth" : ISODate("1999-07-29T04:00:00Z")
}
```

Note: ObjectId là biến được hệ thống tạo, bạn có thể tự config để cài đặt lại

## Summary

Bạn đã setup thành công một cluster trên MongoDB Cloud rồi đó. Hãy vọc vạch và đào sâu để lên đỉnh theo cách của mình nhé

```
https://docs.atlas.mongodb.com
```
