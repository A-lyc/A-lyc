---
uuid: 1bfa2d75-e612-3688-5607-a57892363ceb
title: sass的使用方法-scss制作组件库
date: 2020-03-30 13:12:58
tags:
category:
---
  <div class="ibm-col-6-4">
        <h1 id="ibm-pagetitle-h1" class="ibm-h1">利用 Sass 改善 CSS 预处理</h1>
        <p class="dw-article-subhead">整合编程灵活性与特性</p>
        <p>如果您打算开发样式表，Syntactically Awesome Stylesheets (Sass) 能够帮助您显著改善整体体验。Sass 是一种样式表语言和层叠式样式表 (CSS) 预处理程序，它为 CSS
            开发添加了传统编程属性。Sass 引进了多种编程特性，包括变量、样式的逻辑嵌套、混入 (mixin)、参数和继承。在为网站开发样式表时，Sass 能够将您的 Sass 转化为标准 CSS 标记，从而显著简化您的工作。
        </p>
        <p>在这篇文章中，您将学习 Sass 的重要准则。本文提供了几个实例，展示了如何使用 Sass 提高 CSS 开发的速度和效率。</p>
        <p>Sassy CSS (SCSS) 是 CSS3 的超集；任何 CSS3 代码均为有效的 SCSS 代码。本文中使用的代码示例均为 SCSS。</p>
        <h2 id="N10049" class="ibm-h2">为何使用 Sass</h2>
        <div class="dw-article-sidebar ibm-background-cool-white-20">
            <h5 id="sasssyntax">Sass 语法</h5>
            <ul class="ibm-bullet-list">
                <li>SCSS 是最常用的语法，它是 CSS 语法的超集。</li>
                <li>缩进式语法 .sass 较为陈旧，使用行缩进来指定代码块。</li>
            </ul>
        </div>
        <p>Sass 是一种可靠的、可配置的、功能丰富的 CSS 开发解决方案。它使用了动态构造方法，而非旧式的静态 CSS 规则。如果您想采用日常开发中使用的编程风格，同时访问编程语言结构，那么 Sass
            将是您最理想的选择。Sass 出色的灵活性和功能完备的解决方案将为您带来惊喜。</p>
        <p>CSS 预处理程序对于所有 Web 开发环境都有着重要的意义。它们将：</p>
        <ul class="ibm-bullet-list">
            <li>缩短您的开发时间。</li>
            <li>为 CSS 开发应用 “不要重复自己” (DRY) 的准则。</li>
            <li>使您的代码更加清晰易读。</li>
        </ul>
        <p>CSS 预处理程序技术多种多样。本文采用 Sass 的原因在于，它高度灵活，并且具有许多特性。</p>
        <h2 id="N10065" class="ibm-h2">安装 Sass</h2>
        <p>Sass 主要使用 Ruby 来实现，但也存在其他实现。您要做的第一步是通过 Ruby 组件安装 Sass。
        </p>
        <ol>
            <li>如果您的系统中尚未安装 Ruby，请下载和安装 Ruby：<ul class="ibm-bullet-list">
                    <li>Windows 用户：<a href="">RubyInstaller for Windows</a></li>
                    <li>Mac OS X：如果您的操作系统中已经安装了 Ruby。</li>
                    <li>Linux：通过您喜爱的程序包管理器安装 Ruby。</li>
                </ul>
            </li>
            <li>通过以下命令安装 Sass Ruby gem：
                <code>gem install sass</code></li>
        </ol>
        <h2 id="N1007D" class="ibm-h2">准备好 Sass 环境</h2>
        <p>使用您喜爱的文本编辑器，创建一个扩展名为 .scss 的文件。文本编辑器对 Sass 的支持级别各有不同。请参见 <a href="#artrelatedtopics">参考资料</a>部分，了解具有不同级别 Sass
            语法突出显示支持的文本编辑器的列表。</p>
        <p>
            将清单 1 中的代码复制粘贴到新的 .scss 文件中。</p>
        <h5 id="listing1" class="ibm-h5">清单 1. Sass 代码样例</h5><span class="dw-code-nohighlight">
            <div class="ibm-syntax-container">
                <div>
                    <div id="highlighter_750455" class="syntaxhighlighter  htmlscript">
                        <table border="0" cellpadding="0" cellspacing="0" role="none">
                            <tbody>
                                <tr>
                                    <td class="gutter">
                                        <div class="line number1 index0 alt2">1</div>
                                        <div class="line number2 index1 alt1">2</div>
                                        <div class="line number3 index2 alt2">3</div>
                                        <div class="line number4 index3 alt1">4</div>
                                        <div class="line number5 index4 alt2">5</div>
                                        <div class="line number6 index5 alt1">6</div>
                                        <div class="line number7 index6 alt2">7</div>
                                    </td>
                                    <td class="code">
                                        <div class="container">
                                            <div class="line number1 index0 alt2"><code
                                                    class="htmlscript plain">#blueBar { </code></div>
                                            <div class="line number2 index1 alt1"><code
                                                    class="htmlscript plain">position: relative; </code></div>
                                            <div class="line number3 index2 alt2"><code
                                                    class="htmlscript plain">height:37px; </code></div>
                                            <div class="line number4 index3 alt1"><code class="htmlscript plain">left:0;
                                                </code></div>
                                            <div class="line number5 index4 alt2"><code
                                                    class="htmlscript plain">right:0; </code></div>
                                            <div class="line number6 index5 alt1"><code class="htmlscript plain">top:0;
                                                </code></div>
                                            <div class="line number7 index6 alt2"><code
                                                    class="htmlscript plain">}</code></div>
                                        </div>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </span>
        <p>
            Sass 语言是 CSS3 的超集。在 Sass 版本 3.2.1 中，所有有效的 CSS 也属于有效的 Sass。<a href="#listing1">清单 1</a>中的代码是有效的 CSS，因此也是有效的
            Sass 代码。无论如何，您都必须将清单 1 中的 Sass 转为 CSS。如果未进行该转化，Web 浏览器将无法正确解释您的样式表。您不必每次使用 <code>sass-convert</code>命令将 Sass
            转为 CSS，而是可以将 Saas 设置为在您每次保存时自动将文件转为 CSS。要使 Sass 自动监视您的文件，请在命令行应用程序中运行清单 2 所示的命令。</p>
        <h5 id="list2" class="ibm-h5">清单 2. 监视文件</h5><span class="dw-code-nohighlight">
            <div class="ibm-syntax-container">
                <div>
                    <div id="highlighter_105057" class="syntaxhighlighter  htmlscript">
                        <table border="0" cellpadding="0" cellspacing="0" role="none">
                            <tbody>
                                <tr>
                                    <td class="gutter">
                                        <div class="line number1 index0 alt2">1</div>
                                        <div class="line number2 index1 alt1">2</div>
                                    </td>
                                    <td class="code">
                                        <div class="container">
                                            <div class="line number1 index0 alt2"><code class="htmlscript plain">sass
                                                    --watch </code></div>
                                            <div class="line number2 index1 alt1"><code
                                                    class="htmlscript plain">style.scss:style.css</code></div>
                                        </div>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </span>
        <p>
            您还可以通过清单 3 中的命令，使用 Sass 监视整个目录。
        </p>
        <h5 id="list3" class="ibm-h5">清单 3. 监视目录</h5><span class="dw-code-nohighlight">
            <div class="ibm-syntax-container">
                <div>
                    <div id="highlighter_871181" class="syntaxhighlighter  htmlscript">
                        <table border="0" cellpadding="0" cellspacing="0" role="none">
                            <tbody>
                                <tr>
                                    <td class="gutter">
                                        <div class="line number1 index0 alt2">1</div>
                                        <div class="line number2 index1 alt1">2</div>
                                    </td>
                                    <td class="code">
                                        <div class="container">
                                            <div class="line number1 index0 alt2"><code class="htmlscript plain">sass
                                                    --watch </code></div>
                                            <div class="line number2 index1 alt1"><code
                                                    class="htmlscript plain">stylesheets/sass:stylesheets/compiled</code>
                                            </div>
                                        </div>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </span>
        <p>这样，您的框架就已经创建完成，每次保存 Saas 文件时，Sass 都会自动将您的代码转为有效的 CSS。现在可以开始使用该框架。</p>
        <h2 id="N100AB" class="ibm-h2">变量</h2>
        <p>CSS 中缺失的一项重要功能就是变量。样式表中往往存在大量重复。一种有用的选项就是一次性写入一个值，然后通过别名进行重用。举例来说，假设您有一个类似于清单 4 的样式表。</p>
        <h5 id="listing4" class="ibm-h5">清单 4. CSS 元素和颜色</h5><span class="dw-code-nohighlight">
            <div class="ibm-syntax-container">
                <div>
                    <div id="highlighter_223459" class="syntaxhighlighter  htmlscript">
                        <table border="0" cellpadding="0" cellspacing="0" role="none">
                            <tbody>
                                <tr>
                                    <td class="gutter">
                                        <div class="line number1 index0 alt2">1</div>
                                        <div class="line number2 index1 alt1">2</div>
                                        <div class="line number3 index2 alt2">3</div>
                                    </td>
                                    <td class="code">
                                        <div class="container">
                                            <div class="line number1 index0 alt2"><code
                                                    class="htmlscript plain">#someElement { color:#541B32; } </code>
                                            </div>
                                            <div class="line number2 index1 alt1"><code
                                                    class="htmlscript plain">#anotherElement { color:#541B32; } </code>
                                            </div>
                                            <div class="line number3 index2 alt2"><code
                                                    class="htmlscript plain">#yetAnotherElement { color:#541B32;
                                                    }</code></div>
                                        </div>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </span>
        <p>标准 CSS 要求您显式写入所有值，因此会导致 <a href="#listing4">清单 4</a>中的冗余内容。但在使用 Sass 时，您可以提高工作效率，如清单 5 所示。</p>
        <h5 id="list5" class="ibm-h5">清单 5. Sass 元素和颜色</h5><span class="dw-code-nohighlight">
            <div class="ibm-syntax-container">
                <div>
                    <div id="highlighter_893378" class="syntaxhighlighter  htmlscript">
                        <table border="0" cellpadding="0" cellspacing="0" role="none">
                            <tbody>
                                <tr>
                                    <td class="gutter">
                                        <div class="line number1 index0 alt2">1</div>
                                        <div class="line number2 index1 alt1">2</div>
                                        <div class="line number3 index2 alt2">3</div>
                                        <div class="line number4 index3 alt1">4</div>
                                    </td>
                                    <td class="code">
                                        <div class="container">
                                            <div class="line number1 index0 alt2"><code
                                                    class="htmlscript plain">$purplishColor:#541B32; </code></div>
                                            <div class="line number2 index1 alt1"><code
                                                    class="htmlscript plain">#someElement { color:$purplishColor; }
                                                </code></div>
                                            <div class="line number3 index2 alt2"><code
                                                    class="htmlscript plain">#anotherElement { color:$purplishColor; }
                                                </code></div>
                                            <div class="line number4 index3 alt1"><code
                                                    class="htmlscript plain">#yetAnotherElement { color:$purplishColor;
                                                    }</code></div>
                                        </div>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </span>
        <p>
            优势显而易见。现在，您可以在一个位置更改所有元素的颜色。在 <a href="#list5">清单 5</a>中，<code>$purplishColor</code>是一个变量，因此您可以随时在 Sass
            文件中对其进行更改。Sass 变量没有类型，您可以为同一个变量指定字符串、整数值，甚至是颜色值。</p>
        <h2 id="N100CE" class="ibm-h2">模块</h2>
        <p>您可以轻松将 CSS 代码拆分为多个模块，再通过 Sass 引擎将它们聚集在一起。可以使用 <code>@import</code>指令来导入模块，如清单 6
            所示。该指令获取一个文件名、一个超链接或其他任意路径。CSS 和 SCSS 文件均载入并合并为同一个文档。</p>
        <h5 id="listing6" class="ibm-h5">清单 6. Sass 导入模块</h5><span class="dw-code-nohighlight">
            <div class="ibm-syntax-container">
                <div>
                    <div id="highlighter_927709" class="syntaxhighlighter  htmlscript">
                        <table border="0" cellpadding="0" cellspacing="0" role="none">
                            <tbody>
                                <tr>
                                    <td class="gutter">
                                        <div class="line number1 index0 alt2">1</div>
                                        <div class="line number2 index1 alt1">2</div>
                                    </td>
                                    <td class="code">
                                        <div class="container">
                                            <div class="line number1 index0 alt2"><code class="htmlscript plain">@import
                                                    "colors.scss"</code></div>
                                            <div class="line number2 index1 alt1"><code class="htmlscript plain">@import
                                                    "links.scss"</code></div>
                                        </div>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </span>
        <p>在 CSS 和 Sass 中拆分代码的方式截然不同。将 CSS 代码拆分为较小的样式表时，每个样式表都会获取一个不同的 HTTP 加载请求。Sass
            <code>@import</code>指令直接获取模块代码，因此能保证您始终使用一个 CSS 文件。</p>
        <h2 id="N100E3" class="ibm-h2">字符串和插值</h2>
        <p>Sass 支持字符串联接和变量插值。变量不再仅仅限于用作属性值，您可以直接将变量值插入属性和选取器名称，如清单 7 所示。</p>
        <h5 id="listing7" class="ibm-h5">清单 7. Sass 字符串和变量操作</h5><span class="dw-code-nohighlight">
            <div class="ibm-syntax-container">
                <div>
                    <div id="highlighter_652010" class="syntaxhighlighter  htmlscript">
                        <table border="0" cellpadding="0" cellspacing="0" role="none">
                            <tbody>
                                <tr>
                                    <td class="gutter">
                                        <div class="line number1 index0 alt2">1</div>
                                        <div class="line number2 index1 alt1">2</div>
                                        <div class="line number3 index2 alt2">3</div>
                                        <div class="line number4 index3 alt1">4</div>
                                        <div class="line number5 index4 alt2">5</div>
                                        <div class="line number6 index5 alt1">6</div>
                                        <div class="line number7 index6 alt2">7</div>
                                        <div class="line number8 index7 alt1">8</div>
                                    </td>
                                    <td class="code">
                                        <div class="container">
                                            <div class="line number1 index0 alt2"><code
                                                    class="htmlscript plain">$image_dir:'images/web/'; </code></div>
                                            <div class="line number2 index1 alt1"><code
                                                    class="htmlscript plain">$page:10; </code></div>
                                            <div class="line number3 index2 alt2">&nbsp;</div>
                                            <div class="line number4 index3 alt1"><code class="htmlscript plain">.button
                                                </code></div>
                                            <div class="line number5 index4 alt2"><code class="htmlscript plain">{
                                                </code></div>
                                            <div class="line number6 index5 alt1"><code
                                                    class="htmlscript plain">background-image: url( $image_dir +
                                                    'image.gif' ); </code></div>
                                            <div class="line number7 index6 alt2"><code class="htmlscript plain">:before
                                                    { content:"Page #{ $page }"; } </code></div>
                                            <div class="line number8 index7 alt1"><code
                                                    class="htmlscript plain">}</code></div>
                                        </div>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </span>
        <p><a href="#listing7">清单 7</a>中的代码将转换为清单 8 中的 CSS 代码。</p>
        <h5 id="listing8" class="ibm-h5">清单 8. CSS 字符串和变量操作</h5><span class="dw-code-nohighlight">
            <div class="ibm-syntax-container">
                <div>
                    <div id="highlighter_835621" class="syntaxhighlighter  htmlscript">
                        <table border="0" cellpadding="0" cellspacing="0" role="none">
                            <tbody>
                                <tr>
                                    <td class="gutter">
                                        <div class="line number1 index0 alt2">1</div>
                                        <div class="line number2 index1 alt1">2</div>
                                        <div class="line number3 index2 alt2">3</div>
                                        <div class="line number4 index3 alt1">4</div>
                                    </td>
                                    <td class="code">
                                        <div class="container">
                                            <div class="line number1 index0 alt2"><code class="htmlscript plain">.button
                                                    { </code></div>
                                            <div class="line number2 index1 alt1"><code
                                                    class="htmlscript plain">background-image:
                                                    url("images/web/image.gif"); } </code></div>
                                            <div class="line number3 index2 alt2"><code
                                                    class="htmlscript plain">.button:before { </code></div>
                                            <div class="line number4 index3 alt1"><code
                                                    class="htmlscript plain">content:"Page 10"; }</code></div>
                                        </div>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </span>
        <h2 id="N100FC" class="ibm-h2">数学运算</h2>
        <p>
            Sass 支持使用标准数学运算符执行数字运算，如清单 9 所示。您可以对变量值执行简单的数学运算。
        </p>
        <h5 id="listing9" class="ibm-h5">清单 9. 对数字执行的 Sass 数学运算</h5><span class="dw-code-nohighlight">
            <div class="ibm-syntax-container">
                <div>
                    <div id="highlighter_893754" class="syntaxhighlighter  htmlscript">
                        <table border="0" cellpadding="0" cellspacing="0" role="none">
                            <tbody>
                                <tr>
                                    <td class="gutter">
                                        <div class="line number1 index0 alt2">1</div>
                                        <div class="line number2 index1 alt1">2</div>
                                        <div class="line number3 index2 alt2">3</div>
                                        <div class="line number4 index3 alt1">4</div>
                                    </td>
                                    <td class="code">
                                        <div class="container">
                                            <div class="line number1 index0 alt2"><code class="htmlscript plain">.block
                                                    { </code></div>
                                            <div class="line number2 index1 alt1"><code
                                                    class="htmlscript plain">$block_width:500px; </code></div>
                                            <div class="line number3 index2 alt2"><code
                                                    class="htmlscript plain">width:$block_width - ( 10px * 2 ) - ( 1px *
                                                    2 ); </code></div>
                                            <div class="line number4 index3 alt1"><code
                                                    class="htmlscript plain">}</code></div>
                                        </div>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </span>
        <p><a href="#listing9">清单 9</a>中的代码将被转换为清单 10 中的 CSS 代码。</p>
        <h5 id="listing10" class="ibm-h5">清单 10. 对数字执行的 CSS 数学运算</h5><span class="dw-code-nohighlight">
            <div class="ibm-syntax-container">
                <div>
                    <div id="highlighter_234899" class="syntaxhighlighter  htmlscript">
                        <table border="0" cellpadding="0" cellspacing="0" role="none">
                            <tbody>
                                <tr>
                                    <td class="gutter">
                                        <div class="line number1 index0 alt2">1</div>
                                        <div class="line number2 index1 alt1">2</div>
                                    </td>
                                    <td class="code">
                                        <div class="container">
                                            <div class="line number1 index0 alt2"><code class="htmlscript plain">.block
                                                    { </code></div>
                                            <div class="line number2 index1 alt1"><code
                                                    class="htmlscript plain">width:478px; }</code></div>
                                        </div>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </span>
        <p>此外还支持对颜色执行数学运算，如清单 11 所示。</p>
        <h5 id="listing11" class="ibm-h5">清单 11. 对颜色执行的 Sass 数学运算</h5><span class="dw-code-nohighlight">
            <div class="ibm-syntax-container">
                <div>
                    <div id="highlighter_697238" class="syntaxhighlighter  htmlscript">
                        <table border="0" cellpadding="0" cellspacing="0" role="none">
                            <tbody>
                                <tr>
                                    <td class="gutter">
                                        <div class="line number1 index0 alt2">1</div>
                                        <div class="line number2 index1 alt1">2</div>
                                        <div class="line number3 index2 alt2">3</div>
                                        <div class="line number4 index3 alt1">4</div>
                                        <div class="line number5 index4 alt2">5</div>
                                    </td>
                                    <td class="code">
                                        <div class="container">
                                            <div class="line number1 index0 alt2"><code class="htmlscript plain">.block
                                                    { </code></div>
                                            <div class="line number2 index1 alt1"><code
                                                    class="htmlscript plain">$color:#010203; </code></div>
                                            <div class="line number3 index2 alt2"><code
                                                    class="htmlscript plain">color:$color; </code></div>
                                            <div class="line number4 index3 alt1"><code
                                                    class="htmlscript plain">border-color:$color - #010101; </code>
                                            </div>
                                            <div class="line number5 index4 alt2"><code
                                                    class="htmlscript plain">}</code></div>
                                        </div>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </span>
        <p><a href="#listing11">清单 11</a>中的代码将被转换为清单 12 中的 CSS 代码。</p>
        <h5 id="listing12" class="ibm-h5">清单 12. 对颜色执行的 CSS 数学运算</h5><span class="dw-code-nohighlight">
            <div class="ibm-syntax-container">
                <div>
                    <div id="highlighter_798765" class="syntaxhighlighter  htmlscript">
                        <table border="0" cellpadding="0" cellspacing="0" role="none">
                            <tbody>
                                <tr>
                                    <td class="gutter">
                                        <div class="line number1 index0 alt2">1</div>
                                        <div class="line number2 index1 alt1">2</div>
                                        <div class="line number3 index2 alt2">3</div>
                                    </td>
                                    <td class="code">
                                        <div class="container">
                                            <div class="line number1 index0 alt2"><code class="htmlscript plain">.block
                                                    { </code></div>
                                            <div class="line number2 index1 alt1"><code
                                                    class="htmlscript plain">color:#010203; </code></div>
                                            <div class="line number3 index2 alt2"><code
                                                    class="htmlscript plain">border-color:#000102; }</code></div>
                                        </div>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </span>
        <h2 id="N1012A" class="ibm-h2">嵌套选取器和属性</h2>
        <p>CSS 中最出色的特性之一就是嵌套选取器（应用在一个选取器内使用另一个选取器的样式）。 要在 CSS 中嵌套选取器，必须繁琐地列出您定义的各子选取器的父选取器。为了在 Saas 中简化此过程，您可以嵌套选取器，如清单
            13 所示。</p>
        <h5 id="listing13" class="ibm-h5">清单 13. Sass 嵌套选取器</h5><span class="dw-code-nohighlight">
            <div class="ibm-syntax-container">
                <div>
                    <div id="highlighter_114739" class="syntaxhighlighter  htmlscript">
                        <table border="0" cellpadding="0" cellspacing="0" role="none">
                            <tbody>
                                <tr>
                                    <td class="gutter">
                                        <div class="line number1 index0 alt2">1</div>
                                        <div class="line number2 index1 alt1">2</div>
                                        <div class="line number3 index2 alt2">3</div>
                                        <div class="line number4 index3 alt1">4</div>
                                    </td>
                                    <td class="code">
                                        <div class="container">
                                            <div class="line number1 index0 alt2"><code class="htmlscript plain">#some {
                                                </code></div>
                                            <div class="line number2 index1 alt1"><code
                                                    class="htmlscript plain">border:1px solid red; </code></div>
                                            <div class="line number3 index2 alt2"><code class="htmlscript plain">.some {
                                                    background: white; } </code></div>
                                            <div class="line number4 index3 alt1"><code
                                                    class="htmlscript plain">}</code></div>
                                        </div>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </span>
        <p><a href="#listing13">清单 13</a>中的代码将被转换为清单 14 中的 CSS 代码。</p>
        <h5 id="listing14" class="ibm-h5">清单 14. CSS 嵌套选取器</h5><span class="dw-code-nohighlight">
            <div class="ibm-syntax-container">
                <div>
                    <div id="highlighter_217966" class="syntaxhighlighter  htmlscript">
                        <table border="0" cellpadding="0" cellspacing="0" role="none">
                            <tbody>
                                <tr>
                                    <td class="gutter">
                                        <div class="line number1 index0 alt2">1</div>
                                        <div class="line number2 index1 alt1">2</div>
                                    </td>
                                    <td class="code">
                                        <div class="container">
                                            <div class="line number1 index0 alt2"><code class="htmlscript plain">#some {
                                                    border:1px solid red; } </code></div>
                                            <div class="line number2 index1 alt1"><code class="htmlscript plain">#some
                                                    .some { background: white; }</code></div>
                                        </div>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </span>
        <h2 id="N10143" class="ibm-h2">控制指令</h2>
        <p>Sass 控制指令为 CSS 代码提供了流和逻辑。本节介绍的基本控制指令包括 @if、@for 和 @each。</p>
        <h3 id="N10149" class="ibm-h3"><code>@if</code></h3>
        <p>Sass 支持基本的 if/else 函数，可将其编译为 CSS。举例来说，在清单 15 中，您希望将所有链接的颜色设置为黑色，底色已经为黑色的情况除外。如果底色已经为黑色，则将链接颜色设置为白色。</p>
        <h5 id="listing15" class="ibm-h5">清单 15. Sass <code>@if</code>示例</h5><span class="dw-code-nohighlight">
            <div class="ibm-syntax-container">
                <div>
                    <div id="highlighter_54157" class="syntaxhighlighter  htmlscript">
                        <table border="0" cellpadding="0" cellspacing="0" role="none">
                            <tbody>
                                <tr>
                                    <td class="gutter">
                                        <div class="line number1 index0 alt2">1</div>
                                        <div class="line number2 index1 alt1">2</div>
                                        <div class="line number3 index2 alt2">3</div>
                                        <div class="line number4 index3 alt1">4</div>
                                        <div class="line number5 index4 alt2">5</div>
                                        <div class="line number6 index5 alt1">6</div>
                                        <div class="line number7 index6 alt2">7</div>
                                        <div class="line number8 index7 alt1">8</div>
                                        <div class="line number9 index8 alt2">9</div>
                                    </td>
                                    <td class="code">
                                        <div class="container">
                                            <div class="line number1 index0 alt2"><code class="htmlscript plain">$color:
                                                    black; </code></div>
                                            <div class="line number2 index1 alt1">&nbsp;</div>
                                            <div class="line number3 index2 alt2"><code class="htmlscript plain">.link {
                                                </code></div>
                                            <div class="line number4 index3 alt1"><code class="htmlscript plain">@if
                                                    $color == black { </code></div>
                                            <div class="line number5 index4 alt2"><code class="htmlscript plain">color:
                                                    white; </code></div>
                                            <div class="line number6 index5 alt1"><code class="htmlscript plain">} @else
                                                    { </code></div>
                                            <div class="line number7 index6 alt2"><code class="htmlscript plain">color:
                                                    black; </code></div>
                                            <div class="line number8 index7 alt1"><code class="htmlscript plain">}
                                                </code></div>
                                            <div class="line number9 index8 alt2"><code
                                                    class="htmlscript plain">}</code></div>
                                        </div>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </span>
        <p><a href="#listing15">清单 15</a>中的代码将被转换为清单 16 中的 CSS 代码。</p>
        <h5 id="listing16" class="ibm-h5">清单 16. CSS <code>@if</code>示例</h5><span class="dw-code-nohighlight">
            <div class="ibm-syntax-container">
                <div>
                    <div id="highlighter_296767" class="syntaxhighlighter  htmlscript">
                        <table border="0" cellpadding="0" cellspacing="0" role="none">
                            <tbody>
                                <tr>
                                    <td class="gutter">
                                        <div class="line number1 index0 alt2">1</div>
                                        <div class="line number2 index1 alt1">2</div>
                                    </td>
                                    <td class="code">
                                        <div class="container">
                                            <div class="line number1 index0 alt2"><code class="htmlscript plain">.link {
                                                </code></div>
                                            <div class="line number2 index1 alt1"><code class="htmlscript plain">color:
                                                    white; }</code></div>
                                        </div>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </span>
        <p>此处的 <code>@if</code>的用途与在其他编程语言中相同。<code>@if</code>语句后面可接 <code>@else if</code>语句和一个 <code>@else</code>语句。如果
            <code>@if</code>语句失败，则依次尝试 <code>@else if</code>语句，直至其中一条语句获得成功或到达 <code>@else</code>为止。</p>
        <h3 id="N10180" class="ibm-h3"><code>@for</code></h3>
        <p><code>@for</code>指令重复地输出一组样式。对于每次重复，都会使用一个计数器变量来调整输出，如清单 17 所示。</p>
        <h5 id="listing17" class="ibm-h5">清单 17. Sass <code>@for</code>示例</h5><span class="dw-code-nohighlight">
            <div class="ibm-syntax-container">
                <div>
                    <div id="highlighter_258372" class="syntaxhighlighter  htmlscript">
                        <table border="0" cellpadding="0" cellspacing="0" role="none">
                            <tbody>
                                <tr>
                                    <td class="gutter">
                                        <div class="line number1 index0 alt2">1</div>
                                        <div class="line number2 index1 alt1">2</div>
                                        <div class="line number3 index2 alt2">3</div>
                                    </td>
                                    <td class="code">
                                        <div class="container">
                                            <div class="line number1 index0 alt2"><code class="htmlscript plain">@for $i
                                                    from 1 through 5 { </code></div>
                                            <div class="line number2 index1 alt1"><code
                                                    class="htmlscript plain">.button-#{$i} { width:1px * $i; } </code>
                                            </div>
                                            <div class="line number3 index2 alt2"><code
                                                    class="htmlscript plain">}</code></div>
                                        </div>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </span>
        <p><a href="#listing17">清单 17</a>中的代码将被转换为清单 18 中的 CSS 代码。</p>
        <h5 id="listing18" class="ibm-h5">清单 18. CSS <code>@for</code>示例</h5><span class="dw-code-nohighlight">
            <div class="ibm-syntax-container">
                <div>
                    <div id="highlighter_170301" class="syntaxhighlighter  htmlscript">
                        <table border="0" cellpadding="0" cellspacing="0" role="none">
                            <tbody>
                                <tr>
                                    <td class="gutter">
                                        <div class="line number1 index0 alt2">1</div>
                                        <div class="line number2 index1 alt1">2</div>
                                        <div class="line number3 index2 alt2">3</div>
                                        <div class="line number4 index3 alt1">4</div>
                                        <div class="line number5 index4 alt2">5</div>
                                        <div class="line number6 index5 alt1">6</div>
                                        <div class="line number7 index6 alt2">7</div>
                                        <div class="line number8 index7 alt1">8</div>
                                        <div class="line number9 index8 alt2">9</div>
                                        <div class="line number10 index9 alt1">10</div>
                                    </td>
                                    <td class="code">
                                        <div class="container">
                                            <div class="line number1 index0 alt2"><code
                                                    class="htmlscript plain">.button-1 { </code></div>
                                            <div class="line number2 index1 alt1"><code
                                                    class="htmlscript plain">width:1px; } </code></div>
                                            <div class="line number3 index2 alt2"><code
                                                    class="htmlscript plain">.button-2 { </code></div>
                                            <div class="line number4 index3 alt1"><code
                                                    class="htmlscript plain">width:2px; } </code></div>
                                            <div class="line number5 index4 alt2"><code
                                                    class="htmlscript plain">.button-3 { </code></div>
                                            <div class="line number6 index5 alt1"><code
                                                    class="htmlscript plain">width:3px; } </code></div>
                                            <div class="line number7 index6 alt2"><code
                                                    class="htmlscript plain">.button-4 { </code></div>
                                            <div class="line number8 index7 alt1"><code
                                                    class="htmlscript plain">width:4px; } </code></div>
                                            <div class="line number9 index8 alt2"><code
                                                    class="htmlscript plain">.button-5 { </code></div>
                                            <div class="line number10 index9 alt1"><code
                                                    class="htmlscript plain">width:5px; }</code></div>
                                        </div>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </span>
        <h3 id="N101A2" class="ibm-h3"><code>@each</code></h3>
        <p><code>@each</code>指令获取列表中的项目，并列出包含所列出的值的样式，如清单 19 所示。</p>
        <h5 id="listing19" class="ibm-h5">清单 19. Sass <code>@each</code>示例</h5><span class="dw-code-nohighlight">
            <div class="ibm-syntax-container">
                <div>
                    <div id="highlighter_571552" class="syntaxhighlighter  htmlscript">
                        <table border="0" cellpadding="0" cellspacing="0" role="none">
                            <tbody>
                                <tr>
                                    <td class="gutter">
                                        <div class="line number1 index0 alt2">1</div>
                                        <div class="line number2 index1 alt1">2</div>
                                        <div class="line number3 index2 alt2">3</div>
                                        <div class="line number4 index3 alt1">4</div>
                                        <div class="line number5 index4 alt2">5</div>
                                    </td>
                                    <td class="code">
                                        <div class="container">
                                            <div class="line number1 index0 alt2"><code class="htmlscript plain">@each
                                                    $company in IBM, Motorola, Google { </code></div>
                                            <div class="line number2 index1 alt1"><code
                                                    class="htmlscript plain">.#{$company}-icon { </code></div>
                                            <div class="line number3 index2 alt2"><code
                                                    class="htmlscript plain">background-image:
                                                    url('/images/#{$company}.jpg'); </code></div>
                                            <div class="line number4 index3 alt1"><code class="htmlscript plain">}
                                                </code></div>
                                            <div class="line number5 index4 alt2"><code
                                                    class="htmlscript plain">}</code></div>
                                        </div>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </span>
        <p><a href="#listing19">清单 19</a>中的代码将被转换为清单 20 中的 CSS 代码。</p>
        <h5 id="listing20" class="ibm-h5">清单 20. CSS <code>@each</code>示例</h5><span class="dw-code-nohighlight">
            <div class="ibm-syntax-container">
                <div>
                    <div id="highlighter_762907" class="syntaxhighlighter  htmlscript">
                        <table border="0" cellpadding="0" cellspacing="0" role="none">
                            <tbody>
                                <tr>
                                    <td class="gutter">
                                        <div class="line number1 index0 alt2">1</div>
                                        <div class="line number2 index1 alt1">2</div>
                                        <div class="line number3 index2 alt2">3</div>
                                        <div class="line number4 index3 alt1">4</div>
                                        <div class="line number5 index4 alt2">5</div>
                                        <div class="line number6 index5 alt1">6</div>
                                    </td>
                                    <td class="code">
                                        <div class="container">
                                            <div class="line number1 index0 alt2"><code
                                                    class="htmlscript plain">.IBM-icon { </code></div>
                                            <div class="line number2 index1 alt1"><code
                                                    class="htmlscript plain">background-image: url("/images/IBM.jpg"); }
                                                </code></div>
                                            <div class="line number3 index2 alt2"><code
                                                    class="htmlscript plain">.Motorola-icon { </code></div>
                                            <div class="line number4 index3 alt1"><code
                                                    class="htmlscript plain">background-image:
                                                    url("/images/Motorola.jpg"); } </code></div>
                                            <div class="line number5 index4 alt2"><code
                                                    class="htmlscript plain">.Google-icon { </code></div>
                                            <div class="line number6 index5 alt1"><code
                                                    class="htmlscript plain">background-image:
                                                    url("/images/Google.jpg"); }</code></div>
                                        </div>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </span>
        <h2 id="N101C4" class="ibm-h2">函数</h2>
        <p>
            您可以在 Sass 中应用函数。通过函数制定计划，重构和抽象构成成功技术基础的方法，以便在后续项目中重用和移植。</p>
        <p>Sass 提供了大量内置函数。例如，您可以使用清单 21 中所示的 <code>rgb()</code>和
            <code>darken()</code>来处理颜色。您可以更改色调、饱和度、亮度、透明度、流动性比例和其他许多方面。您还可以定义自定义函数，并在需要时重用它们。</p>
        <h5 id="listing21" class="ibm-h5">清单 21. Sass 函数</h5><span class="dw-code-nohighlight">
            <div class="ibm-syntax-container">
                <div>
                    <div id="highlighter_106659" class="syntaxhighlighter  htmlscript">
                        <table border="0" cellpadding="0" cellspacing="0" role="none">
                            <tbody>
                                <tr>
                                    <td class="gutter">
                                        <div class="line number1 index0 alt2">1</div>
                                        <div class="line number2 index1 alt1">2</div>
                                        <div class="line number3 index2 alt2">3</div>
                                        <div class="line number4 index3 alt1">4</div>
                                        <div class="line number5 index4 alt2">5</div>
                                        <div class="line number6 index5 alt1">6</div>
                                    </td>
                                    <td class="code">
                                        <div class="container">
                                            <div class="line number1 index0 alt2"><code
                                                    class="htmlscript plain">#someElement { </code></div>
                                            <div class="line number2 index1 alt1"><code class="htmlscript plain">color:
                                                    rgb(150, 50, 100); </code></div>
                                            <div class="line number3 index2 alt2"><code class="htmlscript plain">}
                                                </code></div>
                                            <div class="line number4 index3 alt1"><code
                                                    class="htmlscript plain">#someDarkYellowElement { </code></div>
                                            <div class="line number5 index4 alt2"><code class="htmlscript plain">color:
                                                    darken(yellow, 33%); </code></div>
                                            <div class="line number6 index5 alt1"><code
                                                    class="htmlscript plain">}</code></div>
                                        </div>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </span>
        <p><a href="#listing21">清单 21</a>中的代码将被转换为清单 22 中的 CSS 代码。</p>
        <h5 id="listing22" class="ibm-h5">清单 22. CSS 函数</h5><span class="dw-code-nohighlight">
            <div class="ibm-syntax-container">
                <div>
                    <div id="highlighter_9494" class="syntaxhighlighter  htmlscript">
                        <table border="0" cellpadding="0" cellspacing="0" role="none">
                            <tbody>
                                <tr>
                                    <td class="gutter">
                                        <div class="line number1 index0 alt2">1</div>
                                        <div class="line number2 index1 alt1">2</div>
                                        <div class="line number3 index2 alt2">3</div>
                                        <div class="line number4 index3 alt1">4</div>
                                    </td>
                                    <td class="code">
                                        <div class="container">
                                            <div class="line number1 index0 alt2"><code
                                                    class="htmlscript plain">#someElement { </code></div>
                                            <div class="line number2 index1 alt1"><code
                                                    class="htmlscript plain">color:#963264; } </code></div>
                                            <div class="line number3 index2 alt2"><code
                                                    class="htmlscript plain">#someDarkYellowElement { </code></div>
                                            <div class="line number4 index3 alt1"><code
                                                    class="htmlscript plain">color:#575700; }</code></div>
                                        </div>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </span>
        <p>Sass 包含数学、字符串、列表、自省等函数。关于完整的 Sass 函数列表，请参见 <a href="#artrelatedtopics">参考资料</a>部分。</p>
        <h2 id="N101EB" class="ibm-h2">混入 (Mixin)</h2>
        <p>可重用您的 CSS 代码切片的混入是使用 <code>@mixin</code>指令定义的，如清单 23
            所示。利用混入，您可以定义属性值对的模式，以便在其他规则集中重用这些模式。混入功能有助于简化样式表，提高其可读性。基本上来说，<code>@mixin</code>是一种用户定义的函数。混入同样可接受参数，也就是说，您可以使用几个混入轻松生成大量样式。
        </p>
        <h5 id="listing23" class="ibm-h5">清单 23. Sass 混入</h5><span class="dw-code-nohighlight">
            <div class="ibm-syntax-container">
                <div>
                    <div id="highlighter_595552" class="syntaxhighlighter  htmlscript">
                        <table border="0" cellpadding="0" cellspacing="0" role="none">
                            <tbody>
                                <tr>
                                    <td class="gutter">
                                        <div class="line number1 index0 alt2">1</div>
                                        <div class="line number2 index1 alt1">2</div>
                                        <div class="line number3 index2 alt2">3</div>
                                        <div class="line number4 index3 alt1">4</div>
                                        <div class="line number5 index4 alt2">5</div>
                                        <div class="line number6 index5 alt1">6</div>
                                        <div class="line number7 index6 alt2">7</div>
                                        <div class="line number8 index7 alt1">8</div>
                                        <div class="line number9 index8 alt2">9</div>
                                        <div class="line number10 index9 alt1">10</div>
                                        <div class="line number11 index10 alt2">11</div>
                                        <div class="line number12 index11 alt1">12</div>
                                    </td>
                                    <td class="code">
                                        <div class="container">
                                            <div class="line number1 index0 alt2"><code class="htmlscript plain">@mixin
                                                    rounded-corners($radius:5px) { </code></div>
                                            <div class="line number2 index1 alt1"><code
                                                    class="htmlscript plain">border-radius:$radius; </code></div>
                                            <div class="line number3 index2 alt2"><code
                                                    class="htmlscript plain">-webkit-border-radius:$radius; </code>
                                            </div>
                                            <div class="line number4 index3 alt1"><code
                                                    class="htmlscript plain">-moz-border-radius:$radius; </code></div>
                                            <div class="line number5 index4 alt2"><code class="htmlscript plain">}
                                                </code></div>
                                            <div class="line number6 index5 alt1">&nbsp;</div>
                                            <div class="line number7 index6 alt2"><code class="htmlscript plain">#header
                                                    { </code></div>
                                            <div class="line number8 index7 alt1"><code
                                                    class="htmlscript plain">@include rounded-corners; </code></div>
                                            <div class="line number9 index8 alt2"><code class="htmlscript plain">}
                                                </code></div>
                                            <div class="line number10 index9 alt1"><code
                                                    class="htmlscript plain">#footer { </code></div>
                                            <div class="line number11 index10 alt2"><code
                                                    class="htmlscript plain">@include rounded-corners(10px); </code>
                                            </div>
                                            <div class="line number12 index11 alt1"><code
                                                    class="htmlscript plain">}</code></div>
                                        </div>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </span>
        <p><a href="#listing23">清单 23</a>中的代码将被转换为清单 24 中的 CSS 代码。</p>
        <h5 id="listing24" class="ibm-h5">清单 24. CSS 混入</h5><span class="dw-code-nohighlight">
            <div class="ibm-syntax-container">
                <div>
                    <div id="highlighter_124201" class="syntaxhighlighter  htmlscript">
                        <table border="0" cellpadding="0" cellspacing="0" role="none">
                            <tbody>
                                <tr>
                                    <td class="gutter">
                                        <div class="line number1 index0 alt2">1</div>
                                        <div class="line number2 index1 alt1">2</div>
                                        <div class="line number3 index2 alt2">3</div>
                                        <div class="line number4 index3 alt1">4</div>
                                        <div class="line number5 index4 alt2">5</div>
                                        <div class="line number6 index5 alt1">6</div>
                                        <div class="line number7 index6 alt2">7</div>
                                        <div class="line number8 index7 alt1">8</div>
                                        <div class="line number9 index8 alt2">9</div>
                                    </td>
                                    <td class="code">
                                        <div class="container">
                                            <div class="line number1 index0 alt2"><code class="htmlscript plain">#header
                                                    { </code></div>
                                            <div class="line number2 index1 alt1"><code
                                                    class="htmlscript plain">border-radius:5px; </code></div>
                                            <div class="line number3 index2 alt2"><code
                                                    class="htmlscript plain">-webkit-border-radius:5px; </code></div>
                                            <div class="line number4 index3 alt1"><code
                                                    class="htmlscript plain">-moz-border-radius:5px; } </code></div>
                                            <div class="line number5 index4 alt2">&nbsp;</div>
                                            <div class="line number6 index5 alt1"><code class="htmlscript plain">#footer
                                                    { </code></div>
                                            <div class="line number7 index6 alt2"><code
                                                    class="htmlscript plain">border-radius:10px; </code></div>
                                            <div class="line number8 index7 alt1"><code
                                                    class="htmlscript plain">-webkit-border-radius:10px; </code></div>
                                            <div class="line number9 index8 alt2"><code
                                                    class="htmlscript plain">-moz-border-radius:10px; }</code></div>
                                        </div>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </span>
        <p>函数和混入极为相似。两者均接受变量，但两者的差异在于，函数总是会返回一个值，但混入的输出是代码行。</p>
        <h2 id="N1020C" class="ibm-h2">Compass</h2>
        <p>Compass 是一种开放源码 CSS 创作框架，它使用了 Sass 样式表语言。其多种可重用设计模式（使用混入）能够帮助您构造可靠、强大的样式表。Compass 常用代码库是 Sass 开发工作中不可或缺的工具。
        </p>
        <p>究其本质而言，Compass 是 CSS 的包装器。它能够通过图像拼合 (spriting) 和其他技术处理常见的 CSS 问题，例如浏览器兼容性、布局和样式表优化。</p>
        <p>与 Sass 相似，Compass 也以 Ruby gem 程序包的形式提供。如需安装 Compass，请在命令行中输入 <code>gem install compass</code>。</p>
        <p>现在，您可以使用 Compass 框架中定义的混入（函数）。Compass 中提供了许多预定义的函数、类和原生 CSS3 支持。 有关 Compass 的更多信息，请参阅 <a
                href="#artrelatedtopics">参考资料</a>部分。</p>
        <h2 id="N1021F" class="ibm-h2">结束语</h2>
        <p>本文介绍了一些有助于在您的网站中成功实现 Sass 的概念。您学习了变量、插值、嵌套、函数和一些指令。</p>
        <p>Sass 并非 Internet 上的惟一 CSS 预处理程序。它最直接的竞争对手 LESS（参见 <a
                href="#artrelatedtopics">参考资料</a>部分）也占据着一定的市场份额。基本上来说，Sass 与 LESS 之间的差异非常弱小。Sass 最重要的资产是 Compass，而 LESS
            中不具备此扩展。您应该试用尽可能多的预处理程序，选定最适合您的风格和需求的产品。</p>
        <!--CMA ID: 937306-->
        <!--Site ID: 10-->
        <!--XSLT stylesheet used to transform this file: dw-document-html-8.0.xsl-->
        <!-- Article Quiz -->

        <!-- Article Resources -->
        <div class="ibm-alternate-rule">
            <hr>
        </div>
        <h4 id="artrelatedtopics" class="ibm-h4">相关主题</h4>
        <ul>
            <li><a href="http://sass-lang.com/">Sass</a>：进一步了解在 CSS 的基础之上构建的元语言 Sass。浏览教程、文档和博客。</li>
            <li><a href="http://sass-lang.com/editors.html">文本编辑器和 Sass 支持</a>：查看对 Saas 具有不同级别支持的文本编辑器列表。</li>
            <li><a href="http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html">Sass 函数列表</a>：查看 Saas 函数的完整列表。
            </li>
            <li><a href="http://compass-style.org/reference/compass/">Compass</a>：了解这种开放源码 CSS 创作框架。</li>
            <li><a href="http://lesscss.org/">LESS</a>：获得有关 LESS 动态样式表语言的更多信息。</li>
            <li><a href="http://css-tricks.com/sass-vs-less/">Sass 与 LESS 的对比</a>：阅读这篇文章，其中提供了有关 CSS 预处理程序的选择指南。</li>
            <li><a href="http://sass-lang.com/download.html">下载 Sass</a>并尝试使用它。</li>
            <li><a href="http://www.ibm.com/developerworks/cn/web/" onmouseover="linkQueryAppend(this)">developerWorks
                    Web development 专区</a>：查找涵盖多种基于 Web 的解决方案的文章。查阅 <a
                    href="http://www.ibm.com/developerworks/views/web/libraryview.jsp"
                    onmouseover="linkQueryAppend(this)">Web 开发技术库</a>，获得广泛的技术文章和技巧、教程、标准和 IBM Redbook。</li>
            <li><a href="http://www.ibm.com/developerworks/cn/briefings/"
                    onmouseover="linkQueryAppend(this)">developerWorks 技术活动和网络广播</a>：在这些活动中了解最新技术动向。</li>
            <li><a href="http://twitter.com/developerworks">Twitter 中的 developerWorks</a>：立即加入，关注 developerWorks 的推文。
            </li>
        </ul>
        <!-- Commenting -->
        <!-- INLINE_COMMENTS_BEGIN: -->
        <div class="ibm-alternate-rule">
            <hr>
        </div>
        <div id="dw-article-cmts-top" class="ibm-columns">
            <div class="ibm-col-6-2">
                <h4 id="icomments" class="ibm-h4">评论</h4>
                <div id="dw-article-cmts-login">
                    <p>添加或订阅评论，请先<a onclick="window.location=userLinks[0].url;" tabindex="0" role="link">登录</a>或<a
                            onclick="window.location=userLinks[1].url;" tabindex="0" role="link">注册</a>。</p>
                </div>
            </div>
            <div class="ibm-col-6-2" id="dw-notify">
                <input type="checkbox" value="1" name="comment_notification" id="comment_notification" disabled="">
                <label for="comment_notification" style="color: rgb(204, 204, 204);">有新评论时提醒我</label>
            </div>
        </div>

        <div class="dw-article-cmts-container">
            <div class="ibm-no-print jquery-comments read-only" id="dw-icomments-container">
                <div class="commenting-field main"><img src="/developerworks/maverick/image/user/user-icon.png" alt=""
                        class="profile-picture round by-current-user">
                    <div class="textarea-wrapper"><span class="close" style="display: none;"><span
                                class="left"></span><span class="right"></span></span>
                        <div class="textarea" data-placeholder="添加评论" contenteditable="true" style="height: 3.65em;">
                        </div>
                        <div class="control-row" style="display: none;"><span
                                class="send save highlight-background">提交</span></div>
                    </div>
                </div>
                <ul class="navigation">
                    <div class="navigation-wrapper">
                        <li data-sort-key="newest" data-container-name="comments" class="active">最新的</li>
                        <li data-sort-key="oldest" data-container-name="comments">历史</li>
                        <li data-sort-key="popularity" data-container-name="comments">热门</li>
                    </div>
                    <div class="navigation-wrapper responsive">
                        <li class="title active">
                            <header>最新的</header>
                        </li>
                        <ul class="dropdown">
                            <li data-sort-key="newest" data-container-name="comments" class="active">最新的</li>
                            <li data-sort-key="oldest" data-container-name="comments">历史</li>
                            <li data-sort-key="popularity" data-container-name="comments">热门</li>
                        </ul>
                    </div>
                </ul>
                <div class="data-container" data-container="comments">
                    <ul id="comment-list" class="main">
                        <li data-id="25097" class="comment">
                            <div class="comment-wrapper"><img src="/developerworks/maverick/image/user/user-icon.png"
                                    alt="" class="profile-picture round"><time data-original="1466612700077">2016 年 06 月
                                    23 日</time>
                                <div class="name"><a
                                        href="">KevinCYChen</a>
                                </div>
                                <div class="wrapper">
                                    <div class="content">Sassy CSS (SCSS) 是 CSS3 的超集；任何 SCSS 代码均为有效的 CSS 代码。本文中使用的代码示例均为
                                        SCSS。应该是
                                        Sassy CSS (SCSS) 是 CSS3 的超集；任何 CSS3 代码均为有效的 SCSS 代码。本文中使用的代码示例均为 SCSS。</div>
                                    <span class="actions"><button class="action reply" type="button">回复</button><span
                                            class="separator">·</span><button class="action upvote"><span
                                                class="upvote-count">0</span><i
                                                class="fa fa-thumbs-up"></i></button><span class="separator">·</span><a
                                            class="action reply">报告滥用<i class="fa image"
                                                style="background-image: url(&quot;/developerworks/maverick/image/report-abuse.png&quot;);"></i></a></span>
                                </div>
                            </div>
                            <ul class="child-comments"></ul>
                        </li>
                    </ul>
                    <div class="no-comments no-data"><i class="fa fa-comments fa-2x"></i><br>第一个评论</div>
                </div>
            </div>
        </div>
