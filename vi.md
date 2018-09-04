[source](https://dev.to/valentinogagliardi/javascript-what-is-the-execution-context-what-is-the-call-stack-407a)

# Javascript: Excution Context là gì? Call Stack là gì?

**Excution Context (ngữ cảnh thực thi) trong Javascript** là gì?

Tôi cá là bạn không biết câu trả lời.

Những thành phần cơ bản nhất của một ngôn ngữ lập trình là gì?

Liệu có phải là biến và các hàm? Mọi người có thể học về cách xây dựng chúng thành các khối.

Nhưng vậy còn những gì ngoài điều cơ bản trên?

Nền tảng cơ bản nhất của Javascript mà bạn nên làm chủ được nó trước khi tự gọi mình là một Javascript developer mức intermediate (hoặc sấp xỉ senior) là gì?

Có rất nhiều thứ như là: Scope, Closure, Callback, Prototype và các thứ khác kiểu như vậy.

Nhưng trước khi đi sâu vào các khái niệm này, ít nhất bạn nên hiểu **Javascript engine làm việc như thế nào**.

Trong bài post này, chúng ta sẽ tìm hiểu về 2 phần cơ bản của mọi Javascript engine: ** Execution Context và Call Stack**.

(Đừng sợ, nó dễ hơn bạn nghĩ nhiều)

Sẵn sàng chưa?

Tài liệu này là một phần trong [Lớp đào tạo 1 1 về Javascript nâng cao từ xa][1] và [đào tạo thực tế tại Europe][2] của tôi.

## Javascript: Execution Context là gì? Bạn sẽ học cái gì?

Trong phần này bạn sẽ được học:

* Javascript engine làm việc như thế nào
* Execution Context trong Javascript
* Call Stack là gì?
* Sự khác biệt giữa Global Execution Context và Local Execution Context

## Javascript: Execution Context là gì? Javascript thực thi code của bạn như thế nào?

Javascript thực thi code của bạn như thế nào?

Nếu bạn là một developer ở mức senior, bạn có thể biết câu trả lời.

Nếu bạn là một beginner, chúng ta sẽ tìm hiểu cùng nhau.

Sự thật là, phần bên trong của Javascript không hề dễ chút nào.

Nhưng tôi đảm bảo là bạn có thể học chúng.

Và khi bạn học chúng, bạn sẽ cảm thấy mình trở lên quyền năng hơn, thông minh hơn.

Bằng cách xem xét các chức năng trong javascript, bạn sẽ trở thành một Javascript developer tốt hơn, ngay cả khi bạn không thể làm chủ được từng chi tiết nhỏ.

Bây giờ, hãy thử xem qua đoạn code sau:
```
var num = 2;

function pow(num) {
    return num * num;
}

```

Xong chưa?

Nó có vẻ không khó!

Bây giờ hãy nói cho tôi: **Trình duyệt sẽ thực hiện code đó theo thứ tự nào?**

Nói cách khác, nếu bạn là trình duyệt, bạn sẽ đọc code đó như thế nào?

Nghe có vẻ dễ dàng nhỉ.

Hầu hết mọi người nghĩ là: "yeah, trình duyệt sẽ thực hiện hàm pow và trả về kết quả, sau đó nó gán 2 cho num".

Bạn có muốn biết câu trả lời của các học sinh của tôi?

_Từ trên xuống dưới_

_Trình duyệt sẽ bắt đầu với hàm pow, thực hiện phép tính num * num_

_JS engine sẽ chạy từng dòng code_(hoặc kiểu như vậy)

Tôi đã mong chờ điều đó.

Tôi cũng đã nói những thứ tương tự thế trong vài năm về trước.

Trong phần tiếp theo, bạn sẽ tìm hiểu về cơ chế đằng sau **các dòng code dường như đơn giản ấy**

## Javascript: Execution Context là gì? Các Javascript Engine

Bạn có muốn mình chỉ là một Javascript developer ở mức trung bình?

Tôi cá là bạn sẽ không muốn.

Nếu bạn muốn tạo được ấn tượng tốt trong cuộc phỏng vấn về Javascript, ít nhất bạn phải biết **Javascript thực hiện code của bạn như thế nào**.

(Và một loạt các thứ khác nữa nhưng tôi sẽ không nêu ra ở đây).

Đừng quá vội vàng bỏ qua các khái niệm này.

Bạn không thể học tất cả mọi thứ trong một ngày. Nó sẽ mất nhiều thời gian.

Vậy tin tốt là gì? Tôi sẽ làm nó trở nên dễ hiểu hơn cho mọi người ( ít nhất là tôi sẽ cố gắng).

Để hiểu được Javascript thực hiện code của bạn như thế nào, chúng ta sẽ phải gặp thứ đáng sợ đầu tiên: **Execution Context** (Ngữ cảnh thực thi)

Vậy Execution Context trong Javascript là gì?

**Mỗi khi bạn chạy javascript trên một trình duyệt** (hoặc trên Node) **Engine sẽ đi qua một loạt các bước**.

Một trong các bước này là: **tạo ra một Global Execution Context**.

Nhưng chờ đã, **một engine là gì**?

Đó, Javascript engine là một "công cụ" thực thi code Javascript.

Hiện nay có hay Javascript engine nổi bật là: **Google V8** và **SpiderMonkey**.

V8 là một Javascript engine mã nguồn mở của Google, được sử dụng cho Google Chrome và Node.js.

SpiderMonkey là Javascript engine của Mozilla, được sử dụng cho Firefox.

Tới đây ta đã có Javascript engine và Execution Context.

Bây giờ là lúc để hiểu chúng làm việc với nhau như thế nào.

## Javascript: Execution Context là gì? Nó làm việc như thế nào?

**Engine tạo ra một Global Execution Context** mỗi khi bạn chạy một đoạn code Javascript nào đó.

Execution Context là một từ ưa thích để mô tả về môi trường mà code Javascript của bạn chạy.

 tôi có thể hiểu được bạn đang khó để hình dung được những thứ trừu tượng này.

Bây giờ hãy nghĩ Global Execution Context như là một cái hộp:

![][3]

Hãy xem lại đoạn code của chúng ta:

``` 
var num = 2;

function pow(num) {
    return num * num;
}
```

Engine đọc code này như thế nào?

Đây là một cách hiểu đơn giản:

**Engine**: Dòng đầu tiên. Đây là một biến! Tốt thôi. Hãy lưu trữ nó và Global Memory.

**Engine**: Dòng thứ 3. Tôi thấy có khai báo hàm. Tốt. Hãy lưu trữ nó voà Global Memory.

**Engine**: Có vẻ như là tôi đã làm xong rồi

Nếu tôi hỏi lại bạn một lần nữa: trình duyệt đã "xem" code sau đây nhưu thế nào, bạn sẽ nói gì?

Yeah, nó thuộc lại từ trên xuống nhưng...

Như bạn thấy, engine không thực hiện hàm pow!

Nó chỉ là **khai báo hàm**, không phải là lời gọi hàm.

Đoạn code trên sẽ được dịch thành các giá trị, lưu trong **Global Memory**: một khai báo hàm và một biến.

**Global Memory**?

Valentino, Tôi đã đang nhầm lẫn về Execution Context và giờ bạn còn ném cả Global Memory vào tôi?

Vâng, đúng thế.

Hãy tìm hiểu xem Global Memory là gì

## Javascript: Execution Context là gì? Global Memory

Một **Javascript engine cũng có Global Memory**.

Global Memory chứa các biến toàn cục và các khai báo hàm để sử dụng về sau.

Nếu bạn đọc về "Scope và Closures" của Kyle Simpson, bạn có thể thấy rằng khái niệm Global Memory trùng với Global Scope.

Sự thật thì chúng là cùng một thứ.

Vì một lý do chính đáng nào đó, tôi đang lơ lửng ở độ cao 10,000 feet đây.

Chúng thực sự là những khái niệm khó.

Nhưng giờ bạn không nên quá lo lắng.

Tôi muốn bạn hiểu được hai phần quan trong câu hỏi của chúng ta.

Khi Javascript engine thực thi code của bạn, nó tạo ra:

* Một Global Execution Context
* một Global Memory (cũng có thể gọi là Global Scope hoặc Global Variable Environment)

Mọi thứ đã rõ ràng chưa?

Nếu tôi là bạn, vào thời điểm này tôi sẽ: 

* Viết lại một vài đoạn code Javascript
* Phân tích cú pháp của code từng bước một khi bạn là một engine
* vẽ ra một đồ thị biểu diễn cả Global Execution Context và Global Memory trong suốt quá trình thực thi.

Bạn có thể viết lại bài tập này lên giấy hoặc bằng các công cụ tạo biểu mẫu.

Đối với ví dụ nhỏ của tôi, bức tranh sẽ trông dạng như:

![][4]

Trong phần tiếp theo, chúng ta sẽ tìm hiểu một thứ đáng sợ khác: **Call Stack**.

## Javascript: Execution Context là gì? Call Stack là gì?

Nếu bạn có một bức tranh rõ ràng về **Executation Context**, **Global Memory** và **Javascript engine có phù hợp với nhau hay không**?

Nếu bạn không dành thời gian để xem lại các phần trước. 

Chúng ta sẽ giới thiệu một phần khác trong vấn đề của chúng ta: **Call Stack**

Hãy xem điều gì xảy ra trong quá trình thực hiện code.

Trước tiên hãy tóm tắt lại xem điều gì xảy ra khi Javascript engine thực thi code của bạn.

Nó tạo ra:

* Một Global Execution Context
* Một Global Memory

Bên cạnh đó trong ví dụ của tôi không có gì khác xảy ra:

``` 
var num = 2;

function pow(num) {
    return num * num;
}
```

Code này chỉ chứa các giá trị thuần tuý.

Hãy thử thêm một bước nữa.

Điều gì sẽ sảy ra nếu tôi gọi hàm?

```
var num = 2;

function pow(num) {
    return num * num;
}

var res = pow(num);
```
Đây là câu hỏi thú vị.

Hành động của **Gọi một hàm trong Javascript khiến cho engine yêu cầu được hỗ trợ**

Và sự giúp đỡ đến từ một người bạn của Javascript engine: **Call Stack**.

Nó có thể không được rõ ràng lắm nhưng **Javascript engine cũng cần phải ghi lại được những gì đang xảy ra**

Nó dựa trên Call Stack để thực hiện điều đó.

Call Stack trong Javascript là gì?

**Call Stack giống như một bản ghi log tiến trình hiện tại của chương trình**

Trong thực tế, nó là một cấu trúc dữ liệu: ngăn xếp (stack).

Vậy thì chính xác Call Stack là việc như thế nào? 

Không có gì đáng ngạc nhiên khi nó có hai phương thức: **push** và **pop**

**Pushing** là một hành động của **đưa thứ gì đó vào stack**.

Có nghĩa là, **Khi bạn chạy một hàm trong Javascript, engine đưa hàm đó vào Call Stack**.

Mọi lời gọi hàm đều được đẩy vào Call Stack

**Thứ đầu tiên được đẩy vào là main()** (hoặc global()), luồng chính thực hiện của chương trình Javascript của bạn.

Bây giờ, bức tranh trước sẽ có dạng như:

![][5]

**Popping** ở phía ngược lại là hành động của **xoá thứ gì đó ra khỏi stack**.

Khi một hàm kết thúc việc thực thi, nó sẽ được bỏ ra khỏi Call Stack

Và Call Stack của chúng ta sẽ có dạng như sau:

![][6]

Và hiện tại bạn đã sẵn sàng làm chủ được mọi khái niệm của Javascript.

Nhưng ở đây chưa xong đâu! Đi tới phần tiếp theo nào!

## Javascript nâng cao: Execution Context là gì? Local Execution Context

Cho tới giờ thì mọi thứ đã khá rõ ràng.

Chúng ta có thiếu gì không?

Chúng ta biết rằng **Javascript engine tạo ra Global Execution Context và Global Memory**.

Sau đó, khi bạn gọi hàm trong code của bạn:

* Javascript engine sẽ đề nghị được giúp đỡ
*Sự giúp đỡ đến từ một người bạn của Javascript engine: **Call Stack**
* **Call Stack theo dõi các hàm được gọi** trong code của bạn

Tuy nhiên **một điều khác xảy ra khi bạn chạy một hàm** trong Javascript.

Đầu tiên, **hàm đó xuất hiện trong Global Execution Context**.

Tiếp theo, **một context nhỏ khác cũng xuất hiện cùng với hàm đó**

Cái hộp mới nhỏ hơn này được gọi là **Local Execution Context**.

Một Local Execution Context được tạo bên trong hàm đó!

Cái gì cơ?

Nếu bạn nhận thấy, ở bức tranh trước , một biến mới xuất hiện trong Global Memory: **var** *res_*.

Các biến *res* có giá trị ban đầu là **underfined**.

Sau đó, ngay khi **pow** xuất hiện trong **Global Execution Context, hàm đó thực hiện, và res lấy giá trị của nó để trả về**.

Trong suốt giai đoạn thực thi, một Local Execution Context được tạo ra, cùng với một Local Memory để lưu các biến cục bộ.

Đây là khái niệm mạnh mẽ

![][7]

Hãy nhớ nó thật kĩ

Hiểu về cả **Global và Local Execution Context là chìa khoá để làm chủ được Scope và Closures**.

## Javascript: Execution Context là gì? Call Stack là gì? Kết thúc

Bạn có thể tin được những gì nằm sau 4 dòng code?

Javascript engine tạo ra **một Execution Context, một Global Memory, và một Call Stack**.

Nhưng khi bạn gọi một hàm, engine tạo ra **một Local Execution Context cùng với một Local Memory**.

Đến cuối của bài viết này, bạn sẽ hiểu được điều gì xảy ra khi bạn chạy vài đoạn code Javascript.

Những nội dung của Javascript bị bỏ qua luôn được xem như những gì bí ẩn đối với các developer mới. 

Tuy nhiên chúng lại là **chìa khoá để làm chủ các khái niệm về Javascript nâng cao**.

Nếu bạn học từ Execution Context, Global Memory và Call Stack, tiếp theo là Scope, Closure, Callback và những thứ khác sẽ dễ dàng như một cơn gió nhẹ.

Đặc biệt, sự hiểu biết về Call Stack là cực kì quan trọng.

Tất cả các loại Javascript sẽ bắt đầu có ý nghĩa khi bạn hình dung ra nó: cuối cùng bạn sẽ hiểu **tại sao Javascript lại bất đòng bộ** và tại sao chúng ta cần Callback.

Bạn đã biết **Cái gì đằng sau 4 dòng code Javascript**?

Bây giờ thì bạn biết rồi đó.

Cám ơn vì đã đọc!

Tài liệu này là một phần trong [Lớp đào tạo 1 1 về Javascript nâng cao từ xa][1] và [đào tạo thực tế tại Europe][2] của tôi.



[1]: https://www.valentinog.com/
[2]: https://www.servermanaged.it/formazione-javascript-react.html
[3]: https://res.cloudinary.com/practicaldev/image/fetch/s--jWWiNqCD--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/ptgf765a4989zd6twwfp.png
[4]: https://res.cloudinary.com/practicaldev/image/fetch/s--Ae3LEIz8--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/7sreqswm895lrphw4xy2.png
[5]: https://res.cloudinary.com/practicaldev/image/fetch/s--u-ktmKSh--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/ww11po2kybdij4zino4r.png
[6]: https://res.cloudinary.com/practicaldev/image/fetch/s--XzP4eR0c--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/2zbqkyan7uu67xrhpx1b.png
[7]: https://res.cloudinary.com/practicaldev/image/fetch/s--Zpkb7RDS--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/8qoepadnbmo8alj8h0vi.png
[8]: https://www.valentinog.com/
[9]: https://www.servermanaged.it/formazione-javascript-react.html