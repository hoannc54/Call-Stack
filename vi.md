[source](https://dev.to/valentinogagliardi/javascript-what-is-the-execution-context-what-is-the-call-stack-407a)

# Javascript: Excution Context là gì? Call Stack là gì?

What is the **Execution Context in Javascript**?

**Excution Context (ngữ cảnh thực thi) trong Javascript** là gì?

Tôi cá là bạn không biết câu trả lời.

What are the most basic components of a programming language?
Những thành phần cơ bản nhất của một ngôn ngữ lập trình là gì?

Variables and functions right? Everybody can learn these building blocks.
Liệu có phải là biến và các hàm? Mọi người có thể học về cách xây dựng chúng thành các khối.

But what lies beyond the basics?
Nhưng vậy còn những gì ngoài điều cơ bản trên?


What are the **pillars of Javascript** that you should master before calling yourself intermediate (or even senior) Javascript developer?
Nền tảng cơ bản nhất của Javascript mà bạn nên làm chủ được nó trước khi tự gọi mình là một Javascript developer mức intermediate (hoặc sấp xỉ senior) là gì?

There are many: Scope, Closure, Callbacks, Prototype, and so on.
Có rất nhiều thứ như là: Scope, Closure, Callback, Prototype và các thứ khác kiểu như vậy.

But before diving deeper into these concepts you should at least understand **how the Javascript engine works**.
Nhưng trước khi đi sâu vào các khái niệm này, ít nhất bạn nên hiểu **Javascript engine làm việc như thế nào**.

In this post we'll walk through two fundamental pieces of every Javascript engine: **the Execution Context and the Call Stack**.
Trong bài post này, chúng ta sẽ tìm hiểu về 2 phần cơ bản của mọi Javascript engine: ** Execution Context và Call Stack**.

(Don't be scared. It's easier than you think).
(Đừng sợ, nó dễ hơn bạn nghĩ nhiều)

Ready?
Sẵn sàng chưa?

This material is part of my [Advanced Javascript class available both as remote 1 to 1 training][1] or as [on site training in Europe][2].
Tài liệu này là một phần trong [Lớp đào tạo 1 1 về Javascript nâng cao từ xa][1] và [đào tạo thực tế tại Europe][2] của tôi.

## Javascript: What Is The Execution Context? What You Will Learn
## Javascript: Execution Context là gì? Bạn sẽ học cái gì?

In this post you'll learn:
Trong phần này bạn sẽ được học:

* how the Javascript Engine works
* Javascript engine làm việc như thế nào
* the Execution Context in Javascript
* Execution Context trong Javascript
* what is the Call Stack
* Call Stack là gì?
* the difference between Global Execution Context and Local Execution Context
* Sự khác biệt giữa Global Execution Context và Local Execution Context

## Javascript: What Is The Execution Context? How Does Javascript Run Your Code?
## Javascript: Execution Context là gì? Javascript thực thi code của bạn như thế nào?

How does Javascript run your code?
Javascript thực thi code của bạn như thế nào?

If you're a senior developer you may already know the answer.
Nếu bạn là một developer ở mức senior, bạn có thể biết câu trả lời.

If you're a beginner we'll work through together.
Nếu bạn là một beginner, chúng ta sẽ tìm hiểu cùng nhau.

The truth is, Javascript internals aren't easy.
Sự thật là, phần bên trong của Javascript không hề dễ chút nào.

But I guarantee you can learn them.
Nhưng tôi đảm bảo là bạn có thể học chúng.

And by the time you learn them you'll feel empowered and smarter.
Và khi bạn học chúng, bạn sẽ cảm thấy mình trở lên quyền năng hơn, thông minh hơn.

By looking at Javascript inner functioning you'll become a better Javascript developer, even if you can't master every single detail.
Bằng cách xem xét các chức năng trong javascript, bạn sẽ trở thành một Javascript developer tốt hơn, ngay cả khi bạn không thể làm chủ được từng chi tiết nhỏ.

Now, take a look at the following code:
Bây giờ, hãy thử xem qua đoạn code sau:
```
var num = 2;

function pow(num) {
    return num * num;
}

```

Done?
Xong chưa?

It doesn't look hard!
Nó có vẻ không khó!

Now tell me: **in what order you think the browser will evaluate that code?**
Bây giờ hãy nói cho tôi: **Trình duyệt sẽ thực hiện code đó theo thứ tự nào?**

In other words, if YOU were the browser, how would you read that code?
Nói cách khác, nếu bạn là trình duyệt, bạn sẽ đọc code đó như thế nào?

It sounds like an easy one.
Nghe có vẻ dễ dàng nhỉ.

Most people think "yeah, the browser executes function pow and returns the result, then it assign 2 to num."
Hầu hết mọi người nghĩ là: "yeah, trình duyệt sẽ thực hiện hàm pow và trả về kết quả, sau đó nó gán 2 cho num".

Do you want to know my student's answer?
Bạn có muốn biết câu trả lời của các học sinh của tôi?

_Top to bottom_
_Từ trên xuống dưới_

_The browser will start at function pow, calculating num * num_
_Trình duyệt sẽ bắt đầu với hàm pow, thực hiện phép tính num * num_

_JS engine will run the code line by line_(kind of)
_JS engine sẽ chạy từng dòng code_(hoặc kiểu như vậy)

I was expecting that.
Tôi đã mong chờ điều đó.

I said the same exact things years ago.
Tôi cũng đã nói những thứ tương tự thế trong vài năm về trước.

In the next sections you'll discover the machinery behind those **apparently simple lines of code**.
Trong phần tiếp theo, bạn sẽ tìm hiểu về cơ chế đằng sau **các dòng code dường như đơn giản ấy**

## Javascript: What Is The Execution Context? Javascript Engines
## Javascript: Execution Context là gì? Các Javascript Engine

Do you want to be an average Javascript developer?
Bạn có muốn mình chỉ là một Javascript developer ở mức trung bình?

I bet you won't.
Tôi cá là bạn sẽ không muốn.

If you want to make a good impression in a Javascript interview you should at least know **how Javascript runs your code**.
Nếu bạn muốn tạo được ấn tượng tốt trong cuộc phỏng vấn về Javascript, ít nhất bạn phải biết **Javascript thực hiện code của bạn như thế nào**.

(And a bunch of other stuff which I won't cover here).
(Và một loạt các thứ khác nữa nhưng tôi sẽ không nêu ra ở đây).

Don't rush on these concepts.
Đừng quá vội vàng bỏ qua các khái niệm này.

You can't learn everything in a day. It will take time.
Bạn không thể học tất cả mọi thứ trong một ngày. Nó sẽ mất nhiều thời gian.

The good news? I'm going to make that stuff understandable for everyone (at least I'll try).
Vậy tin tốt là gì? Tôi sẽ làm nó trở nên dễ hiểu hơn cho mọi người ( ít nhất là tôi sẽ cố gắng).

To understand how Javascript runs your code we should meet the first scary thing: **the Execution Context**.
Để hiểu được Javascript thực hiện code của bạn như thế nào, chúng ta sẽ phải gặp thứ đáng sợ đầu tiên: **Execution Context** (Ngữ cảnh thực thi)

What is the Execution Context in Javascript?
Vậy Execution Context trong Javascript là gì?

**Every time you run Javascript in a browser** (or in Node) **the engine goes through a series of steps**.
**Mỗi khi bạn chạy javascript trên một trình duyệt** (hoặc trên Node) **Engine sẽ đi qua một loạt các bước**.

One of this steps involves **the creation of the Global Execution Context**.
Một trong các bước này là: **tạo ra một Global Execution Context**.

But wait, **what is the engine**?
Nhưng chờ đã, **một engine là gì**?

That is, the Javascript engine is the "engine" that runs Javascript code.
Đó, Javascript engine là một "công cụ" thực thi code Javascript.

Nowadays there are two prominent Javascript engines: **Google V8** and **SpiderMonkey**.
Hiện nay có hay Javascript engine nổi bật là: **Google V8** và **SpiderMonkey**.

V8 is the Google’s open source JavaScript engine, used in Google Chrome and Node.js.
V8 là một Javascript engine mã nguồn mở của Google, được sử dụng cho Google Chrome và Node.js.

SpiderMonkey is the Mozilla's JavaScript engine, used in Firefox.
SpiderMonkey là Javascript engine của Mozilla, được sử dụng cho Firefox.

So far we have the Javascript engine and an Execution Context.
Tới đây ta đã có Javascript engine và Execution Context.

Now it’s time to understand how they work together.
Bây giờ là lúc để hiểu chúng làm việc với nhau như thế nào.

## Javascript: What Is The Execution Context? How It Works?
## Javascript: Execution Context là gì? Nó làm việc như thế nào?

**The engine creates a Global Execution Context** every time you run some Javascript code.
**Engine tạo ra một Global Execution Context** mỗi khi bạn chạy một đoạn code Javascript nào đó.

Execution Context is a fancy word for describing the environment in which your Javascript code runs.
Execution Context là một từ ưa thích để mô tả về môi trường mà code Javascript của bạn chạy.

It's hard to visualize these abstract things, I feel you.
 tôi có thể hiểu được bạn đang khó để hình dung được những thứ trừu tượng này.

For now think of the Global Execution Context as a box:
Bây giờ hãy nghĩ Global Execution Context như là một cái hộp:

![][3]

Let's look again at our code:
Hãy xem lại đoạn code của chúng ta:

``` 
var num = 2;

function pow(num) {
    return num * num;
}
```

How does the engine read that code?
Engine đọc code này như thế nào?

Here is a simplified version:
Đây là một cách hiểu đơn giản:

**Engine**: Line one. There's a variable! Cool. Let's store it in the Global Memory.
**Engine**: Dòng đầu tiên. Đây là một biến! Tốt thôi. Hãy lưu trữ nó và Global Memory.

**Engine**: Line three. I see a function declaration. Cool. Let's store that in the Global Memory too!
**Engine**: Dòng thứ 3. Tôi thấy có khai báo hàm. Tốt. Hãy lưu trữ nó voà Global Memory.

**Engine**: Looks like I'm done.
**Engine**: Có vẻ như là tôi đã làm xong rồi

If I were to ask you again: how does the browser "see" the following code, what would you say?
Nếu tôi hỏi lại bạn một lần nữa: trình duyệt đã "xem" code sau đây nhưu thế nào, bạn sẽ nói gì?

Yeah, it's kind of top to bottom but ...
Yeah, nó thuộc lại từ trên xuống nhưng...

As you can see the engine does not run the function pow!
Như bạn thấy, engine không thực hiện hàm pow!

It's a **function declaration**, not a function call.
Nó chỉ là **khai báo hàm**, không phải là lời gọi hàm.

The above code will translate in some values stored in the **Global Memory**: a function declaration and a variable.
Đoạn code trên sẽ được dịch thành các giá trị, lưu trong **Global Memory**: một khai báo hàm và một biến.

**Global Memory**?
**Global Memory**?

Valentino, I'm already confused by the Execution Context and now you're throwing the Global Memory at me?
Valentino, Tôi đã đang nhầm lẫn về Execution Context và giờ bạn còn ném cả Global Memory vào tôi?

Yes I am.
Vâng, đúng thế.

Let's see what the Global Memory is.
Hãy tìm hiểu xem Global Memory là gì

## Javascript: What Is The Execution Context? The Global Memory
## Javascript: Execution Context là gì? Global Memory

The **Javascript engine has a Global Memory too**.
Một **Javascript engine cũng có Global Memory**.

The Global Memory contains global variables and function declarations for later use.
Global Memory chứa các biến toàn cục và các khai báo hàm để sử dụng về sau.

If you read "Scope and Closures" by Kyle Simpson you may find that the Global Memory overlaps with the concept of Global Scope.
Nếu bạn đọc về "Scope và Closures" của Kyle Simpson, bạn có thể thấy rằng khái niệm Global Memory trùng với Global Scope.

In fact they are the same thing.
Sự thật thì chúng là cùng một thứ.

I'm flying 10,000 feet high here, for a good reason.
Vì một lý do chính đáng nào đó, tôi đang lơ lửng ở độ cao 10,000 feet đây.

Those are hard concepts.
Chúng thực sự là những khái niệm khó.

But you shouldn't worry for now.
Nhưng giờ bạn không nên quá lo lắng.

I want you to understand two important pieces of our puzzle.
Tôi muốn bạn hiểu được hai phần quan trong câu hỏi của chúng ta.

When the Javascript engine runs your code it creates:
Khi Javascript engine thực thi code của bạn, nó tạo ra:

* a Global Execution context
* Một Global Execution Context
* a Global Memory (also called Global Scope or Global Variable Environment)
* một Global Memory (cũng có thể gọi là Global Scope hoặc Global Variable Environment)

Is everything clear?
Mọi thứ đã rõ ràng chưa?

If I were you at this point I'll:
Nếu tôi là bạn, vào thời điểm này tôi sẽ: 

* write down some Javascript code
* Viết lại một vài đoạn code Javascript
* parse the code step by step as you were the engine
* Phân tích cú pháp của code từng bước một khi bạn là một engine
* make a graphic representation of both the Global Execution context and the Global Memory during the execution
* vẽ ra một đồ thị biểu diễn cả Global Execution Context và Global Memory trong suốt quá trình thực thi.

You can write the exercise on paper or with a prototyping tool.
Bạn có thể viết lại bài tập này lên giấy hoặc bằng các công cụ tạo biểu mẫu.

For my tiny example the picture will look like so:
Đối với ví dụ nhỏ của tôi, bức tranh sẽ trông dạng như:

![][4]

In the next section we'll look at another scary thing: the **Call Stack**.
Trong phần tiếp theo, chúng ta sẽ tìm hiểu một thứ đáng sợ khác: **Call Stack**.

## Javascript: What Is The Execution Context? What Is The Call Stack?
## Javascript: Execution Context là gì? Call Stack là gì?

Do you have a clear picture of **how** the **Execution Context**, the **Global Memory** and the **Javascript engine fits together**?
Nếu bạn có một bức tranh rõ ràng về **Executation Context**, **Global Memory** và **Javascript engine có phù hợp với nhau hay không**?

If not take your time to review the previous section.
Nếu bạn không dành thời gian để xem lại các phần trước. 

We're going to introduce another piece in our puzzle: the **Call Stack**.
Chúng ta sẽ giới thiệu một phần khác trong vấn đề của chúng ta: **Call Stack**

Let's see what happens during code execution.
Hãy xem điều gì xảy ra trong quá trình thực hiện code.

Let's first recap what happens when the Javascript engin runs your code.
Trước tiên hãy tóm tắt lại xem điều gì xảy ra khi Javascript engine thực thi code của bạn.

It creates:
Nó tạo ra:

* a Global Execution context
* Một Global Execution Context
* a Global Memory
* Một Global Memory

Besides that in our example nothing more happened:
Bên cạnh đó trong ví dụ của tôi không có gì khác xảy ra:

``` 
var num = 2;

function pow(num) {
    return num * num;
}
```

The code is a pure allocation of values.
Code này chỉ chứa các giá trị thuần tuý.

Let's take a step further.
Hãy thử thêm một bước nữa.

What happens if I call the function?
Điều gì sẽ sảy ra nếu tôi gọi hàm?

```
var num = 2;

function pow(num) {
    return num * num;
}

var res = pow(num);
```
Interesting question.
Đây là câu hỏi thú vị.

The act of **calling a function in Javascript makes the engine ask for help**.
Hành động của **Gọi một hàm trong Javascript khiến cho engine yêu cầu được hỗ trợ**

And that help comes from a friend of the Javascript engine: the **Call Stack**.
Và sự giúp đỡ đến từ một người bạn của Javascript engine: **Call Stack**.

It might not sound obvious but the **Javascript engine needs to keep track of what's happening**.
Nó có thể không được rõ ràng lắm nhưng **Javascript engine cũng cần phải ghi lại được những gì đang xảy ra**

It relies on the Call Stack for that.
Nó dựa trên Call Stack để thực hiện điều đó.

What is the Call Stack in Javascript?
Call Stack trong Javascript là gì?

The **Call Stack is like a log of the current execution of the program**.
**Call Stack giống như một bản ghi log tiến trình hiện tại của chương trình**

In reality it's a data structure: a stack.
Trong thực tế, nó là một cấu trúc dữ liệu: ngăn xếp (stack).

How does exactly the Call Stack work?
Vậy thì chính xác Call Stack là việc như thế nào? 

Unsurprisingly it has two methods: **push** and **pop**.
Không có gì đáng ngạc nhiên khi nó có hai phương thức: **push** và **pop**

**Pushing** is the act of **putting something into the stack**.
**Pushing** là một hành động của **đưa thứ gì đó vào stack**.

That is, **when you run a function in Javascript, the engine pushes that function into the Call Stack**.
Có nghĩa là, **Khi bạn chạy một hàm trong Javascript, engine đưa hàm đó vào Call Stack**.

Every function call gets pushed into the Call Stack.
Mọi lời gọi hàm đều được đẩy vào Call Stack

The **first thing that gets pushed is main()** (or global()), the main thread of execution of your Javascript program.
**Thứ đầu tiên được đẩy vào là main()** (hoặc global()), luồng chính thực hiện của chương trình Javascript của bạn.

Now, the previous picture will look like so:
Bây giờ, bức tranh trước sẽ có dạng như:

![][5]

**Popping** on the other end is the act of **removing something from the stack**.
**Popping** ở phía ngược lại là hành động của **xoá thứ gì đó ra khỏi stack**.

When a function ends executing it gets popped from the Call Stack.
Khi một hàm kết thúc việc thực thi, nó sẽ được bỏ ra khỏi Call Stack

And our Call Stack will look like the following:
Và Call Stack của chúng ta sẽ có dạng như sau:

![][6]

And now you're ready to master every Javascript concept out there.
Và hiện tại bạn đã sẵn sàng làm chủ được mọi khái niệm của Javascript.

But where not done! Go to the next section!
Nhưng ở đây chưa xong đâu! Đi tới phần tiếp theo nào!

## Advanced Javascript: What Is The Execution Context? The Local Execution Context
## Javascript nâng cao: Execution Context là gì? Local Execution Context

Everything seems clear so far.
Cho tới giờ thì mọi thứ đã khá rõ ràng.

Are we missing something?
Chúng ta có thiếu gì không?

We know that the **Javascript engine creates a Global Execution context and a Global Memory**.
Chúng ta biết rằng **Javascript engine tạo ra Global Execution Context và Global Memory**.

Then, when you call a function in your code:
Sau đó, khi bạn gọi hàm trong code của bạn:

* the Javascript engine asks for help
* Javascript engine sẽ đề nghị được giúp đỡ
* that help comes from a friend of the Javascript engine: the **Call Stack**
*Sự giúp đỡ đến từ một người bạn của Javascript engine: **Call Stack**
* the **Call Stack keep tracks of what function is being called** in your code
* **Call Stack theo dõi các hàm được gọi** trong code của bạn

Yet **another thing has to happen when you run a function** in Javascript.
Tuy nhiên **một điều khác xảy ra khi bạn chạy một hàm** trong Javascript.

First, **the function appears in the Global Execution context**.
Đầu tiên, **hàm đó xuất hiện trong Global Execution Context**.

Then, **another mini-context appears alongside the function**.
Tiếp theo, **một context nhỏ khác cũng xuất hiện cùng với hàm đó**

That little new box is called **Local Execution Context**.
Cái hộp mới nhỏ hơn này được gọi là **Local Execution Context**.

A Local Execution context gets created inside that function!
Một Local Execution Context được tạo bên trong hàm đó!

What??
Cái gì cơ?

If you noticed, in the previous picture a new variable appears in the Global memory: **var** *res_*.
Nếu bạn nhận thấy, ở bức tranh trước , một biến mới xuất hiện trong Global Memory: **var** *res_*.

The variables *res* has a value of **undefined at first**.
Các biến *res* có giá trị ban đầu là **underfined**.

Then as soon as **pow** appears in the **Global Execution Context, the function executes and res takes its return value**.
Sau đó, ngay khi **pow** xuất hiện trong **Global Execution Context, hàm đó thực hiện, và res lấy giá trị của nó để trả về**.

During the execution phase a Local Execution Context gets created, alongside a Local Memory for holding up local variables.
Trong suốt giai đoạn thực thi, một Local Execution Context được tạo ra, cùng với một Local Memory để lưu các biến cục bộ.

What a powerful concept.
Đây là khái niệm mạnh mẽ

![][7]

Keep that in mind.
Hãy nhớ nó thật kĩ

Understanding both the **Global and the Local Execution Context is the key for mastering Scope and Closures**.
Hiểu về cả **Global và Local Execution Context là chìa khoá để làm chủ được Scope và Closures**.

## Javascript: What Is The Execution Context? What Is The Call Stack? Wrapping up
## Javascript: Execution Context là gì? Call Stack là gì? Kết thúc

Can you believe what's behind 4 lines of code?
Bạn có thể tin được những gì nằm sau 4 dòng code?

The Javascript engine creates an **Execution Context, a Global Memory, and a Call Stack**.
Javascript engine tạo ra **một Execution Context, một Global Memory, và một Call Stack**.

But once you call a function the engine creates a **Local Execution Context which has a Local Memory**.
Nhưng khi bạn gọi một hàm, engine tạo ra **một Local Execution Context cùng với một Local Memory**.

By the end of this post you should be able to understand what happens when you run some Javascript code.
Đến cuối của bài viết này, bạn sẽ hiểu được điều gì xảy ra khi bạn chạy vài đoạn code Javascript.

Often overlooked, Javascript internals are always seen as misterious things by new developers.
Những nội dung của Javascript bị bỏ qua luôn được xem như những gì bí ẩn đối với các developer mới. 

Yet they are the **key for mastering advanced Javascript concepts**.
Tuy nhiên chúng lại là **chìa khoá để làm chủ các khái niệm về Javascript nâng cao**.

If you learn Execution Context, Global Memory, and the Call Stack, then Scope, Closures, Callbacks and other stuff will be easy as a breeze.
Nếu bạn học từ Execution Context, Global Memory và Call Stack, tiếp theo là Scope, Closure, Callback và những thứ khác sẽ dễ dàng như một cơn gió nhẹ.

In particular, understanding the Call Stack is paramount.
Đặc biệt, sự hiểu biết về Call Stack là cực kì quan trọng.

All the Javascript will start to make sense once you visualize it: you will finally understand **why Javascript is asynchronous** and why we do need Callbacks.
Tất cả các loại Javascript sẽ bắt đầu có ý nghĩa khi bạn hình dung ra nó: cuối cùng bạn sẽ hiểu **tại sao Javascript lại bất đòng bộ** và tại sao chúng ta cần Callback.

Did you know **what's behind 4 lines of Javascript code**?
Bạn đã biết **Cái gì đằng sau 4 dòng code Javascript**?

Now you know.
Bây giờ thì bạn biết rồi đó.

Thanks for reading!
Cám ơn vì đã đọc!

This material is part of my [Advanced Javascript class available both as remote 1 to 1 training][8] or [as on site training in Europe][9].
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