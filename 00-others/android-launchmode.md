references:
https://inthecheesefactory.com/blog/understand-android-activity-launchmode/en
https://www.mobomo.com/2011/06/android-understanding-activity-launchmode/

## Hiểu các lauchMode trong Android Activity: standard, singleTop, singleTask, singleInstance

Tác giả: **nuuneoi** (Android GDE, CTO & CEO at The Cheese Factory)

Người dịch: Phạm Văn Khắc

Thời gian: 25-08-2016

Trước khi đi sâu vào từng loại launch mode của activity, chúng ta cần hiểu một thuật ngữ quan trọng: 'Task'. Một task là ngăn xếp (stack - "Last in, First out"), nó chứa các activity bên trong. Android có thể giữ nhiều task tại cùng một thời điểm và chỉ một task là ở trên cùng (được người dùng có thể nhìn thấy được). Tương tự với việc khi nhấn 2 lần vào nút Home trên iOS, nếu chúng ta nhấn giữ nút HOME trên Android (hoặc nút RECENT APPS) bạn sẽ thấy danh sách các ứng dụng đang chạy. Bạn có thể chọn một ứng dụng bên dưới để mang nó lên trên cùng và tương tác. Tất nhiên, vì các ứng dụng này được lưu lại ở bên dưới nên bạn nên để càng ít càng tốt giúp thiết bị của bạn chạy tốt hơn.

Vì mỗi Activity dùng cho các mục đích khác nhau, và sẽ có trường hợp bạn cần tạo mới hoặc muốn sử dụng lại một activity cũ đã có trước đó. LaunchMode được thiết kế ra để dụng cho các trường hợp này.

### Khai báo sử dụng

Cơ bản, chúng ta có thể sử dụng trực tiếp trong `AndroidManifest.xml` :

````
<activity
    android:name=".SingleTaskActivity"
    android:label="singleTask launchMode"
    android:launchMode="singleTask">
````

và có 4 loại launchMode, sau đây chúng ta sẽ tìm hiểu từng loại một.

### standard

Đây là chế độ mặc định.

Thuộc tính này làm cho một activity mới luôn được tạo ra một cách độc lập với mỗi Intent được gửi đi. Tưởng tượng, nếu có 10 Intent gửi đi để soạn thảo email thì sẽ có 10 Activity được tạo ra với các Intent đó. Cuối cùng, số activity được tạo ra là không giới hạn trong thiết bị.

#### Với thiết bị dưới Lollipop

Activity sẽ được tạo ra và đặt trên đỉnh của stack trong cùng một task với cái đã gửi Intent đó.

![image][standardtopstandard]

[standardtopstandard]: images/standardtopstandard.jpg "standard topstandard"
[gallerystandardl1]: images/gallerystandardl1.jpg "gallery standard l1"