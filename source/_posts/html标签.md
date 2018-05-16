---
title: html标签
date: 2018-04-6 22:02:22
tags:
---
<div class="RichText Post-RichText">

从接触前端到现在整整一个月。总结一下自己学了什么吧：

命令行，http，数据结构，html，css，js，DOM，然后就是jQuery。

因为太懒了...所以一直没有更新..现在要加把劲了！

* * *

由于DOM的API非常难记...而且不便利，所以作为优化DOM的jQuery出现了。

那一个jQuery的API是怎么实现的呢？

在我理解看来，jQuery也是基于DOM的API实现，只是封装集成了一起，通过 $('___') 函数 + 事件绑定实现。

那我们自己实现一下以下功能：
<div class="highlight">

    <span></span>var $div = $('div')    //输入div
    $div.addClass('red')  // 可将所有 div 的 class 添加一个 red
    $div.setText('hi')  // 可将所有 div 的 textContent 变为 hi
    `</pre></div>

    也就是说输入div标签名，所有div都会被执行，而且可以addClass还有setText。

    ## **第一：传递参数**
    <div class="highlight"><pre>`<span></span>var $div = $('div')  
    `</pre></div>

    要实现上面这个功能，我的理解是首先设置window对象jQuery为函数返回的结果，然后把参数'div'传递到函数内的DOM，作为变量进行下一步的操作。
    <div class="highlight"><pre>`<span></span><span class="nb">window</span><span class="p">.</span><span class="nx">jQuery</span><span class="o">=</span><span class="kd">function</span><span class="err">（</span><span class="nx">selector</span><span class="err">）</span><span class="p">{</span>  
    <span class="kd">let</span> <span class="nx">alldiv</span><span class="o">=</span><span class="nb">document</span><span class="p">.</span><span class="nx">querySelectorAll</span><span class="p">(</span><span class="nx">selector</span><span class="p">)</span>  
    <span class="p">}</span>

    <span class="nb">window</span><span class="p">.</span><span class="nx">$</span> <span class="o">=</span> <span class="nx">jQuery</span>
    `</pre></div>

    ## **第二：返回对象**
    <div class="highlight"><pre>`<span></span>$div.addClass('red')  // 可将所有 div 的 class 添加一个 red
    $div.setText('hi')  // 可将所有 div 的 textContent 变为 hi
    `</pre></div>

    可以观察得到$div是一个对象，它的key有addClass和setText。

    那我也返回一个含有addClass和setClass的对象。
    <div class="highlight"><pre>`<span></span><span class="nb">window</span><span class="p">.</span><span class="nx">jQuery</span><span class="o">=</span><span class="kd">function</span> <span class="p">(</span><span class="nx">selector</span><span class="p">){</span>
    <span class="kd">let</span> <span class="nx">alldiv</span><span class="o">=</span><span class="nb">document</span><span class="p">.</span><span class="nx">querySelectorAll</span><span class="p">(</span><span class="nx">selector</span><span class="p">)</span>  
      <span class="k">return</span><span class="p">{</span>
           <span class="nx">addClass</span><span class="o">:</span><span class="kd">function</span> <span class="p">(){},</span>
           <span class="nx">setClass</span><span class="err">：</span><span class="kd">function</span><span class="p">(){}</span>
      <span class="p">}</span>
    <span class="p">}</span>
    <span class="nb">window</span><span class="p">.</span><span class="nx">$</span> <span class="o">=</span> <span class="nx">jQuery</span>
    `</pre></div>

    ## **第三：补全对象的key**

    我们得到了alldiv，它是一个伪数组。如果每个div都要添加class的话，需要一个for循环，然后每一次用DOM的方法里的classList.add功能对div添加class。
    <div class="highlight"><pre>`<span></span><span class="nx">addClass</span><span class="o">:</span> <span class="kd">function</span><span class="p">(){</span>
                   <span class="k">for</span><span class="p">(</span><span class="kd">var</span> <span class="nx">i</span><span class="o">=</span><span class="mi">0</span><span class="p">;</span><span class="nx">i</span><span class="o">&lt;</span><span class="nx">alldiv</span><span class="p">.</span><span class="nx">length</span><span class="p">;</span><span class="nx">i</span><span class="o">++</span><span class="p">){</span>
                       <span class="nx">alldiv</span><span class="p">[</span><span class="nx">i</span><span class="p">].</span><span class="nx">classList</span><span class="p">.</span><span class="nx">add</span><span class="p">(</span><span class="s2">"red"</span><span class="p">)</span>
                   <span class="p">}</span>
    `</pre></div>

    同理有
    <div class="highlight"><pre>`<span></span><span class="nx">setText</span><span class="o">:</span> <span class="kd">function</span><span class="p">(){</span>
                   <span class="k">for</span><span class="p">(</span><span class="kd">var</span> <span class="nx">i</span><span class="o">=</span><span class="mi">0</span><span class="p">;</span><span class="nx">i</span><span class="o">&lt;</span><span class="nx">alldiv</span><span class="p">.</span><span class="nx">length</span><span class="p">;</span><span class="nx">i</span><span class="o">++</span><span class="p">){</span>
                      <span class="nx">alldiv</span><span class="p">[</span><span class="nx">i</span><span class="p">].</span><span class="nx">textContent</span> <span class="o">=</span> <span class="s2">"hi"</span>
                  <span class="p">}</span>
    `</pre></div>

    把addClass和setText的内容结合到函数内，得到：
    <div class="highlight"><pre>`<span></span><span class="nb">window</span><span class="p">.</span><span class="nx">jQuery</span><span class="o">=</span><span class="kd">function</span> <span class="p">(</span><span class="nx">selector</span><span class="p">){</span>
      <span class="kd">let</span> <span class="nx">alldiv</span><span class="o">=</span><span class="nb">document</span><span class="p">.</span><span class="nx">querySelectorAll</span><span class="p">(</span><span class="nx">selector</span><span class="p">)</span>
      <span class="k">return</span><span class="p">{</span>
           <span class="nx">addClass</span><span class="o">:</span> <span class="kd">function</span><span class="p">(){</span>
                   <span class="k">for</span><span class="p">(</span><span class="kd">var</span> <span class="nx">i</span><span class="o">=</span><span class="mi">0</span><span class="p">;</span><span class="nx">i</span><span class="o">&lt;</span><span class="nx">alldiv</span><span class="p">.</span><span class="nx">length</span><span class="p">;</span><span class="nx">i</span><span class="o">++</span><span class="p">){</span>
                       <span class="nx">alldiv</span><span class="p">[</span><span class="nx">i</span><span class="p">].</span><span class="nx">classList</span><span class="p">.</span><span class="nx">add</span><span class="p">(</span><span class="s2">"red"</span><span class="p">)</span>
                   <span class="p">}</span>
           <span class="p">},</span>
           <span class="nx">setText</span><span class="o">:</span> <span class="kd">function</span><span class="p">(){</span>
                   <span class="k">for</span><span class="p">(</span><span class="kd">var</span> <span class="nx">i</span><span class="o">=</span><span class="mi">0</span><span class="p">;</span><span class="nx">i</span><span class="o">&lt;</span><span class="nx">alldiv</span><span class="p">.</span><span class="nx">length</span><span class="p">;</span><span class="nx">i</span><span class="o">++</span><span class="p">){</span>
                      <span class="nx">alldiv</span><span class="p">[</span><span class="nx">i</span><span class="p">].</span><span class="nx">textContent</span> <span class="o">=</span> <span class="s2">"hi"</span>
                  <span class="p">}</span>
          <span class="p">}</span>
       <span class="p">}</span>
    <span class="p">}</span>
    <span class="nb">window</span><span class="p">.</span><span class="nx">$</span> <span class="o">=</span> <span class="nx">jQuery</span>

</div>

## **总结**

以上代码实现一个 jQuery 的 API。
</div>