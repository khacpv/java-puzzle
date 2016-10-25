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

Chính xác, điểm cuối cùng tôi muốn nhắc đến chính là điều này. Chúng ta không chỉ làm việc trong thế giới 3D, chúng ta còn có chiều thứ 4: thời gian. Những sự vật trong thế giới 3D cần tương tác lẫn nhau, cần phải di chuyển, va chạm, thay đổi. Và như tôi đã nói ở trên, việc thay đổi đó trong thế giới 3D tạo ra nhiều kết quả khác nhau.

Vậy đến lúc này chúng ta đã định nghĩa được 3D là: "việc mô phỏng mắt của con người và mô phỏng sự chuyển động của các sự vật".

## Áp dụng OpenGL vào thế giới 3D

Bây giờ mới là lúc nói về cái thú vị của bài blog này: Hãy nói về OpenGL. Đầu tiên chúng ta cần cảm ơn các nhà toán học như Ơ-le, Hamilton, Pythago và rất nhiều nhà khoa học khác. Nhờ có họ mà ngày nay chúng ta có quá nhiều công thức và công cụ để làm việc với không gian 3 chiều. OpenGL đã sử dụng tất cả kiến thức đó để tạo ra một thế giới 3D ngay trước mặt chúng ta. Có hàng nghìn, thậm chí hàng triệu phép tính toán trên một giây sử dụng các công thức toán học để mô phỏng lại vẻ đẹp mà mắt người trông thấy.

OpenGL là một MÁY TRẠNG THÁI (state machine) - điều này có nghĩa là toàn bộ OpenGL làm việc trên mô hình thiết kế theo trạng thái. Để minh họa cho OpenGL, hãy tưởng tượng một máy Cần Trục cảng Hải Phòng chẳng hạn. Ở đó có rất nhiều các thùng hàng (container) với rất nhiều các kiện hàng nhỏ bên trong. OpenGL giống như toàn bộ khu cảng đó bao gồm:

* Các thùng hàng (container) là các đối tượng trong OpenGL (Textures, Shaders, Meshes, và các thứ tương tự)
* Các kiện hàng bên trong mỗi container là những gì chúng ta tạo ra trong ứng dụng sử dụng OpenGL. Đó là những thứ chúng ta nhìn thấy.
* Máy cần trục là các OpenGL API, cái mà chúng ta sẽ sử dụng.

Vậy khi chúng ta thực hiện một hàm trong OpenGL giống như việc sử dụng cần trục. Chiếc cần trục lấy các container trong cảng, nâng nó lên, xử lý những gì bên trong và cuối cùng thả xuống vào một nơi đã định sẵn. Bạn không phải trực tiếp sử dụng cảng đó, không thể thay đổi nội dung các thùng hàng, không thể thay thế chúng, bạn cũng không thể trực tiếp đụng chạm vào các thùng hàng trong cảng. Tất cả chỉ là bạn ra lệnh cho chiếc cần trục thực hiện. Chỉ có chiếc cần trục đó mới có thể tác động lên các thùng hàng trong container. Hãy nhớ điều này! Nó là cực kỳ quan trọng về OpenGL cho đến bây giờ. Chiếc cần trục chỉ là một thứ có thể quản lý các thùng hàng trong cảng.

![Port][opengl_port_crane_example]

Có vẻ OpenGL bị giới hạn bởi các API? nhưng không, các cần trục của OpenGL là cực kỳ mạnh mẽ. Nó có thể lặp lại việc xử lý gắp-và-thả hàng nghìn cho tới hàng triệu các container trong một giây. Một thứ tuyệt với khác của OpenGL là sử dụng mô hình State Machine, đó là chúng ta không phải lưu giữ bất kỳ thể hiện nào, chúng ta không cần phải tạo các đối tượng trực tiếp, chúng ta chỉ cần giữ các id, hoặc trong ví dụ trên: chúng ta chỉ cần biết về id của các container.

## Vậy OpenGL làm việc như thế nào?

Đi sâu vào bộ xử lý tính toán của OpenGL, ta thấy OpenGL sử dụng tính toán trực tiếp trên GPU - phần cứng xử lý đồ họa cho máy tính - và sử dụng dấu phảy động (floating points). CPU (Central Processing Unit) là chip xử lý của máy tính. Còn GPU (Graphics Processing Unit) là chip xử lý đồ họa. Chip xử lý đồ họa này giúp cho CPU giảm thiểu các công việc nặng nhọc: nó có thể xử lý ảnh trước khi hiện ra màn hình. Hay sâu hơn, những gì OpenGL thực hiện chính là trên GPU, thay vì tính toán trên CPU. GPU nhanh hơn rất rất nhiều với các số thực so với CPU. Đây là lý do cơ bản để các trò chơi 3D chạy nhanh hơn khi có card đồ họa. Đây cũng chính là lý do cho các phần mềm xử lý 3D chuyên nghiệp giúp chúng ta làm việc với "Software's Render" (xử lý trên CPU) hoặc "Graphics Card's Render" (xử lý trên GPU). Một vài phần mềm cũng cho chúng ta một tùy chọn: Có dùng OpenGL hay không? Và đó, giờ bạn đã biết rồi. Tùy chọn đó chính là dành cho việc xử lý sẽ ở trên GPU hay không. Vì vậy, OpenGL sẽ hoàn toàn làm việc với GPU hay không? câu trả lời là không chắc. Chỉ những thao tác xử lý ảnh và vài thứ khác thôi. OpenGL cho chúng ta rất nhiều chức năng để lưu trữ ảnh, dữ liệu, thông tin trong một định dạng cụ thể. Những định dạng này được xử lý trực tiếp bởi GPU. Vì vậy OpenGL phụ thuộc vào phần cứng. Nếu phần cứng không hỗ trợ OpenGL, chúng ta không thể sử dụng nó. Quá buồn! Các phiên bản mới của OpenGL thường xuyên cần những tính năng mới của GPU. Nhưng cũng đừng lo lắng, OpenGL luôn cần những nhà sản xuất tích hợp vào, chúng ta (những developer - coder) sẽ làm việc trên các phiên bản OpenGL mới chỉ khi thiết bị đã sẵn sàng. Trong thực tế, tât cả các chip đồ họa hiện tại đều hỗ trợ OpenGL. Vì vậy bạn có thể sử dụng OpenGL với rất nhiều ngôn ngữ và thiết bị. Thậm chí với Microsoft Windows.

## Logic của OpenGL

OpenGL là thư viện đồ họa khá ngắn gọn và súc tích. Những gì bạn thấy trong các phần mềm xử lý 3D chuyên nghiệp là cực kỳ phức tạp hoạt động trên OpenGL. Vì Logic của OpenGL có những thứ sau:

* Primitives (Các đối tượng cơ bản)
* Buffers (Lưu vào bộ đệm)
* Rasterize (Xử lý đồ họa)

OpenGL hoạt động xoay quanh 3 khái niệm này. Mỗi khái niệm ở trên độc lập nhau và làm thế nào để cả 3 có thể cùng nhau tạo ra các hiệu ứng 3D đẹp mắt (hay kể cả với các ảnh 2D vì nó chỉ là dạng 3D với độ sâu Z=0, chúng ta sẽ nói vấn đề này sau).

## Primitives

Bao gồm 3 loại đói tượng:

* Các điểm trong hệ không gian 3 chiều (x, y và z)
* Các đường thẳng trong không gian (sự kết hợp của 2 điểm)
* về một tam giác trong không gian (sự kết hợp của 3 điểm)

Một điểm 3D có thể coi như một hạt trong không gian.
Một đường thẳng luôn có thể coi như một vector.
Một tam giác có thể coi là một mặt trong 1 cái lưới với hàng nghìn, triệu tam giác kết hợp lại.

Một vài phiên bản OpenGL hỗ trợ quads (hình tứ diện với 4 điểm), cũng là một loại của tam giác, nhưng để OpenGL ES đạt hiệu quả cao nhất thì cái này không được hỗ trợ.

## Buffers

Giờ hãy 






[cube_example]: image/cube_example.gif "cube example"
[opengl_es_logo]: image/opengl-es-logo.jpg "opengl-es-logo"
[opengl_part1]: image/opengl_part1.png "opengl-part1"
[opengl_port_crane_example]: image/opengl_port_crane_example.jpg "opengl-port-crane-example"
[shaders_example]: image/shader_example.gif "shader-example"





























