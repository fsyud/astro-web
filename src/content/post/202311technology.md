---
title: "November Technology Outlook"
description: "Java has Java native interfaces. For JavaScript, this lower-level language is WebAssembly."
publishDate: "29 Nov  2023"
tags: ["webAssembly", "dart"]
---

## 总结

- WebAssembly

- Dart

### WebAssembly

自从 Web 成为一个不仅可用于文档以及应用的平台以来，一些最先进的应用将网络浏览器发挥到了极限。通过与低阶语言进行交互来提升性能，已出现在许多较高级别的语言中，从而达到“更接近现实”的方法。例如，Java 具有 Java 原生接口。对于 JavaScript，这种较低级别的语言是 WebAssembly。

可进一步参考：<https://wasmbyexample.dev/>

汇编语言
您是否使用过汇编语言进行编程？在计算机编程中，汇编语言（通常简称为汇编语言，通常简称为 ASM 或 asm）是任何低级别编程语言，其中的指令与架构的机器代码指令之间有非常强的对应关系。

例如，以 Intel® 64 和 IA-32 架构 (PDF) 为例，MUL 指令（用于多乘）会对第一个运算数（目标运算数）和第二个运算数（源运算数）进行无符号乘法，并将结果存储在目标运算数中。非常简单地，目标运算数是位于寄存器 AX 中的隐式运算数，源运算数位于通用寄存器（例如 CX）中。结果会再次存储在寄存器 AX 中。请参考以下 x86 代码示例：

```
mov ax, 5  ; Set the value of register AX to 5.
mov cx, 10 ; Set the value of register CX to 10.
mul cx     ; Multiply the value of register AX (5)
           ; and the value of register CX (10), and
           ; store the result in register AX.

```

相比之下，如果任务的目标是将 5 和 10 相乘，那么您可能会在 JavaScript 中编写类似如下的代码：

```
const factor1 = 5;
const factor2 = 10;
const result = factor1 * factor2;
```

采用汇编方法的优势在于，此类低级别和机器优化的代码比高级别、人工优化的代码更高效。上述情况无关紧要，但您可以想象，对于更复杂的操作，差异可能会很大。

顾名思义，x86 代码依赖于 x86 架构。如果有一种方法的汇编代码不依赖于特定架构，但会继承汇编的性能优势，会怎么样？

#### asm.js

在编写没有架构依赖项的汇编代码时，第一步是 asm.js。asm.js 是 JavaScript 的严格子集，可用作编译器的低级高效目标语言。该子语言有效地描述了 C 或 C++ 等不安全语言（如 C 或 C++）的沙盒虚拟机。静态验证和动态验证相结合，使 JavaScript 引擎能够针对有效的 asm.js 代码采用预先 (AOT) 优化编译策略。使用手动内存管理（例如 C）编写的静态类型语言编写的代码由源到源编译器（例如 early Emscripten（基于 LLVM））翻译过来。

通过将语言功能限制为可进行 AOT 的操作，提升了性能。Firefox 22 是首个支持 asm.js 的浏览器，并以 OdinMonkey 的名义发布。Chrome 已在版本 61 中添加了 asm.js 支持。虽然 asm.js 仍可在浏览器中使用，但它已被 WebAssembly 取代。之所以使用 asm.js，是出于不支持不支持 WebAssembly 的浏览器的目的。

#### WebAssembly

WebAssembly 是一种类似汇编的低级别语言，具有紧凑的二进制格式，能够以接近原生的性能运行，并提供 C/C++ 和 Rust 等语言以及许多语言，以便它们能够在 Web 上运行。我们正在对 Java、Kotlin 和 Dart 等内存管理语言的支持正在开发中，预计很快就会推出。WebAssembly 旨在与 JavaScript 一起运行，使这两者能够协同工作。

除了浏览器之外，WebAssembly 程序还可以在其他运行时中运行，这得益于 WASI（WebAssembly 系统接口，即 WebAssembly 的模块化系统接口）。WASI 旨在实现跨操作系统移植，旨在保障安全并能够在沙盒化环境中运行。

WebAssembly 代码（二进制代码，即字节码）旨在在便携式虚拟机堆栈机器 (VM) 上运行。字节码的解析和执行速度比 JavaScript 快，并且具有紧凑的代码表示法。

指令的概念性执行是通过传统程序计数器继续进行。在实践中，大多数 Wasm 引擎将 Wasm 字节码编译为机器码，然后执行该字节码。说明分为两类：

- 构成控件结构并将其参数值从堆栈中弹出的控制指令，可以更改程序计数器，以及将结果值推送到堆栈。

- 简单指令：从堆栈中弹出其参数值，对值应用运算符，然后将结果值推送到堆栈，然后是程序计数器的隐式前进。

回到前面的示例，以下 WebAssembly 代码等同于本文开头的 x86 代码：

```
i32.const 5  ; Push the integer value 5 onto the stack.
i32.const 10 ; Push the integer value 10 onto the stack.
i32.mul      ; Pop the two most recent items on the stack,
             ; multiply them, and push the result onto the stack.
```

虽然 asm.js 完全在软件中实现，也就是说，其代码可以在任何 JavaScript 引擎（即使未经优化）中运行，但 WebAssembly 需要所有浏览器供应商商定的新功能。WebAssembly 于 2015 年发布，并于 2017 年 3 月首次发布，并于 2019 年 12 月 5 日成为 W3C 建议。W3C 遵循该标准，并受到所有主要浏览器供应商和其他相关方的贡献。自 2017 年以来，浏览器已成为普遍支持。

WebAssembly 有两种表示法：文本和二进制。您在上方看到的是文本表示。

#### 以文本形式表示

文本表示基于 S 表达式，通常使用文件扩展名 .wat（适用于 Web 汇编 ext 格式）。如果您确实想这么做，可以手写起来。参考上面的乘法示例，通过不再硬编码因子使其更有用，您也许可以理解以下代码：

```
(module
  (func $mul (param $factor1 i32) (param $factor2 i32) (result i32)
    local.get $factor1
    local.get $factor2
    i32.mul)
  (export "mul" (func $mul))
)
```

#### 二进制表示法

使用文件扩展名 .wasm 的二进制格式不适合人类消费，更不用说是人工创建。您可以使用 wat2wasm 之类的工具将上述代码转换为以下二进制表示形式。（注释通常不是二进制表示的一部分，而是由 wat2wasm 工具添加的，以便于我们理解。）

```
0000000: 0061 736d                             ; WASM_BINARY_MAGIC
0000004: 0100 0000                             ; WASM_BINARY_VERSION
; section "Type" (1)
0000008: 01                                    ; section code
0000009: 00                                    ; section size (guess)
000000a: 01                                    ; num types
; func type 0
000000b: 60                                    ; func
000000c: 02                                    ; num params
000000d: 7f                                    ; i32
000000e: 7f                                    ; i32
000000f: 01                                    ; num results
0000010: 7f                                    ; i32
0000009: 07                                    ; FIXUP section size
; section "Function" (3)
0000011: 03                                    ; section code
0000012: 00                                    ; section size (guess)
0000013: 01                                    ; num functions
0000014: 00                                    ; function 0 signature index
0000012: 02                                    ; FIXUP section size
; section "Export" (7)
0000015: 07                                    ; section code
0000016: 00                                    ; section size (guess)
0000017: 01                                    ; num exports
0000018: 03                                    ; string length
0000019: 6d75 6c                          mul  ; export name
000001c: 00                                    ; export kind
000001d: 00                                    ; export func index
0000016: 07                                    ; FIXUP section size
; section "Code" (10)
000001e: 0a                                    ; section code
000001f: 00                                    ; section size (guess)
0000020: 01                                    ; num functions
; function body 0
0000021: 00                                    ; func body size (guess)
0000022: 00                                    ; local decl count
0000023: 20                                    ; local.get
0000024: 00                                    ; local index
0000025: 20                                    ; local.get
0000026: 01                                    ; local index
0000027: 6c                                    ; i32.mul
0000028: 0b                                    ; end
0000021: 07                                    ; FIXUP func body size
000001f: 09                                    ; FIXUP section size
; section "name"
0000029: 00                                    ; section code
000002a: 00                                    ; section size (guess)
000002b: 04                                    ; string length
000002c: 6e61 6d65                       name  ; custom section name
0000030: 01                                    ; name subsection type
0000031: 00                                    ; subsection size (guess)
0000032: 01                                    ; num names
0000033: 00                                    ; elem index
0000034: 03                                    ; string length
0000035: 6d75 6c                          mul  ; elem name 0
0000031: 06                                    ; FIXUP subsection size
0000038: 02                                    ; local name type
0000039: 00                                    ; subsection size (guess)
000003a: 01                                    ; num functions
000003b: 00                                    ; function index
000003c: 02                                    ; num locals
000003d: 00                                    ; local index
000003e: 07                                    ; string length
000003f: 6661 6374 6f72 31            factor1  ; local name 0
0000046: 01                                    ; local index
0000047: 07                                    ; string length
0000048: 6661 6374 6f72 32            factor2  ; local name 1
0000039: 15                                    ; FIXUP subsection size
000002a: 24                                    ; FIXUP section size
```

#### 编译为 WebAssembly

如您所见，.wat 和 .wasm 都不是特别友好。这正是 Emscripten 等编译器的作用。它可让您从 C 和 C++ 等更高级别的语言进行编译。我们还提供适用于 Rust 等其他语言的其他编译器。请参考以下 C 代码：

```
#include <stdio.h>

int main() {
  printf("Hello World\n");
  return 0;
}
```

通常，您可以使用编译器 gcc 编译此 C 程序。

```
gcc hello.c -o hello
```

安装 Emscripten 后，您可以使用 emcc 命令和几乎相同的参数将其编译为 WebAssembly：

```
emcc hello.c -o hello.html
```

这将创建一个 hello.wasm 文件和 HTML 封装容器文件 hello.html。从网络服务器提供 hello.html 文件时，您将看到在开发者工具控制台中输出的 "Hello World"。

还有一种方法可以在不使用 HTML 封装容器的情况下编译为 WebAssembly：

```c
emcc hello.c -o hello.js
```

与之前一样，这将创建一个 hello.wasm 文件，但这次是创建一个 hello.js 文件，而不是 HTML 封装容器。如需进行测试，请使用 Node.js 等运行生成的 JavaScript 文件 hello.js：

```js
$ node hello.js
Hello World
```

### 了解详情

对 WebAssembly 的简要介绍只是冰山一角。如需详细了解 WebAssembly，请参阅关于 MDN 的 WebAssembly 文档以及 Emscripten 文档。事实上，使用 WebAssembly 可能会有点像如何绘制猫头鹰表情包，尤其是因为熟悉 HTML、CSS 和 JavaScript 的 Web 开发者并不一定精通 C 等语言进行编译。幸运的是，有一些渠道（例如 StackOverflow 的 webassembly 标签），如果您能问得好，专家通常非常乐意为您提供帮助。

- 注意 ：请继续阅读将 mkbitmap 编译为 WebAssembly 一文，其中介绍了如何将一个相当简单但又不过度复杂的 C 程序编译到 WebAssembly。本文以 mkbitmap 为例，介绍了如何在 JavaScript 中将 Wasm 程序用作库，以便将文件作为输入并输出图像。

参考 <https://web.dev/articles/what-is-webassembly?hl=zh-cn>

## Dart 概览

Dart 是一种针对客户端优化的语言，可在任何平台上开发快速的应用程序。其目标是为多平台开发提供最高效的编程语言，并为应用程序框架搭配了 灵活的运行时执行平台。

通常来说，编程语言会包含一些 技术壁垒，即语言在设计中的抉择决定了其功能和优势。 Dart 的语言设计针对客户端开发，它优先考虑多平台 (Web，移动端和桌面端) 上的开发 (亚秒级的状态热重载) 和高质量生产环境体验。

Dart 也是 Flutter 的基础。 Dart 作为 Flutter 应用程序的编程语言，为驱动应用运行提供了环境，同时 Dart 还支持许多核心的开发任务，例如格式化，分析和代码测试。

#### Dart 语言

Dart 语言是类型安全的；它使用静态类型检查来确保变量的值 始终 与变量的静态类型相匹配。这也叫健全类型。尽管类型是强制性的，但由于 Dart 支持类型推断，类型注释仍是可选的。 Dart 的类型系统也很灵活，允许结合使用 dynamic 类型与运行时检查，在测试开发期间，或是遇到需要特别指定为动态类型的代码时，这项特性很有帮助。

Dart 内置了 健全的空值安全，这意味着只有您声明值可以为空的情况下，值才可以为空；当 Dart 确定变量不可为空时，该变量 永远 不可为空。凭借健全的空值安全，Dart 可以通过静态代码分析在运行时保护您免受空值异常的影响。如果你在调试器中审查正在运行的代码，你会看到不可为空性仍在运行时被保留，所以是完全的空安全。

以下代码示例展示了 Dart 语言的一些功能，包括库、异步调用、可空和不可空的类型、箭头语法、生成器、流和 getter。

```dart
import 'dart:math' show Random;

void main() async {
  print('Compute π using the Monte Carlo method.');
  await for (final estimate in computePi().take(100)) {
    print('π ≅ $estimate');
  }
}

/// Generates a stream of increasingly accurate estimates of π.
Stream<double> computePi({int batch = 100000}) async* {
  var total = 0; // Inferred to be of type int
  var count = 0;
  while (true) {
    final points = generateRandom().take(batch);
    final inside = points.where((p) => p.isInsideUnitCircle);

    total += batch;
    count += inside.length;
    final ratio = count / total;

    // Area of a circle is A = π⋅r², therefore π = A/r².
    // So, when given random points with x ∈ <0,1>,
    // y ∈ <0,1>, the ratio of those inside a unit circle
    // should approach π / 4. Therefore, the value of π
    // should be:
    yield ratio * 4;
  }
}

Iterable<Point> generateRandom([int? seed]) sync* {
  final random = Random(seed);
  while (true) {
    yield Point(random.nextDouble(), random.nextDouble());
  }
}

class Point {
  final double x;
  final double y;

  const Point(this.x, this.y);

  bool get isInsideUnitCircle => x * x + y * y <= 1;
}
```

### Dart 库

- Dart 拥有 丰富的核心库，为许多日常编程任务提供了必要工具：

- 为每个 Dart 程序提供的内置类型，集合与其他核心功能 (dart:core)

- 更丰富的集合类型，诸如队列、链接列表、哈希图和二叉树 (dart:collection)

- 用于在不同的数据表示形式之间进行转换编码器和解码器，包括 JSON 和 UTF-8 (dart:convert)

- 数学常数和函数，以及随机数生成 (dart:math)

- 为非 Web 应用程序提供的文件、套接字、HTTP 和其他 I/O 支持 (dart:io)

- 异步编程支持，比如 Future 和 Stream 类 (dart:async)

- 能够有效处理固定大小的数据（例如，无符号的 8 字节整数）和 SIMD 数字类型的列表 (dart:typed_data)

- 用于提供 C 语言风格代码互通性支持的外部函数接口 (dart:ffi)

- 使用 isolates 的并发编程 — 这些独立的工作程序与线程相似但它们不共享内存并仅通过消息进行通信 (dart:isolate)

- 基于 Web 的应用程序中需要与浏览器和文档对象模型 (DOM) 交互的 HTML 元素和其他资源 (dart:html)

更多参考 <https://dart.cn/overview>
