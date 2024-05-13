# Writeup KCSC-CTF-2024

## Forensic

### Chall3 - Jumper In Disguise



![image-20240513200642909](./image/image-20240513200642909.png)



#### Phân tích sơ qua... 

- File sau khi giải nén là file .docm -> đây thường là dạng bài file doc có gắn marco vì thế tư duy đầu tiên mn hay là sẽ là dùng **olevba** để dump đoạn marco ra.

![image-20240513200717458](./image/image-20240513200717458.png)

![image-20240513200918800](./image/image-20240513200918800.png)



Mình cũng làm như vậy ...

Sau khi đọc qua thì thấy là khi người dùng ấn allow cho chạy script marco thì sẽ hiện messagebox kèm flag giả :v 



![image-20240513201046308](./image/image-20240513201046308.png)



Đoạn dưới thì thực hiện tách binary ở đâu đó ra -> giải mã XOR với key là bbb 

:v một điều mình chưa giải thích đc là khi **olevba**  thì *bbb = 4444* 

Còn khi mình làm theo hướng mở file lên thì bbb=1337 

Có lẽ đây là một trick gì đó của tác giả :v Thật là hay mà



![image-20240513201346459](./image/image-20240513201346459.png)





- Một điều nữa khi tải về mình kiểm tra thấy kích thước file nặng khá bất thường -> có lẽ đã được nhúng thêm gì đó đính kèm rồi :v

![image-20240513201520126](./image/image-20240513201520126.png)

- Nhanh tay chuyển sang dạng zip rồi giải nén ra 

![image-20240513201553053](./image/image-20240513201553053.png)

- kích thước cái ảnh đúng 4.16MB :)) quá bất thường cho một con ảnh -> HXD xem như nào .... 



![image-20240513201626232](./image/image-20240513201626232.png)



Nhưng mà trong lúc thi nên mình sẽ chọn cách nhanh hơn :))  *(Chút nữa quay lại phân tích script VBA sau)*

Trong lúc thi thường lú lắm nên mình sẽ chọn cách chạy docm trên máy ảo để lấy file exe luôn.

- Kết hợp đọc qua code mình biết được vị trí file exe sẽ được lưu ra 

![image-20240513201924713](./image/image-20240513201924713.png)

- dễ dàng tìm được chỗ lấy EXE về nghịch tiếp mà không tốn sức đọc vba decode .... 

![image-20240513201939677](./image/image-20240513201939677.png)



#### Phân tích file thực thi Acheron

![image-20240513202128881](./image/image-20240513202128881.png)

Nhìn qua là biết e nó viết bằng python rồi, quá đơn giản ròi 

Dùng **pyinstxtractor** trích pyc ra thôi

https://github.com/extremecoders-re/pyinstxtractor/blob/master/pyinstxtractor.py

![image-20240513202309119](./image/image-20240513202309119.png)

- Code được viết bằng python3.7, file chính là file lmao.pyc . Chỉ lưu ý là chỗ version này extract ra không để ý là hay lỗi và cách bước sau không lấy được code...
- Đến đây có thể dùng nhiều cách. Lười thì up lên **decompiler**

https://www.decompiler.com/jar/4b750a9863b5479d9c0c1c7e40736b9c/lmao.py

![image-20240513202403883](./image/image-20240513202403883.png)

- Mà chăm hơn thì **Uncompyle6**

![image-20240513202458955](./image/image-20240513202458955.png)



- Chăm nữa thì **pycdc**

![image-20240513202634039](./image/image-20240513202634039.png)

- Đường cùng thi **pycdas**

![image-20240513202719883](./image/image-20240513202719883.png)

nói chung là python là không sợ :))



- Đọc code python thoi. Cũng không phức tạp lắm.

![image-20240513203108014](./image/image-20240513203108014.png)



Giờ cần lấy được key của RC4 ở sys.argv[1] 

Phải dở code VBA ra đọc lại rồi



![image-20240513203255051](./image/image-20240513203255051.png)

- Gọi tên file + nifal

![image-20240513203246858](./image/image-20240513203246858.png)

- nifal thì sau cái hàm zzz gì gì kia 

:)) làm theo cách người lười thoi

![image-20240513203430521](./image/image-20240513203430521.png)



- Sức mạnh của AI chưa :3

![image-20240513203536391](./image/image-20240513203536391.png)

sửa cái xor_key thành 1337 là có key rồi 

=> **"Kyoutei saitaku, shoudou sakusen jikkou!!!"**

- *Còn một cách dành cho người lười nữa là sửa trên vba xong chạy trên sample luôn*

![image-20240513203827707](./image/image-20240513203827707.png)

![image-20240513203849276](./image/image-20240513203849276.png)

 



- Còn 4 cái bytes của file EXE thì dễ, cứ mở HXD lên copy thôi

![image-20240513203734093](./image/image-20240513203734093.png)



- rồi giờ sửa lại cái code gốc một tí cho nó in ra 

![image-20240513203932276](./image/image-20240513203932276.png)



- Thấy luôn flag ở đây rồi 

![image-20240513204021173](./image/image-20240513204021173.png)



>  KCSC{I_@m_daStomp_dat_1z_4Ppr0/\ch1n9!}



:)) Bài học rút ra là gì ? => Hãy lười đừng chăm chỉ quá 





#### Quay lại phân tích VBA (rảnh xem lại sau)

............... đang lười lắm ..............

### Chall1- Externet Inplorer



![image-20240513204301845](./image/image-20240513204301845.png)



Bài này khá dễ . Dùng tool parse thời gian ra luôn https://dfir.blog/unfurl/



![image-20240513204345724](./image/image-20240513204345724.png)



**2023-09-18 08:32:22.547027**





## Reverse

## f@k3

![image-20240513204603728](./image/image-20240513204603728.png)



