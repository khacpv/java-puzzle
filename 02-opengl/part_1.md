# All about OpenGL ES 2.x - Part 1/3

reference: http://blog.db-in.com/all-about-opengl-es-2-x-part-1

### Tác giả: [Diney Bomfim](http://blog.db-in.com/all-about-opengl-es-2-x-part-1)

### Người dịch: Phạm Văn Khắc

![OpenGL part 1][opengl_part1]

Xin chào!

Rất vui vì bạn đang đọc loạt bài hướng dẫn này. Đây là loạt bài về thế giới 3D. Cụ thể là OpenGL. Tôi đã tập trung toàn bộ thời gian trong 5 tháng vừa rồi để tìm hiểu về công nghệ 3D, Tôi đang hoàn thành Engine 3D của tôi (dường như là một dự án lớn nhất mà tôi đã từng làm) và bây giờ là thời gian tôi chia sẻ với bạn về những gì mà tôi đã thu lượm được - tất cả tài liệu, tất cả sách, hướng dẫn, và tất nhiên bao gồm cả những ý kiến phản hồi của các bạn.

Loạt bài này gồm 3 phần:

* Phần 1 - Khái niệm cơ bản về 3D và OpenGL
* Phần 2 - Đi sâu vào OpengGL ES 2.0
* Phần 3 - Kỹ năng Jedi trong OpenGL ES 2.0 và đồ họa 2D

Nếu bạn thích xem code luôn thì hãy ngó qua phần 2/3 trước vì trong phần đầu tôi chỉ trình bày về các khái niệm thôi.

Nào, bắt đầu thôi.

## Tổng quan

Có ai chưa từng nghe về OpenGL không? OpenGL là viết tắt cho "Open Graphics Library" và nó được dùng rất nhiều trong ngôn ngữ máy tính. OpenGL là nơi gần nhất giữa CPU (cái mà chúng ta - những developer - chạy các ứng dụng trên các ngôn ngữ lập trình) và GPU (chip xử lý đồ họa). Vì vậy OpenGL cần phải được hỗ trợ bởi các nhà sản xuất card đồ họa (vd: NVidia) và được cài đặt bởi những đối tác phát triển hệ điều hành (giống như Apple hay Microsoft) và cuối cùng OpenGL cho chúng ta một API thống nhất để làm việc. API này là các "Ngôn ngữ miễn phí" (hoặc hầu như miễn phí). Việc này quá thuận lợi vì nếu bạn sử dụng C hoặc C++ hay thậm chí là Objective-C, Perl, C# hay Javascript hay bất kỳ ngôn ngữ nào bạn muốn, API của OpenGL luôn giống nhau, xử lý giống nhau, chung chức năng, dòng lệnh và là nơi mà loạt bài hướng dẫn này bắt đầu!

Trước khi nói về OpenGL API chúng ta cần phải có kiến thức về 3D. Lịch sử của 3D trong ngôn ngữ máy tính được gói trong lịch sử của OpenGL. Vì vậy hãy cùng tìm hiểu qua một chút về lịch sử của nó.

## Một câu chuyện nhỏ

![OpenGL Logo][opengl_es_logo]

Khoảng 20 năm trước có một người tên là Silicon Graphics (SGI) tạo ra một loại thiết bị nhỏ. Thiết bị này có thể hiện ra các ảo ảnh rất thực tế. Với ảnh 2D, thiết bị đó dám hiện được các ảnh kiểu 3D, mô phỏng cái nhìn và chiều sâu theo mắt của con người. Thiết bị đó được gọi là IrisGL (có lẽ vì nó cố thử mô phỏng theo mắt của con người).

Ghê chưa, thiết bị đó là thư viện đồ họa đầu tiên. Nhưng nó chết quá nhanh vì để làm được như thế, anh ta cần điều khiển quá nhiều thứ trong máy tính như card đồ họa, hệ thống windows, các ngôn ngữ lập trình và giao diện tương tác với người dùng cuối. Quá nhiều thứ thậm chí để cho 1 công ty quản lý, chưa nói đến 1 người. Vì vậy SGI bắt đầu chuyển sang một thứ giống như "thẻ tạo đồ họa", "hệ thống quản lý cửa sổ", "tạo giao diện người dùng" cho các công ty khác và tập trung vào phần quan trọng nhất của thư viện đồ họa. Năm 1992 đã ra đời OpenGL thế hệ đầu tiên.

Vào năm 1995 Microsoft giới thiệu Direct3D, đối thủ cạnh tranh đáng ghờm với OpenGL.
Và chỉ tới năm 1997 OpenGL 1.1 đã được tung ra. Nhưng tới tận năm 2004, OpenGL mới có thể thu hút được tôi, OpenGL 2.0 với những tính năng tuyệt vời. Tôi thực sự thích Shaders, lập trình pipeline (programmable pipeline).

Cuối cùng vào 2007 OpenGL ES 2.0 đã mang sức mạnh của Shader và Programmable pipeline tới các hệ thống nhúng.

Ngày nay, bạn có thể thấy logo của OpenGL (hoặc OpenGL ES) trong rất nhiều trò chơi, ứng dụng 3D, 2D và rất nhiều phần mềm khác (đặc biệt là phần mềm 3D). OpenGL ES được dùng bởi PlayStation, Android, Nintendo 3DS, Nokia, Samsung, Symbian và tất nhiên cả Apple với MacOS và iOS.

## Đối thủ lớn nhất của OpenGL

Vâng, chúng ta đang nhắc tới hệ điều hành Windows.

Bạn có nhớ tôi đã nói rằng bản đầu tiên của OpenGL là 1992? cùng thời điểm đó, Microsoft đã có Windows 3. Như Microsoft luôn tin rằng "không gì được tạo ra, mọi thứ được copy", Microsot đã có gắng copy OpenGL trong những gì họ gọi là DirectX và được giới thiệu lần đầu năm 1995 trong Windows 95 tương ứng.

Một năm sau, 1996, Microsoft giới thiệu Direct3D, một phiên bản copy của OpenGL. Điểm đáng nói là Microsoft đã chiếm ưu thế trên thị trường trong nhiều năm liền, và DirectX hoặc (Direct3D) đã ăn sâu vào các máy tính PCs và khi Microsoft bắt đầu chuyển sang thị trường di động và Video Game, DirectX cũng lấn sân sang.

Ngày nay DirectX có cấu trúc rất tương tự với OpenGL: dùng ngôn ngữ Shader, cũng có programmable pipeline, thậm chí cũng hỗ trợ cả fix pipeline, hơn nữa là tên của các hàm trong API cũng tương tự luôn. Sự khác nhau chỉ là OpenGL là miễn phí, luôn luôn mở, nhưng DirectX là đóng. OpenGL cho iOS, MacOS, hệ thống Linux trong khi DirectX chỉ dành cho hệ điều hành của Microsoft.

Giờ là lúc chúng ta có thể bắt đầu khám phá thế giới lập trình 3D được rồi!

## 3D - Con mắt nhìn thứ nhất

Từ lúc tôi lớn lên, tôi đã đam mê công nghệ 3D và các trò chơi game 3D. Tất cả các nhân vật trong đó, con người, biết về mô phỏng thế giới thực trong các hiệu ứng 3D đến từ một nơi duy nhất: con mắt.

Con mắt là cơ bản trong thế giới 3D. Tất cả những điều chúng ta làm là mô phỏng nó, làm sao thật đẹp, thật lôi cuốn giống như những gì mắt con người có thể thấy. Những khái niệm liên quan tới mắt như góc nhìn, tầm nhìn, ống kính, thấu kính, các loại sự vật là sẽ được nhắc tới nhiều khi các bạn làm quen với 3D.

Mọi thứ chúng ta làm là cố gắng tái tạo từ mắt người những thứ: góc nhìn, điểm mù, biến dạng, độ sâu ảnh, tiêu cự, góc nhìn...

## Điểm tiếp theo - Không gian 3 chiều

Nghe có vẻ ngớ ngẩn nhưng thực sự phải nói rằng: thế giới 3D là 3D vì nó có 3 chiều. Tôi nói cái này vì nó rất quan trọng để nói thêm rằng nó nhiều hơn 2D một chiều và phức tạp hơn rất rất nhiều so với 2D.

Hãy xem, trong thế giới 2D khi chúng ta cần quay một hình vuông rất đơn giản: 45 độ luôn là 45 độ. Nhưng với 3D, để quay một hình vuông yêu cầu 3 trục X, Y, Z. Phụ thuộc trên thứ tự của góc quay, kết quả cuối cùng có thể rất khác nhau. Những thứ bình thường sẽ trở lên linh tinh khi chúng ta thử quay với các trục khác nhau. Ví dụ: quay x=25, y=20 là một kiểu, x=10, y=20 và sau đó x=10 lại tạo ra một kết quả mới.

Như vậy có thể nói khi thêm một chiều khác làm cho công việc của chúng ta phức tạp lên gấp nhiều lần.

## Điểm thứ 3 - Không phải 3D mà phải là 4D

Hmmm, lại thêm 1 chiều nữa ư?

Chính xác, điểm cuối cùng tôi muốn nhắc đến chính là điều này. 


[cube_example]: image/cube_example.gif "cube example"
[opengl_es_logo]: image/opengl-es-logo.jpg "opengl-es-logo"
[opengl_part1]: image/opengl_part1.png "opengl-part1"
[opengl_port_crane_example]: image/opengl_port_crane_example.jpg "opengl-port-crane-example"
[shaders_example]: image/shader_example.gif "shader-example"