# 第2章 从零开始创建 Rust 库

在[快速入门](https://3442853561.gitbooks.io/rustcasestudymanual/content/chapter1.html)中我们学习了 Rust 的基础语法，特别是对于那些本来是学习别的语言的读者来说我们搞清楚了诸多语法细节。而从这一章开始我们就要真正的学习 Rust 语言了。由于我们学习的过程是由简及难的，所以整个过程中代码进行了反复的改动和重构。为了使这部分更加清晰，笔者给各部分增加了小标题。

## 创建库以及最开始的工作

我们可以通过 Rust 强大的包管理器—— Cargo 来创建一个库。您只需要在终端输入以下命令。

```shell
cargo new --lib <库名称>
```

这里我们用来做示范的库名称叫做 ```my_crate``` ,您会发现它的名称是使用蛇底写法的。然而这并不是一个好的库名称，好的库名称尽可能在库名上体现出您库的功能。就我们的示例库来说这是一个用于科学计算的基础库，当然我们只做了非常简单的矩阵运算的部分。您可以通过以下命令进入库目录。

```shell
cd my_crate
```

您可以通过 ```cargo build``` 来编译这个项目。具体的内容我们在[上一章](https://3442853561.gitbooks.io/rustcasestudymanual/content/chapter1.html)已经讲解过了，这里不再赘余。

现在请打开项目根目录下的 ```src``` 文件夹，会发现里面有一个名为 ```lib.rs``` 的文件。这个文件中包含了一个测试，以至于您一开始就可以通过 ```cargo test``` 来测试它是否可以正常运转。

```rust
#[cfg(test)]
mod tests {
    #[test]
    fn it_works() {
        assert_eq!(2 + 2, 4);
    }
}
```

不过暂时这是和我们工作无关的东西，我们直接删掉它就好了。不过这提醒了我们开发 Rust 程序的时候请务必注意测试驱动开发（TTD）这件事儿。

## 创建 ```Matrix<T>``` 结构体

```rust
struct Matrix<T> {
    size: (usize, usize),
    data: Vec<T>,
    default: T,
}
```

当然为了我们后期方便，现在为其实现 ```Clone``` 这个特征：

```rust
#[derive(Clone)]
struct Matrix<T: Clone> {
    size: (usize, usize),
    data: Vec<T>,
    default: T,
}
```

并且实现 ```new``` 方法和 ```Debug``` 特征：

```rust
impl<T: Clone> Matrix<T> {
    // 这里的 new_height 和 new_width 的顺序是颠倒的
    // 理由是线性代数中习惯把纵向信息放在前面
    fn new(new_height: usize, new_width: usize, new_default: T) -> Self {
        Matrix {
            size: (new_width, new_height),
            data: vec![new_default.clone(); new_width * new_height],
            default: new_default,
        }
    }
}

use std::fmt;
impl<T: Clone + fmt::Debug> fmt::Debug for Matrix<T> {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        // 这里将 size 类型分解为宽和高
        let (width, height) = self.size;
        let mut debug_string = "".to_string();
        for h in 0..height {
            for w in 0..width {
                // format 宏的返回值总是 String 类型的
                debug_string += &*format!("{:?}, ", self.data[h*width+w]);
            }
            // 输入完一行之后需要增加一个换行符
            debug_string.push('\n');
        }  
        write!(f, "{}", debug_string)
    }
}
```

不要忘记测试驱动开发这件事儿，我们给这段程序写一个测试：

```rust
#[test]
fn test_new_matrix() {
    let my_matrix = Matrix::new(2, 5, 0);
    println!("{:?}", my_matrix);
}
```

在终端输入下面的命令就可以看测试结果了。如果您只写 ```cargo test``` 实际上是看不到输出的。

```shell
cargo test -- --nocapture
```

这里我们总是在 ```debug``` 里面写似乎不是一个好的解决方案，所以我们为其实现 ```ToString``` 特征：

```rust
use std::fmt;
impl<T: Clone + fmt::Debug> fmt::Debug for Matrix<T> {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        write!(f, "{}", self.to_string())
    }
}
```

阅读下面的代码您会发现我们不仅仅是把代码转移了位置，并且增加了在矩阵过长时显示其转置矩阵的功能：

```rust
impl<T> ToString for Matrix<T> 
where T: Clone + fmt::Debug{
    fn to_string(&self) -> String {
        let mut result = "".to_string();
        let (width, height) = self.size;

        if height == 1 {
            result += "[ ";
            for w in 0..width {
                result += &*format!("{:?} ", self.data[w]);
            }
            result += "]";
            return result;
        }

        // 这一部分是获取输出的最大对齐
        let mut max_len_value = 0;
        for x in &self.data {
            let len = format!("{:?}", x).len();
            if len > max_len_value {
                max_len_value = len;
            }
        }
    
        if height > width {
            result += "┌ ";
            for _ in 0..(max_len_value + 1)*height {
                result += " ";
            }
            result += "┐T\n";
            for w in 0..width {
                result += "│ ";
                for h in 0..height {
                    result += &*format!("{:?}", self.data[w*height+h]);
                    for _ in format!("{:?}",self.data[w*height+h]).len()..max_len_value + 1{
                        result += " ";
                    }
                }
                result += "│\n";
            }
            result += "└ ";
            for _ in 0..(max_len_value + 1)*height {
                result += " ";
            }
            result += "┘";
        } else {
            result += "┌ ";
            for _ in 0..(max_len_value + 1)*width {
                result += " ";
            }
            result += "┐\n";
            for h in 0..height {
                result += "│ ";
                for w in 0..width {
                    result += &*format!("{:?}", self.data[h*width+w]);
                    for _ in format!("{:?}",self.data[h*width+w]).len()..max_len_value + 1{
                        result += " ";
                    }
                }
                result += "│\n";
            }
            result += "└ ";
            for _ in 0..(max_len_value + 1)*width {
                result += " ";
            }
            result += "┘";
        }
        result
    }
}
```

增加完这部分代码后我们发现原本的 ```lib.rs``` 文件已经非常长了，这对于我们日后维护是一个极大的障碍。所以我们现在就对其进行分文件处理，在 ```lib.rs``` 只保留：

```rust
pub mod matrix;
```

目前的其他内容全选，并复制到名为 ```matrix.rs``` 的文件内，并对需要对外部暴露的自定义方法和类型进行 ```pub```:

```rust
#[derive(Clone)]
pub struct Matrix<T: Clone> {
    size: (usize, usize),
    data: Vec<T>,
    default: T,
}

impl<T: Clone> Matrix<T> {
    // 这里的 new_height 和 new_width 的顺序是颠倒的
    // 理由是线性代数中习惯把纵向信息放在前面
    pub fn new(new_height: usize, new_width: usize, new_default: T) -> Self {
        Matrix {
            size: (new_width, new_height),
            data: vec![new_default.clone(); new_width * new_height],
            default: new_default,
        }
    }
}

impl<T> ToString for Matrix<T> 
where T: Clone + fmt::Debug{
    fn to_string(&self) -> String {
        let mut result = "".to_string();
        let (width, height) = self.size;

        if height == 1 {
            result += "[ ";
            for w in 0..width {
                result += &*format!("{:?} ", self.data[w]);
            }
            result += "]";
            return result;
        }

        // 这一部分是获取输出的最大对齐
        let mut max_len_value = 0;
        for x in &self.data {
            let len = format!("{:?}", x).len();
            if len > max_len_value {
                max_len_value = len;
            }
        }
    
        if height > width {
            result += "┌ ";
            for _ in 0..(max_len_value + 1)*height {
                result += " ";
            }
            result += "┐T\n";
            for w in 0..width {
                result += "│ ";
                for h in 0..height {
                    result += &*format!("{:?}", self.data[w*height+h]);
                    for _ in format!("{:?}",self.data[w*height+h]).len()..max_len_value + 1{
                        result += " ";
                    }
                }
                result += "│\n";
            }
            result += "└ ";
            for _ in 0..(max_len_value + 1)*height {
                result += " ";
            }
            result += "┘";
        } else {
            result += "┌ ";
            for _ in 0..(max_len_value + 1)*width {
                result += " ";
            }
            result += "┐\n";
            for h in 0..height {
                result += "│ ";
                for w in 0..width {
                    result += &*format!("{:?}", self.data[h*width+w]);
                    for _ in format!("{:?}",self.data[h*width+w]).len()..max_len_value + 1{
                        result += " ";
                    }
                }
                result += "│\n";
            }
            result += "└ ";
            for _ in 0..(max_len_value + 1)*width {
                result += " ";
            }
            result += "┘";
        }
        result
    }
}

use std::fmt;
impl<T: Clone + fmt::Debug> fmt::Debug for Matrix<T> {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        write!(f, "{}", self.to_string())
    }
}

#[test]
fn test_new_matrix() {
    let my_matrix = Matrix::new(5, 2, 0);
    println!("{:?}", my_matrix);
    let another_matrix = Matrix::new(2, 5, 1);
    println!("{:?}", another_matrix);
    let yet_another_matrix = Matrix::new(1, 10, 2);
    println!("{:?}", yet_another_matrix);
}
```



