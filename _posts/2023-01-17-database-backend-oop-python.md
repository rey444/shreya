---
keywords: fastai
title: Database/Model, Backend, OOP, Python Notes & Hacks
toc: true
branch: master
badges: true
comments: true
categories: [t2]
nb_path: _notebooks/2023-01-17-database-backend-oop-python.ipynb
layout: notebook
---

<!--
#################################################
### THIS FILE WAS AUTOGENERATED! DO NOT EDIT! ###
#################################################
# file to edit: _notebooks/2023-01-17-database-backend-oop-python.ipynb
-->

<div class="container" id="notebook-container">
        
<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h2 id="Vocab">Vocab<a class="anchor-link" href="#Vocab"> </a></h2><ul>
<li>table = a model/schema within a database</li>
<li>SQL = structure and query language<ul>
<li>the way we interact with databases</li>
<li>we will do regular coding, and it will allow us to interact with the database</li>
</ul>
</li>
<li>persistent data = data that is saved somewhere into a database</li>
</ul>

</div>
</div>
</div>
<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p><strong>User Table Schema</strong></p>
<ul>
<li>Defining the title of the table and what goes into it is the <code>schema</code></li>
<li><code>db.Model</code> allows us to inherit the properties of db into our user class</li>
<li>database = collection of spreadsheets/tables --&gt; should be on our backend</li>
<li><code>db.Model</code> is inhertied into the <code>class User(db.model)</code></li>
<li>each <code>db.Column</code> gets properties according to the capabilities of SQL</li>
</ul>
<p><strong>How it Works</strong></p>
<ul>
<li>we ask the user for an input</li>
<li>it takes the input to the backend --&gt; stores it into a database</li>
<li>we can extract information from the database to bring it back to the frontend, if needed<ul>
<li>ex. setting a password --&gt; when the site tells you your password is incorrect, it's crosschecking with the table created on the backend</li>
</ul>
</li>
</ul>

</div>
</div>
</div>
    {% raw %}
    
<div class="cell border-box-sizing code_cell rendered">
<div class="input">

<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">db</span> <span class="o">=</span> <span class="n">SQLAlchemy</span><span class="p">(</span><span class="n">app</span><span class="p">)</span> 

<span class="sd">&quot;&quot;&quot; database dependencies to support sqliteDB examples &quot;&quot;&quot;</span>

<span class="kn">from</span> <span class="nn">__init__</span> <span class="kn">import</span> <span class="n">app</span><span class="p">,</span> <span class="n">db</span>
<span class="kn">from</span> <span class="nn">sqlalchemy.exc</span> <span class="kn">import</span> <span class="n">IntegrityError</span>
<span class="kn">from</span> <span class="nn">werkzeug.security</span> <span class="kn">import</span> <span class="n">generate_password_hash</span><span class="p">,</span> <span class="n">check_password_hash</span>


<span class="sd">&quot;&quot;&quot; Key additions to User Class for Schema definition &quot;&quot;&quot;</span>

<span class="c1"># Define the User class to manage actions in the &#39;users&#39; table</span>
<span class="c1"># -- Object Relational Mapping (ORM) is the key concept of SQLAlchemy</span>
<span class="c1"># -- a.) db.Model is like an inner layer of the onion in ORM</span>
<span class="c1"># -- b.) User represents data we want to store, something that is built on db.Model</span>
<span class="c1"># -- c.) SQLAlchemy ORM is layer on top of SQLAlchemy Core, then SQLAlchemy engine, SQL</span>
<span class="k">class</span> <span class="nc">User</span><span class="p">(</span><span class="n">db</span><span class="o">.</span><span class="n">Model</span><span class="p">):</span>
    <span class="n">__tablename__</span> <span class="o">=</span> <span class="s1">&#39;users&#39;</span>  <span class="c1"># table name is plural, class name is singular</span>

    <span class="c1"># Define the User schema with &quot;vars&quot; from object</span>
    <span class="c1"># creates multiple columns, one for each type of data (name, uid, password, dob)</span>
    <span class="c1"># defining the database</span>
    <span class="nb">id</span> <span class="o">=</span> <span class="n">db</span><span class="o">.</span><span class="n">Column</span><span class="p">(</span><span class="n">db</span><span class="o">.</span><span class="n">Integer</span><span class="p">,</span> <span class="n">primary_key</span><span class="o">=</span><span class="kc">True</span><span class="p">)</span> 
    <span class="n">_name</span> <span class="o">=</span> <span class="n">db</span><span class="o">.</span><span class="n">Column</span><span class="p">(</span><span class="n">db</span><span class="o">.</span><span class="n">String</span><span class="p">(</span><span class="mi">255</span><span class="p">),</span> <span class="n">unique</span><span class="o">=</span><span class="kc">False</span><span class="p">,</span> <span class="n">nullable</span><span class="o">=</span><span class="kc">False</span><span class="p">)</span> <span class="c1"># associating the properties with the database; allows us to move the infor in and out of the database using the column name</span>
    <span class="n">_uid</span> <span class="o">=</span> <span class="n">db</span><span class="o">.</span><span class="n">Column</span><span class="p">(</span><span class="n">db</span><span class="o">.</span><span class="n">String</span><span class="p">(</span><span class="mi">255</span><span class="p">),</span> <span class="n">unique</span><span class="o">=</span><span class="kc">True</span><span class="p">,</span> <span class="n">nullable</span><span class="o">=</span><span class="kc">False</span><span class="p">)</span>
    <span class="n">_password</span> <span class="o">=</span> <span class="n">db</span><span class="o">.</span><span class="n">Column</span><span class="p">(</span><span class="n">db</span><span class="o">.</span><span class="n">String</span><span class="p">(</span><span class="mi">255</span><span class="p">),</span> <span class="n">unique</span><span class="o">=</span><span class="kc">False</span><span class="p">,</span> <span class="n">nullable</span><span class="o">=</span><span class="kc">False</span><span class="p">)</span>
    <span class="n">_dob</span> <span class="o">=</span> <span class="n">db</span><span class="o">.</span><span class="n">Column</span><span class="p">(</span><span class="n">db</span><span class="o">.</span><span class="n">Date</span><span class="p">)</span>

    <span class="c1"># Defines a relationship between User record and Notes table, one-to-many (one user to many notes)</span>
    <span class="n">posts</span> <span class="o">=</span> <span class="n">db</span><span class="o">.</span><span class="n">relationship</span><span class="p">(</span><span class="s2">&quot;Post&quot;</span><span class="p">,</span> <span class="n">cascade</span><span class="o">=</span><span class="s1">&#39;all, delete&#39;</span><span class="p">,</span> <span class="n">backref</span><span class="o">=</span><span class="s1">&#39;users&#39;</span><span class="p">,</span> <span class="n">lazy</span><span class="o">=</span><span class="kc">True</span><span class="p">)</span>

    <span class="c1"># constructor of a User object, initializes the instance variables within object (self)</span>
    <span class="k">def</span> <span class="fm">__init__</span><span class="p">(</span><span class="bp">self</span><span class="p">,</span> <span class="n">name</span><span class="p">,</span> <span class="n">uid</span><span class="p">,</span> <span class="n">password</span><span class="o">=</span><span class="s2">&quot;123qwerty&quot;</span><span class="p">,</span> <span class="n">dob</span><span class="o">=</span><span class="n">date</span><span class="o">.</span><span class="n">today</span><span class="p">()):</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">_name</span> <span class="o">=</span> <span class="n">name</span>    <span class="c1"># variables with self prefix become part of the object, </span>
        <span class="bp">self</span><span class="o">.</span><span class="n">_uid</span> <span class="o">=</span> <span class="n">uid</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">set_password</span><span class="p">(</span><span class="n">password</span><span class="p">)</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">_dob</span> <span class="o">=</span> <span class="n">dob</span>
</pre></div>

    </div>
</div>
</div>

</div>
    {% endraw %}

<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p><strong>User Table CRUD Operations</strong></p>
<ul>
<li>allow us to move data in and out of the database</li>
<li>all the <code>def</code>s are methods of the User Class</li>
<li>python sessions with the database</li>
<li>we are going to try to use these methods to create our own database for our CPT</li>
<li>we need to tell our code where our database file is in order to access it</li>
</ul>

</div>
</div>
</div>
    {% raw %}
    
<div class="cell border-box-sizing code_cell rendered">
<div class="input">

<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="k">def</span> <span class="nf">create</span><span class="p">(</span><span class="bp">self</span><span class="p">):</span>
    <span class="k">try</span><span class="p">:</span>
        <span class="c1"># creates a person object from User(db.Model) class, passes initializers</span>
        <span class="n">db</span><span class="o">.</span><span class="n">session</span><span class="o">.</span><span class="n">add</span><span class="p">(</span><span class="bp">self</span><span class="p">)</span>  <span class="c1"># add prepares to persist person object to Users table</span>
        <span class="n">db</span><span class="o">.</span><span class="n">session</span><span class="o">.</span><span class="n">commit</span><span class="p">()</span>  <span class="c1"># SqlAlchemy &quot;unit of work pattern&quot; requires a manual commit</span>
        <span class="k">return</span> <span class="bp">self</span>
    <span class="k">except</span> <span class="n">IntegrityError</span><span class="p">:</span>
        <span class="n">db</span><span class="o">.</span><span class="n">session</span><span class="o">.</span><span class="n">remove</span><span class="p">()</span>
        <span class="k">return</span> <span class="kc">None</span>

<span class="c1"># CRUD read converts self to dictionary</span>
<span class="c1"># returns dictionary</span>
<span class="k">def</span> <span class="nf">read</span><span class="p">(</span><span class="bp">self</span><span class="p">):</span>
    <span class="k">return</span> <span class="p">{</span>
        <span class="s2">&quot;id&quot;</span><span class="p">:</span> <span class="bp">self</span><span class="o">.</span><span class="n">id</span><span class="p">,</span>
        <span class="s2">&quot;name&quot;</span><span class="p">:</span> <span class="bp">self</span><span class="o">.</span><span class="n">name</span><span class="p">,</span>
        <span class="s2">&quot;uid&quot;</span><span class="p">:</span> <span class="bp">self</span><span class="o">.</span><span class="n">uid</span><span class="p">,</span>
        <span class="s2">&quot;dob&quot;</span><span class="p">:</span> <span class="bp">self</span><span class="o">.</span><span class="n">dob</span><span class="p">,</span>
        <span class="s2">&quot;age&quot;</span><span class="p">:</span> <span class="bp">self</span><span class="o">.</span><span class="n">age</span><span class="p">,</span>
        <span class="s2">&quot;posts&quot;</span><span class="p">:</span> <span class="p">[</span><span class="n">post</span><span class="o">.</span><span class="n">read</span><span class="p">()</span> <span class="k">for</span> <span class="n">post</span> <span class="ow">in</span> <span class="bp">self</span><span class="o">.</span><span class="n">posts</span><span class="p">]</span>
    <span class="p">}</span>

<span class="c1"># CRUD update: updates user name, password, phone</span>
<span class="c1"># returns self</span>
<span class="k">def</span> <span class="nf">update</span><span class="p">(</span><span class="bp">self</span><span class="p">,</span> <span class="n">name</span><span class="o">=</span><span class="s2">&quot;&quot;</span><span class="p">,</span> <span class="n">uid</span><span class="o">=</span><span class="s2">&quot;&quot;</span><span class="p">,</span> <span class="n">password</span><span class="o">=</span><span class="s2">&quot;&quot;</span><span class="p">):</span>
    <span class="sd">&quot;&quot;&quot;only updates values with length&quot;&quot;&quot;</span>
    <span class="k">if</span> <span class="nb">len</span><span class="p">(</span><span class="n">name</span><span class="p">)</span> <span class="o">&gt;</span> <span class="mi">0</span><span class="p">:</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">name</span> <span class="o">=</span> <span class="n">name</span>
    <span class="k">if</span> <span class="nb">len</span><span class="p">(</span><span class="n">uid</span><span class="p">)</span> <span class="o">&gt;</span> <span class="mi">0</span><span class="p">:</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">uid</span> <span class="o">=</span> <span class="n">uid</span>
    <span class="k">if</span> <span class="nb">len</span><span class="p">(</span><span class="n">password</span><span class="p">)</span> <span class="o">&gt;</span> <span class="mi">0</span><span class="p">:</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">set_password</span><span class="p">(</span><span class="n">password</span><span class="p">)</span>
    <span class="n">db</span><span class="o">.</span><span class="n">session</span><span class="o">.</span><span class="n">commit</span><span class="p">()</span>
    <span class="k">return</span> <span class="bp">self</span>

<span class="c1"># CRUD delete: remove self</span>
<span class="c1"># None</span>
<span class="k">def</span> <span class="nf">delete</span><span class="p">(</span><span class="bp">self</span><span class="p">):</span>
    <span class="n">db</span><span class="o">.</span><span class="n">session</span><span class="o">.</span><span class="n">delete</span><span class="p">(</span><span class="bp">self</span><span class="p">)</span>
    <span class="n">db</span><span class="o">.</span><span class="n">session</span><span class="o">.</span><span class="n">commit</span><span class="p">()</span>
    <span class="k">return</span> <span class="kc">None</span>
</pre></div>

    </div>
</div>
</div>

</div>
    {% endraw %}

<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h2 id="DevOps-and-Databases">DevOps and Databases<a class="anchor-link" href="#DevOps-and-Databases"> </a></h2><ul>
<li>can create a database which runs locally to work on/actually code on<ul>
<li>run locally as we develop on a test database</li>
</ul>
</li>
<li>create another database which is used to actually deploy the work; copy over from the database which runs locally once you have it working</li>
<li>we store our database into the "volumes" directory<ul>
<li>persistent data is stored into the databse</li>
<li>must be careful to ensure that the persistent data isn't erased</li>
</ul>
</li>
<li><a href="https://nighthawkcoders.github.io/APCSP//2023/01/17/PBL-database.html#Outline-to-understand-Devops-and-Databases">Link to DevOps section of lecture for refernece</a></li>
<li>there are administrative tools that help us open the database<ul>
<li>ex. SQLite</li>
</ul>
</li>
</ul>

</div>
</div>
</div>
<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h2 id="SQL-Administrative-Tools">SQL Administrative Tools<a class="anchor-link" href="#SQL-Administrative-Tools"> </a></h2><ul>
<li>we are the admins<ul>
<li>use tools to help us figure out what's going on in the backend</li>
</ul>
</li>
</ul>

</div>
</div>
</div>
</div>
 
