[IrisCTF](https://github.com/dnamgithub33/Write_up_CTF_2024/tree/main?tab=readme-ov-file#irisctf-2024)

[Grey cat the flag](https://github.com/dnamgithub33/Write_up_CTF_2024/tree/main?tab=readme-ov-file#grey-cat-the-flag-2024)

[PTITCTF](#ptitctf-2024)

# IrisCTF 2024
1. What's My Password?

    Đề bài cho ta source code của trang web như sau:
    
    ![img](image_iris/1.png)

    Từ source code và database đề bài cung cấp ta biết được khi đăng nhập đúng tài khoản thì trang web sẽ trả về một JSON gồm tài khoản và mật khẩu đã đăng nhập. Khi đăng nhập, server sẽ truy vấn tài khoản với câu lệnh ```SELECT * FROM users WHERE username = \"%s\" AND password = \"%s\"```.

    Từ database ta biết được flag là password của tài khoản ```skat```.
    
    ![img](image_iris/2.png)

    Câu lệnh này đã có sự tham số hóa nhưng chưa triệt để khiến xuất hiện SQL injection. Tiến hành khai thác như sau:

    Vì ở trường ```Username``` đã bị filter các ký tự đặc biệt chỉ cho phép các ký tự thường và số nhưng ở ```Password``` thì không nên ta sẽ Injection vào đó:

    ![img](image_iris/3.png)

    Thử với payload ```" OR 1 = 1 -- -``` kết quả trả về:

    ![img](image_iris/4.png)

    Mặc dù mình đã đăng nhập với tài khoản ```skat``` nhưng kết quả trả về lại là tài khoản ```root``` đó là do câu lệnh truy vấn có mệnh để ```WHERE``` luôn đúng và lấy ra toàn bộ tài khoản nhưng server chỉ cho thấy kết quả đầu tiên. Vì database sử dụng MySQL nên sử dụng ```LIMIT 1,1``` để lấy ra kết quả cần thiết:

    ![img](image_iris/5.png)

    flag: ```irisctf{my_p422W0RD_1S_SQl1}```
# Grey cat the flag 2024
1. Baby Web

    Đề bài cho ta source như sau

    ![img](image_grey/1.png)

    Trang web xác thực người dùng bằng ```session``` với key là ```baby-web```, trang sẽ trả về flag khi người dùng admin ở trong session.

    Để đơn giản nhất, ta sửa lại đoạn code được cho như sau:

    ![img](image_grey/2.png)

    Chạy và lấy được session có admin:

    ![img](image_grey/3.png)

    Thay thế session vào web của chall:

    ![img](image_grey/4.png)

    Xem HTML mà web trả về, ta thấy có đoạn mà web cho phép redirect sang ```/flag```

    ![img](image_grey/5.png)

    Chuyển hướng URL đến ```/flag``` và tìm được flag:

    ![img](image_grey/6.png)

    flag:```grey{0h_n0_mY_5up3r_53cr3t_4dm1n_fl4g}```
2. Grey survey
    
    * Chưa dựng lại được chall

    Challenge cho phép ta gửi lên một giá trị vote bắt buộc giá trị phải lớn hơn -1 và nhỏ hơn 1. 

    ![img](image_grey/7.png)

    Để challenge trả về flag thì phải vote một giá trị sao cho phần nguyên của giá trị đó phải cộng với score lớn hơn 1 với score ban đầu được khởi tạo là ```-0.42069```. Tuy nhiên, giá trị này chỉ được phép nhỏ hơn 1 và lớn hơn -1 nên phần nguyên luôn bằng 0, không làm tăng score được. Lỗi nằm ở hàm lấy phần nguyên ```parseInt()``` của javascript, khi truyền giá trị ```0.0000001``` vào hàm này thì kết quả trả về bằng 1 thay vì bằng 0. Khi gửi giá trị vote ```0.0000001``` và nhận data trả về là score sau khi cộng thì đã có sự thay đổi, tiếp tục gửi một lần nữa và nhận được flag.

    flag: ```grey{50m371m35_4_l177l3_6035_4_l0n6_w4y}```

# PTITCTF 2024
1. Don't reverse

    Đề bài cho ta một trang web như sau và được biết flag nằm ở ```/flag.txt```:

    ![img](image_PTITCTF/1.png)

    Chức năng: cho phép truyền vào một địa chỉ IP và trả về kết quả là ```PTITCTF2024```.

    ![img](image_PTITCTF/1.png)

    Có vẻ như trang web đang xử lý dữ liệu liên quan đến địa chỉ IP (phỏng đoán là Ping). Vì chức năng chỉ trả về kết quả là  ```PTITCTF2024```dù đúng dù sai nên ta thử payload timebase ```8.8.8.8; sleep 5``` và thấy thời gian trả về cao hơn so với bình thường 5 giây:

    ![img](image_PTITCTF/3.png)

    ![img](image_PTITCTF/4.png)

    Flag nằm ở ```/flag.txt```, tiến hành kiểm tra flag có độ dài bao nhiêu bằng lệnh comand sau: 

    ```do_dai=$(cat ../flag.txt | wc -m);if [ $do_dai -lt 52 ]; then   sleep 5; fi```

    Thử dần và biết được flag dài 51 ký tự

    ![img](image_PTITCTF/5.png)

    Thử kiểm tra chữ cái đầu tiên có phải ```P``` không bằng payload:

    ```chuoi=$(cat ../flag.txt | cut -c1);if [ "$chuoi" = "P" ]; then   sleep 5; fi```

    ![img](image_PTITCTF/7.png)

    Tiến hành Intruder với payload 1 là từ vị trí từ 1 đến 51, payload 2 là các ký tự, số, và các ký hiệu:

    ![ịmg](image_PTITCTF/8.png)

    ![img](image_PTITCTF/9.png)

    ![img](image_PTITCTF/10.png)

    Ta nhận được kết quả:

    ![img](image_PTITCTF/11.png)

    Vì chall này được dựng lại mà không có flag giống như trong cuộc thi nên ta nhận được flag test:

    ```PTITCTF{fake_flag_for_test}```
2. Còn một bài nhưng mất source :(

# AlpacaHack

1. Simple Login

    Đề bài cho ta một form login

![img](image_alpacahack/1.png)


    
    
    



