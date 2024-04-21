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
# Grey cat the flag 2024
1. Baby Web

    Đề bài cho ta source như sau

    ![img](https://github.com/dnamgithub33/Write_up_CTF_2024/blob/7df287f7c2fc34abeac2180dfbc2e03aad1261b1/image_grey/1.png)

    Trang web xác thực người dùng bằng ```session``` với key là ```baby-web```, trang sẽ trả về flag khi người dùng admin ở trong session.

    Để đơn giản nhất, ta sửa lại đoạn code được cho như sau:

    ![img](https://github.com/dnamgithub33/Write_up_CTF_2024/blob/7df287f7c2fc34abeac2180dfbc2e03aad1261b1/image_grey/2.png)

    Chạy và lấy được session có admin:

    ![img](https://github.com/dnamgithub33/Write_up_CTF_2024/blob/7df287f7c2fc34abeac2180dfbc2e03aad1261b1/image_grey/3.png)

    Thay thế session vào web của chall:

    ![img](https://github.com/dnamgithub33/Write_up_CTF_2024/blob/7df287f7c2fc34abeac2180dfbc2e03aad1261b1/image_grey/4.png)

    Xem HTML mà web trả về, ta thấy có đoạn mà web cho phép redirect sang ```/flag```

    ![img](https://github.com/dnamgithub33/Write_up_CTF_2024/blob/7df287f7c2fc34abeac2180dfbc2e03aad1261b1/image_grey/5.png)

    Chuyển hướng URL đến ```/flag``` và tìm được flag:

    ![img](https://github.com/dnamgithub33/Write_up_CTF_2024/blob/7df287f7c2fc34abeac2180dfbc2e03aad1261b1/image_grey/6.png)

    flag:```grey{0h_n0_mY_5up3r_53cr3t_4dm1n_fl4g}```
