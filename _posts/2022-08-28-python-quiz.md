---
keywords: fastai
description: A simple quiz made using Python.
title: Python Quiz!
toc: true
branch: master
badges: true
comments: true
categories: [tri1]
nb_path: _notebooks/2022-08-28-python-quiz.ipynb
layout: notebook
---

<!--
#################################################
### THIS FILE WAS AUTOGENERATED! DO NOT EDIT! ###
#################################################
# file to edit: _notebooks/2022-08-28-python-quiz.ipynb
-->

<div class="container" id="notebook-container">
        
    {% raw %}
    
<div class="cell border-box-sizing code_cell rendered">
<div class="input">

<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="kn">import</span> <span class="nn">getpass</span><span class="o">,</span> <span class="nn">sys</span>

<span class="k">def</span> <span class="nf">question_with_response</span><span class="p">(</span><span class="n">prompt</span><span class="p">):</span>
    <span class="nb">print</span><span class="p">(</span><span class="s2">&quot;Question: &quot;</span> <span class="o">+</span> <span class="n">prompt</span><span class="p">)</span>
    <span class="n">msg</span> <span class="o">=</span> <span class="nb">input</span><span class="p">()</span>
    <span class="k">return</span> <span class="n">msg</span>

<span class="n">questions</span> <span class="o">=</span> <span class="mi">3</span>
<span class="n">correct</span> <span class="o">=</span> <span class="mi">0</span>
<span class="n">question_and_answer</span> <span class="o">=</span> <span class="nb">input</span> 

<span class="nb">print</span><span class="p">(</span><span class="s1">&#39;Hello, &#39;</span> <span class="o">+</span> <span class="n">getpass</span><span class="o">.</span><span class="n">getuser</span><span class="p">()</span> <span class="o">+</span> <span class="s2">&quot; running &quot;</span> <span class="o">+</span> <span class="n">sys</span><span class="o">.</span><span class="n">executable</span><span class="p">)</span>
<span class="nb">print</span><span class="p">(</span><span class="s2">&quot;You will be asked &quot;</span> <span class="o">+</span> <span class="nb">str</span><span class="p">(</span><span class="n">questions</span><span class="p">)</span> <span class="o">+</span> <span class="s2">&quot; questions.&quot;</span><span class="p">)</span>
<span class="n">question_and_answer</span><span class="p">(</span><span class="s2">&quot;Are you ready to take a test?&quot;</span><span class="p">)</span>

<span class="n">rsp</span> <span class="o">=</span> <span class="n">question_with_response</span><span class="p">(</span><span class="s2">&quot;What kernel is used to make sure your code in jupyter notebooks shows up on your fastpages?&quot;</span><span class="p">)</span>
<span class="k">if</span> <span class="n">rsp</span> <span class="o">==</span> <span class="s2">&quot;bash&quot;</span><span class="p">:</span>
    <span class="nb">print</span><span class="p">(</span><span class="n">rsp</span> <span class="o">+</span> <span class="s2">&quot; is correct!&quot;</span><span class="p">)</span>
    <span class="n">correct</span> <span class="o">+=</span> <span class="mi">1</span>
<span class="k">else</span><span class="p">:</span>
    <span class="nb">print</span><span class="p">(</span><span class="n">rsp</span> <span class="o">+</span> <span class="s2">&quot; is incorrect!&quot;</span><span class="p">)</span>

<span class="n">rsp</span> <span class="o">=</span> <span class="n">question_with_response</span><span class="p">(</span><span class="s2">&quot;What command is used to evaluate correct or incorrect response in this example?&quot;</span><span class="p">)</span>
<span class="k">if</span> <span class="n">rsp</span> <span class="o">==</span> <span class="s2">&quot;if&quot;</span><span class="p">:</span>
    <span class="nb">print</span><span class="p">(</span><span class="n">rsp</span> <span class="o">+</span> <span class="s2">&quot; is correct!&quot;</span><span class="p">)</span>
    <span class="n">correct</span> <span class="o">+=</span> <span class="mi">1</span>
<span class="k">else</span><span class="p">:</span>
    <span class="nb">print</span><span class="p">(</span><span class="n">rsp</span> <span class="o">+</span> <span class="s2">&quot; is incorrect!&quot;</span><span class="p">)</span>

<span class="n">rsp</span> <span class="o">=</span> <span class="n">question_with_response</span><span class="p">(</span><span class="s2">&quot;How can you create a _posts page with an image?&quot;</span><span class="p">)</span>
<span class="k">if</span> <span class="n">rsp</span> <span class="o">==</span> <span class="s2">&quot;squiggly brackets&quot;</span><span class="p">:</span>
    <span class="nb">print</span><span class="p">(</span><span class="n">rsp</span> <span class="o">+</span> <span class="s2">&quot; is correct!&quot;</span><span class="p">)</span>
    <span class="n">correct</span> <span class="o">+=</span> <span class="mi">1</span>
<span class="k">else</span><span class="p">:</span>
    <span class="nb">print</span><span class="p">(</span><span class="n">rsp</span> <span class="o">+</span> <span class="s2">&quot; is incorrect!&quot;</span><span class="p">)</span>

<span class="nb">print</span><span class="p">(</span><span class="n">getpass</span><span class="o">.</span><span class="n">getuser</span><span class="p">()</span> <span class="o">+</span> <span class="s2">&quot; you scored &quot;</span> <span class="o">+</span> <span class="nb">str</span><span class="p">(</span><span class="n">correct</span><span class="p">)</span> <span class="o">+</span><span class="s2">&quot;/&quot;</span> <span class="o">+</span> <span class="nb">str</span><span class="p">(</span><span class="n">questions</span><span class="p">))</span>
</pre></div>

    </div>
</div>
</div>

<div class="output_wrapper">
<div class="output">

<div class="output_area">

<div class="output_subarea output_stream output_stdout output_text">
<pre>Hello, shreyasapkal running /home/shreyasapkal/anaconda3/bin/python
You will be asked 3 questions.
Question: What kernel is used to make sure your code in jupyter notebooks shows up on your fastpages?
bash] is incorrect!
Question: What command is used to evaluate correct or incorrect response in this example?
if is correct!
Question: How can you create a _posts page with an image?
squiggly brackets is correct!
shreyasapkal you scored 2/3
</pre>
</div>
</div>

</div>
</div>

</div>
    {% endraw %}

</div>
 

