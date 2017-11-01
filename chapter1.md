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

这里还有一个用于 Linux 的版本，由 GitHub 上的 @Linger2 提供。在这里笔者为他做出的无私贡献表示深深地感谢。获取地址是：

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
