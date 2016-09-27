references:
https://medium.com/@cesarmcferreira/why-you-should-be-doing-functional-reactive-programming-858bd9bb8001#.ek8rx0pdj

## Đánh giá Functional Reactive Programming

Tác giả: **César Ferreira** (Lead Android Engineer @flingtheworld)

Người dịch: Phạm Văn Khắc

Thời gian: 25-09-2016

Dưới đây là bài dựa từ bản gốc [Why you should be doing Functional Reactive Programming](https://medium.com/@cesarmcferreira/why-you-should-be-doing-functional-reactive-programming-858bd9bb8001#.ek8rx0pdj) của tác giả [César Ferreira](https://www.linkedin.com/in/cesarferreira). Mình cũng có một thời gian làm quen với Reactive Programming và tư tưởng của nó nên sẽ bổ sung thêm một số ý kiến của cá nhân. Bạn nào muốn xem bài gốc thì có thể tham khảo ở link trên nhé.

Thực sự thì khi nói đến Reactive Programming (chưa nói đến Functional Reactive Programming) thì không hẳn nhiều người đã từng trải nghiệm cái này. Hiểu đơn giản, với Reactive Programming (gọn là: Rx) thì tất cả dữ liệu đều được truyền đi dưới dạng các event (sự kiện). Các event này được truyền giữa các luồng với nhau đó là các stream.

Lấy một ví dụ cụ thể: ứng dụng [Foody](https://www.foody.vn): ở màn hình Home, bạn chọn một cửa hàng sẽ mở ra màn hình chi tiết của cửa hàng đó. Thì ở đây đã xuất hiện một event (sự kiện) có kiểu là "chọn cửa hàng" với dữ liệu là tên của cửa hàng chẳng hạn. Còn stream (luồng) chính là từ Home chuyển tới Restaurant-Detail.

Khi sử dụng Rx các bạn thường sẽ thấy các đoạn code như thế này: (JavaScript)

````Js
/* Get restaurant data somehow */
const source = getAsyncRestaurantData();

const subscription = source
  .filter(quote => quote.price > 30)
  .map(quote => quote.price)
  .subscribe(
    price => console.log(`Prices higher than $30: ${price}`),
    err => console.log(`Something went wrong: ${err.message}`);
  );

/* When we're done */
subscription.dispose();
````

Trên đây là một đoạn ngắn mã Javascript mô phỏng lấy dữ liệu từ server, chọn ra những cửa hàng nào có chi phí > 30, sau đó in ra các cửa hàng đó (có thể là lỗi nếu có). Cuối cùng là kết thúc quá trình xử lý.

Thì mỗi đoạn `.filter`, `.map`, `.subscribe` chính là các hàm (functional) 'biến đổi' các event trong stream hiện tại để đạt được kết quả mong muốn.

Việc dùng các hàm này giúp cho chương trình ngắn gọn, dễ hiểu hơn. Thực sự khi biết đến, mình đã rất ấn tượng từ cách cài đặt cho tới tư tưởng của Rx (cảm ơn Microsoft đã tạo ra nó).

Một số lợi ích khi sử dụng Rx mà tác giả **César Ferreira** đã kể ra:

- **1. Trách việc gọi quá nhiều [callback](https://www.quora.com/What-is-callback-hell).** 

Bình thường bạn phải tạo callback (là các hàm hay interface) và gọi khi cần thông báo một kết quả trả về.

ví dụ một `callback`:
````
callbackAB = function() {
    console.log("we are done");
};
callbackB = function (b) {
    writeFileContents("ab.txt", callbackAB);
};
callbackA = function (a){
    readFileContents("b.txt", callbackB);
};
readFileContents("a.txt", callbackA);
````

hay dưới dạng `nest functions`:

````
readFileContents("a.txt", function (a){
     readFileContents("b.txt", function (b){
           writeFileContents("ab.txt", function(){
               console.log("we are done");
           });
     });
}); 
````

Thực sự khá là ác mộng khi phải đọc code kiểu như vậy. Có quá nhiều callback và quá nhiều xử lý lồng nhau.

Thay vì như thế, bạn có thể viết đơn giản hơn:

````
const readA = fromEvent(readFileContents("a.txt"));
const readB = fromEvent(readFileContents("b.txt"));
const writeAB = fromEvent(writeFileContents("ab.txt"));

const doneAB = concat(readA,readB,writeAB).subcribe(
    function() {
        console.log("we are done");
    }
)
````

Mặc dù công việc là tuần tự nhưng đọc code chúng ta thấy luồng xử lý đã đơn giản hơn rất nhiều.

- **2. Đơn giản hơn cho việc xử lý bất đồng bộ (threading)**

Thay vì tạo các thread phức tạp để xử lý và gọi lại callback để cập nhật giao diện (UI) thì có thể để Rx tự tạo các thread và quản lý cho chúng ta.

ví dụ với RxJava, tạo một thread xử lý và thông báo kết quả trả về thông qua:

````
.subscribeOn(Schedulers.io())
.observeOn(AndroidSchedulers.mainThread())
````

- **3. Có một [cơ chế chuẩn](https://github.com/ReactiveX/RxJava/wiki/Error-Handling-Operators) để bắt lỗi với mọi thao tác**

Với Javascript thì hàm xử lý chung có dạng `function(err, data)`. Hoặc với Java thì bạn sẽ có 3 phương thức `onNext(data)`, `onError(throwable)`, `onCompleted()`.

Ngoài ra còn rất nhiều phương thức mà bạn có thể dùng như: [`onErrorResumeNext( )`](http://reactivex.io/documentation/operators/catch.html), [`onErrorReturn( )`](http://reactivex.io/documentation/operators/catch.html), [`onExceptionResumeNext( )`](http://reactivex.io/documentation/operators/catch.html), [`retry( )`](http://reactivex.io/documentation/operators/retry.html), [`retryWhen( )`](http://reactivex.io/documentation/operators/retry.html)

- **4. Dễ dàng chuyển tiếp tới các [luồng](http://blog.stablekernel.com/replace-asynctask-asynctaskloader-rx-observable-rxjava-android-patterns/) khác**

Cũng từ việc sử dụng `.subscribeOn(Schedulers.io()).observeOn(AndroidSchedulers.mainThread())` là bạn có thể xử lý dưới background thread và truyền kết quả trả về lên UI-Thread. Việc truyền dữ liệu trở nên đơn giản hơn rất nhiều so với việc bạn sử dụng AsyncTask hay Thread để quản lý.

- **5. Dễ dàng bắt các [tương tác với giao diện](https://medium.com/swlh/party-tricks-with-rxjava-rxandroid-retrolambda-1b06ed7cd29c#.vrs2lolzn)**

Với Button bạn có thể dùng `RxView.clicks(submitButton).subscribe(o -> log("submit button clicked!"));`, 

với EditText thì có `RxTextView.textChangeEvents(email);`, 

với ViewPager thì dùng `RxViewPager.pageScrollStateChanges(o -> log("page change"))` 

Ngoài ra còn rất nhiều tiện ích khác.

- **6. Dùng chung một cơ chế cho [truy cập database, UI, xử lý logic, truy cập mạng](https://medium.com/swlh/party-tricks-with-rxjava-rxandroid-retrolambda-1b06ed7cd29c)**

Bạn có thể dùng `map`, `flatMap`, `concat`, `debounce`, `each`,... cho tất cả các loại trên. Bởi vì Rx coi tất cả đều là stream, biến đổi dữ liệu ở các event trong stream đó.

- **7. Dễ dàng hơn rất nhiều với các [thread phức tạp](http://blog.stablekernel.com/replace-asynctask-asynctaskloader-rx-observable-rxjava-android-patterns/), xử lý bất đồng bộ, song song, và xử lý khi tất cả đã hoàn thành.**

Bạn có thể thay thế hoàn toàn Thread, AsyncTask, AsyncTaskLoader với Rx.Observable. Xử lý đồng thời nhiều logic, kết hợp chúng, biến đổi rồi lại kết hợp và xử lý khi tất cả các tác vụ đó hoàn thành.

- **8. Cực kỳ dễ dàng khi làm việc với UI-Thread**

Thông qua các hàm `onNext`, `onError`, `onComplete`, bạn thoải mái xử lý code logic ở thread khác và cập nhật dữ liệu lên view ở MainThread. Tất cả đã được Rx tối ưu.

- **9. Thoải mái xử lý bất đồng bộ**

Bạn có thể tạo bao nhiêu stream nếu muốn, mỗi stream bạn có thể chạy trên một vài thread khác nhau. Kết hợp các thread đó hoặc chạy tuần tự. Tự do tùy chỉnh cấu hình chạy mà không cần phải sửa nhiều code. Việc này có lợi khi bạn thiết kế theo hướng module, mỗi module tách biệt xử lý các công việc. Và nếu bạn muốn biết khi nào xử lý xong, bạn sẽ được thông báo kịp thời.

- **10. Có rất rất nhiều [chức năng - Operators](https://github.com/ReactiveX/RxJava/wiki/Alphabetical-List-of-Observable-Operators) bạn có thể sử dụng sẵn**

Thoải mái sử dụng những hàm tiện ích phù hợp với yêu cầu. Bạn có thể kết hợp nhiều hàm với nhau, tách biệt từng quá trình xử lý nhưng lại gộp chung chỉ trong một câu lệnh. Thực sự quá tiện ích so với callback thông thường.

- **11. Bạn có thể áp dụng luôn những [chức năng - Operators](https://github.com/ReactiveX/RxJava/wiki/Alphabetical-List-of-Observable-Operators) trên mà chưa cần hiểu hết**

Bởi vì chúng quá rõ ràng để sử dụng. Ví dụ `concat` cho phép thực hiện liên tục nhiều observable, giống như việc ghép chuỗi vậy. hay `count` thực hiện đếm số phần tử được thực hiện khi bạn xử lý một mảng.

- **12. Cực kỳ dễ dàng để thêm/sửa/xóa các khối code khi cần**

Ví dụ, ban đầu bạn code là hiển thị `lastName` của class `User` từ server trả về. Nhưng sau này yêu cầu thay đổi, bạn cần hiển thị cả `firstName + lastName`, bạn chỉ việc thêm một hàm map(user -> `user.firstName + user.lastName)` vào hàm xử lý trước đó. Mà bạn không phải thay đổi hàng loạt các hàm `getUserName` như trước.

- **13. Code của bạn dễ hiểu, dễ test, dễ bảo trì hơn rất nhiều**

Viết code ngắn gọn, theo các nguyên tắc chung, tuần tự theo đúng như các bước xử lý giúp cho code của bạn dễ đọc hiểu. Tách ra các module xử lý riêng biệt giúp bạn dễ viết test-case hơn. Từ đó tránh các lỗi khi sử dụng app, tăng trải nghiệm của người dùng.

- **14. Tuy nhiên, còn một số hạn chế:**

không phải cái gì cũng tốt hết, Rx cũng như vậy, bạn phải bỏ thời gian để làm quen, đọc hiểu khái niệm và cách sử dụng chúng. Việc này đòi hỏi mất nhiều công sức, hơn nữa tư tưởng Rx cũng khá là khác so với tư duy lập trình truyền thống.

Bạn dễ bị sa vào các lỗi rò rỉ bộ nhớ nếu sử dụng không đúng cách. Vì việc xử lý giữa các thread đã bị làm mờ đi bởi Rx, bạn sẽ phải làm quen với việc hiểu đoạn code nào sẽ được thực thi ở WorkerThread hay MainThread. Bạn cũng cần phải giải phóng các đối tượng khi xử lý xong hoặc hủy bỏ lắng nghe khi dữ liệu trả về.

Mặc dù là thư viện khá là nhẹ nhưng với RxJava đã chiếm khoảng 3500 phương thức. Bạn cũng cần cân nhắc khi sử dụng trong dự án của mình và có thể phải dùng đến [multidex](https://medium.com/@rotxed/dex-skys-the-limit-no-65k-methods-is-28e6cb40cf71) để tránh trường hợp này.