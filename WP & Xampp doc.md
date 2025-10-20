

Sau khi tải XAMPP trên máy tính ta sẽ thường có một folder như sau

```python
 C:/xampp
```

Folder này sẽ chứa tất cả file cần để chạy XAMPP cùng với đó là đường dẫn

```python
C:/xampp/htdocs
```

Path *htdocs* sẽ chứa các folder code viết bằng php để có thể sử dụng code trên xampp
- Ví dụ ở Folder htdocs thì ta sẽ có folder sau

```python
C:/xampp/htdocs/wordpress
```

Và để truy cập ứng dụng wordpress ta chỉ cần đơn giản truy cập đường dẫn

```python
http://127.0.0.1/wordpress
```
# **Debug PHP bằng cách sử dụng PHP storm **


Trước hết mở PHP storm thì ta sẽ sử dụng chức năng folder và  mở folder `C:/xampp/htdocs/wordpress` .Sau khi mở file này thì ta sẽ thấy được code PHP của wordpress

Để debug php thì ta trước hết cần cài đặt xdebug với các bước như sau :
- B1 : Vào trong folder /xampp/php và đánh lệnh sau “.\php -i “ trên cmd sau khi đánh lệnh này thì ta sẽ có được thông tin về phiên bản php được cài đặt
- B2 : Vào link sau   https://xdebug.org/wizard để lấy được link download (làm theo hướng dẫn ở link https://viblo.asia/p/toi-da-debug-code-php-nhu-nao-RnB5pWA7lPG)
- B3 : Sau khi xdebug đã được tải xuống và lưu tại `C:/xampp/php/ext` ta chỉnh file php.ini để cài đặt


# **Lưu ý  **
Ta thường đi tìm lỗi của plugin và folder chứa plugin nằm ở 

```python
/wordpress/wp-content/plugins
```

# **Cách cài đặt Plugins  **
tải đúng phiên bản về → giải nén ném vào 

```python
C:\xampp\htdocs\wordpress\wp-content\plugins
```

đăng nhập tài khoản wp
check plugins đã active chưa 

DEGBUG THUI
