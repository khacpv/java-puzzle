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

Nói một cách đơn giản, buffer là một vùng lưu trữ tạm thời giúp cho việc tối ưu. Có 3 loại Buffer:

* Frame Buffer
* Render Buffer
* Buffer Object

**Frame Buffer** là trừu tượng và khó hiểu nhất trong cả 3 loại. Khi bạn tạo một cảnh, bạn có thể gửi ảnh trực tiếp tới màn hình của thiết bị hoặc tới Frame Buffer. Về cơ bản Frame Buffer là một vùng để lưu dữ liệu ảnh tạm. Cụ thể hơn, bạn có thể tưởng tượng nó như một đầu ra từ bộ render của OpenGL và gồm rất nhiều ảnh, không phải chỉ có một. Những ảnh đó là gì? Đó là những ảnh về các đối tượng 3D, về độ gần/xa của các đối tượng trong không gian, các vùng giao nhau của các đối tượng và về các phần được hiện ra. Vì vậy Frame Buffer giống như một tập gồm nhiều ảnh. Tất cả đều được lưu trữ dưới dạng thông tin mảng các giá trị pixel.

**Render Buffer** là một vùng nhớ tạm của một ảnh duy nhất. Giờ bạn có thể rõ ràng hơn khi một Frame Buffer chính là bao gồm nhiều Render Buffer. Có vài loại Render Buffer: Màu, độ sâu, hình khối.

* Màu: lưu trữ những màu cuối cùng được sinh bởi OpenGL render và lưu dưới dạng ảnh màu (RGB).
* Độ sâu lưu trữ giá trị Z của đối tượng. Nếu bạn đã quen với các phần mềm 3D, bạn sẽ biết về độ sâu của ảnh là gì. Nó là tỉ lệ độ màu xám của ảnh phụ thuộc theo vị trí so với màn hình. Trong trường hợp đầy đủ độ trắng khi đối tượng ở gần nhất và màu đen hoàn toàn khi nó ở xa nhất.
* Hình khối ám chỉ về thành phần hiện ra của đối tượng. Giống như một lớp mask cho các vùng được nhìn thấy. Được lưu dưới dạn ảnh chỉ gồm 2 màu trắng & đen.

**Buffer Object** là một vùng nhớ mà OpenGL gọi là "server side" (hoặc không gian địa chỉ lưu trữ). Buffer Object cũng là một vùng nhớ tạm thời, nhưng có vẻ lâu dài hơn các loại trước. Một Buffer Object có thể lưu trữ trong suốt quá trình sử dụng ứng dụng. Buffer Object có thể giữ các thông tin về đối tượng 3D dưới dạng đã được tối ưu. Những thông tin này có thể có 2 loại: Structure và Indice.

Structure là mảng mô tả các đối tượng 3D, vd: một mảng các đỉnh, mảng các tọa độ texture hoặc một mảng của những gì bạn muốn. Indice thì rõ ràng hơn, mảng này được dùng để xác định có bao nhiêu tam giác trong lưới sẽ được tạo ra dựa trên mảng structure.

Bạn vẫn khó hiểu ư? Hãy xem ví dụ dưới đây:

Tưởng tượng một khối 3D. Khối này có 6 mặt được sinh ra bởi 8 đỉnh?

![Cube][cube_example]

Mỗi mặt là một hình vuông, nhưng bạn có nhớ rằng OpenGL chỉ biết về các tam giác đúng không? Vì vậy chúng ta cần chuyển những hình vuông đó thành các tam giác để làm việc với OpenGL. Như vậy chúng ta có 12 tam giác tất cả!
Ảnh ở trên được tạo ra bởi phần mềm Modo, hãy xem góc dưới bên phải. Chúng là những thông tin về vật thể của chúng ta. Bạn có thể thấy có 8 đỉnh và 12 mặt (GL: 12).

Các tam giác trong OpenGL là sự kết hợp của các đỉnh 3D. Vì vậy để tạo ra bề mặt của chiếc hộp, chúng ta cần OpenGL tạo theo cách như này: {đỉnh 1, đỉnh 2, đỉnh 3}, {đỉnh 1, đỉnh 3, đỉnh 4}.

Hay nói cách khác, chúng ta cần lặp lại 2 đỉnh ở một mặt của hình vuông. Nếu thay hình hộp bằng một hình có 12 mặt thì mỗi đỉnh cần lặp 4 lần, nếu thay bằng một hình 16 mặt, chúng ta cần 6 lần lặp lại. Và cứ thế, việc tăng số lặp của các đỉnh lên nhanh chóng.

Vì vậy OpenGL cho phép chúng ta một cách làm đơn giản hơn: sử dụng một mảng các Indice. Ở ví dụ trên, chúng ta có thể có một mảng 8 đỉnh: {đỉnh 1, đỉnh 2, đỉnh 3, đỉnh 4,...} và thay vì viết lại những thông tin cho mỗi mặt, chúng ta tạo một mảng các Indicate: {0,1,2,0,2,3,2,6,3,2,5,6,...}. Mỗi sự kết hợp 3 phần tử trong mảng {0,1,2-0,2,3-2,6,3} biểu diễn một mặt tam giác. Với tính chất này, chúng ta có thể viết thông tin các đỉnh 1 lần và tái sử dụng nhiều lần trong mảng các indicate.

Giờ quay trở lại với Buffer Object, loại đầu tiên là một mảng các structure, như {đỉnh 1, đỉnh 2, đỉnh 3, đỉnh 4,...} và loại thứ 2 là mảng các indice {0,1,2,0,2,3,2,6,3,2,5,6...}.

Lợi ích của Buffer Object là chúng được tối ưu để làm việc trực tiếp với GPU và bạn không cần giữ mảng đó trong ứng dụng của bạn sau khi tạo một Buffer Object.

## Rasterize

Rastersize là quá trình xử lý bởi OpenGL nhận tất cả thông tin về đối tượng 3D (bao gồm đỉnh, tọa độ, tính toán...) để tạo ra một ảnh 2D. Ảnh này sẽ bị một vài thay đổi và sau đó hiển thị trên màn hình của thiết bị.

Nhưng ở bước cuối cùng này, việc kết nối giữa thông tin các pixel và thiết bị màn hình thuộc về các nhà sản xuất thiết bị. Nhóm Khronos cung cấp API khác gọi là EGL, nhưng đây chỉ là api của các nhà sản xuất. Chúng ta, các developer sẽ không làm việc trực tiếp với Khronos EGL, mà với phiên bản đã được thay đổi của các nhà cung cấp.

Vì vậy khi bạn tạo một OpenGL render, bạn có thể chọn quá trình render trực tiếp lên màn hình, sử dụng EGL, hoặc render tới các Frame Buffer. Việc render tới các Frame Buffer, bạn vẫn ở trong OpenGL API, nhưng nội dụng sẽ chưa được hiện ra trên màn hình. Việc hiển thị lên màn hình nằm ngoài phạm vi của OpenGL API, nó là của EGL API. Vì vậy tại thời điểm render, bạn có thể chọn một trong 2 loại để xuất ra.

Nhưng đừng thử ngay, vì như tôi đã nói, mỗi nhà sản xuất tạo riêng cho họ các EGL API riêng. Ví dụ Apple không cho phép bạn render trực tiếp tới màn hình thiết bị, bạn luôn luôn cần render tới Frame Buffer và sau đó dùng EGL (bởi Appple) để hiện nội dung lên màn hình.

## OpenGL pipelines

Như tôi đã nói về "Programmable pipeline" và "fixed pipeline", Programmable pipeline là thư viện đồ họa cho phép chúng ta làm mọi thứ liên quan tới Camera, Ánh sáng, Chất liệu và hiệu ứng. Và chúng ta có thể làm việc này với Shader. Vì vậy mỗi khi bạn nghe về "programmable pipeline" hãy nghĩ về Shader!

Thử cùng tìm hiểu Shader là gì nhỉ.

Shader giống như một đoạn code, kiểu như một chương trình nhỏ, làm việc trực tiếp trong GPU để thực hiện các phép tính phức tạp. Nếu diễn đạt một cách phức tạp thì như thế này: Màu sắc cuối cùng của bề mặt vật thể, cái mà có một texture là T, bị thay đổi khi đâm vào một vật có texture là TB, sử dụng màu SC với phản chiếu mức SL, dưới ánh sáng L với cường độ LP theo góc LA từ khoảng cách Z với độ rọi F và tất cả đang nhìn bởi mắt của Camera C tại vị trí P với ống kính T.

Những điều này có nghĩa nó cực kỳ phức tạp để xử lý trên CPU và quá nhiều thứ cần làm đẩy cho thư viện xử lý đồ họa. Vì vậy programmable pipeline chỉ là cách chúng ta quản lý những thứ đó như thế nào.

Còn fixed pipeline ngược với programmable pipeline! Fixed pipeline là thư viện đồ họa xử lý về tất cả các loại và cho chúng ta api để cài đặt Camera, Material, ánh sáng, hiệu ứng.

Để tạo Shader chúng ta dùng ngôn ngữ tương tự như C, đó là OpenGL Shader Language (GLSL). OpenGL ES sử dụng với một chút giới hạn được gọi là OpenGL ES Shader Language (GLSL ES hoặc ESSL). Sự khác nhau là bạn có thêm một số hàm và có thể viết thêm các biến trong GLSL hơn với GLSL ES, nhưng cú pháp là giống nhau.

Điểm một chút về cơ chế làm việc của những Shader này:

Bạn tạo chúng trong các file riêng biệt hoặc viết trực tiếp trong code của bạn, miễn làm sao nội dung cuối cùng chứa Shader Language sẽ được gửi đến OpenGL và biên dịch Shader cho bạn (bạn thậm chí có thể dùng tiền-biên-dịch dưới dạng mã nhị phân, nhưng không thuộc trong nội dung bài viết này).

Shader làm việc theo cặp: Vertex Shader và Fragment Shader. Chủ đề này cần rất quan trọng, hãy xem chi tiết Vertex và Fragment như thế nào. Để hiểu những gì mỗi loại shader làm, hãy quay trở lại ví dụ hình hộp ở trên.

![Box][shaders_example]

## Vertex Shader

Vertex Shader cũng được biết đến như là VS hoặc VSH là một chương trình nhỏ sẽ được thực thi ở mỗi đỉnh của mesh (mỗi vật thể được tạo nên bằng một lưới các tọa độ, gọi là mesh). Hãy xem hình hộp ở trên, hộp này có 8 đỉnh (giờ trong ảnh chỉ có 5 đỉnh đang được hiện ra thôi), bạn sẽ hiểu ngay sau đây thôi. Như vậy VSH sẽ được xử lý 8 lần bởi CPU.

Những gì Vertex Shader sẽ làm là xác định vị trí cuối cùng của đỉnh. Bạn có nhớ rằng Programmable pipeline cho phép chúng ta điều khiển camera? chính là ở đây, vị trí và độ mở của ống kính camera có thể thay đổi vị trí cuối cùng của các đỉnh. Vertex Shader cũng có trách nhiệm chuẩn bị và xuất ra vài variable (giá trị, biến) cho Fragment Shader. Trong OpenGL chúng ta định nghĩa các biến ở Vertex Shader, nhưng không phải cho Fragment Shader trực tiếp. Vì vậy, Vertex Shader phải gửi các biến tới Fragment Shader.

Nhưng tại sao chúng ta không thể trực tiếp truy cập tới Fragment Shader?

Hãy xem FSH (Fragment Shader) và bạn sẽ hiểu.

## Fragment Shader

Giờ hãy xem lại hình lập phương một lần nữa.

![Box][shaders_example]

Bạn có thấy đỉnh số 5 bị ẩn hay không? Vì với vị trí cụ thể, góc quay cụ thể, chúng ta chỉ có thể nhìn thấy 3 mặt được sinh ra từ 7 đỉnh.

Đây là những gì Fragment Shader thực hiện! FSH sẽ được xử lý ở mỗi mặt **được hiện** của ảnh cuối cùng (được hiện trên màn hình). Ở đây bạn có thể hiểu một fragment như là 1 pixel. Nhưng nói chung không hẳn chính xác như pixel, vì giữa OpenGL render và việc hiện ra ảnh cuối cùng trên màn hình thiết bị sẽ bị giãn ra theo cả 2 chiều (ví dụ camera thu lại theo tỉ lệ 4:3 mà màn hình là tỉ lệ 16:9 vậy). Vì vậy một fragment có thể có ít hơn hoặc nhiều hơn số pixel thực tế, phụ thuộc trên thiết bị và cấu hình việc render. Trong hình lập phương ở trên, Fragment Shader sẽ được xử lý ở mỗi pixel của ba mặt đang hiện lên từ 7 đỉnh đó.

Bên trong Fragment Shader, chúng ta sẽ làm việc với mọi thứ liên quan tới bề mặt của vật thể, như chất liệu, hiệu ứng phản chiếu, đổ bóng, ánh sáng, tương phản, khúc xạ, tổ chức và bất kỳ các hiệu ứng khác mà bạn muốn. Sản phẩm cuối cùng là màu của các pixel dưới định dạng RGBA.

Giờ thứ cuối cùng mà bạn cần biết đó là về VSH và FSH làm việc với nhau như thế nào. Đó là bắt buộc phải có 1 Vertex Shader và chỉ 1 Fragment Shader, không hơn không kém, phải chính xác là 1-1. Để đảm bảo chúng ta không phạm lỗi, OpenGL có một thứ gọi là **Program**. Một program trong OpenGL chỉ được biên dịch theo cặp 1 VSH và 1 FSH mà thôi.

## Tổng kết

Đây là tất cả về khái niệm OpenGL. Hãy nhớ những điểm này:

1. OpenGL logic được tạo nên bởi 3 khái niệm cơ bản: Primitive, Buffer và Rasterize.

* Primitives gồm điểm, đường, tam giác
* Buffer có thể là Frame Buffer, Render Buffer hoặc Buffer Object.
* Rasterize là quá trình biến đổi OpenGL tính toán ra các pixel

2. OpenGL làm việc với cả Fixed hoặc Programmable pipeline.

* Fixed pipeline đã cũ, chậm, cồng kềnh. Có rất nhiều hàm làm việc với Camera, ánh sáng, vật liệu, hiệu ứng
* Programmable pipeline dễ dùng hơn, nhanh và dễ hiểu hơn, vì trong Programmable cho phép OpenGL lập trình được với Camera, ánh sáng, vật liệu và hiệu ứng.

3. Programmable pipeline là giống như Shader: Vertex Shader, mỗi đỉnh của mesh, và Fragment Shader - ở mỗi phần hiện ra của mesh. Mỗi cặp Vertex Shader và Fragment Shader được biên dịch trong một thứ gọi là Program.

Hãy xem cả 3 bài, OpenGL dường như rất đơn giản và dễ hiểu. Đúng! nó đơn giản để hiểu nhưng để học thì....haizzz
3 điểm ở trên liệt kê ra các nhánh của OpenGL và để học tất cả chúng bạn có lẽ cần vài tháng hoặc hơn.

Những gì tôi cố gắng đề cập ở 2 phần tiếp theo của loạt bài này chính là tất cả những gì tôi đã học được trong 6 tháng cực kỳ tập trung, đi sâu vào OpenGL. Trong phần tiếp theo tôi sẽ cho bạn xem các hàm cơ bản và cấu trúc của một ứng dụng 3D sử dụng OpenGL. Độc lập với ngôn ngữ lập trình mà bạn đang sử dụng hoặc với thiết bị bạn đang sử dụng.

Nhưng trước khi sang phần tiếp theo, tôi muốn nói với bạn thêm một thứ nữa:

## OpenGL Error API

OpenGL là một máy trạng thái tuyệt vời làm việc giống như một cần trục ở cảng Hải Phòng và bạn không cần phải trực tiếp thực hiện những gì xảy ra bên trong. Vậy nếu có lỗi xảy ra thì sao? không có gì xảy ra với ứng dụng vì OpenGL hoàn toàn là ở bên ngoài.

Nhưng làm thế nào để biết khi shader của bạn có lỗi? Làm thế nào để biết nếu quán trình render vào các buffer không hoạt động đúng như mong muốn?

Để bắt được các lỗi đó, OpenGL cho chúng ta Error API. Đây là API cực kỳ đơn giản, nó có vài hàm cung cấp sẵn theo từng cặp. Một cái kiểm tra đơn giản, có/không, chỉ để biết nếu có thứ gì đó chạy đúng hay không. Cặp khác là để lấy ra được thông báo lỗi cụ thể. Vì vậy cực kỳ đơn giản. Đầu tiên bạn kiểm tra, rất nhanh, nếu có một lỗi, bạn lấy thông báo ra.

Nói chung chúng ta đặt vài điểm kiểm tra ở những nơi có thể xảy ra lỗi, gioogns như việc biên dịch Shader hoặc cấu hình Buffer để tránh các lỗi cơ bản.

## Phần tiếp theo

Ở phần tiếp theo, Chúng ta hãy xem chi tiết bên trong đoạn code thực tế, bạn sẽ đươc code rất nhiều.

Cảm ơn bạn và hy vọng gặp lại bạn trong phần tiếp theo!


[cube_example]: image/cube_example.gif "cube example"
[opengl_es_logo]: image/opengl-es-logo.jpg "opengl-es-logo"
[opengl_part1]: image/opengl_part1.png "opengl-part1"
[opengl_port_crane_example]: image/opengl_port_crane_example.jpg "opengl-port-crane-example"
[shaders_example]: image/shaders_example.gif "shader-example"





























