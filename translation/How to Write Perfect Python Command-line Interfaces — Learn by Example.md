# How to Write Perfect Python Command-line Interfaces — Learn by Example

# 如何编写完美的 Python 命令行界面—示例教学

This article will show you how to make perfect **Python command line interfaces**, to improve your **team productivity** and **comfort**.

本文会向你展示如何编写完美的 Python 命令行界面，以期提升你所在团队的工作效率和舒适度。

------

As Python developers, we always use and write **command-line interfaces**. On my Data Science projects, for example, I run several **scripts** from command-line to train my models and to compute the accuracy of my algorithms.

作为 Python 开发者，我们经常使用和编写**命令行界面**。比如，在我的数据科学项目中，我使用命令行运行一些**脚本**来训练模型并计算算法的精确度。

This is why a good way to improve your productivity is to make your scripts as **handy** **and** **straightforward** as possible, especially when you are several developers working on the same project.

这就是为什么一个提高工作效率的好方法就是让你的脚本尽可能地方便和直接，尤其当一个项目中有多个开发者协同工作时。

In order to achieve that, I advise you to respect 4 guidelines:

1. You should provide **default arguments values** when possible
2. All **error cases** should be handled (ex: a missing argument, a wrong type, a file not found)
3. All arguments and options have to be **documented**
4. A **progress bar** should be printed for not instantaneous tasks

为了实现这一点，我建议你遵从以下 4 条准则：

1. 你应尽可能地提供**默认参数值**
2. 你应解决所有的**错误情况**（比如：缺少参数，类型错误，文件未找到）
3. 你应将所有的参数和选项**记录在文档中**
4. 你应该为非即时任务打印**进度条**

------

### Let’s take a simple example

# 简单示例

Let’s try to apply these rules to a concrete simple example: a script to encrypt and decrypt messages using [**Caesar cipher**](https://en.wikipedia.org/wiki/Caesar_cipher).

让我们将上述准则应用到一个简单的实例：一个使用[**Caesar cipher**](https://en.wikipedia.org/wiki/Caesar_cipher)对信息进行加密和解密的脚本。

Imagine that you have an already written `encrypt` function (implemented as below), and you want to create a simple script which allows to encrypt and decrypt messages. We want to let the user choose the mode between *encryption* (by default) and *decryption*, and choose the *key* (1 by default) with command line arguments.

假设你有一个已经编写好的`encrypt`函数（实现如下），你想创建一个实现加密和解密信息的简单脚本。我们想允许用户选择 *加密* 模式（默认）还是 *解密* 模式，同时使用命令行参数选择 *key*（默认为 1）。

  The first thing our script needs to do is to get the values of **command line arguments**. And when I google *“python command line arguments”,* [the first result I get](https://www.pythonforbeginners.com/system/python-sys-argv) is about `sys.argv`. So let’s try to use this method…

我们的脚本需要做的第一件事是获取**命令行参数**的值。当我使用谷歌搜索“ python 命令行参数”时，[我得到的第一个结果](https://www.pythonforbeginners.com/system/python-sys-argv)关于` sys.argv `。那么就让我们试着用这个方法...

#### The “beginners” method

#### “初学者”方法

`sys.argv` is a list containing all the arguments typed by the user when running your script (including the script name itself).

` sys.argv ` 是一个 list ，存储了运行脚本时用户输入的所有参数（包括脚本本身的名称）。

For example, if I type:

举个例子，如果我输入：

```
> python caesar_script.py --key 23 --decrypt my secret message
pb vhfuhw phvvdjh
```

the list contains:

该列表包含：

```
['caesar_script.py', '--key', '23', '--decrypt', 'my', 'secret', 'message'] 
```

So we would loop on this arguments list, looking for a `'--key'` (or `'-k'` ) to know the key value, and looking for a `'--decrypt'` to use the decryption mode (actually by simply using the opposite of the key as the key).

所以，我们可以在该参数列表中循环查找 `'--key'`  （或者 `'-k'` ）来获得 key 值，查找` '--decrypt' `来使用解密模式（其实就是用键的反义词作为键）。

Our script would finally look like this piece of code:

This script more or less respects the recommendations stated above:

1. There are a **default** *key* value and a default mode
2. Basic **error cases** are handled (no input text provided or unknown arguments)
3. A succinct **documentation** is printed in these error cases, and when calling the script with no argument:

我们的脚本可能最终会如下面这段代码：

这个脚本或多或少地遵从了上面所述的建议：

1. 有一个 **默认** *key* 值和一个默认的模式
2. 可以处理基本的 **错误情况** （没有提供输入文本或者未知参数）
3. 在这些错误情况下，会打印一个简洁的**文档**，并且在不带参数调用脚本时：

```
> python caesar_script_using_sys_argv.py
Usage: python caesar.py [ --key <key> ] [ --encrypt|decrypt ] <text>
```

However, this version of the Caesar script is quite **long** (39 lines, which doesn’t even include the logic of the encryption itself) and **ugly**.

然而，这个版本的 Caesar 脚本的非常**长**（ 39 行，甚至不包括加密本身的逻辑）并且很**丑**。

There has to be a better way to parse command line arguments…

必须有更好的方法来解析命令行参数…

#### What about argparse?

#### 那么 argparse 呢？

`argparse` is the Python standard library module for **parsing command-line arguments**.

` argparse `是 Python 标准库模块，用于**解析命令行参数**。

Let us see how would our Caesar script look like using `argparse` :

让我们看看使用` argparse ` 的 Caesar 脚本：

This would respect our guidelines, and provide a more precise documentation and a more interactive error handling than the handmade script above:

这将遵从我们的准则，并提供一个更精确的文件和一个比上面手工脚本互动性更强的错误处理方式：

```
> python caesar_script_using_argparse.py --encode My message

usage: caesar_script_using_argparse.py [-h] [-e | -d] [-k KEY] [text [text ...]]
caesar_script_using_argparse.py: error: unrecognized arguments: --encode
> python caesar_script_using_argparse.py --help
                                                                                                                      
usage: caesar_script_using_argparse.py [-h] [-e | -d] [-k KEY] [text [text ...]]
positional arguments:
  text
optional arguments:
  -h, --help         show this help message and exit
  -e, --encrypt
  -d, --decrypt
  -k KEY, --key KEY
```

However, regarding the code, I find that — this is a bit subjective — the first lines of my function (from line 7 to line 13), where the arguments are defined, are not very elegant: it is **too verbose** and *programmatic* whereas it could be done in a more compact and *declarative* way.

虽然这有一点主观，但是关于代码，我发现函数的前几行（从第 7 行到 13 行)，也就是定义参数的地方，写得不是很优雅：它太**冗长**且 *程序化*，可以用更紧凑和 *声明性* 的方式来完成。

#### Do better with click!

#### 使用 click 做得更好！

We’re in luck: there is a Python library which offers the same features as `argparse` (and more), with a **prettier code style**: its name is `**click**`.

我们很幸运：有一个 Python 库提供了与` argparse `（以及更多）相同的特性，具有**更漂亮的代码风格**：它的名称是`**click**`。

Here is the third version of our Caesar script, using `click`:

Notice that the arguments and options are now declared in **decorators** which make them directly accessible as parameters of my function.

这是我们使用了` click ` 的第三版 Caesar 脚本:

注意，现在参数和选项是在**decorator **中声明，这使得它们可以作为函数的参数直接访问。

Let me clarify some subtleties in the above code:

- The `nargs` parameter for a script argument specifies the number of successive words expected for this argument (with “a quoted string like this” counting as 1 word). The default value is 1. Here, `nargs=-1` allow providing any number of words.
- The notation `--encrypt/--decrypt` allow to define **mutually exclusive options** (like with the `add_mutually_exclusive_group` function from `argparse`) which will result in a boolean parameter.
- The `click.echo` is a small utility provided by the library, which does the same as a `print` but which is compatible with Python 2 and 3, and has some extra features (colors handling, etc.).

让我阐明一下上面代码中的一些细微之处：

- 脚本参数的` nargs `参数指定该参数预期的连续单词数（“像这样的带引号的字符串”计算为 1 个单词）。默认值是 1 。这里，` nargs=-1 `允许提供任意数量的单词。
- 符号` -encrypt/- decrypt `允许定义**互斥选项**（类似于` argparse `中的` add_mutually_exclusive_group `函数），它将生成一个布尔参数。
-  `click.echo` 是库提供的一个实用小程序，它与` print `功能相同，但兼容 Python 2 和 Python 3，并具有一些额外的特性（颜色处理等）。

#### Adding some confidentiality

#### 添加一些保密性质

Our script arguments are supposed to be top secret messages that will be encrypted. Isn’t it ironic to ask the user to type his plain texts directly in his terminal, leaving them in his commands history?

我们的脚本参数应该是被加密的绝密信息。要求用户直接在终端中输入纯文本，并将其留在命令历史记录中，这难道不具有讽刺意味吗？

A solution to do it in a more secure way is to use a **hidden prompt**. Or we could read the text from an **input** **file**, which would be much more practical for long texts. Or why not let the choice to the user?

一种更安全的方法是使用**隐藏提示符**。或者我们可以从**输入文本**读取文本，这对于长文本来说更加实用。或者为什么不让用户自己选择呢？

And let’s do the same for the output: the user could ever save it into a file, or print it in the terminal. This leads us to this last improved version of our Caesar script:

让我们对输出做同样的处理:用户可以将它保存到一个文件中，或者在终端中打印它。这就引出了凯撒脚本的最后一个改进版本：

What is new in this version?

这个版本中有哪些更新？

- First, notice that I added a `help` parameter to each argument or option. Since the script becomes a little bit more complex, it allows to add some details about its behavior to the **documentation**, which now looks like this:
- 首先，请注意我为每个参数或选项添加了一个` help `参数。由于脚本变得有点复杂，它允许向**文档**添加一些关于其操作的细节，目前看起来是这样的：

```
> python caesar_script_v2.py --help
Usage: caesar_script_v2.py [OPTIONS]
Options:
  --input_file FILENAME          File in which there is the text you want to encrypt/decrypt. If not provided, a prompt will allow you to type the input text.
  --output_file FILENAME         File in which the encrypted/decrypted text will be written. If not provided, the output text will just be printed.
  -d, --decrypt / -e, --encrypt  Whether you want to encrypt the input text or decrypt it.
  -k, --key INTEGER              The numeric key to use for the caesar encryption / decryption.
  --help                         Show this message and exit.
```

- We have two new parameters, `input_file`, and `output_file`, of type `click.File`. The library manages to open the files in the correct mode before entering into the function and handle the errors that can happen. For instance:
- 我们有两个新参数，` input_file `和` output_file `，类型为` click.File `。库在进入函数之前以正确的模式打开文件，并处理可能发生的错误。例如：

```
> python caesar_script_v2.py --decrypt --input_file wrong_file.txt
Usage: caesar_script_v2.py [OPTIONS]
Error: Invalid value for "--input_file": Could not open file: wrong_file.txt: No such file or directory
```

- As explained in the `help` text, if the `input_file` is not provided, we use `click.prompt` to allow the user to directly type his text in a prompt, which will be hidden for the encryption mode. This will look like this:
- 正如在` help `文本中解释的那样，如果没有提供` input_file `，我们将使用` click.prompt `允许用户直接在提示符中输入文本，该提示符将在加密模式下隐藏。它看起来是这样的：

```
> python caesar_script_v2.py --encrypt --key 2
Enter a text: **************
yyy.ukectc.eqo
```

#### Let’s break the cipher!

#### 让我们破译密码吧!

You are now a hacker: you want to decrypt a secret text encrypted with Caesar cipher, but *you don’t know the key*.

你现在是一个黑客：您想解密一个用 Caesar 密码加密的秘密文本，但是 *你不知道密钥* 。

A simple strategy could be to call our decryption function 25 times, with all the possible keys, and to read all the resulting texts, looking for one which makes sense.

一个简单的策略是使用所有可能的密钥调用我们的解密函数 25 次，并读取所有的结果文本，寻找一个有意义的文本。

But since you’re smart and lazy, you would prefer to automate the process. A way to select the most likely original text among all of these 25 texts, is to count the numbers of real English words in all these texts. Let’s do this using the [*PyEnchant*](https://github.com/rfk/pyenchant) module:

但既然你聪明又懒惰，你更愿意让这个过程自动化。一种在这 25 篇文章中选择最可能是原文的方法，就是计算所有这些文章中真正的英语单词的数量。让我们使用 [*PyEnchant*](https://github.com/rfk/pyenchant) 模块：

This works like a charm, but if you remember well, there is one rule of a good command line interface which is not respected:

这就像魔力一般，但如果你记得很清楚，有一个不被遵从的完美命令行界面准则：

> \4. A **progress bar** should be printed for not instantaneous tasks

With the example text of 10⁴ words that I used, the script takes about 5 seconds to output the decrypted text. This is quite normal, considering that it has to check for 25 values of the key, for 10⁴ words, if they belong to the English dictionary.

对于我使用的包含 10⁴ 个单词的示例文本，这个脚本需要大约 5 秒输出解密文本。考虑到10⁴个单词中，如果它们属于英语词典，脚本必须检查 25 个关键值，这（所需 5 秒时间）是很正常的。

Imagine that you want to decrypt a text containing 10⁵ words, it would take **50 seconds** before printing any results, which could be **very frustrating** for a user.

想象一下你想解密的文本包含 10⁵ 个单词，在输出任何结果之前需要 **50秒**，这可能会使用户感到**十分沮丧**。

That is why I recommend printing **progress bars** for this kind of tasks, especially since it is *so* **easy to implement**.

这就是为什么我建议为这类任务打印**进度条**，尤其是因为它非常**容易实现**。

Here is the same script printing a progress bar:

下面是打印进度条的脚本：

Do you see any difference? It is not so easy to spot because the difference consists of 4 letters: [**TQDM**](https://pypi.org/project/tqdm/).

你觉得有什么不同吗？这并不容易发现，因为该区别由 4 个字母组成：[**TQDM**](https://pypi.org/project/tqdm/)。

This is the name of a Python library and this is the name of its unique class, with which you **wrap any iterable** to print the corresponding progression.

这是 Python 库的名称，这是它唯一的类名称，您可以使用它**包装任何迭代器**来打印相应的进程。

```
for key in tqdm(range(26)):
```

And this results in a beautiful progress bar. Personally, I still find it too good to be true.

这就产生了一个漂亮的进度条。就我个人而言，我仍然觉得它优秀得令人难以置信。

By the way, `click` also provides a similar utility to print progress bars (`click.progress_bar`), but I find the appearance a bit less readable and the code to write is less minimalist.

顺便说一下，` click `还提供了一个类似的工具来打印进度条（` click.progress_bar `），但我发现它的外观可读性较差，要编写的代码也没那么简单。

------

To conclude, I hope that I have convinced you to make more efforts in **improving the developer experience** of your scripts.

最后，我希望我已经说服你在**提升开发人员阅读脚本体验**方面做出更多的努力。

If some of you have other recommendations or tips that you use in your own scripts, do not hesitate to **share them in the comments**!

如果有在你自己脚本中使用的其他建议或技巧，请不要犹豫，在**评论中分享它们**！

Want more articles like this one? Don’t forget to click the *follow* button!

想要更多这样的文章吗？别忘了点击 *关注* 按钮！
