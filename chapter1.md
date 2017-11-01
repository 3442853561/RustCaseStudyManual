# 第1章 快速入门

对于任何一个想要自我提升的开发者来说，学习新的语言绝对是一个值得花费时间和精力尝试的事情。Rust 在最近两年成为了一部分优秀的开发者中眼中的当红语言。不过对于很多其他语言的程序员来说，Rust 似乎有着非常复杂的语法。至少它的报错信息给人一种感觉——我在别的语言里面做同样的事情是不会报错，这让很多程序员对 Rust 敬而远之。作为一个过去的 C/C++ 程序员（目前在工作中用到了 Rust），笔者将展示一系列的案例来向已经掌握至少一门编程语言的读者们讲解如何着手 Rust 的开发，以及怎么在工作中用到 Rust。那么阅读这个教程之前请您确认自己能够读懂 C、C++、Java 或 C# 中至少一种语言的源代码。这并不是一个面向毫无编程基础的读者的教程。如果您还没有编程基础，笔者建议您先去看一看其他的教程。

那么现在我们就来开始我们的开发案例。很多程序设计的教程都会让您在屏幕上先输出 “Hello, World”。不过鉴于本教程的读者都是有经验的程序员（估计其中有一些人已经是高级工程师或者技术总监了），我们就跳过这个无聊的例子好了。我们一上来就做一个有点儿意思的程序。大致就是对可以自称是工程师的那些人的 “Hello, World” ——我们来做一个电话簿。当然了这个电话簿根本就没有图形化的界面，只能在控制台运行。笔者在刚刚当上程序员的时候（大约2004年前后），做这种东西就能轻车熟路。这里我们只是把它当成一个欢迎大家学习Rust语言的例子就好了。

因为这个例子需要对文件进行操作，而且还需要有输入的功能。所以您不得不下载 Rust 的开发工具。您可以在官方网站（ [https://www.rust-lang.org/zh-CN/](https://www.rust-lang.org/zh-CN/) ）找到多种形式以及配合不同环境的工具。在 Linux 或 Mac（OS X）上您仅需要在终端输入：

```
curl https://sh.rustup.rs -sSf | sh
```

而在Windows需要下载 rustup-init.exe 当然您也可以下载离线的安装包，不过使用 rustup 实际上是一个一劳永逸的方法，因为您不必要像使用离线安装包那样在升级和切换默认工具链时忙手忙脚。这也是笔者作为一个有经验的 Rust 开发者给您的忠告。当然了，您也可以选择使用镜像。只需要设置两个环境变量就可以了：

```
RUSTUP_DIST_SERVER=https://mirrors.ustc.edu.cn/rust-static
RUSTUP_UPDATE_ROOT=https://mirrors.ustc.edu.cn/rust-static/rustup
```

在Windows系统下笔者为您写好了一个批处理，当然了前提条件是单纯为了从镜像获取升级为目的使用批处理，并且您已经安装好了 rustup。批处理的获取地址是：

```
https://github.com/3442853561/rust-housework/blob/master/batch/update.bat
```

这里还有一个用于 Linux 的版本，由 GitHub 上的 [@Linger2](https://github.com/Linger2) 提供。在这里笔者为他做出的无私贡献表示深深地感谢。获取地址是：

```
https://github.com/3442853561/rust-housework/blob/master/batch/update.sh
```

此外您还可以下载离线的安装包，虽然这是不推荐的做法。现在就让我们开始愉快的编程吧。首先要注意的是 Rust 在保存时必须以 UTF-8 进行编码（建议无 BOM 头），并且以 .rs 作为扩展名。此外请务必暂时忘记其他语言中学到的某些经验，特别是那些像 Java 那样的语言中的经验。

Rust允许我们使用汉字作为标识符了，不过您必须选择 Nightly 版本的 Rust 进行开发。并需要在开头写上：

```
#![feature(non_ascii_idents)]
```

我们的第一个程序是这样的：

```
#![feature(non_ascii_idents)]

#[derive(Debug)]
struct 联系人 {
    姓名: String,
    电话: String,
}

impl 联系人 {
    fn 构建新的联系人<T: Into<String>>(新的人姓名: T, 新的人电话: T) -> Self {
        联系人 {
            姓名: 新的人姓名.into(),
            电话: 新的人电话.into(),
        }
    }
}

fn main() {
    let mut 联系人列表: Vec<联系人> = Vec::new();
    联系人列表.push(联系人::构建新的联系人("刘祺", "156********"));
    println!("{:?}", 联系人列表[0]);
}
```

不过对于任何一个有经验的开发者来说，使用汉字作为标识符都是非常不舒服的行为。因为您必须要在中文和英文的输入法之间来回切换，而且有的时候编译器会仅仅是因为使用了汉字就进行报错。而使用英语字母则不会出现这个问题。这里笔者举这个例子只是为了告诉您 Rust 是可以把汉字当做标识符的，而不是鼓励您这样做。与之相反，任何一个成熟的程序员都应该对这种做法嗤之以鼻。此外把汉语拼音当做标识符仍然是不妥的。因为在我们的语言里面，存在着同音异意的词汇。这很容易引起误解。对于标识符的选择来说，如果您的母语本来就是属于类似英语的语言，譬如说西班牙语。那么您去掉几个重音符号也许勉强还能接受。不过如果您的母语是像中文、日语这样存在大量汉字的语言。那么，建议您还是老老实实地把标识符写成英语。当我们修改之后程序就变成了：

```
#[derive(Debug)]
struct Contact {
    name: String,
    phone: String,
}

impl Contact {
    fn new<T: Into<String>>(new_name: T, new_phone: T) -> Self {
        Contact {
            name: new_name.into(),
            phone: new_phone.into(),
        }
    }
}

fn main() {
    let mut contacts_list: Vec<Contact> = Vec::new();
    contacts_list.push(Contact::new("刘祺", "156********"));
    println!("{:?}", contacts_list[0]);
}
```

现在这个程序就舒服多了。我们来讲解一些这个程序都干了什么。以及怎么干的。当然了鉴于本教程的读者都应该是有经验的程序员。笔者并不会巨细靡遗地讲解什么是主函数，主函数是程序的入口之类的话题。我们会发现 `fn` 实际上是一个关键字来表示函数（在 C# 之类的语言里面叫做方法）那么 `fn main()` 是程序的主函数。也就是程序的入口。我们从这里看起。第一句是： `let mut contacts_list: Vec<Contact> = Vec::new();` 这句话定义了一个 `Vec<Contact>` 类型的 `contacts_list` 变量。在 Rust 中这叫做绑定，和定义变量的含义并不完全相同，不过目的是一样的。`Vec<Contact>` 是一个带有泛型的类型。它的含义相当于每个元素都是 `Contact` 类型的栈，不过这个东西实际上叫动态数组。它包括 `push`、`pop` 等一系列方法。第二句是 `contacts_list.push(Contact::new("刘祺", "156********"));` 这是为 `contacts_list` 数组增加（`push`）了一个元素。这个元素是 `Contact::new("刘祺", "156********")` 这个方法的返回值。具体这个方法是什么我们一会儿再说。最后一句是 `println!("{:?}", contacts_list[0]);` 它的含义是以 debug 的方式输出 `contacts_list` 数组中下标为0的元素。 `{:?}` 的含义是以 debug 方式的意思。 想要使用 `{:?}` 必须要明确输出的内容已经实现了一个叫做 `Debug` 的特征。在程序的最开始我们写了这样几行代码：

```
#[derive(Debug)]
struct Contact {
    name: String,
    phone: String,
}
```

第一句就是为下面紧挨着的构造类型（或枚举类型）实现 `Debug` 特征的意思。下面构建了一个构造类型。相当于 C++，Java 和 C# 中类中的数据的部分。下面的 `impl` 则是相当于类的方法，在 Rust 中叫做方法语法。不过 Rust 中为了让您能够好好写程序把数据和方法分开处理了。您不能认为方法语法和构造类型就等于类。这是很不一样的东西。因为在 Rust 中，数据没有继承。而对于方法的继承也要通过特征来实现。这样做有一个好处就是避免了多继承中的菱形继承。而且还能把数据和方法分开管理。方法语法中的函数（方法）默认是私有的。不过我们现在没有分文件，也就没有必要对其进行公开（`pub`）。这个函数实现的功能是返回一个自身的类型，来构建一个新的 `Contact` 类型的值。`Self` 的意思就是自身的类型，这里代指 `Contact`。在函数末尾的 `->` 用来标注返回值的类型。函数中实际上只有一个表达式：

```
Contact {
    name: new_name.into(),
    phone: new_phone.into(),
}
```

注意这一部分是一条表达式，而不是一条语句。在 Rust 中函数的最后一个表达式的计算结果代表返回值。如果您给它加上分号改成语句是不可以的。此外我们还没有讲 `T: Into<String>` 是什么东西。`T` 这里是一个泛型。它又一个约束条件，就是实现了 `Into<String>` 的类型。凡是实现了 `Into<String>` 特征的类型都可以通过值在后面接一个 `.into()` 来转换成一个 `String` 类型的值。用双引号包裹的值的类型是 `&str` 而不是 `String`。所以这里需要这个泛型来简化编程。我们为了让程序变得美一点儿可以把它改写成：

```
use std::fmt;

struct Contact {
    name: String,
    phone: String,
}

impl Contact {
    fn new<T: Into<String>>(new_name: T, new_phone: T) -> Self {
        Contact {
            name: new_name.into(),
            phone: new_phone.into(),
        }
    }
}

impl fmt::Debug for Contact {
    fn fmt(&self, f: &mut fmt::Formatter) -> Result<(), fmt::Error> {
        write!(f, "联系人: {}\n电话: {}", self.name, self.phone)
    }
}

fn main() {
    let mut contacts_list: Vec<Contact> = Vec::new();
    contacts_list.push(Contact::new("刘祺", "156********"));
    println!("{:?}", contacts_list[0]);
}
```

这里我们发现多了两个东西，一个是 `use std::fmt;` 这条语句的意思是使用一个模块。类似于 C++ 里面的名字空间。这个名字空间实际上来自于标准库。标准库中的一大部分内容实际上都会被默认引入，不过还是有一些内容需要我们手动的去引入。另一个大家伙就是：

```
impl fmt::Debug for Contact {
    fn fmt(&self, f: &mut fmt::Formatter) -> Result<(), fmt::Error> {
        write!(f, "联系人: {}\n电话: {}", self.name, self.phone)
    }
}
```

这个东西就是特征（`trait`）的实现。有一个叫做 `Debug` 的特征已经在标准库里面写好了，我们这里只是把它实现一下。这类似于 C# 里面的接口。`impl fmt::Debug for Contact` 的意思是为 `Contact` 类型实现 `Debug` 特征。里面有一个 `fmt` 函数（方法）：

```
fn fmt(&self, f: &mut fmt::Formatter) -> Result<(), fmt::Error> {
    write!(f, "联系人: {}\n电话: {}", self.name, self.phone)
}
```

这个函数的细节在标准库里面有明确的定义，我们能改的只有 `write!(f, "联系人: {}\n电话: {}", self.name, self.phone)` 这条表达式，而且这条表达式实际上还是个宏。在 Rust 中，宏长得非常像函数，不过宏会在宏名后面加一个叹号，当然也有比较特殊的 `try!` 宏写成一个问号。这些细节我们以后再讲。不过如果我们瞎改的话，实际上我们就得不到 `Result<(), fmt::Error>` 的返回值了。说到底我们只能改  `"联系人: {}\n电话: {}", self.name, self.phone` 这一小部分。大致和 C 语言的情况类似。不过 Rust 中不区分类型的用 `{}` 表示格式占位。

那么接下来为了方便联系人列表的增删改查我们最好还是为其建立一个数据结构。

```
use std::collections::HashMap;

struct ContactsData {
    list: Vec<Contact>,
    index: HashMap<String, usize>,
}
```

注意这里我们使用了 HashMap 这个数据类型以至于我们需要通过 use std::collections::HashMap; 将其引入。它是标准库里面的一种集合类型和我们之前见过的 Vec 一样，不过 Vec 是自动被引入的。在 Rust 语言中构造类型的名称一般采用一种改良的驼峰式写法，即首字母需要大写的驼峰式写法。如果想要写出优美的语义化程序，请您保持这种写法。当您见到一个有着良好规范的 Rust 程序这种被改良的驼峰式写法往往表示这一个独立的事物——一个构造类型、一个枚举类型或者一个枚举类型中的某种枚举情况。因为在 Rust 语言中枚举的含义包含了共用体的含义。所以这种首字母大写的驼峰写法是恰当的。有一些程序员可能更习惯采用首字母小写的驼峰写法，这样在 Rust 语言中引起一种混乱——当一个构造类型只有一个单词的时候，那么我们就很难区分它到底是不是 Rust 中提供的基础类型了。而且会导致编译器发出一个 warning。在您编写 Rust 程序的一般情况下您无需理会 warning，不过当您重构或优化代码时这些信息就起到了重要的作用。这和某些必须要调整 warning 的语言是不同的。在 Rust 中 warning 表示一些不痛的问题，换句话说它并不会出现运行时报错这种难以预料的灾难。不过如果不修复它则可能给生成代码的质量以及后期维护带来本不必要的开销。

这里我们也涉及了哈希表（HashMap）这种类型，如果您是一个成熟的程序员一定了解类似的数据结构。简单的说它就是一个键对应着一个值。它的类型中仍然包括泛型 HashMap<键的类型, 值的类型> 不过哈希表类型在 Rust 中有一个特殊的要求，就是其键的类型必须的可哈希的。具体在 Rust 中表现为类型实现了 Hash 特征（trait）的类型。而且 Rust 还额外需要您键的类型是可比较的也就是实现了 PartialEq 和 Eq 的特征。


我们首先要实现的功能就是联系人的增加和查询，以及整个联系人列表的新建。初步的可以把程序写成：

```
use std::fmt;
use std::collections::HashMap;

#[derive(Clone)]
struct Contact {
    name: String,
    phone: String,
}

impl Contact {
    fn new<T: Into<String>>(new_name: T, new_phone: T) -> Self {
        Contact {
            name: new_name.into(),
            phone: new_phone.into(),
        }
    }
}

impl fmt::Debug for Contact {
    fn fmt(&self, f: &mut fmt::Formatter) -> Result<(), fmt::Error> {
        write!(f, "联系人: {}\n电话: {}", self.name, self.phone)
    }
}

#[derive(Clone)]
struct ContactsData {
    list: Vec<Contact>,
    index: HashMap<String, usize>,
}

impl ContactsData {
    fn new() -> Self {
        ContactsData {
            list: Vec::new(),
            index: HashMap::new(),
        }
    }

	// 这里有一个可变引用，可以把它简单理解为给引用加上了可变性
    fn add<T: Into<String>>(&mut self, new_name: T, new_phone: T) {
        let name = new_name.into();
        self.list.push(Contact::new(name.clone(), new_phone.into()));
        if self.index.contains_key(&name) {
            println!("已有同名联系人");
        } else {
            self.index.insert(name.clone(), self.list.len() - 1);
        }
    }

    fn query<T: Into<String>>(&self, name: T) -> Option<Contact> {
        if let Some(query_index) = self.index.get(&name.into()) {
            Some(self.list[*query_index].clone())
        } else {
            None
        }
    }
    
    fn print_query<T: Into<String>>(&self, name: T) {
        let _name = name.into();
        match self.query(_name.clone()) {
            Some(query_result) => println!("{:?}", query_result),
            _ =>  println!("没有找到联系人\"{}\"", _name),
        }
    }
}

fn main() {
    let mut contacts = ContactsData::new();
    contacts.add("刘祺", "156********");
    contacts.add("张三", "130********");
    contacts.print_query("刘祺");
}
```

我们最先注意到底就是我们都使用 #[derive(Clone)] 为两个函数自动实现了一个新的特征 Clone。这个特征和 Rust 中的所有权系统有关。对于初学者来说，Rust 语言中的所有权系统简直就是最臭名昭著的东西。以至于很多没有学习过 Rust 语言的人都知道它的污名。实际上它是一个好用不好学的概念。一旦学会就会非常好用。这里我们采用一种简单途径来学习所有权系统——即笔者先不介绍所有权系统的概念，而是先讲解一些使用上的经验和投机取巧的办法。来让大家体会一下所有权，等到我们已经能够熟练的使用这种编程套路之后，再来系统的讲解它的概念。所有权系统是为了更科学的维护和管理内存而提出的。它的主要思想就是不让一个变量到处乱用，造成内存的难以维护。在 Rust 中我们一个变量如果直接扔给一个函数，那么这个变量就整体被这个函数拿走了（函数获得了变量的所有权）。也就是说您相当于把变量送给了一个函数，这样这个变量就再也不是您的了，您不能把它在送给另一个函数。这是一直常见的错误叫做“use of moved value”。在 Rust 中这是被禁止的。那么当您想给一个函数传递一个值的时候，最好就是您不要主动把变量送给函数啦，最好是让函数来借。这就产生了借用和引用。当然主要就表现为引用。当函数的某一个参数前出现了 & 符号，那么就说明这个参数函数要管别人去借。俗话说“好借好还，再借不难”。也就是说函数并不持有这个变量的所有权，只是借一下。用完了以后这个参数会还回去。那么这个参数当然就可以随便借给多个函数啦。不过有的时候借东西也会对东西本事造成改变。譬如说您和别人借钱肯定要如数奉还，不过有的时候我们还有利息的问题。（笑）那么函数也是这样，这就产生了可变引用。也就是对变量产生了改变的情况。不过还有的时候如果用引用实在是不方便，或者借出去的东西不用还，譬如说生活中我们经常会借纸巾。那么这种东西是不用还的，不过您是想保留这个变量，那么就产生了克隆（clone）。克隆是指实现了 Clone 特征的类型调用 clone 函数的过程。这里需要特别注意，虽然克隆的特征和函数调用的拼写是一样的。不过它们的大小写是不一致的。而 Rust 又是区分大小写的语言。这里就用到了我们之前说的技巧，所以我们可以判断出开头大写的是特征，完全小写的是函数。因为特征是一个独立的项，所以用开头大写的驼峰式写法，而函数是用全小写的蛇形写法。对于一个引用值的类型也是引用。所以有的时候您需要通过 * 来解引用。

除此之外我们还遇到了 Option 类型的数据。这是一个有意思的类型，在 Rust 中会考虑值为空的情况。所以就有了 Option 类型。它是 Rust 中的枚举，不过它相当于别的语言里面的共用体和枚举的组合形式。它的值是 Some(值) 或 None。前者表示存在一个值，后者表示值为空。这里我们用了两种方法来匹配 Option 类型。比较常规的是用 match，譬如说这样写：

```
match self.query(_name.clone()) {
    Some(query_result) => println!("{:?}", query_result),
    _ =>  println!("没有找到联系人\"{}\"", _name),
}
```

Rust 的编程哲学是要穷举所有情况。用 _ 来表示未考虑的其它情况。对于枚举类型您也可以穷举它的所有情况啦。截止到笔者写作的时候，Rust 对数值类型进行穷举上还存在着一定的缺陷。所以建议您总是要写 _ 来表示未明确考虑的情况。您可以参考官方 GitHub 的讨论页面中的 issue #12483 和 issue #30578。类似的讨论还有 issue #45590。

另外一种就是比较简明，而且是比较推荐的高手做法。即使用 if let 的语法糖：

```
if let Some(query_index) = self.index.get(&name.into()) {
    Some(self.list[*query_index].clone())
} else {
   None
}
```

它实际上就是 match 的意思，不过它允许不写 else。就显得更为清爽了。这是 Rust 高手更喜欢的做法。
