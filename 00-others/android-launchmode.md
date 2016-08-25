references:
https://inthecheesefactory.com/blog/understand-android-activity-launchmode/en
https://www.mobomo.com/2011/06/android-understanding-activity-launchmode/

## Tìm hiểu các lauchMode trong Android Activity: standard, singleTop, singleTask, singleInstance

Tác giả: **nuuneoi** (Android GDE, CTO & CEO at The Cheese Factory)

Người dịch: Phạm Văn Khắc

Thời gian: 25-08-2016

Trước khi đi sâu vào từng loại launch mode của activity, chúng ta cần hiểu một thuật ngữ quan trọng: 'Task'. Một task là ngăn xếp (stack - "Last in, First out"), nó chứa các activity bên trong. Android có thể giữ nhiều task tại cùng một thời điểm và chỉ một task là ở trên cùng (người dùng có thể nhìn thấy được). Tương tự với việc khi nhấn 2 lần vào nút Home trên iOS, nếu chúng ta nhấn giữ nút HOME trên Android (hoặc nút RECENT APPS) bạn sẽ thấy danh sách các ứng dụng đang chạy. Bạn có thể chọn một ứng dụng bên dưới để mang nó lên trên cùng và tương tác. Tất nhiên, vì các ứng dụng này được lưu lại ở bên dưới nên bạn nên để càng ít càng tốt giúp thiết bị của bạn chạy tốt hơn.

Vì mỗi Activity dùng cho các mục đích khác nhau, và sẽ có trường hợp bạn cần tạo mới hoặc muốn sử dụng lại một activity cũ đã có trước đó. LaunchMode được thiết kế ra để dụng cho các trường hợp này.

# Khai báo sử dụng

Cơ bản, chúng ta có thể sử dụng trực tiếp trong `AndroidManifest.xml` :

````
<activity
    android:name=".SingleTaskActivity"
    android:label="singleTask launchMode"
    android:launchMode="singleTask">
````

và có 4 loại launchMode, sau đây chúng ta sẽ tìm hiểu từng loại một.

# standard

Đây là chế độ mặc định.

Thuộc tính này làm cho một activity mới luôn được tạo ra một cách độc lập với mỗi Intent được gửi đi. Tưởng tượng, nếu có 10 Intent gửi đi để soạn thảo email thì sẽ có 10 Activity được tạo ra với các Intent đó. Cuối cùng, số activity được tạo ra là không giới hạn trong thiết bị.

### Với thiết bị dưới Lollipop

Activity sẽ được tạo ra và đặt trên đỉnh của stack trong cùng một task với cái đã gửi Intent đó.

![image][standardtopstandard]

Ảnh dưới đây chỉ ra điều gì sẽ xảy ra khi chọn chức năng chia sẻ ảnh tới một activity. Activity mới sẽ được chèn vào trong cùng task mặc dù chúng thuộc hai ứng dụng khác nhau.

![image][standardgallery2]

Và đây là những gì bạn sẽ nhìn thấy trong Task Manager:

![image][gallerystandard]

Nếu chúng ta chuyển sang một ứng dụng khác, sau đó quay lại ứng dụng Gallery, thì ta vẫn nhìn thấy rằng standard lauchMode đặt trên đỉnh task của Gallery. Nếu ta cần làm bất kỳ thứ gì với ứng dụng Gallery, chúng ta phải hoàn thành trong activity sau đó.

### Đối với thiết bị Lollipop trở lên

Nếu những activity này là từ cùng một ứng dụng, nó sẽ làm việc giống như trước Lollipop, activity mới sẽ được đặt trên đỉnh của task trước đó.

![image][standardstandardl]

Nhưng với trường hợp một Intent được gửi từ một ứng dụng khác, một task mới sẽ được tạo và activity mới sẽ được đặt vào trong task này:

![image][standardgalleryl]

Và đây là những gì bạn sẽ thấy trong Task Manager:

![image][gallerystandardl1]

Điều này xảy ra bởi vì Task Management được chỉnh sửa để hợp lý hơn. Bạn có thể quay trở lại Gallery bởi vì chúng ở trong các task khác nhau. Bạn có thể bắn một Intent khác, một task mới sẽ được tạo cho Intent đó giống như cái trước.

![image][gallerystandardl2]

Một ví dụ của loại Activity này là Soạn thảo Email hoặc Post status của Facebook. Nếu bạn nghĩ một Activity có thể hoạt động độc lập từ các Intent, hãy sử dụng `standard`

# singleTop

Chế độ này hoạt động giống như `standard`, tức là nó có thể tạo nhiều activity bao nhiêu cũng được. Nó chỉ khác trong trường hợp nếu đã có một instance của activity đó đã tồn tại với cùng loại ở trên đỉnh của stack trong task của activity gọi nó. Nó sẽ không tạo activity mới, thay vào đó, một Intent sẽ gửi tới activity đã tồn tại thông qua hàm `onNewIntent()`.

![image][singletop]

Trong chế độ singleTop, bạn phải xử lý Intent được gửi đến trong cả `onCreate()` và `onNewIntent()` để nó có thể hoạt động trong mọi trường hợp.

Một ví dụ thường thấy là chức năng tìm kiếm (Search). Hãy tưởng tượng tạo một ô tìm kiếm phía ở trên SearchActivity để xem danh sách kết quả. Bình thường chúng ta luôn đặt ô tìm kiếm cùng với trang kết quả để giúp người dùng tiếp tục tìm kiếm với từ khóa khác mà không cần sử dụng nút Back.

Bây giờ hãy tưởng tượng nếu chúng ta luôn chạy một SearchActivity mới để thực hiện việc tìm kiếm, 10 Activity mới cho 10 lần tìm kiếm. Đây thực sự là một sự bất tiện không thể chấp nhận được khi bạn phải nhấn back 10 lần để xem hết 10 kết quả tìm kiếm này rồi mới quay trở lại activity ban đầu.

Thay vì đó, nếu SearchActivity ở trên đầu của task, chúng ta sẽ gửi Intent tới activity đó luôn và để cập nhật kết quả vào chính nó. Giờ sẽ chỉ có một SearchActivity được đặt trên đầu của stack và bạn chỉ cần đơn giản nhấn nút Back một lần để quay trở lại Activity ban đầu. Như vậy UX sẽ hợp lý hơn nhiều đúng không?

Dù sao thì singleTop chỉ hoạt động với activity gọi nó trên cùng task đó. Nếu bạn muốn một Intent được gửi tới một Activity đã ở trên một task khác, rất tiếc là nó không làm việc được. Trong trường hợp Intent được gửi từ một ứng dụng khác tới một singleTop activity, một activity mới sẽ chạy giống như trường hợp `standard` của lauchMode. (với trước Lollipop: sẽ đặt trên cùng của task gọi nó, Lollipop: một task mới sẽ được tạo ra).

# singleTask

Rất khác so với 2 loại trước. **Một Activity với singleTask sẽ chỉ có 1 thể hiện trong hệ thống (Singleton)**. Nếu có một Activity đã tồn tại trong hệ thống, toàn bộ task chứa activity đó sẽ được đẩy lên đầu tiên trong khi Intent sẽ được truyền qua hàm `onNewIntent()`. Nếu không, một activity mới sẽ được tạo ra và đặt vào task mà chứa activity gọi nó.

### Trường hợp trong cùng một ứng dụng

Nếu chưa có activity singleTask nào đã tồn tại trước đó, một cái mới sẽ được tạo và đơn giản đặt trên đỉnh của stack cùng với task đó.

![image][singleTask1]

**Nhưng nếu đã có một activity rồi, tất cả các activity đặt trên activity-singleTask đó sẽ được tự động xóa đi (theo đúng lifecycle của activity đó) để làm cho activity chúng ta muốn được hiện lên trên đầu của stack.** Điều đó có nghĩa một Intent sẽ được gửi tới singleTask activity thông qua hàm `onNewIntent()`.

![image][singleTaskD]

Điều này có vẻ không hợp lý với UX của người dùng nhưng nó được thiết kế đúng như vậy.

Bạn có thể đọc được từ [nguồn](http://developer.android.com/guide/components/tasks-and-back-stack.html) của Android Developer rằng:

`The system creates a new task and instantiates the activity at the root of the new task.`

Nhưng điều này (theo kinh nghiệm), thực tế dường như không làm việc đúng như vậy. Một singleTask activity vẫn đứng trên đỉnh của task như chúng ta có thể thấy từ command: `dumpsys activity`:

````
Task id #239
  TaskRecord{428efe30 #239 A=com.thecheesefactory.lab.launchmode U=0 sz=2}
  Intent { act=android.intent.action.MAIN cat=[android.intent.category.LAUNCHER] flg=0x10000000 cmp=com.thecheesefactory.lab.launchmode/.StandardActivity }
    Hist #1: ActivityRecord{429a88d0 u0 com.thecheesefactory.lab.launchmode/.SingleTaskActivity t239}
      Intent { cmp=com.thecheesefactory.lab.launchmode/.SingleTaskActivity }
      ProcessRecord{42243130 18965:com.thecheesefactory.lab.launchmode/u0a123}
    Hist #0: ActivityRecord{425fec98 u0 com.thecheesefactory.lab.launchmode/.StandardActivity t239}
      Intent { act=android.intent.action.MAIN cat=[android.intent.category.LAUNCHER] flg=0x10000000 cmp=com.thecheesefactory.lab.launchmode/.StandardActivity }
      ProcessRecord{42243130 18965:com.thecheesefactory.lab.launchmode/u0a123}
````

Nếu bạn muốn để một singleTask hoạt động giống như tài liệu mô tả: tạo một task mới và đặt activity ở dưới cùng. Bạn cần phải gán thuộc tính `taskAffinity` tới singleTask activity như sau:

````
<activity
    android:name=".SingleTaskActivity"
    android:label="singleTask launchMode"
    android:launchMode="singleTask"
    android:taskAffinity="">
````

Và đây là kết quả khi khởi tạo SingleTaskActivity:

![image][singleTaskTaskAffinity]

![image][screenshot17]

Công việc của bạn là phải xem xét khi nào cần dùng `taskAffinity` tùy theo activity hoạt động mà bạn cần mong muốn.

### Trường hợp với ứng dụng khác

Một khi Intent được gửi từ ứng dụng khác và chưa có instance của Activity nào thì một task mới sẽ được tạo ra với singleTask activity trong đó.

![image][singleTaskAnotherApp1]

![image][singletaskfromapp2]

Nếu có một task của ứng dụng mà chứa singelTask activity đã tồn tại, một activity mới sẽ được đặt trên đỉnh của nó.

![image][singleTaskAnotherApp2]

Trong trường hợp mà có một Activity đã tồn tại trong một task nào đó, toàn bộ task sẽ được chuyển lên trên cùng và mọi activity ở trên singleTask activity sẽ bị hủy(destroy) theo đúng vòng đời(lifecycle) của nó. Nếu nút Back được nhấn, người dùng phải đi qua các activity trong stack đó trước khi quay trở về task ban đầu.

![image][singleTaskAnotherApp3]

Một ví dụ của trường hợp này là các activity khởi đầu (MAIN) cho các ứng dụng. Những activity này không được thiết kế để có nhiều hơn 1 thể hiện (instance) vì vậy singleTask sẽ hoạt động đúng. Dù sao bạn cũng nên linh động khi sử dụng vì nó có thể bị tự động hủy người dùng có thể không biết như đã nói ở trên.

# singleInstance

Mode này khá giống với singleTask, chỉ khác ở chỗ chỉ duy nhất một thể hiện (instance) của activity có thể tồn tại trong hệ thống. **Sự khác nhau là Task giữ activity này chỉ có thể có duy nhất một activity, mà là singleInstance**. Nếu một activity khác được gọi từ loại activity này, một task mới sẽ tự động tạo ra để đặt activity mới vào. Ngoài ra, nếu singleInstance activity này được gọi, một task mới sẽ được tạo ra để đặt activity đó.

Dù sao, kết quả cuối cùng là khá linh tinh. Từ thông tin được cung cấp bởi `dumpsys`, cho thấy rằng có 2 task trong hệ thống nhưng chỉ 1 cái hiện trong Task Manager phụ thuộc trên cái nào là cuối cùng được di chuyển lên trên. Và hệ quả là, mặc dù có 1 task vẫn đang làm việc bên dưới (background) nhưng chúng ta không thể mang nó lên trên (foreground). Thực sự việc này là không hợp lý chút nào.

Đây là những gì xảy ra khi singleInstance Activity được gọi trong khi có vài activity đã tồn tại trong stack.

![image][singleInstance]

Nhưng đây là những gì chúng ta thấy từ Task Manager:

![image][singleInstances]

Bởi vì task này chỉ có duy nhất 1 activity, chúng ta không thể quay trở lại tới task#1 nữa. Cách duy nhất để làm là chạy lại ứng dụng từ laucher nhưng thấy rằng singleInstance task sẽ ẩn bên dưới (background).

Tuy nhiên có vài cách để không mắc vấn đề này. Chỉ cần chúng ta thêm `taskAffinity` tới singleInstance Activity để cho phép nhiều task trên Task Manager.

````
<activity
    android:name=".SingleInstanceActivity"
    android:label="singleInstance launchMode"
    android:launchMode="singleInstance"
    android:taskAffinity="">
````

Bây giờ đã hợp lý hơn:

![image][screenshot18]

Chế độ này ít khi được sử dụng. Một vài trường hợp thực tế là một activity Lancher hoặc ứng dụng mà bạn chắc chắn 100% chỉ có duy nhất 1 activity. Dù sao thì bạn cũng đừng bao giờ sử dụng trừ khi nó thực sự cần thiết.

# Intent flags

Bên cạnh việc gán các launch mode trực tiếp từ `AndroidManifest.xml`, chúng ta cũng có thể gán thông qua **Intent Flags**, ví dụ:

````
Intent intent = new Intent(StandardActivity.this, StandardActivity.class);
intent.addFlags(Intent.FLAG_ACTIVITY_SINGLE_TOP);
startActivity(intent);
````

sẽ chạy một standard activity với chế độ singleTop .

Ngoài ra có rất nhiều Flag mà bạn có thể thử. Bạn có thể đọc thêm ở [Intent](http://developer.android.com/reference/android/content/Intent.html#FLAG_ACTIVITY_BROUGHT_TO_FRONT)


[standardtopstandard]: images/standardtopstandard.jpg "standard topstandard"
[standardgallery2]: images/standardgallery2.jpg "standardgallery2"
[gallerystandard]: images/gallerystandard.jpg "gallerystandard"
[standardstandardl]: images/standardstandardl.jpg "standardstandardl"
[standardgalleryl]: images/standardgalleryl.jpg "standardgalleryl"
[gallerystandardl1]: images/gallerystandardl1.jpg "gallerystandardl1"
[gallerystandardl2]: images/gallerystandardl2.jpg "gallerystandardl2"
[singletop]: images/singletop.jpg "singletop"
[singleTask1]: images/singleTask1.jpg "singleTask1"
[singleTaskD]: images/singleTaskD.jpg "singleTaskD"
[singleTaskTaskAffinity]: images/singleTaskTaskAffinity.jpg "singleTaskTaskAffinity"
[screenshot17]: images/screenshot17.jpg "screenshot17"
[singleTaskAnotherApp1]: images/singleTaskAnotherApp1.jpg "singleTaskAnotherApp1"
[singletaskfromapp2]: images/singletaskfromapp2.jpg "singletaskfromapp2"
[singleTaskAnotherApp2]: images/singleTaskAnotherApp2.jpg "singleTaskAnotherApp2"
[singleTaskAnotherApp3]: images/singleTaskAnotherApp3.jpg "singleTaskAnotherApp3"
[singleInstance]: images/singleInstance.jpg "singleInstance"
[singleInstances]: images/singleInstances.jpg "singleInstances"
[screenshot18]: images/screenshot18.jpg "screenshot18"

























