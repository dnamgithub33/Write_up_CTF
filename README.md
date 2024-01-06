# IrisCTF 2024
1. What's My Password?

    Đề bài cho ta source code của trang web như sau:
    
    ![img](https://github.com/dnamgithub33/Write_up_CTF_2024/blob/8507b91bde0dacf66b8a740bc335d421ce0ac7e5/image_iris/1.png)

    Từ source code và database đề bài cung cấp ta biết được khi đăng nhập đúng tài khoản thì trang web sẽ trả về một JSON gồm tài khoản và mật khẩu đã đăng nhập. Khi đăng nhập, server sẽ truy vấn tài khoản với câu lệnh ```SELECT * FROM users WHERE username = \"%s\" AND password = \"%s\"```.

    Từ database ta biết được flag là password của tài khoản ```skat```.
    
    ![img](https://github.com/dnamgithub33/Write_up_CTF_2024/blob/015ada975dfdc753782e42777e5cf646e1298ea0/image_iris/2.png)

    Câu lệnh này đã có sự tham số hóa nhưng chưa triệt để khiến xuất hiện SQL injection. Tiến hành khai thác như sau:

    Vì ở trường ```Username``` đã bị filter các ký tự đặc biệt chỉ cho phép các ký tự thường và số nhưng ở ```Password``` thì không nên ta sẽ Injection vào đó:

    ![img](https://github.com/dnamgithub33/Write_up_CTF_2024/blob/015ada975dfdc753782e42777e5cf646e1298ea0/image_iris/3.png)

    Thử với payload ```" OR 1 = 1 -- -``` kết quả trả về:

    ![img](https://github.com/dnamgithub33/Write_up_CTF_2024/blob/015ada975dfdc753782e42777e5cf646e1298ea0/image_iris/4.png)

    Mặc dù mình đã đăng nhập với tài khoản ```skat``` nhưng kết quả trả về lại là tài khoản ```root``` đó là do câu lệnh truy vấn có mệnh để ```WHERE``` luôn đúng và lấy ra toàn bộ tài khoản nhưng server chỉ cho thấy kết quả đầu tiên. Vì database sử dụng MySQL nên sử dụng ```LIMIT 1,1``` để lấy ra kết quả cần thiết:

    ![img](https://github.com/dnamgithub33/Write_up_CTF_2024/blob/015ada975dfdc753782e42777e5cf646e1298ea0/image_iris/5.png)

    flag: ```irisctf{my_p422W0RD_1S_SQl1}```