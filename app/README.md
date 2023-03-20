### Docker
```
# syntax=docker/dockerfile:1

FROM node:18-alpine
WORKDIR /app
COPY . .
RUN yarn install --production
CMD ["node", "src/index.js"]
EXPOSE 3000
```

1. Build container image
```
docker build -t getting-started .
```
 - `docker build` sử dụng Dockerfile để build 1 container image mới.
 - Bạn có thể nhận thấy rằng Docker đã download rất nhiều `layers`. Điều này là do bạn đã hướng dẫn trình xây dựng(builder) rằng bạn muốn start từ `node:18-alpine` image. Tuy nhiên, vì bạn không có thứ gì trên máy của mình nên Docker cần tải xuống hình ảnh (image).
 - Sau khi Docker download image, các hướng dẫn từ Dockerfile được sao chép (copy) trong ứng dụng của bạn và sử dụng `yarn` để install dependencies của ứng dụng.
 - Lệnh CMD chỉ định mệnh lệnh mặc định sẽ chạy khi start container từ image này.
 - Flag (cờ) `-t` gắn thẻ image của bạn. Hiểu đơn giản là đặt 1 image có tên getting-started. Vì bạn đã đặt tên cho image `getting-started` nên bạn có thể tham chiếu image đó khi chạy container.
 - Ở cuối câu lệnh `docker build` có dấu chấm (.) nói với Docker rằng nó sẽ tìm kiếm Dockerfile trong thư mục hiện tại.

2. Start an app container
- Bây giờ bạn đã có 1 image, bạn có thể chạy ứng dụng trong container. Để làm như vậy hãy sử dụng lệnh `docker run`.
  1. Bắt đầu container của bạn bằng lệnh `docker run` và chỉ định tên image bạn vừa tạo.
  ```
  docker run -dp 3000:3000 getting-started
  ```
  - Bạn có thể flag `-d` để chạy container mới ở chế độ `detached`(dưới nền).
  - Bạn cũng sử dụng flag `-p` để tạo ánh xạ(mapping) giữa port 3000 của máy chủ với port 3000 của container. Nếu không có ánh xạ (mapping) port, bạn không thể truy cập ứng dụng.
  2. Sau vài giây, truy cập web browser tới  http://localhost:3000. Bạn sẽ nhìn thấy ứng dụng.
- Xem 'process status'.
    ```
    docker ps
    ```
  
3. Remove the old container
- Get id của container sử dụng lệnh
```
docker ps -a
```
- Dừng container bằng id lấy từ `docker ps -a`
```
docker stop <the-container-id>
```
- Khi container dừng lại, có thể xóa bằng cách sử dụng lệnh `docker rm`
```
docker rm <the-container-id>
```
*Lưu ý:
- Có thể stop và remove 1 container trong 1 lệnh duy nhất.
```
docker rm -f <the-container-id>
```
