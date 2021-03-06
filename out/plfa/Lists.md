---
src       : "src/plfa/Lists.lagda.md"
title     : "Lists: 列表与高阶函数"
layout    : page
prev      : /Decidable/
permalink : /Lists/
next      : /Lambda/
translators: ["Fangyi Zhou"]
progress  : 100
---

{% raw %}<pre class="Agda"><a id="170" class="Keyword">module</a> <a id="177" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}" class="Module">plfa.Lists</a> <a id="188" class="Keyword">where</a>
</pre>{% endraw %}
{::comment}
This chapter discusses the list data type.  It gives further examples
of many of the techniques we have developed so far, and provides
examples of polymorphic types and higher-order functions.
{:/}

本章节讨论列表（List）数据类型。我们用列表作为例子，来使用我们之前学习的技巧。同时，
列表也给我们带来多态类型（Polymorphic Types）和高阶函数（Higher-order Functions）的例子。

{::comment}
## Imports
{:/}

## 导入

{% raw %}<pre class="Agda"><a id="561" class="Keyword">import</a> <a id="568" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.html" class="Module">Relation.Binary.PropositionalEquality</a> <a id="606" class="Symbol">as</a> <a id="609" class="Module">Eq</a>
<a id="612" class="Keyword">open</a> <a id="617" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.html" class="Module">Eq</a> <a id="620" class="Keyword">using</a> <a id="626" class="Symbol">(</a><a id="627" href="Agda.Builtin.Equality.html#125" class="Datatype Operator">_≡_</a><a id="630" class="Symbol">;</a> <a id="632" href="Agda.Builtin.Equality.html#182" class="InductiveConstructor">refl</a><a id="636" class="Symbol">;</a> <a id="638" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#939" class="Function">sym</a><a id="641" class="Symbol">;</a> <a id="643" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#984" class="Function">trans</a><a id="648" class="Symbol">;</a> <a id="650" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#1090" class="Function">cong</a><a id="654" class="Symbol">)</a>
<a id="656" class="Keyword">open</a> <a id="661" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2499" class="Module">Eq.≡-Reasoning</a>
<a id="676" class="Keyword">open</a> <a id="681" class="Keyword">import</a> <a id="688" href="https://agda.github.io/agda-stdlib/v1.1/Data.Bool.html" class="Module">Data.Bool</a> <a id="698" class="Keyword">using</a> <a id="704" class="Symbol">(</a><a id="705" href="Agda.Builtin.Bool.html#135" class="Datatype">Bool</a><a id="709" class="Symbol">;</a> <a id="711" href="Agda.Builtin.Bool.html#160" class="InductiveConstructor">true</a><a id="715" class="Symbol">;</a> <a id="717" href="Agda.Builtin.Bool.html#154" class="InductiveConstructor">false</a><a id="722" class="Symbol">;</a> <a id="724" href="https://agda.github.io/agda-stdlib/v1.1/Data.Bool.Base.html#1480" class="Function">T</a><a id="725" class="Symbol">;</a> <a id="727" href="https://agda.github.io/agda-stdlib/v1.1/Data.Bool.Base.html#1015" class="Function Operator">_∧_</a><a id="730" class="Symbol">;</a> <a id="732" href="https://agda.github.io/agda-stdlib/v1.1/Data.Bool.Base.html#1073" class="Function Operator">_∨_</a><a id="735" class="Symbol">;</a> <a id="737" href="https://agda.github.io/agda-stdlib/v1.1/Data.Bool.Base.html#961" class="Function">not</a><a id="740" class="Symbol">)</a>
<a id="742" class="Keyword">open</a> <a id="747" class="Keyword">import</a> <a id="754" href="https://agda.github.io/agda-stdlib/v1.1/Data.Nat.html" class="Module">Data.Nat</a> <a id="763" class="Keyword">using</a> <a id="769" class="Symbol">(</a><a id="770" href="Agda.Builtin.Nat.html#165" class="Datatype">ℕ</a><a id="771" class="Symbol">;</a> <a id="773" href="Agda.Builtin.Nat.html#183" class="InductiveConstructor">zero</a><a id="777" class="Symbol">;</a> <a id="779" href="Agda.Builtin.Nat.html#196" class="InductiveConstructor">suc</a><a id="782" class="Symbol">;</a> <a id="784" href="Agda.Builtin.Nat.html#298" class="Primitive Operator">_+_</a><a id="787" class="Symbol">;</a> <a id="789" href="Agda.Builtin.Nat.html#501" class="Primitive Operator">_*_</a><a id="792" class="Symbol">;</a> <a id="794" href="Agda.Builtin.Nat.html#388" class="Primitive Operator">_∸_</a><a id="797" class="Symbol">;</a> <a id="799" href="https://agda.github.io/agda-stdlib/v1.1/Data.Nat.Base.html#895" class="Datatype Operator">_≤_</a><a id="802" class="Symbol">;</a> <a id="804" href="https://agda.github.io/agda-stdlib/v1.1/Data.Nat.Base.html#960" class="InductiveConstructor">s≤s</a><a id="807" class="Symbol">;</a> <a id="809" href="https://agda.github.io/agda-stdlib/v1.1/Data.Nat.Base.html#918" class="InductiveConstructor">z≤n</a><a id="812" class="Symbol">)</a>
<a id="814" class="Keyword">open</a> <a id="819" class="Keyword">import</a> <a id="826" href="https://agda.github.io/agda-stdlib/v1.1/Data.Nat.Properties.html" class="Module">Data.Nat.Properties</a> <a id="846" class="Keyword">using</a>
  <a id="854" class="Symbol">(</a><a id="855" href="https://agda.github.io/agda-stdlib/v1.1/Data.Nat.Properties.html#11578" class="Function">+-assoc</a><a id="862" class="Symbol">;</a> <a id="864" href="https://agda.github.io/agda-stdlib/v1.1/Data.Nat.Properties.html#11679" class="Function">+-identityˡ</a><a id="875" class="Symbol">;</a> <a id="877" href="https://agda.github.io/agda-stdlib/v1.1/Data.Nat.Properties.html#11734" class="Function">+-identityʳ</a><a id="888" class="Symbol">;</a> <a id="890" href="https://agda.github.io/agda-stdlib/v1.1/Data.Nat.Properties.html#18464" class="Function">*-assoc</a><a id="897" class="Symbol">;</a> <a id="899" href="https://agda.github.io/agda-stdlib/v1.1/Data.Nat.Properties.html#17362" class="Function">*-identityˡ</a><a id="910" class="Symbol">;</a> <a id="912" href="https://agda.github.io/agda-stdlib/v1.1/Data.Nat.Properties.html#17426" class="Function">*-identityʳ</a><a id="923" class="Symbol">)</a>
<a id="925" class="Keyword">open</a> <a id="930" class="Keyword">import</a> <a id="937" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.html" class="Module">Relation.Nullary</a> <a id="954" class="Keyword">using</a> <a id="960" class="Symbol">(</a><a id="961" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.html#535" class="Function Operator">¬_</a><a id="963" class="Symbol">;</a> <a id="965" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.html#605" class="Datatype">Dec</a><a id="968" class="Symbol">;</a> <a id="970" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.html#641" class="InductiveConstructor">yes</a><a id="973" class="Symbol">;</a> <a id="975" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.html#668" class="InductiveConstructor">no</a><a id="977" class="Symbol">)</a>
<a id="979" class="Keyword">open</a> <a id="984" class="Keyword">import</a> <a id="991" href="https://agda.github.io/agda-stdlib/v1.1/Data.Product.html" class="Module">Data.Product</a> <a id="1004" class="Keyword">using</a> <a id="1010" class="Symbol">(</a><a id="1011" href="https://agda.github.io/agda-stdlib/v1.1/Data.Product.html#1162" class="Function Operator">_×_</a><a id="1014" class="Symbol">;</a> <a id="1016" href="https://agda.github.io/agda-stdlib/v1.1/Data.Product.html#1364" class="Function">∃</a><a id="1017" class="Symbol">;</a> <a id="1019" href="https://agda.github.io/agda-stdlib/v1.1/Data.Product.html#1783" class="Function">∃-syntax</a><a id="1027" class="Symbol">)</a> <a id="1029" class="Keyword">renaming</a> <a id="1038" class="Symbol">(</a><a id="1039" href="Agda.Builtin.Sigma.html#209" class="InductiveConstructor Operator">_,_</a> <a id="1043" class="Symbol">to</a> <a id="1046" href="Agda.Builtin.Sigma.html#209" class="InductiveConstructor Operator">⟨_,_⟩</a><a id="1051" class="Symbol">)</a>
<a id="1053" class="Keyword">open</a> <a id="1058" class="Keyword">import</a> <a id="1065" href="https://agda.github.io/agda-stdlib/v1.1/Function.html" class="Module">Function</a> <a id="1074" class="Keyword">using</a> <a id="1080" class="Symbol">(</a><a id="1081" href="https://agda.github.io/agda-stdlib/v1.1/Function.html#1099" class="Function Operator">_∘_</a><a id="1084" class="Symbol">)</a>
<a id="1086" class="Keyword">open</a> <a id="1091" class="Keyword">import</a> <a id="1098" href="https://agda.github.io/agda-stdlib/v1.1/Level.html" class="Module">Level</a> <a id="1104" class="Keyword">using</a> <a id="1110" class="Symbol">(</a><a id="1111" href="Agda.Primitive.html#408" class="Postulate">Level</a><a id="1116" class="Symbol">)</a>
<a id="1118" class="Keyword">open</a> <a id="1123" class="Keyword">import</a> <a id="1130" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Isomorphism.md %}{% raw %}" class="Module">plfa.Isomorphism</a> <a id="1147" class="Keyword">using</a> <a id="1153" class="Symbol">(</a><a id="1154" href="plfa.Isomorphism.html#5843" class="Record Operator">_≃_</a><a id="1157" class="Symbol">;</a> <a id="1159" href="plfa.Isomorphism.html#15256" class="Record Operator">_⇔_</a><a id="1162" class="Symbol">)</a>
</pre>{% endraw %}

{::comment}
## Lists
{:/}

## 列表

{::comment}
Lists are defined in Agda as follows:
{:/}

Agda 中的列表如下定义：
{% raw %}<pre class="Agda"><a id="1279" class="Keyword">data</a> <a id="List"></a><a id="1284" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#1284" class="Datatype">List</a> <a id="1289" class="Symbol">(</a><a id="1290" href="plfa.Lists.html#1290" class="Bound">A</a> <a id="1292" class="Symbol">:</a> <a id="1294" class="PrimitiveType">Set</a><a id="1297" class="Symbol">)</a> <a id="1299" class="Symbol">:</a> <a id="1301" class="PrimitiveType">Set</a> <a id="1305" class="Keyword">where</a>
  <a id="List.[]"></a><a id="1313" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#1313" class="InductiveConstructor">[]</a>  <a id="1317" class="Symbol">:</a> <a id="1319" href="plfa.Lists.html#1284" class="Datatype">List</a> <a id="1324" href="plfa.Lists.html#1290" class="Bound">A</a>
  <a id="List._∷_"></a><a id="1328" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#1328" class="InductiveConstructor Operator">_∷_</a> <a id="1332" class="Symbol">:</a> <a id="1334" href="plfa.Lists.html#1290" class="Bound">A</a> <a id="1336" class="Symbol">→</a> <a id="1338" href="plfa.Lists.html#1284" class="Datatype">List</a> <a id="1343" href="plfa.Lists.html#1290" class="Bound">A</a> <a id="1345" class="Symbol">→</a> <a id="1347" href="plfa.Lists.html#1284" class="Datatype">List</a> <a id="1352" href="plfa.Lists.html#1290" class="Bound">A</a>

<a id="1355" class="Keyword">infixr</a> <a id="1362" class="Number">5</a> <a id="1364" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#1328" class="InductiveConstructor Operator">_∷_</a>
</pre>{% endraw %}
{::comment}
Let's unpack this definition. If `A` is a set, then `List A` is a set.
The next two lines tell us that `[]` (pronounced _nil_) is a list of
type `A` (often called the _empty_ list), and that `_∷_` (pronounced
_cons_, short for _constructor_) takes a value of type `A` and a value
of type `List A` and returns a value of type `List A`.  Operator `_∷_`
has precedence level 5 and associates to the right.
{:/}

我们来仔细研究这个定义。如果 `A` 是个集合，那么 `List A` 也是一个集合。接下来的两行告诉我们
`[]` （读作 *nil*）是一个类型为 `A` 的列表（通常被叫做*空*列表），`_∷_`（读作 *cons*，是
*constructor* 的简写）取一个类型为 `A` 的值，和一个类型为 `List A` 的值，返回一个类型为
`List A` 的值。`_∷_` 运算符的优先级是 5，向右结合。

{::comment}
For example,
{:/}

例如：

{% raw %}<pre class="Agda"><a id="2043" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#2043" class="Function">_</a> <a id="2045" class="Symbol">:</a> <a id="2047" href="plfa.Lists.html#1284" class="Datatype">List</a> <a id="2052" href="Agda.Builtin.Nat.html#165" class="Datatype">ℕ</a>
<a id="2054" class="Symbol">_</a> <a id="2056" class="Symbol">=</a> <a id="2058" class="Number">0</a> <a id="2060" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#1328" class="InductiveConstructor Operator">∷</a> <a id="2062" class="Number">1</a> <a id="2064" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="2066" class="Number">2</a> <a id="2068" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="2070" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a>
</pre>{% endraw %}
{::comment}
denotes the list of the first three natural numbers.  Since `_∷_`
associates to the right, the term parses as `0 ∷ (1 ∷ (2 ∷ []))`.
Here `0` is the first element of the list, called the _head_,
and `1 ∷ (2 ∷ [])` is a list of the remaining elements, called the
_tail_. A list is a strange beast: it has a head and a tail,
nothing in between, and the tail is itself another list!
{:/}

表示了一个三个自然数的列表。因为 `_∷_` 向右结合，这一项被解析成 `0 ∷ (1 ∷ (2 ∷ []))`。
在这里，`0` 是列表的第一个元素，称之为*头*（Head），`1 ∷ (2 ∷ [])` 是剩下元素的列表，
称之为*尾*（Tail）。列表是一个奇怪的怪兽：它有一头一尾，中间没有东西，然而它的尾巴又是一个列表！

{::comment}
As we've seen, parameterised types can be translated to
indexed types. The definition above is equivalent to the following:
{:/}

正如我们所见，参数化的类型可以被转换成索引类型。上面的定义与下列等价：

{% raw %}<pre class="Agda"><a id="2825" class="Keyword">data</a> <a id="List′"></a><a id="2830" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#2830" class="Datatype">List′</a> <a id="2836" class="Symbol">:</a> <a id="2838" class="PrimitiveType">Set</a> <a id="2842" class="Symbol">→</a> <a id="2844" class="PrimitiveType">Set</a> <a id="2848" class="Keyword">where</a>
  <a id="List′.[]′"></a><a id="2856" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#2856" class="InductiveConstructor">[]′</a>  <a id="2861" class="Symbol">:</a> <a id="2863" class="Symbol">∀</a> <a id="2865" class="Symbol">{</a><a id="2866" href="plfa.Lists.html#2866" class="Bound">A</a> <a id="2868" class="Symbol">:</a> <a id="2870" class="PrimitiveType">Set</a><a id="2873" class="Symbol">}</a> <a id="2875" class="Symbol">→</a> <a id="2877" href="plfa.Lists.html#2830" class="Datatype">List′</a> <a id="2883" href="plfa.Lists.html#2866" class="Bound">A</a>
  <a id="List′._∷′_"></a><a id="2887" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#2887" class="InductiveConstructor Operator">_∷′_</a> <a id="2892" class="Symbol">:</a> <a id="2894" class="Symbol">∀</a> <a id="2896" class="Symbol">{</a><a id="2897" href="plfa.Lists.html#2897" class="Bound">A</a> <a id="2899" class="Symbol">:</a> <a id="2901" class="PrimitiveType">Set</a><a id="2904" class="Symbol">}</a> <a id="2906" class="Symbol">→</a> <a id="2908" href="plfa.Lists.html#2897" class="Bound">A</a> <a id="2910" class="Symbol">→</a> <a id="2912" href="plfa.Lists.html#2830" class="Datatype">List′</a> <a id="2918" href="plfa.Lists.html#2897" class="Bound">A</a> <a id="2920" class="Symbol">→</a> <a id="2922" href="plfa.Lists.html#2830" class="Datatype">List′</a> <a id="2928" href="plfa.Lists.html#2897" class="Bound">A</a>
</pre>{% endraw %}
{::comment}
Each constructor takes the parameter as an implicit argument.
Thus, our example list could also be written:
{:/}

每个构造子将参数作为隐式参数。因此我们列表的例子也可以写作：

{% raw %}<pre class="Agda"><a id="3097" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#3097" class="Function">_</a> <a id="3099" class="Symbol">:</a> <a id="3101" href="plfa.Lists.html#1284" class="Datatype">List</a> <a id="3106" href="Agda.Builtin.Nat.html#165" class="Datatype">ℕ</a>
<a id="3108" class="Symbol">_</a> <a id="3110" class="Symbol">=</a> <a id="3112" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#1328" class="InductiveConstructor Operator">_∷_</a> <a id="3116" class="Symbol">{</a><a id="3117" href="Agda.Builtin.Nat.html#165" class="Datatype">ℕ</a><a id="3118" class="Symbol">}</a> <a id="3120" class="Number">0</a> <a id="3122" class="Symbol">(</a><a id="3123" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">_∷_</a> <a id="3127" class="Symbol">{</a><a id="3128" href="Agda.Builtin.Nat.html#165" class="Datatype">ℕ</a><a id="3129" class="Symbol">}</a> <a id="3131" class="Number">1</a> <a id="3133" class="Symbol">(</a><a id="3134" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">_∷_</a> <a id="3138" class="Symbol">{</a><a id="3139" href="Agda.Builtin.Nat.html#165" class="Datatype">ℕ</a><a id="3140" class="Symbol">}</a> <a id="3142" class="Number">2</a> <a id="3144" class="Symbol">(</a><a id="3145" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a> <a id="3148" class="Symbol">{</a><a id="3149" href="Agda.Builtin.Nat.html#165" class="Datatype">ℕ</a><a id="3150" class="Symbol">})))</a>
</pre>{% endraw %}
{::comment}
where here we have provided the implicit parameters explicitly.
{:/}

此处我们将隐式参数显式地声明。

{::comment}
Including the pragma:
{:/}

包含下面的编译器指令

    {-# BUILTIN LIST List #-}

{::comment}
tells Agda that the type `List` corresponds to the Haskell type
list, and the constructors `[]` and `_∷_` correspond to nil and
cons respectively, allowing a more efficient representation of lists.
{:/}

告诉 Agda，`List` 类型对应了 Haskell 的列表类型，构造子 `[]` 和 `_∷_`
分别代表了 nil 和 cons，这可以让列表的表示更加的有效率。

{::comment}
## List syntax
{:/}

## 列表语法

{::comment}
We can write lists more conveniently by introducing the following definitions:
{:/}

我们可以用下面的定义，更简便地表示列表：

{% raw %}<pre class="Agda"><a id="3810" class="Keyword">pattern</a> <a id="[_]"></a><a id="3818" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#3818" class="InductiveConstructor Operator">[_]</a> <a id="3822" href="plfa.Lists.html#3826" class="Bound">z</a> <a id="3824" class="Symbol">=</a> <a id="3826" href="plfa.Lists.html#3826" class="Bound">z</a> <a id="3828" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="3830" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a>
<a id="3833" class="Keyword">pattern</a> <a id="[_,_]"></a><a id="3841" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#3841" class="InductiveConstructor Operator">[_,_]</a> <a id="3847" href="plfa.Lists.html#3853" class="Bound">y</a> <a id="3849" href="plfa.Lists.html#3857" class="Bound">z</a> <a id="3851" class="Symbol">=</a> <a id="3853" href="plfa.Lists.html#3853" class="Bound">y</a> <a id="3855" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="3857" href="plfa.Lists.html#3857" class="Bound">z</a> <a id="3859" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="3861" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a>
<a id="3864" class="Keyword">pattern</a> <a id="[_,_,_]"></a><a id="3872" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#3872" class="InductiveConstructor Operator">[_,_,_]</a> <a id="3880" href="plfa.Lists.html#3888" class="Bound">x</a> <a id="3882" href="plfa.Lists.html#3892" class="Bound">y</a> <a id="3884" href="plfa.Lists.html#3896" class="Bound">z</a> <a id="3886" class="Symbol">=</a> <a id="3888" href="plfa.Lists.html#3888" class="Bound">x</a> <a id="3890" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="3892" href="plfa.Lists.html#3892" class="Bound">y</a> <a id="3894" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="3896" href="plfa.Lists.html#3896" class="Bound">z</a> <a id="3898" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="3900" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a>
<a id="3903" class="Keyword">pattern</a> <a id="[_,_,_,_]"></a><a id="3911" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#3911" class="InductiveConstructor Operator">[_,_,_,_]</a> <a id="3921" href="plfa.Lists.html#3931" class="Bound">w</a> <a id="3923" href="plfa.Lists.html#3935" class="Bound">x</a> <a id="3925" href="plfa.Lists.html#3939" class="Bound">y</a> <a id="3927" href="plfa.Lists.html#3943" class="Bound">z</a> <a id="3929" class="Symbol">=</a> <a id="3931" href="plfa.Lists.html#3931" class="Bound">w</a> <a id="3933" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="3935" href="plfa.Lists.html#3935" class="Bound">x</a> <a id="3937" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="3939" href="plfa.Lists.html#3939" class="Bound">y</a> <a id="3941" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="3943" href="plfa.Lists.html#3943" class="Bound">z</a> <a id="3945" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="3947" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a>
<a id="3950" class="Keyword">pattern</a> <a id="[_,_,_,_,_]"></a><a id="3958" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#3958" class="InductiveConstructor Operator">[_,_,_,_,_]</a> <a id="3970" href="plfa.Lists.html#3982" class="Bound">v</a> <a id="3972" href="plfa.Lists.html#3986" class="Bound">w</a> <a id="3974" href="plfa.Lists.html#3990" class="Bound">x</a> <a id="3976" href="plfa.Lists.html#3994" class="Bound">y</a> <a id="3978" href="plfa.Lists.html#3998" class="Bound">z</a> <a id="3980" class="Symbol">=</a> <a id="3982" href="plfa.Lists.html#3982" class="Bound">v</a> <a id="3984" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="3986" href="plfa.Lists.html#3986" class="Bound">w</a> <a id="3988" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="3990" href="plfa.Lists.html#3990" class="Bound">x</a> <a id="3992" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="3994" href="plfa.Lists.html#3994" class="Bound">y</a> <a id="3996" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="3998" href="plfa.Lists.html#3998" class="Bound">z</a> <a id="4000" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="4002" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a>
<a id="4005" class="Keyword">pattern</a> <a id="[_,_,_,_,_,_]"></a><a id="4013" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#4013" class="InductiveConstructor Operator">[_,_,_,_,_,_]</a> <a id="4027" href="plfa.Lists.html#4041" class="Bound">u</a> <a id="4029" href="plfa.Lists.html#4045" class="Bound">v</a> <a id="4031" href="plfa.Lists.html#4049" class="Bound">w</a> <a id="4033" href="plfa.Lists.html#4053" class="Bound">x</a> <a id="4035" href="plfa.Lists.html#4057" class="Bound">y</a> <a id="4037" href="plfa.Lists.html#4061" class="Bound">z</a> <a id="4039" class="Symbol">=</a> <a id="4041" href="plfa.Lists.html#4041" class="Bound">u</a> <a id="4043" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="4045" href="plfa.Lists.html#4045" class="Bound">v</a> <a id="4047" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="4049" href="plfa.Lists.html#4049" class="Bound">w</a> <a id="4051" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="4053" href="plfa.Lists.html#4053" class="Bound">x</a> <a id="4055" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="4057" href="plfa.Lists.html#4057" class="Bound">y</a> <a id="4059" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="4061" href="plfa.Lists.html#4061" class="Bound">z</a> <a id="4063" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="4065" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a>
</pre>{% endraw %}
{::comment}
This is our first use of pattern declarations.  For instance,
the third line tells us that `[ x , y , z ]` is equivalent to
`x ∷ y ∷ z ∷ []`, and permits the former to appear either in
a pattern on the left-hand side of an equation, or a term
on the right-hand side of an equation.
{:/}

这是我们第一次使用模式声明。举例来说，第三行告诉我们 `[ x , y , z ]` 等价于
`x ∷ y ∷ z ∷ []`。前者可以在模式或者等式的左手边，或者是等式右手边的项中出现。

{::comment}
## Append
{:/}

## 附加

{::comment}
Our first function on lists is written `_++_` and pronounced
_append_:
{:/}

我们对于列表的第一个函数写作 `_++_`，读作*附加*（Append）：

{% raw %}<pre class="Agda"><a id="4636" class="Keyword">infixr</a> <a id="4643" class="Number">5</a> <a id="4645" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#4651" class="Function Operator">_++_</a>

<a id="_++_"></a><a id="4651" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#4651" class="Function Operator">_++_</a> <a id="4656" class="Symbol">:</a> <a id="4658" class="Symbol">∀</a> <a id="4660" class="Symbol">{</a><a id="4661" href="plfa.Lists.html#4661" class="Bound">A</a> <a id="4663" class="Symbol">:</a> <a id="4665" class="PrimitiveType">Set</a><a id="4668" class="Symbol">}</a> <a id="4670" class="Symbol">→</a> <a id="4672" href="plfa.Lists.html#1284" class="Datatype">List</a> <a id="4677" href="plfa.Lists.html#4661" class="Bound">A</a> <a id="4679" class="Symbol">→</a> <a id="4681" href="plfa.Lists.html#1284" class="Datatype">List</a> <a id="4686" href="plfa.Lists.html#4661" class="Bound">A</a> <a id="4688" class="Symbol">→</a> <a id="4690" href="plfa.Lists.html#1284" class="Datatype">List</a> <a id="4695" href="plfa.Lists.html#4661" class="Bound">A</a>
<a id="4697" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#1313" class="InductiveConstructor">[]</a>       <a id="4706" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="4709" href="plfa.Lists.html#4709" class="Bound">ys</a>  <a id="4713" class="Symbol">=</a>  <a id="4716" href="plfa.Lists.html#4709" class="Bound">ys</a>
<a id="4719" class="Symbol">(</a><a id="4720" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#4720" class="Bound">x</a> <a id="4722" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="4724" href="plfa.Lists.html#4724" class="Bound">xs</a><a id="4726" class="Symbol">)</a> <a id="4728" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="4731" href="plfa.Lists.html#4731" class="Bound">ys</a>  <a id="4735" class="Symbol">=</a>  <a id="4738" href="plfa.Lists.html#4720" class="Bound">x</a> <a id="4740" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="4742" class="Symbol">(</a><a id="4743" href="plfa.Lists.html#4724" class="Bound">xs</a> <a id="4746" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="4749" href="plfa.Lists.html#4731" class="Bound">ys</a><a id="4751" class="Symbol">)</a>
</pre>{% endraw %}
{::comment}
The type `A` is an implicit argument to append, making it a
_polymorphic_ function (one that can be used at many types).  The
empty list appended to another list yields the other list.  A
non-empty list appended to another list yields a list with head the
same as the head of the first list and tail the same as the tail of
the first list appended to the second list.
{:/}

`A` 类型是附加的隐式参数，这让这个函数变为一个*多态*（Polymorphic）函数
（即可以用作多种类型）。空列表附加到另一个列表得到是第二个列表。非空列表附加到
另一个列表，得到的列表的头是第一个列表的头，尾是第一个列表的尾附加至第二个列表的结果。

{::comment}
Here is an example, showing how to compute the result
of appending two lists:
{:/}

我们举个例子，来展示将两个列表附加的计算过程：

{% raw %}<pre class="Agda"><a id="5399" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#5399" class="Function">_</a> <a id="5401" class="Symbol">:</a> <a id="5403" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">[</a> <a id="5405" class="Number">0</a> <a id="5407" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">,</a> <a id="5409" class="Number">1</a> <a id="5411" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">,</a> <a id="5413" class="Number">2</a> <a id="5415" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">]</a> <a id="5417" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="5420" href="plfa.Lists.html#3841" class="InductiveConstructor Operator">[</a> <a id="5422" class="Number">3</a> <a id="5424" href="plfa.Lists.html#3841" class="InductiveConstructor Operator">,</a> <a id="5426" class="Number">4</a> <a id="5428" href="plfa.Lists.html#3841" class="InductiveConstructor Operator">]</a> <a id="5430" href="Agda.Builtin.Equality.html#125" class="Datatype Operator">≡</a> <a id="5432" href="plfa.Lists.html#3958" class="InductiveConstructor Operator">[</a> <a id="5434" class="Number">0</a> <a id="5436" href="plfa.Lists.html#3958" class="InductiveConstructor Operator">,</a> <a id="5438" class="Number">1</a> <a id="5440" href="plfa.Lists.html#3958" class="InductiveConstructor Operator">,</a> <a id="5442" class="Number">2</a> <a id="5444" href="plfa.Lists.html#3958" class="InductiveConstructor Operator">,</a> <a id="5446" class="Number">3</a> <a id="5448" href="plfa.Lists.html#3958" class="InductiveConstructor Operator">,</a> <a id="5450" class="Number">4</a> <a id="5452" href="plfa.Lists.html#3958" class="InductiveConstructor Operator">]</a>
<a id="5454" class="Symbol">_</a> <a id="5456" class="Symbol">=</a>
  <a id="5460" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2597" class="Function Operator">begin</a>
    <a id="5470" class="Number">0</a> <a id="5472" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#1328" class="InductiveConstructor Operator">∷</a> <a id="5474" class="Number">1</a> <a id="5476" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="5478" class="Number">2</a> <a id="5480" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="5482" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a> <a id="5485" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="5488" class="Number">3</a> <a id="5490" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="5492" class="Number">4</a> <a id="5494" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="5496" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a>
  <a id="5501" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="5509" class="Number">0</a> <a id="5511" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#1328" class="InductiveConstructor Operator">∷</a> <a id="5513" class="Symbol">(</a><a id="5514" class="Number">1</a> <a id="5516" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="5518" class="Number">2</a> <a id="5520" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="5522" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a> <a id="5525" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="5528" class="Number">3</a> <a id="5530" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="5532" class="Number">4</a> <a id="5534" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="5536" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a><a id="5538" class="Symbol">)</a>
  <a id="5542" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="5550" class="Number">0</a> <a id="5552" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#1328" class="InductiveConstructor Operator">∷</a> <a id="5554" class="Number">1</a> <a id="5556" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="5558" class="Symbol">(</a><a id="5559" class="Number">2</a> <a id="5561" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="5563" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a> <a id="5566" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="5569" class="Number">3</a> <a id="5571" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="5573" class="Number">4</a> <a id="5575" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="5577" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a><a id="5579" class="Symbol">)</a>
  <a id="5583" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="5591" class="Number">0</a> <a id="5593" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#1328" class="InductiveConstructor Operator">∷</a> <a id="5595" class="Number">1</a> <a id="5597" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="5599" class="Number">2</a> <a id="5601" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="5603" class="Symbol">(</a><a id="5604" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a> <a id="5607" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="5610" class="Number">3</a> <a id="5612" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="5614" class="Number">4</a> <a id="5616" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="5618" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a><a id="5620" class="Symbol">)</a>
  <a id="5624" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="5632" class="Number">0</a> <a id="5634" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#1328" class="InductiveConstructor Operator">∷</a> <a id="5636" class="Number">1</a> <a id="5638" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="5640" class="Number">2</a> <a id="5642" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="5644" class="Number">3</a> <a id="5646" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="5648" class="Number">4</a> <a id="5650" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="5652" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a>
  <a id="5657" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2892" class="Function Operator">∎</a>
</pre>{% endraw %}
{::comment}
Appending two lists requires time linear in the
number of elements in the first list.
{:/}

附加两个列表需要对于第一个列表元素个数线性的时间。


{::comment}
## Reasoning about append
{:/}

## 论证附加

{::comment}
We can reason about lists in much the same way that we reason
about numbers.  Here is the proof that append is associative:
{:/}

我们可以与用论证数几乎相同的方法来论证列表。下面是附加满足结合律的证明：
{% raw %}<pre class="Agda"><a id="++-assoc"></a><a id="6032" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#6032" class="Function">++-assoc</a> <a id="6041" class="Symbol">:</a> <a id="6043" class="Symbol">∀</a> <a id="6045" class="Symbol">{</a><a id="6046" href="plfa.Lists.html#6046" class="Bound">A</a> <a id="6048" class="Symbol">:</a> <a id="6050" class="PrimitiveType">Set</a><a id="6053" class="Symbol">}</a> <a id="6055" class="Symbol">(</a><a id="6056" href="plfa.Lists.html#6056" class="Bound">xs</a> <a id="6059" href="plfa.Lists.html#6059" class="Bound">ys</a> <a id="6062" href="plfa.Lists.html#6062" class="Bound">zs</a> <a id="6065" class="Symbol">:</a> <a id="6067" href="plfa.Lists.html#1284" class="Datatype">List</a> <a id="6072" href="plfa.Lists.html#6046" class="Bound">A</a><a id="6073" class="Symbol">)</a>
  <a id="6077" class="Symbol">→</a> <a id="6079" class="Symbol">(</a><a id="6080" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#6056" class="Bound">xs</a> <a id="6083" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="6086" href="plfa.Lists.html#6059" class="Bound">ys</a><a id="6088" class="Symbol">)</a> <a id="6090" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="6093" href="plfa.Lists.html#6062" class="Bound">zs</a> <a id="6096" href="Agda.Builtin.Equality.html#125" class="Datatype Operator">≡</a> <a id="6098" href="plfa.Lists.html#6056" class="Bound">xs</a> <a id="6101" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="6104" class="Symbol">(</a><a id="6105" href="plfa.Lists.html#6059" class="Bound">ys</a> <a id="6108" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="6111" href="plfa.Lists.html#6062" class="Bound">zs</a><a id="6113" class="Symbol">)</a>
<a id="6115" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#6032" class="Function">++-assoc</a> <a id="6124" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a> <a id="6127" href="plfa.Lists.html#6127" class="Bound">ys</a> <a id="6130" href="plfa.Lists.html#6130" class="Bound">zs</a> <a id="6133" class="Symbol">=</a>
  <a id="6137" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2597" class="Function Operator">begin</a>
    <a id="6147" class="Symbol">(</a><a id="6148" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#1313" class="InductiveConstructor">[]</a> <a id="6151" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="6154" href="plfa.Lists.html#6127" class="Bound">ys</a><a id="6156" class="Symbol">)</a> <a id="6158" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="6161" href="plfa.Lists.html#6130" class="Bound">zs</a>
  <a id="6166" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="6174" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#6127" class="Bound">ys</a> <a id="6177" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="6180" href="plfa.Lists.html#6130" class="Bound">zs</a>
  <a id="6185" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="6193" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#1313" class="InductiveConstructor">[]</a> <a id="6196" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="6199" class="Symbol">(</a><a id="6200" href="plfa.Lists.html#6127" class="Bound">ys</a> <a id="6203" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="6206" href="plfa.Lists.html#6130" class="Bound">zs</a><a id="6208" class="Symbol">)</a>
  <a id="6212" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2892" class="Function Operator">∎</a>
<a id="6214" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#6032" class="Function">++-assoc</a> <a id="6223" class="Symbol">(</a><a id="6224" href="plfa.Lists.html#6224" class="Bound">x</a> <a id="6226" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="6228" href="plfa.Lists.html#6228" class="Bound">xs</a><a id="6230" class="Symbol">)</a> <a id="6232" href="plfa.Lists.html#6232" class="Bound">ys</a> <a id="6235" href="plfa.Lists.html#6235" class="Bound">zs</a> <a id="6238" class="Symbol">=</a>
  <a id="6242" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2597" class="Function Operator">begin</a>
    <a id="6252" class="Symbol">(</a><a id="6253" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#6224" class="Bound">x</a> <a id="6255" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="6257" href="plfa.Lists.html#6228" class="Bound">xs</a> <a id="6260" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="6263" href="plfa.Lists.html#6232" class="Bound">ys</a><a id="6265" class="Symbol">)</a> <a id="6267" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="6270" href="plfa.Lists.html#6235" class="Bound">zs</a>
  <a id="6275" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="6283" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#6224" class="Bound">x</a> <a id="6285" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="6287" class="Symbol">(</a><a id="6288" href="plfa.Lists.html#6228" class="Bound">xs</a> <a id="6291" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="6294" href="plfa.Lists.html#6232" class="Bound">ys</a><a id="6296" class="Symbol">)</a> <a id="6298" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="6301" href="plfa.Lists.html#6235" class="Bound">zs</a>
  <a id="6306" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="6314" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#6224" class="Bound">x</a> <a id="6316" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="6318" class="Symbol">((</a><a id="6320" href="plfa.Lists.html#6228" class="Bound">xs</a> <a id="6323" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="6326" href="plfa.Lists.html#6232" class="Bound">ys</a><a id="6328" class="Symbol">)</a> <a id="6330" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="6333" href="plfa.Lists.html#6235" class="Bound">zs</a><a id="6335" class="Symbol">)</a>
  <a id="6339" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2714" class="Function Operator">≡⟨</a> <a id="6342" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#1090" class="Function">cong</a> <a id="6347" class="Symbol">(</a><a id="6348" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#6224" class="Bound">x</a> <a id="6350" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷_</a><a id="6352" class="Symbol">)</a> <a id="6354" class="Symbol">(</a><a id="6355" href="plfa.Lists.html#6032" class="Function">++-assoc</a> <a id="6364" href="plfa.Lists.html#6228" class="Bound">xs</a> <a id="6367" href="plfa.Lists.html#6232" class="Bound">ys</a> <a id="6370" href="plfa.Lists.html#6235" class="Bound">zs</a><a id="6372" class="Symbol">)</a> <a id="6374" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2714" class="Function Operator">⟩</a>
    <a id="6380" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#6224" class="Bound">x</a> <a id="6382" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="6384" class="Symbol">(</a><a id="6385" href="plfa.Lists.html#6228" class="Bound">xs</a> <a id="6388" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="6391" class="Symbol">(</a><a id="6392" href="plfa.Lists.html#6232" class="Bound">ys</a> <a id="6395" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="6398" href="plfa.Lists.html#6235" class="Bound">zs</a><a id="6400" class="Symbol">))</a>
  <a id="6405" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="6413" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#6224" class="Bound">x</a> <a id="6415" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="6417" href="plfa.Lists.html#6228" class="Bound">xs</a> <a id="6420" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="6423" class="Symbol">(</a><a id="6424" href="plfa.Lists.html#6232" class="Bound">ys</a> <a id="6427" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="6430" href="plfa.Lists.html#6235" class="Bound">zs</a><a id="6432" class="Symbol">)</a>
  <a id="6436" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2892" class="Function Operator">∎</a>
</pre>{% endraw %}
{::comment}
The proof is by induction on the first argument. The base case instantiates
to `[]`, and follows by straightforward computation.
The inductive case instantiates to `x ∷ xs`,
and follows by straightforward computation combined with the
inductive hypothesis.  As usual, the inductive hypothesis is indicated by a recursive
invocation of the proof, in this case `++-assoc xs ys zs`.
{:/}

证明对于第一个参数进行归纳。起始步骤将列表实例化为 `[]`，由直接的运算可证。
归纳步骤将列表实例化为 `x ∷ xs`，由直接的运算配合归纳假设可证。
与往常一样，归纳假设由递归使用证明函数来表明，此处为 `++-assoc xs ys zs`。

{::comment}
Recall that Agda supports [sections]({{ site.baseurl }}/Induction/#sections).
Applying `cong (x ∷_)` promotes the inductive hypothesis:
{:/}

回忆到 Agda 支持[片段]({{ site.baseurl }}/Induction/#sections)。使用 `cong (x ∷_)`
可以将归纳假设：

    (xs ++ ys) ++ zs ≡ xs ++ (ys ++ zs)

{::comment}
to the equality:
{:/}

提升至等式：

    x ∷ ((xs ++ ys) ++ zs) ≡ x ∷ (xs ++ (ys ++ zs))

{::comment}
which is needed in the proof.
{:/}

即证明中所需。

{::comment}
It is also easy to show that `[]` is a left and right identity for `_++_`.
That it is a left identity is immediate from the definition:
{:/}

我们也可以简单地证明 `[]` 是 `_++_` 的左幺元和右幺元。
左幺元的证明从定义中即可得：

{% raw %}<pre class="Agda"><a id="++-identityˡ"></a><a id="7608" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#7608" class="Function">++-identityˡ</a> <a id="7621" class="Symbol">:</a> <a id="7623" class="Symbol">∀</a> <a id="7625" class="Symbol">{</a><a id="7626" href="plfa.Lists.html#7626" class="Bound">A</a> <a id="7628" class="Symbol">:</a> <a id="7630" class="PrimitiveType">Set</a><a id="7633" class="Symbol">}</a> <a id="7635" class="Symbol">(</a><a id="7636" href="plfa.Lists.html#7636" class="Bound">xs</a> <a id="7639" class="Symbol">:</a> <a id="7641" href="plfa.Lists.html#1284" class="Datatype">List</a> <a id="7646" href="plfa.Lists.html#7626" class="Bound">A</a><a id="7647" class="Symbol">)</a> <a id="7649" class="Symbol">→</a> <a id="7651" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a> <a id="7654" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="7657" href="plfa.Lists.html#7636" class="Bound">xs</a> <a id="7660" href="Agda.Builtin.Equality.html#125" class="Datatype Operator">≡</a> <a id="7662" href="plfa.Lists.html#7636" class="Bound">xs</a>
<a id="7665" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#7608" class="Function">++-identityˡ</a> <a id="7678" href="plfa.Lists.html#7678" class="Bound">xs</a> <a id="7681" class="Symbol">=</a>
  <a id="7685" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2597" class="Function Operator">begin</a>
    <a id="7695" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#1313" class="InductiveConstructor">[]</a> <a id="7698" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="7701" href="plfa.Lists.html#7678" class="Bound">xs</a>
  <a id="7706" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="7714" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#7678" class="Bound">xs</a>
  <a id="7719" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2892" class="Function Operator">∎</a>
</pre>{% endraw %}
{::comment}
That it is a right identity follows by simple induction:
{:/}

右幺元的证明可由简单的归纳得到：
{% raw %}<pre class="Agda"><a id="++-identityʳ"></a><a id="7822" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#7822" class="Function">++-identityʳ</a> <a id="7835" class="Symbol">:</a> <a id="7837" class="Symbol">∀</a> <a id="7839" class="Symbol">{</a><a id="7840" href="plfa.Lists.html#7840" class="Bound">A</a> <a id="7842" class="Symbol">:</a> <a id="7844" class="PrimitiveType">Set</a><a id="7847" class="Symbol">}</a> <a id="7849" class="Symbol">(</a><a id="7850" href="plfa.Lists.html#7850" class="Bound">xs</a> <a id="7853" class="Symbol">:</a> <a id="7855" href="plfa.Lists.html#1284" class="Datatype">List</a> <a id="7860" href="plfa.Lists.html#7840" class="Bound">A</a><a id="7861" class="Symbol">)</a> <a id="7863" class="Symbol">→</a> <a id="7865" href="plfa.Lists.html#7850" class="Bound">xs</a> <a id="7868" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="7871" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a> <a id="7874" href="Agda.Builtin.Equality.html#125" class="Datatype Operator">≡</a> <a id="7876" href="plfa.Lists.html#7850" class="Bound">xs</a>
<a id="7879" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#7822" class="Function">++-identityʳ</a> <a id="7892" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a> <a id="7895" class="Symbol">=</a>
  <a id="7899" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2597" class="Function Operator">begin</a>
    <a id="7909" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#1313" class="InductiveConstructor">[]</a> <a id="7912" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="7915" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a>
  <a id="7920" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="7928" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#1313" class="InductiveConstructor">[]</a>
  <a id="7933" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2892" class="Function Operator">∎</a>
<a id="7935" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#7822" class="Function">++-identityʳ</a> <a id="7948" class="Symbol">(</a><a id="7949" href="plfa.Lists.html#7949" class="Bound">x</a> <a id="7951" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="7953" href="plfa.Lists.html#7953" class="Bound">xs</a><a id="7955" class="Symbol">)</a> <a id="7957" class="Symbol">=</a>
  <a id="7961" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2597" class="Function Operator">begin</a>
    <a id="7971" class="Symbol">(</a><a id="7972" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#7949" class="Bound">x</a> <a id="7974" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="7976" href="plfa.Lists.html#7953" class="Bound">xs</a><a id="7978" class="Symbol">)</a> <a id="7980" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="7983" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a>
  <a id="7988" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="7996" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#7949" class="Bound">x</a> <a id="7998" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="8000" class="Symbol">(</a><a id="8001" href="plfa.Lists.html#7953" class="Bound">xs</a> <a id="8004" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="8007" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a><a id="8009" class="Symbol">)</a>
  <a id="8013" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2714" class="Function Operator">≡⟨</a> <a id="8016" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#1090" class="Function">cong</a> <a id="8021" class="Symbol">(</a><a id="8022" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#7949" class="Bound">x</a> <a id="8024" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷_</a><a id="8026" class="Symbol">)</a> <a id="8028" class="Symbol">(</a><a id="8029" href="plfa.Lists.html#7822" class="Function">++-identityʳ</a> <a id="8042" href="plfa.Lists.html#7953" class="Bound">xs</a><a id="8044" class="Symbol">)</a> <a id="8046" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2714" class="Function Operator">⟩</a>
    <a id="8052" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#7949" class="Bound">x</a> <a id="8054" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="8056" href="plfa.Lists.html#7953" class="Bound">xs</a>
  <a id="8061" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2892" class="Function Operator">∎</a>
</pre>{% endraw %}
{::comment}
As we will see later,
these three properties establish that `_++_` and `[]` form
a _monoid_ over lists.
{:/}

我们之后会了解到，这三条性质表明了 `_++_` 和 `[]` 在列表上构成了一个*幺半群*（Monoid）。

{::comment}
## Length
{:/}

## 长度

{::comment}
Our next function finds the length of a list:
{:/}

在下一个函数里，我们来寻找列表的长度：

{% raw %}<pre class="Agda"><a id="length"></a><a id="8371" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#8371" class="Function">length</a> <a id="8378" class="Symbol">:</a> <a id="8380" class="Symbol">∀</a> <a id="8382" class="Symbol">{</a><a id="8383" href="plfa.Lists.html#8383" class="Bound">A</a> <a id="8385" class="Symbol">:</a> <a id="8387" class="PrimitiveType">Set</a><a id="8390" class="Symbol">}</a> <a id="8392" class="Symbol">→</a> <a id="8394" href="plfa.Lists.html#1284" class="Datatype">List</a> <a id="8399" href="plfa.Lists.html#8383" class="Bound">A</a> <a id="8401" class="Symbol">→</a> <a id="8403" href="Agda.Builtin.Nat.html#165" class="Datatype">ℕ</a>
<a id="8405" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#8371" class="Function">length</a> <a id="8412" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a>        <a id="8422" class="Symbol">=</a>  <a id="8425" href="Agda.Builtin.Nat.html#183" class="InductiveConstructor">zero</a>
<a id="8430" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#8371" class="Function">length</a> <a id="8437" class="Symbol">(</a><a id="8438" href="plfa.Lists.html#8438" class="Bound">x</a> <a id="8440" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="8442" href="plfa.Lists.html#8442" class="Bound">xs</a><a id="8444" class="Symbol">)</a>  <a id="8447" class="Symbol">=</a>  <a id="8450" href="Agda.Builtin.Nat.html#196" class="InductiveConstructor">suc</a> <a id="8454" class="Symbol">(</a><a id="8455" href="plfa.Lists.html#8371" class="Function">length</a> <a id="8462" href="plfa.Lists.html#8442" class="Bound">xs</a><a id="8464" class="Symbol">)</a>
</pre>{% endraw %}
{::comment}
Again, it takes an implicit parameter `A`.
The length of the empty list is zero.
The length of a non-empty list
is one greater than the length of the tail of the list.
{:/}

同样，它取一个隐式参数 `A`。
空列表的长度为零。非空列表的长度比其尾列表长度多一。

{::comment}
Here is an example showing how to compute the length of a list:
{:/}

我们用下面的例子来展示如何计算列表的长度：
{% raw %}<pre class="Agda"><a id="8810" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#8810" class="Function">_</a> <a id="8812" class="Symbol">:</a> <a id="8814" href="plfa.Lists.html#8371" class="Function">length</a> <a id="8821" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">[</a> <a id="8823" class="Number">0</a> <a id="8825" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">,</a> <a id="8827" class="Number">1</a> <a id="8829" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">,</a> <a id="8831" class="Number">2</a> <a id="8833" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">]</a> <a id="8835" href="Agda.Builtin.Equality.html#125" class="Datatype Operator">≡</a> <a id="8837" class="Number">3</a>
<a id="8839" class="Symbol">_</a> <a id="8841" class="Symbol">=</a>
  <a id="8845" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2597" class="Function Operator">begin</a>
    <a id="8855" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#8371" class="Function">length</a> <a id="8862" class="Symbol">(</a><a id="8863" class="Number">0</a> <a id="8865" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="8867" class="Number">1</a> <a id="8869" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="8871" class="Number">2</a> <a id="8873" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="8875" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a><a id="8877" class="Symbol">)</a>
  <a id="8881" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="8889" href="Agda.Builtin.Nat.html#196" class="InductiveConstructor">suc</a> <a id="8893" class="Symbol">(</a><a id="8894" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#8371" class="Function">length</a> <a id="8901" class="Symbol">(</a><a id="8902" class="Number">1</a> <a id="8904" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="8906" class="Number">2</a> <a id="8908" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="8910" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a><a id="8912" class="Symbol">))</a>
  <a id="8917" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="8925" href="Agda.Builtin.Nat.html#196" class="InductiveConstructor">suc</a> <a id="8929" class="Symbol">(</a><a id="8930" href="Agda.Builtin.Nat.html#196" class="InductiveConstructor">suc</a> <a id="8934" class="Symbol">(</a><a id="8935" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#8371" class="Function">length</a> <a id="8942" class="Symbol">(</a><a id="8943" class="Number">2</a> <a id="8945" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="8947" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a><a id="8949" class="Symbol">)))</a>
  <a id="8955" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="8963" href="Agda.Builtin.Nat.html#196" class="InductiveConstructor">suc</a> <a id="8967" class="Symbol">(</a><a id="8968" href="Agda.Builtin.Nat.html#196" class="InductiveConstructor">suc</a> <a id="8972" class="Symbol">(</a><a id="8973" href="Agda.Builtin.Nat.html#196" class="InductiveConstructor">suc</a> <a id="8977" class="Symbol">(</a><a id="8978" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#8371" class="Function">length</a> <a id="8985" class="Symbol">{</a><a id="8986" href="Agda.Builtin.Nat.html#165" class="Datatype">ℕ</a><a id="8987" class="Symbol">}</a> <a id="8989" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a><a id="8991" class="Symbol">)))</a>
  <a id="8997" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="9005" href="Agda.Builtin.Nat.html#196" class="InductiveConstructor">suc</a> <a id="9009" class="Symbol">(</a><a id="9010" href="Agda.Builtin.Nat.html#196" class="InductiveConstructor">suc</a> <a id="9014" class="Symbol">(</a><a id="9015" href="Agda.Builtin.Nat.html#196" class="InductiveConstructor">suc</a> <a id="9019" href="Agda.Builtin.Nat.html#183" class="InductiveConstructor">zero</a><a id="9023" class="Symbol">))</a>
  <a id="9028" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2892" class="Function Operator">∎</a>
</pre>{% endraw %}
{::comment}
Computing the length of a list requires time
linear in the number of elements in the list.
{:/}

计算列表的长度需要关于列表元素个数线性的时间。

{::comment}
In the second-to-last line, we cannot write simply `length []` but
must instead write `length {ℕ} []`.  Since `[]` has no elements, Agda
has insufficient information to infer the implicit parameter.
{:/}

在倒数第二行中，我们不可以直接写 `length []`，而需要写 `length {ℕ} []`。
因为 `[]` 没有元素，Agda 没有足够的信息来推导其隐式参数。

{::comment}
## Reasoning about length
{:/}

## 论证长度

{::comment}
The length of one list appended to another is the
sum of the lengths of the lists:
{:/}

两个附加在一起的列表的长度是两列表长度之和：

{% raw %}<pre class="Agda"><a id="length-++"></a><a id="9655" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#9655" class="Function">length-++</a> <a id="9665" class="Symbol">:</a> <a id="9667" class="Symbol">∀</a> <a id="9669" class="Symbol">{</a><a id="9670" href="plfa.Lists.html#9670" class="Bound">A</a> <a id="9672" class="Symbol">:</a> <a id="9674" class="PrimitiveType">Set</a><a id="9677" class="Symbol">}</a> <a id="9679" class="Symbol">(</a><a id="9680" href="plfa.Lists.html#9680" class="Bound">xs</a> <a id="9683" href="plfa.Lists.html#9683" class="Bound">ys</a> <a id="9686" class="Symbol">:</a> <a id="9688" href="plfa.Lists.html#1284" class="Datatype">List</a> <a id="9693" href="plfa.Lists.html#9670" class="Bound">A</a><a id="9694" class="Symbol">)</a>
  <a id="9698" class="Symbol">→</a> <a id="9700" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#8371" class="Function">length</a> <a id="9707" class="Symbol">(</a><a id="9708" href="plfa.Lists.html#9680" class="Bound">xs</a> <a id="9711" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="9714" href="plfa.Lists.html#9683" class="Bound">ys</a><a id="9716" class="Symbol">)</a> <a id="9718" href="Agda.Builtin.Equality.html#125" class="Datatype Operator">≡</a> <a id="9720" href="plfa.Lists.html#8371" class="Function">length</a> <a id="9727" href="plfa.Lists.html#9680" class="Bound">xs</a> <a id="9730" href="Agda.Builtin.Nat.html#298" class="Primitive Operator">+</a> <a id="9732" href="plfa.Lists.html#8371" class="Function">length</a> <a id="9739" href="plfa.Lists.html#9683" class="Bound">ys</a>
<a id="9742" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#9655" class="Function">length-++</a> <a id="9752" class="Symbol">{</a><a id="9753" href="plfa.Lists.html#9753" class="Bound">A</a><a id="9754" class="Symbol">}</a> <a id="9756" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a> <a id="9759" href="plfa.Lists.html#9759" class="Bound">ys</a> <a id="9762" class="Symbol">=</a>
  <a id="9766" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2597" class="Function Operator">begin</a>
    <a id="9776" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#8371" class="Function">length</a> <a id="9783" class="Symbol">(</a><a id="9784" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a> <a id="9787" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="9790" href="plfa.Lists.html#9759" class="Bound">ys</a><a id="9792" class="Symbol">)</a>
  <a id="9796" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="9804" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#8371" class="Function">length</a> <a id="9811" href="plfa.Lists.html#9759" class="Bound">ys</a>
  <a id="9816" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="9824" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#8371" class="Function">length</a> <a id="9831" class="Symbol">{</a><a id="9832" href="plfa.Lists.html#9753" class="Bound">A</a><a id="9833" class="Symbol">}</a> <a id="9835" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a> <a id="9838" href="Agda.Builtin.Nat.html#298" class="Primitive Operator">+</a> <a id="9840" href="plfa.Lists.html#8371" class="Function">length</a> <a id="9847" href="plfa.Lists.html#9759" class="Bound">ys</a>
  <a id="9852" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2892" class="Function Operator">∎</a>
<a id="9854" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#9655" class="Function">length-++</a> <a id="9864" class="Symbol">(</a><a id="9865" href="plfa.Lists.html#9865" class="Bound">x</a> <a id="9867" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="9869" href="plfa.Lists.html#9869" class="Bound">xs</a><a id="9871" class="Symbol">)</a> <a id="9873" href="plfa.Lists.html#9873" class="Bound">ys</a> <a id="9876" class="Symbol">=</a>
  <a id="9880" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2597" class="Function Operator">begin</a>
    <a id="9890" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#8371" class="Function">length</a> <a id="9897" class="Symbol">((</a><a id="9899" href="plfa.Lists.html#9865" class="Bound">x</a> <a id="9901" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="9903" href="plfa.Lists.html#9869" class="Bound">xs</a><a id="9905" class="Symbol">)</a> <a id="9907" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="9910" href="plfa.Lists.html#9873" class="Bound">ys</a><a id="9912" class="Symbol">)</a>
  <a id="9916" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="9924" href="Agda.Builtin.Nat.html#196" class="InductiveConstructor">suc</a> <a id="9928" class="Symbol">(</a><a id="9929" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#8371" class="Function">length</a> <a id="9936" class="Symbol">(</a><a id="9937" href="plfa.Lists.html#9869" class="Bound">xs</a> <a id="9940" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="9943" href="plfa.Lists.html#9873" class="Bound">ys</a><a id="9945" class="Symbol">))</a>
  <a id="9950" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2714" class="Function Operator">≡⟨</a> <a id="9953" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#1090" class="Function">cong</a> <a id="9958" href="Agda.Builtin.Nat.html#196" class="InductiveConstructor">suc</a> <a id="9962" class="Symbol">(</a><a id="9963" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#9655" class="Function">length-++</a> <a id="9973" href="plfa.Lists.html#9869" class="Bound">xs</a> <a id="9976" href="plfa.Lists.html#9873" class="Bound">ys</a><a id="9978" class="Symbol">)</a> <a id="9980" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2714" class="Function Operator">⟩</a>
    <a id="9986" href="Agda.Builtin.Nat.html#196" class="InductiveConstructor">suc</a> <a id="9990" class="Symbol">(</a><a id="9991" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#8371" class="Function">length</a> <a id="9998" href="plfa.Lists.html#9869" class="Bound">xs</a> <a id="10001" href="Agda.Builtin.Nat.html#298" class="Primitive Operator">+</a> <a id="10003" href="plfa.Lists.html#8371" class="Function">length</a> <a id="10010" href="plfa.Lists.html#9873" class="Bound">ys</a><a id="10012" class="Symbol">)</a>
  <a id="10016" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="10024" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#8371" class="Function">length</a> <a id="10031" class="Symbol">(</a><a id="10032" href="plfa.Lists.html#9865" class="Bound">x</a> <a id="10034" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="10036" href="plfa.Lists.html#9869" class="Bound">xs</a><a id="10038" class="Symbol">)</a> <a id="10040" href="Agda.Builtin.Nat.html#298" class="Primitive Operator">+</a> <a id="10042" href="plfa.Lists.html#8371" class="Function">length</a> <a id="10049" href="plfa.Lists.html#9873" class="Bound">ys</a>
  <a id="10054" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2892" class="Function Operator">∎</a>
</pre>{% endraw %}
{::comment}
The proof is by induction on the first argument. The base case
instantiates to `[]`, and follows by straightforward computation.  As
before, Agda cannot infer the implicit type parameter to `length`, and
it must be given explicitly.  The inductive case instantiates to
`x ∷ xs`, and follows by straightforward computation combined with the
inductive hypothesis.  As usual, the inductive hypothesis is indicated
by a recursive invocation of the proof, in this case `length-++ xs ys`,
and it is promoted by the congruence `cong suc`.
{:/}

证明对于第一个参数进行归纳。起始步骤将列表实例化为 `[]`，由直接的运算可证。
如同之前一样，Agda 无法推导 `length` 的隐式参数，所以我们必须显式地给出这个参数。
归纳步骤将列表实例化为 `x ∷ xs`，由直接的运算配合归纳假设可证。
与往常一样，归纳假设由递归使用证明函数来表明，此处为 `length-++ xs ys`，
由 `cong suc` 来提升。

{::comment}
## Reverse
{:/}

## 反转

{::comment}
Using append, it is easy to formulate a function to reverse a list:
{:/}

我们可以使用附加，来简单地构造一个函数来反转一个列表：
{% raw %}<pre class="Agda"><a id="reverse"></a><a id="10957" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#10957" class="Function">reverse</a> <a id="10965" class="Symbol">:</a> <a id="10967" class="Symbol">∀</a> <a id="10969" class="Symbol">{</a><a id="10970" href="plfa.Lists.html#10970" class="Bound">A</a> <a id="10972" class="Symbol">:</a> <a id="10974" class="PrimitiveType">Set</a><a id="10977" class="Symbol">}</a> <a id="10979" class="Symbol">→</a> <a id="10981" href="plfa.Lists.html#1284" class="Datatype">List</a> <a id="10986" href="plfa.Lists.html#10970" class="Bound">A</a> <a id="10988" class="Symbol">→</a> <a id="10990" href="plfa.Lists.html#1284" class="Datatype">List</a> <a id="10995" href="plfa.Lists.html#10970" class="Bound">A</a>
<a id="10997" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#10957" class="Function">reverse</a> <a id="11005" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a>        <a id="11015" class="Symbol">=</a>  <a id="11018" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a>
<a id="11021" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#10957" class="Function">reverse</a> <a id="11029" class="Symbol">(</a><a id="11030" href="plfa.Lists.html#11030" class="Bound">x</a> <a id="11032" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="11034" href="plfa.Lists.html#11034" class="Bound">xs</a><a id="11036" class="Symbol">)</a>  <a id="11039" class="Symbol">=</a>  <a id="11042" href="plfa.Lists.html#10957" class="Function">reverse</a> <a id="11050" href="plfa.Lists.html#11034" class="Bound">xs</a> <a id="11053" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="11056" href="plfa.Lists.html#3818" class="InductiveConstructor Operator">[</a> <a id="11058" href="plfa.Lists.html#11030" class="Bound">x</a> <a id="11060" href="plfa.Lists.html#3818" class="InductiveConstructor Operator">]</a>
</pre>{% endraw %}
{::comment}
The reverse of the empty list is the empty list.
The reverse of a non-empty list
is the reverse of its tail appended to a unit list
containing its head.
{:/}

空列表的反转是空列表。
非空列表的反转是其头元素构成的单元列表附加至其尾列表反转之后的结果。

{::comment}
Here is an example showing how to reverse a list:
{:/}

下面的例子展示了如何反转一个列表。
{% raw %}<pre class="Agda"><a id="11376" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#11376" class="Function">_</a> <a id="11378" class="Symbol">:</a> <a id="11380" href="plfa.Lists.html#10957" class="Function">reverse</a> <a id="11388" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">[</a> <a id="11390" class="Number">0</a> <a id="11392" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">,</a> <a id="11394" class="Number">1</a> <a id="11396" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">,</a> <a id="11398" class="Number">2</a> <a id="11400" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">]</a> <a id="11402" href="Agda.Builtin.Equality.html#125" class="Datatype Operator">≡</a> <a id="11404" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">[</a> <a id="11406" class="Number">2</a> <a id="11408" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">,</a> <a id="11410" class="Number">1</a> <a id="11412" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">,</a> <a id="11414" class="Number">0</a> <a id="11416" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">]</a>
<a id="11418" class="Symbol">_</a> <a id="11420" class="Symbol">=</a>
  <a id="11424" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2597" class="Function Operator">begin</a>
    <a id="11434" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#10957" class="Function">reverse</a> <a id="11442" class="Symbol">(</a><a id="11443" class="Number">0</a> <a id="11445" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="11447" class="Number">1</a> <a id="11449" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="11451" class="Number">2</a> <a id="11453" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="11455" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a><a id="11457" class="Symbol">)</a>
  <a id="11461" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="11469" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#10957" class="Function">reverse</a> <a id="11477" class="Symbol">(</a><a id="11478" class="Number">1</a> <a id="11480" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="11482" class="Number">2</a> <a id="11484" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="11486" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a><a id="11488" class="Symbol">)</a> <a id="11490" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="11493" href="plfa.Lists.html#3818" class="InductiveConstructor Operator">[</a> <a id="11495" class="Number">0</a> <a id="11497" href="plfa.Lists.html#3818" class="InductiveConstructor Operator">]</a>
  <a id="11501" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="11509" class="Symbol">(</a><a id="11510" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#10957" class="Function">reverse</a> <a id="11518" class="Symbol">(</a><a id="11519" class="Number">2</a> <a id="11521" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="11523" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a><a id="11525" class="Symbol">)</a> <a id="11527" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="11530" href="plfa.Lists.html#3818" class="InductiveConstructor Operator">[</a> <a id="11532" class="Number">1</a> <a id="11534" href="plfa.Lists.html#3818" class="InductiveConstructor Operator">]</a><a id="11535" class="Symbol">)</a> <a id="11537" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="11540" href="plfa.Lists.html#3818" class="InductiveConstructor Operator">[</a> <a id="11542" class="Number">0</a> <a id="11544" href="plfa.Lists.html#3818" class="InductiveConstructor Operator">]</a>
  <a id="11548" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="11556" class="Symbol">((</a><a id="11558" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#10957" class="Function">reverse</a> <a id="11566" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a> <a id="11569" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="11572" href="plfa.Lists.html#3818" class="InductiveConstructor Operator">[</a> <a id="11574" class="Number">2</a> <a id="11576" href="plfa.Lists.html#3818" class="InductiveConstructor Operator">]</a><a id="11577" class="Symbol">)</a> <a id="11579" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="11582" href="plfa.Lists.html#3818" class="InductiveConstructor Operator">[</a> <a id="11584" class="Number">1</a> <a id="11586" href="plfa.Lists.html#3818" class="InductiveConstructor Operator">]</a><a id="11587" class="Symbol">)</a> <a id="11589" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="11592" href="plfa.Lists.html#3818" class="InductiveConstructor Operator">[</a> <a id="11594" class="Number">0</a> <a id="11596" href="plfa.Lists.html#3818" class="InductiveConstructor Operator">]</a>
  <a id="11600" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="11608" class="Symbol">((</a><a id="11610" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#1313" class="InductiveConstructor">[]</a> <a id="11613" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="11616" href="plfa.Lists.html#3818" class="InductiveConstructor Operator">[</a> <a id="11618" class="Number">2</a> <a id="11620" href="plfa.Lists.html#3818" class="InductiveConstructor Operator">]</a><a id="11621" class="Symbol">)</a> <a id="11623" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="11626" href="plfa.Lists.html#3818" class="InductiveConstructor Operator">[</a> <a id="11628" class="Number">1</a> <a id="11630" href="plfa.Lists.html#3818" class="InductiveConstructor Operator">]</a><a id="11631" class="Symbol">)</a> <a id="11633" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="11636" href="plfa.Lists.html#3818" class="InductiveConstructor Operator">[</a> <a id="11638" class="Number">0</a> <a id="11640" href="plfa.Lists.html#3818" class="InductiveConstructor Operator">]</a>
  <a id="11644" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="11652" class="Symbol">((</a><a id="11654" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#1313" class="InductiveConstructor">[]</a> <a id="11657" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="11660" class="Number">2</a> <a id="11662" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="11664" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a><a id="11666" class="Symbol">)</a> <a id="11668" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="11671" class="Number">1</a> <a id="11673" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="11675" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a><a id="11677" class="Symbol">)</a> <a id="11679" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="11682" class="Number">0</a> <a id="11684" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="11686" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a>
  <a id="11691" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="11699" class="Symbol">(</a><a id="11700" class="Number">2</a> <a id="11702" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#1328" class="InductiveConstructor Operator">∷</a> <a id="11704" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a> <a id="11707" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="11710" class="Number">1</a> <a id="11712" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="11714" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a><a id="11716" class="Symbol">)</a> <a id="11718" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="11721" class="Number">0</a> <a id="11723" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="11725" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a>
  <a id="11730" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="11738" class="Number">2</a> <a id="11740" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#1328" class="InductiveConstructor Operator">∷</a> <a id="11742" class="Symbol">(</a><a id="11743" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a> <a id="11746" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="11749" class="Number">1</a> <a id="11751" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="11753" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a><a id="11755" class="Symbol">)</a> <a id="11757" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="11760" class="Number">0</a> <a id="11762" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="11764" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a>
  <a id="11769" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="11777" class="Symbol">(</a><a id="11778" class="Number">2</a> <a id="11780" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#1328" class="InductiveConstructor Operator">∷</a> <a id="11782" class="Number">1</a> <a id="11784" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="11786" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a><a id="11788" class="Symbol">)</a> <a id="11790" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="11793" class="Number">0</a> <a id="11795" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="11797" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a>
  <a id="11802" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="11810" class="Number">2</a> <a id="11812" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#1328" class="InductiveConstructor Operator">∷</a> <a id="11814" class="Symbol">(</a><a id="11815" class="Number">1</a> <a id="11817" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="11819" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a> <a id="11822" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="11825" class="Number">0</a> <a id="11827" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="11829" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a><a id="11831" class="Symbol">)</a>
  <a id="11835" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="11843" class="Number">2</a> <a id="11845" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#1328" class="InductiveConstructor Operator">∷</a> <a id="11847" class="Number">1</a> <a id="11849" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="11851" class="Symbol">(</a><a id="11852" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a> <a id="11855" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="11858" class="Number">0</a> <a id="11860" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="11862" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a><a id="11864" class="Symbol">)</a>
  <a id="11868" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="11876" class="Number">2</a> <a id="11878" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#1328" class="InductiveConstructor Operator">∷</a> <a id="11880" class="Number">1</a> <a id="11882" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="11884" class="Number">0</a> <a id="11886" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="11888" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a>
  <a id="11893" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="11901" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#3872" class="InductiveConstructor Operator">[</a> <a id="11903" class="Number">2</a> <a id="11905" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">,</a> <a id="11907" class="Number">1</a> <a id="11909" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">,</a> <a id="11911" class="Number">0</a> <a id="11913" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">]</a>
  <a id="11917" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2892" class="Function Operator">∎</a>
</pre>{% endraw %}
{::comment}
Reversing a list in this way takes time _quadratic_ in the length of
the list. This is because reverse ends up appending lists of lengths
`1`, `2`, up to `n - 1`, where `n` is the length of the list being
reversed, append takes time linear in the length of the first
list, and the sum of the numbers up to `n - 1` is `n * (n - 1) / 2`.
(We will validate that last fact in an exercise later in this chapter.)
{:/}

这样子反转一个列表需要列表长度**二次**的时间。这是因为反转一个长度为 `n` 的列表需要
将长度为 `1`、`2` 直到 `n - 1` 的列表附加起来，而附加两个列表需要第一个列表长度线性的时间，
因此加起来就需要 `n * (n - 1) / 2` 的时间。（我们将在本章节后部分验证这一结果）

{::comment}
#### Exercise `reverse-++-commute` (recommended)
{:/}

#### 练习 `reverse-++-commute` （推荐）

{::comment}
Show that the reverse of one list appended to another is the
reverse of the second appended to the reverse of the first:
{:/}

证明一个列表附加到另外一个列表的反转即是反转后的第二个列表附加至反转后的第一个列表：
{% raw %}<pre class="Agda"><a id="12791" class="Keyword">postulate</a>
  <a id="reverse-++-commute"></a><a id="12803" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#12803" class="Postulate">reverse-++-commute</a> <a id="12822" class="Symbol">:</a> <a id="12824" class="Symbol">∀</a> <a id="12826" class="Symbol">{</a><a id="12827" href="plfa.Lists.html#12827" class="Bound">A</a> <a id="12829" class="Symbol">:</a> <a id="12831" class="PrimitiveType">Set</a><a id="12834" class="Symbol">}</a> <a id="12836" class="Symbol">{</a><a id="12837" href="plfa.Lists.html#12837" class="Bound">xs</a> <a id="12840" href="plfa.Lists.html#12840" class="Bound">ys</a> <a id="12843" class="Symbol">:</a> <a id="12845" href="plfa.Lists.html#1284" class="Datatype">List</a> <a id="12850" href="plfa.Lists.html#12827" class="Bound">A</a><a id="12851" class="Symbol">}</a>
    <a id="12857" class="Symbol">→</a> <a id="12859" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#10957" class="Function">reverse</a> <a id="12867" class="Symbol">(</a><a id="12868" href="plfa.Lists.html#12837" class="Bound">xs</a> <a id="12871" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="12874" href="plfa.Lists.html#12840" class="Bound">ys</a><a id="12876" class="Symbol">)</a> <a id="12878" href="Agda.Builtin.Equality.html#125" class="Datatype Operator">≡</a> <a id="12880" href="plfa.Lists.html#10957" class="Function">reverse</a> <a id="12888" href="plfa.Lists.html#12840" class="Bound">ys</a> <a id="12891" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="12894" href="plfa.Lists.html#10957" class="Function">reverse</a> <a id="12902" href="plfa.Lists.html#12837" class="Bound">xs</a>
</pre>{% endraw %}
{::comment}
#### Exercise `reverse-involutive` (recommended)
{:/}

#### 练习 `reverse-involutive` （推荐）

{::comment}
A function is an _involution_ if when applied twice it acts
as the identity function.  Show that reverse is an involution:
{:/}

当一个函数应用两次后与恒等函数作用相同，那么这个函数是一个**对合**（Involution）。
证明反转是一个对合：

{% raw %}<pre class="Agda"><a id="13218" class="Keyword">postulate</a>
  <a id="reverse-involutive"></a><a id="13230" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#13230" class="Postulate">reverse-involutive</a> <a id="13249" class="Symbol">:</a> <a id="13251" class="Symbol">∀</a> <a id="13253" class="Symbol">{</a><a id="13254" href="plfa.Lists.html#13254" class="Bound">A</a> <a id="13256" class="Symbol">:</a> <a id="13258" class="PrimitiveType">Set</a><a id="13261" class="Symbol">}</a> <a id="13263" class="Symbol">{</a><a id="13264" href="plfa.Lists.html#13264" class="Bound">xs</a> <a id="13267" class="Symbol">:</a> <a id="13269" href="plfa.Lists.html#1284" class="Datatype">List</a> <a id="13274" href="plfa.Lists.html#13254" class="Bound">A</a><a id="13275" class="Symbol">}</a>
    <a id="13281" class="Symbol">→</a> <a id="13283" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#10957" class="Function">reverse</a> <a id="13291" class="Symbol">(</a><a id="13292" href="plfa.Lists.html#10957" class="Function">reverse</a> <a id="13300" href="plfa.Lists.html#13264" class="Bound">xs</a><a id="13302" class="Symbol">)</a> <a id="13304" href="Agda.Builtin.Equality.html#125" class="Datatype Operator">≡</a> <a id="13306" href="plfa.Lists.html#13264" class="Bound">xs</a>
</pre>{% endraw %}

{::comment}
## Faster reverse
{:/}

## 更快地反转

{::comment}
The definition above, while easy to reason about, is less efficient than
one might expect since it takes time quadratic in the length of the list.
The idea is that we generalise reverse to take an additional argument:
{:/}

上面的定义虽然论证起来方便，但是它比期望中的实现更低效，因为它的运行时间是关于列表长度的二次函数。
我们可以将反转进行推广，使用一个额外的参数：

{% raw %}<pre class="Agda"><a id="shunt"></a><a id="13675" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#13675" class="Function">shunt</a> <a id="13681" class="Symbol">:</a> <a id="13683" class="Symbol">∀</a> <a id="13685" class="Symbol">{</a><a id="13686" href="plfa.Lists.html#13686" class="Bound">A</a> <a id="13688" class="Symbol">:</a> <a id="13690" class="PrimitiveType">Set</a><a id="13693" class="Symbol">}</a> <a id="13695" class="Symbol">→</a> <a id="13697" href="plfa.Lists.html#1284" class="Datatype">List</a> <a id="13702" href="plfa.Lists.html#13686" class="Bound">A</a> <a id="13704" class="Symbol">→</a> <a id="13706" href="plfa.Lists.html#1284" class="Datatype">List</a> <a id="13711" href="plfa.Lists.html#13686" class="Bound">A</a> <a id="13713" class="Symbol">→</a> <a id="13715" href="plfa.Lists.html#1284" class="Datatype">List</a> <a id="13720" href="plfa.Lists.html#13686" class="Bound">A</a>
<a id="13722" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#13675" class="Function">shunt</a> <a id="13728" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a>       <a id="13737" href="plfa.Lists.html#13737" class="Bound">ys</a>  <a id="13741" class="Symbol">=</a>  <a id="13744" href="plfa.Lists.html#13737" class="Bound">ys</a>
<a id="13747" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#13675" class="Function">shunt</a> <a id="13753" class="Symbol">(</a><a id="13754" href="plfa.Lists.html#13754" class="Bound">x</a> <a id="13756" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="13758" href="plfa.Lists.html#13758" class="Bound">xs</a><a id="13760" class="Symbol">)</a> <a id="13762" href="plfa.Lists.html#13762" class="Bound">ys</a>  <a id="13766" class="Symbol">=</a>  <a id="13769" href="plfa.Lists.html#13675" class="Function">shunt</a> <a id="13775" href="plfa.Lists.html#13758" class="Bound">xs</a> <a id="13778" class="Symbol">(</a><a id="13779" href="plfa.Lists.html#13754" class="Bound">x</a> <a id="13781" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="13783" href="plfa.Lists.html#13762" class="Bound">ys</a><a id="13785" class="Symbol">)</a>
</pre>{% endraw %}
{::comment}
The definition is by recursion on the first argument. The second argument
actually becomes _larger_, but this is not a problem because the argument
on which we recurse becomes _smaller_.
{:/}

这个定义对于第一个参数进行递归。第二个参数会变_大_，但这样做没有问题，因为我们递归的参数
在变_小_。

{::comment}
Shunt is related to reverse as follows:
{:/}

转移（Shunt）与反转的关系如下：
{% raw %}<pre class="Agda"><a id="shunt-reverse"></a><a id="14132" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#14132" class="Function">shunt-reverse</a> <a id="14146" class="Symbol">:</a> <a id="14148" class="Symbol">∀</a> <a id="14150" class="Symbol">{</a><a id="14151" href="plfa.Lists.html#14151" class="Bound">A</a> <a id="14153" class="Symbol">:</a> <a id="14155" class="PrimitiveType">Set</a><a id="14158" class="Symbol">}</a> <a id="14160" class="Symbol">(</a><a id="14161" href="plfa.Lists.html#14161" class="Bound">xs</a> <a id="14164" href="plfa.Lists.html#14164" class="Bound">ys</a> <a id="14167" class="Symbol">:</a> <a id="14169" href="plfa.Lists.html#1284" class="Datatype">List</a> <a id="14174" href="plfa.Lists.html#14151" class="Bound">A</a><a id="14175" class="Symbol">)</a>
  <a id="14179" class="Symbol">→</a> <a id="14181" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#13675" class="Function">shunt</a> <a id="14187" href="plfa.Lists.html#14161" class="Bound">xs</a> <a id="14190" href="plfa.Lists.html#14164" class="Bound">ys</a> <a id="14193" href="Agda.Builtin.Equality.html#125" class="Datatype Operator">≡</a> <a id="14195" href="plfa.Lists.html#10957" class="Function">reverse</a> <a id="14203" href="plfa.Lists.html#14161" class="Bound">xs</a> <a id="14206" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="14209" href="plfa.Lists.html#14164" class="Bound">ys</a>
<a id="14212" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#14132" class="Function">shunt-reverse</a> <a id="14226" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a> <a id="14229" href="plfa.Lists.html#14229" class="Bound">ys</a> <a id="14232" class="Symbol">=</a>
  <a id="14236" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2597" class="Function Operator">begin</a>
    <a id="14246" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#13675" class="Function">shunt</a> <a id="14252" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a> <a id="14255" href="plfa.Lists.html#14229" class="Bound">ys</a>
  <a id="14260" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="14268" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#14229" class="Bound">ys</a>
  <a id="14273" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="14281" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#10957" class="Function">reverse</a> <a id="14289" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a> <a id="14292" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="14295" href="plfa.Lists.html#14229" class="Bound">ys</a>
  <a id="14300" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2892" class="Function Operator">∎</a>
<a id="14302" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#14132" class="Function">shunt-reverse</a> <a id="14316" class="Symbol">(</a><a id="14317" href="plfa.Lists.html#14317" class="Bound">x</a> <a id="14319" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="14321" href="plfa.Lists.html#14321" class="Bound">xs</a><a id="14323" class="Symbol">)</a> <a id="14325" href="plfa.Lists.html#14325" class="Bound">ys</a> <a id="14328" class="Symbol">=</a>
  <a id="14332" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2597" class="Function Operator">begin</a>
    <a id="14342" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#13675" class="Function">shunt</a> <a id="14348" class="Symbol">(</a><a id="14349" href="plfa.Lists.html#14317" class="Bound">x</a> <a id="14351" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="14353" href="plfa.Lists.html#14321" class="Bound">xs</a><a id="14355" class="Symbol">)</a> <a id="14357" href="plfa.Lists.html#14325" class="Bound">ys</a>
  <a id="14362" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="14370" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#13675" class="Function">shunt</a> <a id="14376" href="plfa.Lists.html#14321" class="Bound">xs</a> <a id="14379" class="Symbol">(</a><a id="14380" href="plfa.Lists.html#14317" class="Bound">x</a> <a id="14382" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="14384" href="plfa.Lists.html#14325" class="Bound">ys</a><a id="14386" class="Symbol">)</a>
  <a id="14390" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2714" class="Function Operator">≡⟨</a> <a id="14393" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#14132" class="Function">shunt-reverse</a> <a id="14407" href="plfa.Lists.html#14321" class="Bound">xs</a> <a id="14410" class="Symbol">(</a><a id="14411" href="plfa.Lists.html#14317" class="Bound">x</a> <a id="14413" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="14415" href="plfa.Lists.html#14325" class="Bound">ys</a><a id="14417" class="Symbol">)</a> <a id="14419" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2714" class="Function Operator">⟩</a>
    <a id="14425" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#10957" class="Function">reverse</a> <a id="14433" href="plfa.Lists.html#14321" class="Bound">xs</a> <a id="14436" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="14439" class="Symbol">(</a><a id="14440" href="plfa.Lists.html#14317" class="Bound">x</a> <a id="14442" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="14444" href="plfa.Lists.html#14325" class="Bound">ys</a><a id="14446" class="Symbol">)</a>
  <a id="14450" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="14458" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#10957" class="Function">reverse</a> <a id="14466" href="plfa.Lists.html#14321" class="Bound">xs</a> <a id="14469" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="14472" class="Symbol">(</a><a id="14473" href="plfa.Lists.html#3818" class="InductiveConstructor Operator">[</a> <a id="14475" href="plfa.Lists.html#14317" class="Bound">x</a> <a id="14477" href="plfa.Lists.html#3818" class="InductiveConstructor Operator">]</a> <a id="14479" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="14482" href="plfa.Lists.html#14325" class="Bound">ys</a><a id="14484" class="Symbol">)</a>
  <a id="14488" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2714" class="Function Operator">≡⟨</a> <a id="14491" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#939" class="Function">sym</a> <a id="14495" class="Symbol">(</a><a id="14496" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#6032" class="Function">++-assoc</a> <a id="14505" class="Symbol">(</a><a id="14506" href="plfa.Lists.html#10957" class="Function">reverse</a> <a id="14514" href="plfa.Lists.html#14321" class="Bound">xs</a><a id="14516" class="Symbol">)</a> <a id="14518" href="plfa.Lists.html#3818" class="InductiveConstructor Operator">[</a> <a id="14520" href="plfa.Lists.html#14317" class="Bound">x</a> <a id="14522" href="plfa.Lists.html#3818" class="InductiveConstructor Operator">]</a> <a id="14524" href="plfa.Lists.html#14325" class="Bound">ys</a><a id="14526" class="Symbol">)</a> <a id="14528" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2714" class="Function Operator">⟩</a>
    <a id="14534" class="Symbol">(</a><a id="14535" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#10957" class="Function">reverse</a> <a id="14543" href="plfa.Lists.html#14321" class="Bound">xs</a> <a id="14546" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="14549" href="plfa.Lists.html#3818" class="InductiveConstructor Operator">[</a> <a id="14551" href="plfa.Lists.html#14317" class="Bound">x</a> <a id="14553" href="plfa.Lists.html#3818" class="InductiveConstructor Operator">]</a><a id="14554" class="Symbol">)</a> <a id="14556" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="14559" href="plfa.Lists.html#14325" class="Bound">ys</a>
  <a id="14564" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="14572" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#10957" class="Function">reverse</a> <a id="14580" class="Symbol">(</a><a id="14581" href="plfa.Lists.html#14317" class="Bound">x</a> <a id="14583" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="14585" href="plfa.Lists.html#14321" class="Bound">xs</a><a id="14587" class="Symbol">)</a> <a id="14589" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="14592" href="plfa.Lists.html#14325" class="Bound">ys</a>
  <a id="14597" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2892" class="Function Operator">∎</a>
</pre>{% endraw %}
{::comment}
The proof is by induction on the first argument.
The base case instantiates to `[]`, and follows by straightforward computation.
The inductive case instantiates to `x ∷ xs` and follows by the inductive
hypothesis and associativity of append.  When we invoke the inductive hypothesis,
the second argument actually becomes *larger*, but this is not a problem because
the argument on which we induct becomes *smaller*.
{:/}

证明对于第一个参数进行归纳。起始步骤将列表实例化为 `[]`，由直接的运算可证。
归纳步骤将列表实例化为 `x ∷ xs`，由归纳假设和附加的结合律可证。
当我们使用归纳假设时，第二个参数实际上变**大**了，但是这样做没有问题，因为我们归纳的参数变**小**了。

{::comment}
Generalising on an auxiliary argument, which becomes larger as the argument on
which we recurse or induct becomes smaller, is a common trick. It belongs in
your quiver of arrows, ready to slay the right problem.
{:/}

使用一个会在归纳或递归的参数变小时，变大的辅助参数来进行推广，是一个常用的技巧。
这个技巧在以后的证明中很有用。

{::comment}
Having defined shunt be generalisation, it is now easy to respecialise to
give a more efficient definition of reverse:
{:/}

在定义了推广的转移之后，我们可以将其特化，作为一个更高效的反转的定义：

{% raw %}<pre class="Agda"><a id="reverse′"></a><a id="15638" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#15638" class="Function">reverse′</a> <a id="15647" class="Symbol">:</a> <a id="15649" class="Symbol">∀</a> <a id="15651" class="Symbol">{</a><a id="15652" href="plfa.Lists.html#15652" class="Bound">A</a> <a id="15654" class="Symbol">:</a> <a id="15656" class="PrimitiveType">Set</a><a id="15659" class="Symbol">}</a> <a id="15661" class="Symbol">→</a> <a id="15663" href="plfa.Lists.html#1284" class="Datatype">List</a> <a id="15668" href="plfa.Lists.html#15652" class="Bound">A</a> <a id="15670" class="Symbol">→</a> <a id="15672" href="plfa.Lists.html#1284" class="Datatype">List</a> <a id="15677" href="plfa.Lists.html#15652" class="Bound">A</a>
<a id="15679" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#15638" class="Function">reverse′</a> <a id="15688" href="plfa.Lists.html#15688" class="Bound">xs</a> <a id="15691" class="Symbol">=</a> <a id="15693" href="plfa.Lists.html#13675" class="Function">shunt</a> <a id="15699" href="plfa.Lists.html#15688" class="Bound">xs</a> <a id="15702" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a>
</pre>{% endraw %}
{::comment}
Given our previous lemma, it is straightforward to show
the two definitions equivalent:
{:/}

因为我们之前证明的引理，我们可以直接地证明两个定义是等价的：

{% raw %}<pre class="Agda"><a id="reverses"></a><a id="15852" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#15852" class="Function">reverses</a> <a id="15861" class="Symbol">:</a> <a id="15863" class="Symbol">∀</a> <a id="15865" class="Symbol">{</a><a id="15866" href="plfa.Lists.html#15866" class="Bound">A</a> <a id="15868" class="Symbol">:</a> <a id="15870" class="PrimitiveType">Set</a><a id="15873" class="Symbol">}</a> <a id="15875" class="Symbol">(</a><a id="15876" href="plfa.Lists.html#15876" class="Bound">xs</a> <a id="15879" class="Symbol">:</a> <a id="15881" href="plfa.Lists.html#1284" class="Datatype">List</a> <a id="15886" href="plfa.Lists.html#15866" class="Bound">A</a><a id="15887" class="Symbol">)</a>
  <a id="15891" class="Symbol">→</a> <a id="15893" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#15638" class="Function">reverse′</a> <a id="15902" href="plfa.Lists.html#15876" class="Bound">xs</a> <a id="15905" href="Agda.Builtin.Equality.html#125" class="Datatype Operator">≡</a> <a id="15907" href="plfa.Lists.html#10957" class="Function">reverse</a> <a id="15915" href="plfa.Lists.html#15876" class="Bound">xs</a>
<a id="15918" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#15852" class="Function">reverses</a> <a id="15927" href="plfa.Lists.html#15927" class="Bound">xs</a> <a id="15930" class="Symbol">=</a>
  <a id="15934" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2597" class="Function Operator">begin</a>
    <a id="15944" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#15638" class="Function">reverse′</a> <a id="15953" href="plfa.Lists.html#15927" class="Bound">xs</a>
  <a id="15958" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="15966" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#13675" class="Function">shunt</a> <a id="15972" href="plfa.Lists.html#15927" class="Bound">xs</a> <a id="15975" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a>
  <a id="15980" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2714" class="Function Operator">≡⟨</a> <a id="15983" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#14132" class="Function">shunt-reverse</a> <a id="15997" href="plfa.Lists.html#15927" class="Bound">xs</a> <a id="16000" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a> <a id="16003" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2714" class="Function Operator">⟩</a>
    <a id="16009" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#10957" class="Function">reverse</a> <a id="16017" href="plfa.Lists.html#15927" class="Bound">xs</a> <a id="16020" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="16023" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a>
  <a id="16028" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2714" class="Function Operator">≡⟨</a> <a id="16031" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#7822" class="Function">++-identityʳ</a> <a id="16044" class="Symbol">(</a><a id="16045" href="plfa.Lists.html#10957" class="Function">reverse</a> <a id="16053" href="plfa.Lists.html#15927" class="Bound">xs</a><a id="16055" class="Symbol">)</a> <a id="16057" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2714" class="Function Operator">⟩</a>
    <a id="16063" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#10957" class="Function">reverse</a> <a id="16071" href="plfa.Lists.html#15927" class="Bound">xs</a>
  <a id="16076" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2892" class="Function Operator">∎</a>
</pre>{% endraw %}
{::comment}
Here is an example showing fast reverse of the list `[ 0 , 1 , 2 ]`:
{:/}

下面的例子展示了如何快速反转列表 `[ 0 , 1 , 2 ]`：

{% raw %}<pre class="Agda"><a id="16209" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#16209" class="Function">_</a> <a id="16211" class="Symbol">:</a> <a id="16213" href="plfa.Lists.html#15638" class="Function">reverse′</a> <a id="16222" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">[</a> <a id="16224" class="Number">0</a> <a id="16226" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">,</a> <a id="16228" class="Number">1</a> <a id="16230" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">,</a> <a id="16232" class="Number">2</a> <a id="16234" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">]</a> <a id="16236" href="Agda.Builtin.Equality.html#125" class="Datatype Operator">≡</a> <a id="16238" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">[</a> <a id="16240" class="Number">2</a> <a id="16242" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">,</a> <a id="16244" class="Number">1</a> <a id="16246" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">,</a> <a id="16248" class="Number">0</a> <a id="16250" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">]</a>
<a id="16252" class="Symbol">_</a> <a id="16254" class="Symbol">=</a>
  <a id="16258" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2597" class="Function Operator">begin</a>
    <a id="16268" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#15638" class="Function">reverse′</a> <a id="16277" class="Symbol">(</a><a id="16278" class="Number">0</a> <a id="16280" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="16282" class="Number">1</a> <a id="16284" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="16286" class="Number">2</a> <a id="16288" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="16290" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a><a id="16292" class="Symbol">)</a>
  <a id="16296" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="16304" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#13675" class="Function">shunt</a> <a id="16310" class="Symbol">(</a><a id="16311" class="Number">0</a> <a id="16313" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="16315" class="Number">1</a> <a id="16317" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="16319" class="Number">2</a> <a id="16321" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="16323" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a><a id="16325" class="Symbol">)</a> <a id="16327" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a>
  <a id="16332" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="16340" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#13675" class="Function">shunt</a> <a id="16346" class="Symbol">(</a><a id="16347" class="Number">1</a> <a id="16349" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="16351" class="Number">2</a> <a id="16353" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="16355" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a><a id="16357" class="Symbol">)</a> <a id="16359" class="Symbol">(</a><a id="16360" class="Number">0</a> <a id="16362" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="16364" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a><a id="16366" class="Symbol">)</a>
  <a id="16370" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="16378" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#13675" class="Function">shunt</a> <a id="16384" class="Symbol">(</a><a id="16385" class="Number">2</a> <a id="16387" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="16389" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a><a id="16391" class="Symbol">)</a> <a id="16393" class="Symbol">(</a><a id="16394" class="Number">1</a> <a id="16396" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="16398" class="Number">0</a> <a id="16400" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="16402" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a><a id="16404" class="Symbol">)</a>
  <a id="16408" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="16416" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#13675" class="Function">shunt</a> <a id="16422" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a> <a id="16425" class="Symbol">(</a><a id="16426" class="Number">2</a> <a id="16428" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="16430" class="Number">1</a> <a id="16432" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="16434" class="Number">0</a> <a id="16436" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="16438" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a><a id="16440" class="Symbol">)</a>
  <a id="16444" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="16452" class="Number">2</a> <a id="16454" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#1328" class="InductiveConstructor Operator">∷</a> <a id="16456" class="Number">1</a> <a id="16458" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="16460" class="Number">0</a> <a id="16462" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="16464" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a>
  <a id="16469" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2892" class="Function Operator">∎</a>
</pre>{% endraw %}
{::comment}
Now the time to reverse a list is linear in the length of the list.
{:/}

现在反转一个列表需要的时间与列表的长度线性相关。

{::comment}
## Map {#Map}
{:/}

## 映射 {#Map}

{::comment}
Map applies a function to every element of a list to generate a corresponding list.
Map is an example of a _higher-order function_, one which takes a function as an
argument or returns a function as a result:
{:/}

映射将一个函数应用于列表中的所有元素，生成一个对应的列表。
映射是一个**高阶函数**（Higher-Order Function）的例子，它取一个函数作为参数，返回一个函数作为结果：

{% raw %}<pre class="Agda"><a id="map"></a><a id="16959" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#16959" class="Function">map</a> <a id="16963" class="Symbol">:</a> <a id="16965" class="Symbol">∀</a> <a id="16967" class="Symbol">{</a><a id="16968" href="plfa.Lists.html#16968" class="Bound">A</a> <a id="16970" href="plfa.Lists.html#16970" class="Bound">B</a> <a id="16972" class="Symbol">:</a> <a id="16974" class="PrimitiveType">Set</a><a id="16977" class="Symbol">}</a> <a id="16979" class="Symbol">→</a> <a id="16981" class="Symbol">(</a><a id="16982" href="plfa.Lists.html#16968" class="Bound">A</a> <a id="16984" class="Symbol">→</a> <a id="16986" href="plfa.Lists.html#16970" class="Bound">B</a><a id="16987" class="Symbol">)</a> <a id="16989" class="Symbol">→</a> <a id="16991" href="plfa.Lists.html#1284" class="Datatype">List</a> <a id="16996" href="plfa.Lists.html#16968" class="Bound">A</a> <a id="16998" class="Symbol">→</a> <a id="17000" href="plfa.Lists.html#1284" class="Datatype">List</a> <a id="17005" href="plfa.Lists.html#16970" class="Bound">B</a>
<a id="17007" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#16959" class="Function">map</a> <a id="17011" href="plfa.Lists.html#17011" class="Bound">f</a> <a id="17013" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a>        <a id="17023" class="Symbol">=</a>  <a id="17026" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a>
<a id="17029" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#16959" class="Function">map</a> <a id="17033" href="plfa.Lists.html#17033" class="Bound">f</a> <a id="17035" class="Symbol">(</a><a id="17036" href="plfa.Lists.html#17036" class="Bound">x</a> <a id="17038" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="17040" href="plfa.Lists.html#17040" class="Bound">xs</a><a id="17042" class="Symbol">)</a>  <a id="17045" class="Symbol">=</a>  <a id="17048" href="plfa.Lists.html#17033" class="Bound">f</a> <a id="17050" href="plfa.Lists.html#17036" class="Bound">x</a> <a id="17052" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="17054" href="plfa.Lists.html#16959" class="Function">map</a> <a id="17058" href="plfa.Lists.html#17033" class="Bound">f</a> <a id="17060" href="plfa.Lists.html#17040" class="Bound">xs</a>
</pre>{% endraw %}
{::comment}
Map of the empty list is the empty list.
Map of a non-empty list yields a list
with head the same as the function applied to the head of the given list,
and tail the same as map of the function applied to the tail of the given list.
{:/}

空列表的映射是空列表。
非空列表的映射生成一个列表，其头元素是原列表的头元素在应用函数之后的结果，
其尾列表是原列表的尾列表映射后的结果。

{::comment}
Here is an example showing how to use map to increment every element of a list:
{:/}

下面的例子展示了如何使用映射来增加列表中的每一个元素：

{% raw %}<pre class="Agda"><a id="17521" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#17521" class="Function">_</a> <a id="17523" class="Symbol">:</a> <a id="17525" href="plfa.Lists.html#16959" class="Function">map</a> <a id="17529" href="Agda.Builtin.Nat.html#196" class="InductiveConstructor">suc</a> <a id="17533" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">[</a> <a id="17535" class="Number">0</a> <a id="17537" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">,</a> <a id="17539" class="Number">1</a> <a id="17541" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">,</a> <a id="17543" class="Number">2</a> <a id="17545" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">]</a> <a id="17547" href="Agda.Builtin.Equality.html#125" class="Datatype Operator">≡</a> <a id="17549" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">[</a> <a id="17551" class="Number">1</a> <a id="17553" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">,</a> <a id="17555" class="Number">2</a> <a id="17557" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">,</a> <a id="17559" class="Number">3</a> <a id="17561" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">]</a>
<a id="17563" class="Symbol">_</a> <a id="17565" class="Symbol">=</a>
  <a id="17569" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2597" class="Function Operator">begin</a>
    <a id="17579" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#16959" class="Function">map</a> <a id="17583" href="Agda.Builtin.Nat.html#196" class="InductiveConstructor">suc</a> <a id="17587" class="Symbol">(</a><a id="17588" class="Number">0</a> <a id="17590" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="17592" class="Number">1</a> <a id="17594" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="17596" class="Number">2</a> <a id="17598" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="17600" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a><a id="17602" class="Symbol">)</a>
  <a id="17606" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="17614" href="Agda.Builtin.Nat.html#196" class="InductiveConstructor">suc</a> <a id="17618" class="Number">0</a> <a id="17620" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#1328" class="InductiveConstructor Operator">∷</a> <a id="17622" href="plfa.Lists.html#16959" class="Function">map</a> <a id="17626" href="Agda.Builtin.Nat.html#196" class="InductiveConstructor">suc</a> <a id="17630" class="Symbol">(</a><a id="17631" class="Number">1</a> <a id="17633" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="17635" class="Number">2</a> <a id="17637" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="17639" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a><a id="17641" class="Symbol">)</a>
  <a id="17645" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="17653" href="Agda.Builtin.Nat.html#196" class="InductiveConstructor">suc</a> <a id="17657" class="Number">0</a> <a id="17659" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#1328" class="InductiveConstructor Operator">∷</a> <a id="17661" href="Agda.Builtin.Nat.html#196" class="InductiveConstructor">suc</a> <a id="17665" class="Number">1</a> <a id="17667" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="17669" href="plfa.Lists.html#16959" class="Function">map</a> <a id="17673" href="Agda.Builtin.Nat.html#196" class="InductiveConstructor">suc</a> <a id="17677" class="Symbol">(</a><a id="17678" class="Number">2</a> <a id="17680" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="17682" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a><a id="17684" class="Symbol">)</a>
  <a id="17688" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="17696" href="Agda.Builtin.Nat.html#196" class="InductiveConstructor">suc</a> <a id="17700" class="Number">0</a> <a id="17702" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#1328" class="InductiveConstructor Operator">∷</a> <a id="17704" href="Agda.Builtin.Nat.html#196" class="InductiveConstructor">suc</a> <a id="17708" class="Number">1</a> <a id="17710" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="17712" href="Agda.Builtin.Nat.html#196" class="InductiveConstructor">suc</a> <a id="17716" class="Number">2</a> <a id="17718" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="17720" href="plfa.Lists.html#16959" class="Function">map</a> <a id="17724" href="Agda.Builtin.Nat.html#196" class="InductiveConstructor">suc</a> <a id="17728" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a>
  <a id="17733" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="17741" href="Agda.Builtin.Nat.html#196" class="InductiveConstructor">suc</a> <a id="17745" class="Number">0</a> <a id="17747" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#1328" class="InductiveConstructor Operator">∷</a> <a id="17749" href="Agda.Builtin.Nat.html#196" class="InductiveConstructor">suc</a> <a id="17753" class="Number">1</a> <a id="17755" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="17757" href="Agda.Builtin.Nat.html#196" class="InductiveConstructor">suc</a> <a id="17761" class="Number">2</a> <a id="17763" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="17765" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a>
  <a id="17770" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="17778" class="Number">1</a> <a id="17780" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#1328" class="InductiveConstructor Operator">∷</a> <a id="17782" class="Number">2</a> <a id="17784" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="17786" class="Number">3</a> <a id="17788" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="17790" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a>
  <a id="17795" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2892" class="Function Operator">∎</a>
</pre>{% endraw %}
{::comment}
Map requires time linear in the length of the list.
{:/}

映射需要关于列表长度线性的时间。

{::comment}
It is often convenient to exploit currying by applying
map to a function to yield a new function, and at a later
point applying the resulting function:
{:/}

我们常常可以利用柯里化，将映射作用于一个函数，获得另一个函数，然后在之后的时候应用获得的函数：

{% raw %}<pre class="Agda"><a id="sucs"></a><a id="18113" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#18113" class="Function">sucs</a> <a id="18118" class="Symbol">:</a> <a id="18120" href="plfa.Lists.html#1284" class="Datatype">List</a> <a id="18125" href="Agda.Builtin.Nat.html#165" class="Datatype">ℕ</a> <a id="18127" class="Symbol">→</a> <a id="18129" href="plfa.Lists.html#1284" class="Datatype">List</a> <a id="18134" href="Agda.Builtin.Nat.html#165" class="Datatype">ℕ</a>
<a id="18136" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#18113" class="Function">sucs</a> <a id="18141" class="Symbol">=</a> <a id="18143" href="plfa.Lists.html#16959" class="Function">map</a> <a id="18147" href="Agda.Builtin.Nat.html#196" class="InductiveConstructor">suc</a>

<a id="18152" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#18152" class="Function">_</a> <a id="18154" class="Symbol">:</a> <a id="18156" href="plfa.Lists.html#18113" class="Function">sucs</a> <a id="18161" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">[</a> <a id="18163" class="Number">0</a> <a id="18165" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">,</a> <a id="18167" class="Number">1</a> <a id="18169" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">,</a> <a id="18171" class="Number">2</a> <a id="18173" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">]</a> <a id="18175" href="Agda.Builtin.Equality.html#125" class="Datatype Operator">≡</a> <a id="18177" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">[</a> <a id="18179" class="Number">1</a> <a id="18181" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">,</a> <a id="18183" class="Number">2</a> <a id="18185" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">,</a> <a id="18187" class="Number">3</a> <a id="18189" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">]</a>
<a id="18191" class="Symbol">_</a> <a id="18193" class="Symbol">=</a>
  <a id="18197" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2597" class="Function Operator">begin</a>
    <a id="18207" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#18113" class="Function">sucs</a> <a id="18212" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">[</a> <a id="18214" class="Number">0</a> <a id="18216" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">,</a> <a id="18218" class="Number">1</a> <a id="18220" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">,</a> <a id="18222" class="Number">2</a> <a id="18224" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">]</a>
  <a id="18228" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="18236" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#16959" class="Function">map</a> <a id="18240" href="Agda.Builtin.Nat.html#196" class="InductiveConstructor">suc</a> <a id="18244" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">[</a> <a id="18246" class="Number">0</a> <a id="18248" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">,</a> <a id="18250" class="Number">1</a> <a id="18252" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">,</a> <a id="18254" class="Number">2</a> <a id="18256" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">]</a>
  <a id="18260" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="18268" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#3872" class="InductiveConstructor Operator">[</a> <a id="18270" class="Number">1</a> <a id="18272" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">,</a> <a id="18274" class="Number">2</a> <a id="18276" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">,</a> <a id="18278" class="Number">3</a> <a id="18280" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">]</a>
  <a id="18284" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2892" class="Function Operator">∎</a>
</pre>{% endraw %}
{::comment}
Any type that is parameterised on another type, such as lists, has a
corresponding map, which accepts a function and returns a function
from the type parameterised on the domain of the function to the type
parameterised on the range of the function. Further, a type that is
parameterised on _n_ types will have a map that is parameterised on
_n_ functions.
{:/}

任何对于另外一个类型参数化的类型，例如列表，都有对应的映射，其接受一个函数，并返回另一个
从由给定函数定义域参数化的类型，到由给定函数值域参数化的函数。除此之外，一个对于 _n_ 个类型
参数化的类型会有一个对于 _n_ 个函数参数化的映射。

{::comment}
#### Exercise `map-compose`
{:/}

#### 练习 `map-compose`

{::comment}
Prove that the map of a composition is equal to the composition of two maps:
{:/}

证明函数组合的映射是两个映射的组合：

{% raw %}<pre class="Agda"><a id="18977" class="Keyword">postulate</a>
  <a id="map-compose"></a><a id="18989" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#18989" class="Postulate">map-compose</a> <a id="19001" class="Symbol">:</a> <a id="19003" class="Symbol">∀</a> <a id="19005" class="Symbol">{</a><a id="19006" href="plfa.Lists.html#19006" class="Bound">A</a> <a id="19008" href="plfa.Lists.html#19008" class="Bound">B</a> <a id="19010" href="plfa.Lists.html#19010" class="Bound">C</a> <a id="19012" class="Symbol">:</a> <a id="19014" class="PrimitiveType">Set</a><a id="19017" class="Symbol">}</a> <a id="19019" class="Symbol">{</a><a id="19020" href="plfa.Lists.html#19020" class="Bound">f</a> <a id="19022" class="Symbol">:</a> <a id="19024" href="plfa.Lists.html#19006" class="Bound">A</a> <a id="19026" class="Symbol">→</a> <a id="19028" href="plfa.Lists.html#19008" class="Bound">B</a><a id="19029" class="Symbol">}</a> <a id="19031" class="Symbol">{</a><a id="19032" href="plfa.Lists.html#19032" class="Bound">g</a> <a id="19034" class="Symbol">:</a> <a id="19036" href="plfa.Lists.html#19008" class="Bound">B</a> <a id="19038" class="Symbol">→</a> <a id="19040" href="plfa.Lists.html#19010" class="Bound">C</a><a id="19041" class="Symbol">}</a>
    <a id="19047" class="Symbol">→</a> <a id="19049" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#16959" class="Function">map</a> <a id="19053" class="Symbol">(</a><a id="19054" href="plfa.Lists.html#19032" class="Bound">g</a> <a id="19056" href="https://agda.github.io/agda-stdlib/v1.1/Function.html#1099" class="Function Operator">∘</a> <a id="19058" href="plfa.Lists.html#19020" class="Bound">f</a><a id="19059" class="Symbol">)</a> <a id="19061" href="Agda.Builtin.Equality.html#125" class="Datatype Operator">≡</a> <a id="19063" href="plfa.Lists.html#16959" class="Function">map</a> <a id="19067" href="plfa.Lists.html#19032" class="Bound">g</a> <a id="19069" href="https://agda.github.io/agda-stdlib/v1.1/Function.html#1099" class="Function Operator">∘</a> <a id="19071" href="plfa.Lists.html#16959" class="Function">map</a> <a id="19075" href="plfa.Lists.html#19020" class="Bound">f</a>
</pre>{% endraw %}
{::comment}
The last step of the proof requires extensionality.
{:/}

证明的最后一步需要外延性。

{::comment}
#### Exercise `map-++-commute`
{:/}

#### 练习 `map-++-commute`

{::comment}
Prove the following relationship between map and append:
{:/}

证明下列关于映射与附加的关系：

{% raw %}<pre class="Agda"><a id="19338" class="Keyword">postulate</a>
  <a id="map-++-commute"></a><a id="19350" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#19350" class="Postulate">map-++-commute</a> <a id="19365" class="Symbol">:</a> <a id="19367" class="Symbol">∀</a> <a id="19369" class="Symbol">{</a><a id="19370" href="plfa.Lists.html#19370" class="Bound">A</a> <a id="19372" href="plfa.Lists.html#19372" class="Bound">B</a> <a id="19374" class="Symbol">:</a> <a id="19376" class="PrimitiveType">Set</a><a id="19379" class="Symbol">}</a> <a id="19381" class="Symbol">{</a><a id="19382" href="plfa.Lists.html#19382" class="Bound">f</a> <a id="19384" class="Symbol">:</a> <a id="19386" href="plfa.Lists.html#19370" class="Bound">A</a> <a id="19388" class="Symbol">→</a> <a id="19390" href="plfa.Lists.html#19372" class="Bound">B</a><a id="19391" class="Symbol">}</a> <a id="19393" class="Symbol">{</a><a id="19394" href="plfa.Lists.html#19394" class="Bound">xs</a> <a id="19397" href="plfa.Lists.html#19397" class="Bound">ys</a> <a id="19400" class="Symbol">:</a> <a id="19402" href="plfa.Lists.html#1284" class="Datatype">List</a> <a id="19407" href="plfa.Lists.html#19370" class="Bound">A</a><a id="19408" class="Symbol">}</a>
   <a id="19413" class="Symbol">→</a>  <a id="19416" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#16959" class="Function">map</a> <a id="19420" href="plfa.Lists.html#19382" class="Bound">f</a> <a id="19422" class="Symbol">(</a><a id="19423" href="plfa.Lists.html#19394" class="Bound">xs</a> <a id="19426" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="19429" href="plfa.Lists.html#19397" class="Bound">ys</a><a id="19431" class="Symbol">)</a> <a id="19433" href="Agda.Builtin.Equality.html#125" class="Datatype Operator">≡</a> <a id="19435" href="plfa.Lists.html#16959" class="Function">map</a> <a id="19439" href="plfa.Lists.html#19382" class="Bound">f</a> <a id="19441" href="plfa.Lists.html#19394" class="Bound">xs</a> <a id="19444" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="19447" href="plfa.Lists.html#16959" class="Function">map</a> <a id="19451" href="plfa.Lists.html#19382" class="Bound">f</a> <a id="19453" href="plfa.Lists.html#19397" class="Bound">ys</a>
</pre>{% endraw %}
{::comment}
#### Exercise `map-Tree`
{:/}

#### 练习 `map-Tree`

{::comment}
Define a type of trees with leaves of type `A` and internal
nodes of type `B`:
{:/}

定义一个树数据类型，其叶节点类型为 `A`，内部节点类型为 `B`：

{% raw %}<pre class="Agda"><a id="19661" class="Keyword">data</a> <a id="Tree"></a><a id="19666" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#19666" class="Datatype">Tree</a> <a id="19671" class="Symbol">(</a><a id="19672" href="plfa.Lists.html#19672" class="Bound">A</a> <a id="19674" href="plfa.Lists.html#19674" class="Bound">B</a> <a id="19676" class="Symbol">:</a> <a id="19678" class="PrimitiveType">Set</a><a id="19681" class="Symbol">)</a> <a id="19683" class="Symbol">:</a> <a id="19685" class="PrimitiveType">Set</a> <a id="19689" class="Keyword">where</a>
  <a id="Tree.leaf"></a><a id="19697" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#19697" class="InductiveConstructor">leaf</a> <a id="19702" class="Symbol">:</a> <a id="19704" href="plfa.Lists.html#19672" class="Bound">A</a> <a id="19706" class="Symbol">→</a> <a id="19708" href="plfa.Lists.html#19666" class="Datatype">Tree</a> <a id="19713" href="plfa.Lists.html#19672" class="Bound">A</a> <a id="19715" href="plfa.Lists.html#19674" class="Bound">B</a>
  <a id="Tree.node"></a><a id="19719" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#19719" class="InductiveConstructor">node</a> <a id="19724" class="Symbol">:</a> <a id="19726" href="plfa.Lists.html#19666" class="Datatype">Tree</a> <a id="19731" href="plfa.Lists.html#19672" class="Bound">A</a> <a id="19733" href="plfa.Lists.html#19674" class="Bound">B</a> <a id="19735" class="Symbol">→</a> <a id="19737" href="plfa.Lists.html#19674" class="Bound">B</a> <a id="19739" class="Symbol">→</a> <a id="19741" href="plfa.Lists.html#19666" class="Datatype">Tree</a> <a id="19746" href="plfa.Lists.html#19672" class="Bound">A</a> <a id="19748" href="plfa.Lists.html#19674" class="Bound">B</a> <a id="19750" class="Symbol">→</a> <a id="19752" href="plfa.Lists.html#19666" class="Datatype">Tree</a> <a id="19757" href="plfa.Lists.html#19672" class="Bound">A</a> <a id="19759" href="plfa.Lists.html#19674" class="Bound">B</a>
</pre>{% endraw %}
{::comment}
Define a suitable map operator over trees:
{:/}

定义一个对于树的映射运算符：

{% raw %}<pre class="Agda"><a id="19847" class="Keyword">postulate</a>
  <a id="map-Tree"></a><a id="19859" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#19859" class="Postulate">map-Tree</a> <a id="19868" class="Symbol">:</a> <a id="19870" class="Symbol">∀</a> <a id="19872" class="Symbol">{</a><a id="19873" href="plfa.Lists.html#19873" class="Bound">A</a> <a id="19875" href="plfa.Lists.html#19875" class="Bound">B</a> <a id="19877" href="plfa.Lists.html#19877" class="Bound">C</a> <a id="19879" href="plfa.Lists.html#19879" class="Bound">D</a> <a id="19881" class="Symbol">:</a> <a id="19883" class="PrimitiveType">Set</a><a id="19886" class="Symbol">}</a>
    <a id="19892" class="Symbol">→</a> <a id="19894" class="Symbol">(</a><a id="19895" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#19873" class="Bound">A</a> <a id="19897" class="Symbol">→</a> <a id="19899" href="plfa.Lists.html#19877" class="Bound">C</a><a id="19900" class="Symbol">)</a> <a id="19902" class="Symbol">→</a> <a id="19904" class="Symbol">(</a><a id="19905" href="plfa.Lists.html#19875" class="Bound">B</a> <a id="19907" class="Symbol">→</a> <a id="19909" href="plfa.Lists.html#19879" class="Bound">D</a><a id="19910" class="Symbol">)</a> <a id="19912" class="Symbol">→</a> <a id="19914" href="plfa.Lists.html#19666" class="Datatype">Tree</a> <a id="19919" href="plfa.Lists.html#19873" class="Bound">A</a> <a id="19921" href="plfa.Lists.html#19875" class="Bound">B</a> <a id="19923" class="Symbol">→</a> <a id="19925" href="plfa.Lists.html#19666" class="Datatype">Tree</a> <a id="19930" href="plfa.Lists.html#19877" class="Bound">C</a> <a id="19932" href="plfa.Lists.html#19879" class="Bound">D</a>
</pre>{% endraw %}

{::comment}
## Fold {#Fold}
{:/}

## 折叠 {#Fold}

{::comment}
Fold takes an operator and a value, and uses the operator to combine
each of the elements of the list, taking the given value as the result
for the empty list:
{:/}

折叠取一个运算符和一个值，并使用运算符将列表中的元素合并至一个值，如果给定的列表为空，
则使用给定的值：

{% raw %}<pre class="Agda"><a id="foldr"></a><a id="20225" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#20225" class="Function">foldr</a> <a id="20231" class="Symbol">:</a> <a id="20233" class="Symbol">∀</a> <a id="20235" class="Symbol">{</a><a id="20236" href="plfa.Lists.html#20236" class="Bound">A</a> <a id="20238" href="plfa.Lists.html#20238" class="Bound">B</a> <a id="20240" class="Symbol">:</a> <a id="20242" class="PrimitiveType">Set</a><a id="20245" class="Symbol">}</a> <a id="20247" class="Symbol">→</a> <a id="20249" class="Symbol">(</a><a id="20250" href="plfa.Lists.html#20236" class="Bound">A</a> <a id="20252" class="Symbol">→</a> <a id="20254" href="plfa.Lists.html#20238" class="Bound">B</a> <a id="20256" class="Symbol">→</a> <a id="20258" href="plfa.Lists.html#20238" class="Bound">B</a><a id="20259" class="Symbol">)</a> <a id="20261" class="Symbol">→</a> <a id="20263" href="plfa.Lists.html#20238" class="Bound">B</a> <a id="20265" class="Symbol">→</a> <a id="20267" href="plfa.Lists.html#1284" class="Datatype">List</a> <a id="20272" href="plfa.Lists.html#20236" class="Bound">A</a> <a id="20274" class="Symbol">→</a> <a id="20276" href="plfa.Lists.html#20238" class="Bound">B</a>
<a id="20278" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#20225" class="Function">foldr</a> <a id="20284" href="plfa.Lists.html#20284" class="Bound Operator">_⊗_</a> <a id="20288" href="plfa.Lists.html#20288" class="Bound">e</a> <a id="20290" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a>        <a id="20300" class="Symbol">=</a>  <a id="20303" href="plfa.Lists.html#20288" class="Bound">e</a>
<a id="20305" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#20225" class="Function">foldr</a> <a id="20311" href="plfa.Lists.html#20311" class="Bound Operator">_⊗_</a> <a id="20315" href="plfa.Lists.html#20315" class="Bound">e</a> <a id="20317" class="Symbol">(</a><a id="20318" href="plfa.Lists.html#20318" class="Bound">x</a> <a id="20320" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="20322" href="plfa.Lists.html#20322" class="Bound">xs</a><a id="20324" class="Symbol">)</a>  <a id="20327" class="Symbol">=</a>  <a id="20330" href="plfa.Lists.html#20318" class="Bound">x</a> <a id="20332" href="plfa.Lists.html#20311" class="Bound Operator">⊗</a> <a id="20334" href="plfa.Lists.html#20225" class="Function">foldr</a> <a id="20340" href="plfa.Lists.html#20311" class="Bound Operator">_⊗_</a> <a id="20344" href="plfa.Lists.html#20315" class="Bound">e</a> <a id="20346" href="plfa.Lists.html#20322" class="Bound">xs</a>
</pre>{% endraw %}
{::comment}
Fold of the empty list is the given value.
Fold of a non-empty list uses the operator to combine
the head of the list and the fold of the tail of the list.
{:/}

空列表的折叠是给定的值。
非空列表的折叠使用给定的运算符，将头元素和尾列表的折叠合并起来。

{::comment}
Here is an example showing how to use fold to find the sum of a list:
{:/}

下面的例子展示了如何使用折叠来对一个列表求和：

{% raw %}<pre class="Agda"><a id="20692" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#20692" class="Function">_</a> <a id="20694" class="Symbol">:</a> <a id="20696" href="plfa.Lists.html#20225" class="Function">foldr</a> <a id="20702" href="Agda.Builtin.Nat.html#298" class="Primitive Operator">_+_</a> <a id="20706" class="Number">0</a> <a id="20708" href="plfa.Lists.html#3911" class="InductiveConstructor Operator">[</a> <a id="20710" class="Number">1</a> <a id="20712" href="plfa.Lists.html#3911" class="InductiveConstructor Operator">,</a> <a id="20714" class="Number">2</a> <a id="20716" href="plfa.Lists.html#3911" class="InductiveConstructor Operator">,</a> <a id="20718" class="Number">3</a> <a id="20720" href="plfa.Lists.html#3911" class="InductiveConstructor Operator">,</a> <a id="20722" class="Number">4</a> <a id="20724" href="plfa.Lists.html#3911" class="InductiveConstructor Operator">]</a> <a id="20726" href="Agda.Builtin.Equality.html#125" class="Datatype Operator">≡</a> <a id="20728" class="Number">10</a>
<a id="20731" class="Symbol">_</a> <a id="20733" class="Symbol">=</a>
  <a id="20737" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2597" class="Function Operator">begin</a>
    <a id="20747" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#20225" class="Function">foldr</a> <a id="20753" href="Agda.Builtin.Nat.html#298" class="Primitive Operator">_+_</a> <a id="20757" class="Number">0</a> <a id="20759" class="Symbol">(</a><a id="20760" class="Number">1</a> <a id="20762" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="20764" class="Number">2</a> <a id="20766" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="20768" class="Number">3</a> <a id="20770" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="20772" class="Number">4</a> <a id="20774" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="20776" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a><a id="20778" class="Symbol">)</a>
  <a id="20782" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="20790" class="Number">1</a> <a id="20792" href="Agda.Builtin.Nat.html#298" class="Primitive Operator">+</a> <a id="20794" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#20225" class="Function">foldr</a> <a id="20800" href="Agda.Builtin.Nat.html#298" class="Primitive Operator">_+_</a> <a id="20804" class="Number">0</a> <a id="20806" class="Symbol">(</a><a id="20807" class="Number">2</a> <a id="20809" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="20811" class="Number">3</a> <a id="20813" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="20815" class="Number">4</a> <a id="20817" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="20819" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a><a id="20821" class="Symbol">)</a>
  <a id="20825" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="20833" class="Number">1</a> <a id="20835" href="Agda.Builtin.Nat.html#298" class="Primitive Operator">+</a> <a id="20837" class="Symbol">(</a><a id="20838" class="Number">2</a> <a id="20840" href="Agda.Builtin.Nat.html#298" class="Primitive Operator">+</a> <a id="20842" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#20225" class="Function">foldr</a> <a id="20848" href="Agda.Builtin.Nat.html#298" class="Primitive Operator">_+_</a> <a id="20852" class="Number">0</a> <a id="20854" class="Symbol">(</a><a id="20855" class="Number">3</a> <a id="20857" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="20859" class="Number">4</a> <a id="20861" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="20863" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a><a id="20865" class="Symbol">))</a>
  <a id="20870" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="20878" class="Number">1</a> <a id="20880" href="Agda.Builtin.Nat.html#298" class="Primitive Operator">+</a> <a id="20882" class="Symbol">(</a><a id="20883" class="Number">2</a> <a id="20885" href="Agda.Builtin.Nat.html#298" class="Primitive Operator">+</a> <a id="20887" class="Symbol">(</a><a id="20888" class="Number">3</a> <a id="20890" href="Agda.Builtin.Nat.html#298" class="Primitive Operator">+</a> <a id="20892" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#20225" class="Function">foldr</a> <a id="20898" href="Agda.Builtin.Nat.html#298" class="Primitive Operator">_+_</a> <a id="20902" class="Number">0</a> <a id="20904" class="Symbol">(</a><a id="20905" class="Number">4</a> <a id="20907" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="20909" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a><a id="20911" class="Symbol">)))</a>
  <a id="20917" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="20925" class="Number">1</a> <a id="20927" href="Agda.Builtin.Nat.html#298" class="Primitive Operator">+</a> <a id="20929" class="Symbol">(</a><a id="20930" class="Number">2</a> <a id="20932" href="Agda.Builtin.Nat.html#298" class="Primitive Operator">+</a> <a id="20934" class="Symbol">(</a><a id="20935" class="Number">3</a> <a id="20937" href="Agda.Builtin.Nat.html#298" class="Primitive Operator">+</a> <a id="20939" class="Symbol">(</a><a id="20940" class="Number">4</a> <a id="20942" href="Agda.Builtin.Nat.html#298" class="Primitive Operator">+</a> <a id="20944" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#20225" class="Function">foldr</a> <a id="20950" href="Agda.Builtin.Nat.html#298" class="Primitive Operator">_+_</a> <a id="20954" class="Number">0</a> <a id="20956" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a><a id="20958" class="Symbol">)))</a>
  <a id="20964" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="20972" class="Number">1</a> <a id="20974" href="Agda.Builtin.Nat.html#298" class="Primitive Operator">+</a> <a id="20976" class="Symbol">(</a><a id="20977" class="Number">2</a> <a id="20979" href="Agda.Builtin.Nat.html#298" class="Primitive Operator">+</a> <a id="20981" class="Symbol">(</a><a id="20982" class="Number">3</a> <a id="20984" href="Agda.Builtin.Nat.html#298" class="Primitive Operator">+</a> <a id="20986" class="Symbol">(</a><a id="20987" class="Number">4</a> <a id="20989" href="Agda.Builtin.Nat.html#298" class="Primitive Operator">+</a> <a id="20991" class="Number">0</a><a id="20992" class="Symbol">)))</a>
  <a id="20998" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2892" class="Function Operator">∎</a>
</pre>{% endraw %}
{::comment}
Fold requires time linear in the length of the list.
{:/}

折叠需要关于列表长度线性的时间。

{::comment}
It is often convenient to exploit currying by applying
fold to an operator and a value to yield a new function,
and at a later point applying the resulting function:
{:/}

我们常常可以利用柯里化，将折叠作用于一个运算符和一个值，获得另一个函数，
然后在之后的时候应用获得的函数：

{% raw %}<pre class="Agda"><a id="sum"></a><a id="21337" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#21337" class="Function">sum</a> <a id="21341" class="Symbol">:</a> <a id="21343" href="plfa.Lists.html#1284" class="Datatype">List</a> <a id="21348" href="Agda.Builtin.Nat.html#165" class="Datatype">ℕ</a> <a id="21350" class="Symbol">→</a> <a id="21352" href="Agda.Builtin.Nat.html#165" class="Datatype">ℕ</a>
<a id="21354" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#21337" class="Function">sum</a> <a id="21358" class="Symbol">=</a> <a id="21360" href="plfa.Lists.html#20225" class="Function">foldr</a> <a id="21366" href="Agda.Builtin.Nat.html#298" class="Primitive Operator">_+_</a> <a id="21370" class="Number">0</a>

<a id="21373" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#21373" class="Function">_</a> <a id="21375" class="Symbol">:</a> <a id="21377" href="plfa.Lists.html#21337" class="Function">sum</a> <a id="21381" href="plfa.Lists.html#3911" class="InductiveConstructor Operator">[</a> <a id="21383" class="Number">1</a> <a id="21385" href="plfa.Lists.html#3911" class="InductiveConstructor Operator">,</a> <a id="21387" class="Number">2</a> <a id="21389" href="plfa.Lists.html#3911" class="InductiveConstructor Operator">,</a> <a id="21391" class="Number">3</a> <a id="21393" href="plfa.Lists.html#3911" class="InductiveConstructor Operator">,</a> <a id="21395" class="Number">4</a> <a id="21397" href="plfa.Lists.html#3911" class="InductiveConstructor Operator">]</a> <a id="21399" href="Agda.Builtin.Equality.html#125" class="Datatype Operator">≡</a> <a id="21401" class="Number">10</a>
<a id="21404" class="Symbol">_</a> <a id="21406" class="Symbol">=</a>
  <a id="21410" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2597" class="Function Operator">begin</a>
    <a id="21420" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#21337" class="Function">sum</a> <a id="21424" href="plfa.Lists.html#3911" class="InductiveConstructor Operator">[</a> <a id="21426" class="Number">1</a> <a id="21428" href="plfa.Lists.html#3911" class="InductiveConstructor Operator">,</a> <a id="21430" class="Number">2</a> <a id="21432" href="plfa.Lists.html#3911" class="InductiveConstructor Operator">,</a> <a id="21434" class="Number">3</a> <a id="21436" href="plfa.Lists.html#3911" class="InductiveConstructor Operator">,</a> <a id="21438" class="Number">4</a> <a id="21440" href="plfa.Lists.html#3911" class="InductiveConstructor Operator">]</a>
  <a id="21444" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="21452" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#20225" class="Function">foldr</a> <a id="21458" href="Agda.Builtin.Nat.html#298" class="Primitive Operator">_+_</a> <a id="21462" class="Number">0</a> <a id="21464" href="plfa.Lists.html#3911" class="InductiveConstructor Operator">[</a> <a id="21466" class="Number">1</a> <a id="21468" href="plfa.Lists.html#3911" class="InductiveConstructor Operator">,</a> <a id="21470" class="Number">2</a> <a id="21472" href="plfa.Lists.html#3911" class="InductiveConstructor Operator">,</a> <a id="21474" class="Number">3</a> <a id="21476" href="plfa.Lists.html#3911" class="InductiveConstructor Operator">,</a> <a id="21478" class="Number">4</a> <a id="21480" href="plfa.Lists.html#3911" class="InductiveConstructor Operator">]</a>
  <a id="21484" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="21492" class="Number">10</a>
  <a id="21497" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2892" class="Function Operator">∎</a>
</pre>{% endraw %}
{::comment}
Just as the list type has two constructors, `[]` and `_∷_`,
so the fold function takes two arguments, `e` and `_⊗_`
(in addition to the list argument).
In general, a data type with _n_ constructors will have
a corresponding fold function that takes _n_ arguments.
{:/}

正如列表由两个构造子 `[]` 和 `_∷_`，折叠函数取两个参数 `e` 和 `_⊗_`
（除去列表参数）。推广来说，一个有 _n_ 个构造子的数据类型，会有对应的
取 _n_ 个参数的折叠函数。

{::comment}
#### Exercise `product` (recommended)
{:/}

#### 练习 `product` （推荐）

{::comment}
Use fold to define a function to find the product of a list of numbers.
For example:
{:/}

使用折叠来定义一个计算列表数字之积的函数。例如：

    product [ 1 , 2 , 3 , 4 ] ≡ 24

{::comment}
{% raw %}<pre class="Agda"><a id="22148" class="Comment">-- Your code goes here</a>
</pre>{% endraw %}{:/}

{% raw %}<pre class="Agda"><a id="22185" class="Comment">-- 请将代码写在此处。</a>
</pre>{% endraw %}
{::comment}
#### Exercise `foldr-++` (recommended)
{:/}

#### 练习 `foldr-++` （推荐）

{::comment}
Show that fold and append are related as follows:
{:/}

证明折叠和附加有如下的关系：

{% raw %}<pre class="Agda"><a id="22373" class="Keyword">postulate</a>
  <a id="foldr-++"></a><a id="22385" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#22385" class="Postulate">foldr-++</a> <a id="22394" class="Symbol">:</a> <a id="22396" class="Symbol">∀</a> <a id="22398" class="Symbol">{</a><a id="22399" href="plfa.Lists.html#22399" class="Bound">A</a> <a id="22401" href="plfa.Lists.html#22401" class="Bound">B</a> <a id="22403" class="Symbol">:</a> <a id="22405" class="PrimitiveType">Set</a><a id="22408" class="Symbol">}</a> <a id="22410" class="Symbol">(</a><a id="22411" href="plfa.Lists.html#22411" class="Bound Operator">_⊗_</a> <a id="22415" class="Symbol">:</a> <a id="22417" href="plfa.Lists.html#22399" class="Bound">A</a> <a id="22419" class="Symbol">→</a> <a id="22421" href="plfa.Lists.html#22401" class="Bound">B</a> <a id="22423" class="Symbol">→</a> <a id="22425" href="plfa.Lists.html#22401" class="Bound">B</a><a id="22426" class="Symbol">)</a> <a id="22428" class="Symbol">(</a><a id="22429" href="plfa.Lists.html#22429" class="Bound">e</a> <a id="22431" class="Symbol">:</a> <a id="22433" href="plfa.Lists.html#22401" class="Bound">B</a><a id="22434" class="Symbol">)</a> <a id="22436" class="Symbol">(</a><a id="22437" href="plfa.Lists.html#22437" class="Bound">xs</a> <a id="22440" href="plfa.Lists.html#22440" class="Bound">ys</a> <a id="22443" class="Symbol">:</a> <a id="22445" href="plfa.Lists.html#1284" class="Datatype">List</a> <a id="22450" href="plfa.Lists.html#22399" class="Bound">A</a><a id="22451" class="Symbol">)</a> <a id="22453" class="Symbol">→</a>
    <a id="22459" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#20225" class="Function">foldr</a> <a id="22465" href="plfa.Lists.html#22411" class="Bound Operator">_⊗_</a> <a id="22469" href="plfa.Lists.html#22429" class="Bound">e</a> <a id="22471" class="Symbol">(</a><a id="22472" href="plfa.Lists.html#22437" class="Bound">xs</a> <a id="22475" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="22478" href="plfa.Lists.html#22440" class="Bound">ys</a><a id="22480" class="Symbol">)</a> <a id="22482" href="Agda.Builtin.Equality.html#125" class="Datatype Operator">≡</a> <a id="22484" href="plfa.Lists.html#20225" class="Function">foldr</a> <a id="22490" href="plfa.Lists.html#22411" class="Bound Operator">_⊗_</a> <a id="22494" class="Symbol">(</a><a id="22495" href="plfa.Lists.html#20225" class="Function">foldr</a> <a id="22501" href="plfa.Lists.html#22411" class="Bound Operator">_⊗_</a> <a id="22505" href="plfa.Lists.html#22429" class="Bound">e</a> <a id="22507" href="plfa.Lists.html#22440" class="Bound">ys</a><a id="22509" class="Symbol">)</a> <a id="22511" href="plfa.Lists.html#22437" class="Bound">xs</a>
</pre>{% endraw %}

{::comment}
#### Exercise `map-is-foldr`
{:/}

#### 练习 `map-is-foldr`

{::comment}
Show that map can be defined using fold:
{:/}

证明映射可以用折叠定义：

{% raw %}<pre class="Agda"><a id="22668" class="Keyword">postulate</a>
  <a id="map-is-foldr"></a><a id="22680" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#22680" class="Postulate">map-is-foldr</a> <a id="22693" class="Symbol">:</a> <a id="22695" class="Symbol">∀</a> <a id="22697" class="Symbol">{</a><a id="22698" href="plfa.Lists.html#22698" class="Bound">A</a> <a id="22700" href="plfa.Lists.html#22700" class="Bound">B</a> <a id="22702" class="Symbol">:</a> <a id="22704" class="PrimitiveType">Set</a><a id="22707" class="Symbol">}</a> <a id="22709" class="Symbol">{</a><a id="22710" href="plfa.Lists.html#22710" class="Bound">f</a> <a id="22712" class="Symbol">:</a> <a id="22714" href="plfa.Lists.html#22698" class="Bound">A</a> <a id="22716" class="Symbol">→</a> <a id="22718" href="plfa.Lists.html#22700" class="Bound">B</a><a id="22719" class="Symbol">}</a> <a id="22721" class="Symbol">→</a>
    <a id="22727" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#16959" class="Function">map</a> <a id="22731" href="plfa.Lists.html#22710" class="Bound">f</a> <a id="22733" href="Agda.Builtin.Equality.html#125" class="Datatype Operator">≡</a> <a id="22735" href="plfa.Lists.html#20225" class="Function">foldr</a> <a id="22741" class="Symbol">(λ</a> <a id="22744" href="plfa.Lists.html#22744" class="Bound">x</a> <a id="22746" href="plfa.Lists.html#22746" class="Bound">xs</a> <a id="22749" class="Symbol">→</a> <a id="22751" href="plfa.Lists.html#22710" class="Bound">f</a> <a id="22753" href="plfa.Lists.html#22744" class="Bound">x</a> <a id="22755" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="22757" href="plfa.Lists.html#22746" class="Bound">xs</a><a id="22759" class="Symbol">)</a> <a id="22761" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a>
</pre>{% endraw %}
{::comment}
This requires extensionality.
{:/}

此证明需要外延性。

{::comment}
#### Exercise `fold-Tree`
{:/}

#### 练习 `fold-Tree`

{::comment}
Define a suitable fold function for the type of trees given earlier:
{:/}

为之前给出的树数据类型定义一个折叠函数：

{% raw %}<pre class="Agda"><a id="23006" class="Keyword">postulate</a>
  <a id="fold-Tree"></a><a id="23018" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#23018" class="Postulate">fold-Tree</a> <a id="23028" class="Symbol">:</a> <a id="23030" class="Symbol">∀</a> <a id="23032" class="Symbol">{</a><a id="23033" href="plfa.Lists.html#23033" class="Bound">A</a> <a id="23035" href="plfa.Lists.html#23035" class="Bound">B</a> <a id="23037" href="plfa.Lists.html#23037" class="Bound">C</a> <a id="23039" class="Symbol">:</a> <a id="23041" class="PrimitiveType">Set</a><a id="23044" class="Symbol">}</a>
    <a id="23050" class="Symbol">→</a> <a id="23052" class="Symbol">(</a><a id="23053" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#23033" class="Bound">A</a> <a id="23055" class="Symbol">→</a> <a id="23057" href="plfa.Lists.html#23037" class="Bound">C</a><a id="23058" class="Symbol">)</a> <a id="23060" class="Symbol">→</a> <a id="23062" class="Symbol">(</a><a id="23063" href="plfa.Lists.html#23037" class="Bound">C</a> <a id="23065" class="Symbol">→</a> <a id="23067" href="plfa.Lists.html#23035" class="Bound">B</a> <a id="23069" class="Symbol">→</a> <a id="23071" href="plfa.Lists.html#23037" class="Bound">C</a> <a id="23073" class="Symbol">→</a> <a id="23075" href="plfa.Lists.html#23037" class="Bound">C</a><a id="23076" class="Symbol">)</a> <a id="23078" class="Symbol">→</a> <a id="23080" href="plfa.Lists.html#19666" class="Datatype">Tree</a> <a id="23085" href="plfa.Lists.html#23033" class="Bound">A</a> <a id="23087" href="plfa.Lists.html#23035" class="Bound">B</a> <a id="23089" class="Symbol">→</a> <a id="23091" href="plfa.Lists.html#23037" class="Bound">C</a>
</pre>{% endraw %}
{::comment}
{% raw %}<pre class="Agda"><a id="23114" class="Comment">-- Your code goes here</a>
</pre>{% endraw %}{:/}

{% raw %}<pre class="Agda"><a id="23151" class="Comment">-- 请将代码写在此处。</a>
</pre>{% endraw %}
{::comment}
#### Exercise `map-is-fold-Tree`
{:/}

#### 练习 `map-is-fold-Tree`

{::comment}
Demonstrate an analogue of `map-is-foldr` for the type of trees.
{:/}

对于树数据类型，证明与 `map-is-foldr` 相似的性质。

{::comment}
{% raw %}<pre class="Agda"><a id="23382" class="Comment">-- Your code goes here</a>
</pre>{% endraw %}{:/}

{% raw %}<pre class="Agda"><a id="23419" class="Comment">-- 请将代码写在此处。</a>
</pre>{% endraw %}
{::comment}
#### Exercise `sum-downFrom` (stretch)
{:/}

#### 证明 `sum-downFrom` （延伸）

{::comment}
Define a function that counts down as follows:
{:/}

定义一个向下数数的函数：

{% raw %}<pre class="Agda"><a id="downFrom"></a><a id="23606" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#23606" class="Function">downFrom</a> <a id="23615" class="Symbol">:</a> <a id="23617" href="Agda.Builtin.Nat.html#165" class="Datatype">ℕ</a> <a id="23619" class="Symbol">→</a> <a id="23621" href="plfa.Lists.html#1284" class="Datatype">List</a> <a id="23626" href="Agda.Builtin.Nat.html#165" class="Datatype">ℕ</a>
<a id="23628" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#23606" class="Function">downFrom</a> <a id="23637" href="Agda.Builtin.Nat.html#183" class="InductiveConstructor">zero</a>     <a id="23646" class="Symbol">=</a>  <a id="23649" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a>
<a id="23652" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#23606" class="Function">downFrom</a> <a id="23661" class="Symbol">(</a><a id="23662" href="Agda.Builtin.Nat.html#196" class="InductiveConstructor">suc</a> <a id="23666" href="plfa.Lists.html#23666" class="Bound">n</a><a id="23667" class="Symbol">)</a>  <a id="23670" class="Symbol">=</a>  <a id="23673" href="plfa.Lists.html#23666" class="Bound">n</a> <a id="23675" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="23677" href="plfa.Lists.html#23606" class="Function">downFrom</a> <a id="23686" href="plfa.Lists.html#23666" class="Bound">n</a>
</pre>{% endraw %}
{::comment}
For example:
{:/}

例如：

{% raw %}<pre class="Agda"><a id="23733" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#23733" class="Function">_</a> <a id="23735" class="Symbol">:</a> <a id="23737" href="plfa.Lists.html#23606" class="Function">downFrom</a> <a id="23746" class="Number">3</a> <a id="23748" href="Agda.Builtin.Equality.html#125" class="Datatype Operator">≡</a> <a id="23750" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">[</a> <a id="23752" class="Number">2</a> <a id="23754" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">,</a> <a id="23756" class="Number">1</a> <a id="23758" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">,</a> <a id="23760" class="Number">0</a> <a id="23762" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">]</a>
<a id="23764" class="Symbol">_</a> <a id="23766" class="Symbol">=</a> <a id="23768" href="Agda.Builtin.Equality.html#182" class="InductiveConstructor">refl</a>
</pre>{% endraw %}
{::comment}
Prove that the sum of the numbers `(n - 1) + ⋯ + 0` is
equal to `n * (n ∸ 1) / 2`:
{:/}

证明数列之和 `(n - 1) + ⋯ + 0` 等于 `n * (n ∸ 1) / 2`：

{% raw %}<pre class="Agda"><a id="23931" class="Keyword">postulate</a>
  <a id="sum-downFrom"></a><a id="23943" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#23943" class="Postulate">sum-downFrom</a> <a id="23956" class="Symbol">:</a> <a id="23958" class="Symbol">∀</a> <a id="23960" class="Symbol">(</a><a id="23961" href="plfa.Lists.html#23961" class="Bound">n</a> <a id="23963" class="Symbol">:</a> <a id="23965" href="Agda.Builtin.Nat.html#165" class="Datatype">ℕ</a><a id="23966" class="Symbol">)</a>
    <a id="23972" class="Symbol">→</a> <a id="23974" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#21337" class="Function">sum</a> <a id="23978" class="Symbol">(</a><a id="23979" href="plfa.Lists.html#23606" class="Function">downFrom</a> <a id="23988" href="plfa.Lists.html#23961" class="Bound">n</a><a id="23989" class="Symbol">)</a> <a id="23991" href="Agda.Builtin.Nat.html#501" class="Primitive Operator">*</a> <a id="23993" class="Number">2</a> <a id="23995" href="Agda.Builtin.Equality.html#125" class="Datatype Operator">≡</a> <a id="23997" href="plfa.Lists.html#23961" class="Bound">n</a> <a id="23999" href="Agda.Builtin.Nat.html#501" class="Primitive Operator">*</a> <a id="24001" class="Symbol">(</a><a id="24002" href="plfa.Lists.html#23961" class="Bound">n</a> <a id="24004" href="Agda.Builtin.Nat.html#388" class="Primitive Operator">∸</a> <a id="24006" class="Number">1</a><a id="24007" class="Symbol">)</a>
</pre>{% endraw %}

{::comment}
## Monoids
{:/}

## 幺半群

{::comment}
Typically when we use a fold the operator is associative and the
value is a left and right identity for the operator, meaning that the
operator and the value form a _monoid_.
{:/}

一般来说，我们会对于折叠函数使用一个满足结合律的运算符，和这个运算符的左右幺元。
这意味着这个运算符和这个值形成了一个**幺半群**（Monoid）。

{::comment}
We can define a monoid as a suitable record type:
{:/}

我们可以用一个合适的记录类型来定义幺半群：

{% raw %}<pre class="Agda"><a id="24417" class="Keyword">record</a> <a id="IsMonoid"></a><a id="24424" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#24424" class="Record">IsMonoid</a> <a id="24433" class="Symbol">{</a><a id="24434" href="plfa.Lists.html#24434" class="Bound">A</a> <a id="24436" class="Symbol">:</a> <a id="24438" class="PrimitiveType">Set</a><a id="24441" class="Symbol">}</a> <a id="24443" class="Symbol">(</a><a id="24444" href="plfa.Lists.html#24444" class="Bound Operator">_⊗_</a> <a id="24448" class="Symbol">:</a> <a id="24450" href="plfa.Lists.html#24434" class="Bound">A</a> <a id="24452" class="Symbol">→</a> <a id="24454" href="plfa.Lists.html#24434" class="Bound">A</a> <a id="24456" class="Symbol">→</a> <a id="24458" href="plfa.Lists.html#24434" class="Bound">A</a><a id="24459" class="Symbol">)</a> <a id="24461" class="Symbol">(</a><a id="24462" href="plfa.Lists.html#24462" class="Bound">e</a> <a id="24464" class="Symbol">:</a> <a id="24466" href="plfa.Lists.html#24434" class="Bound">A</a><a id="24467" class="Symbol">)</a> <a id="24469" class="Symbol">:</a> <a id="24471" class="PrimitiveType">Set</a> <a id="24475" class="Keyword">where</a>
  <a id="24483" class="Keyword">field</a>
    <a id="IsMonoid.assoc"></a><a id="24493" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#24493" class="Field">assoc</a> <a id="24499" class="Symbol">:</a> <a id="24501" class="Symbol">∀</a> <a id="24503" class="Symbol">(</a><a id="24504" href="plfa.Lists.html#24504" class="Bound">x</a> <a id="24506" href="plfa.Lists.html#24506" class="Bound">y</a> <a id="24508" href="plfa.Lists.html#24508" class="Bound">z</a> <a id="24510" class="Symbol">:</a> <a id="24512" href="plfa.Lists.html#24434" class="Bound">A</a><a id="24513" class="Symbol">)</a> <a id="24515" class="Symbol">→</a> <a id="24517" class="Symbol">(</a><a id="24518" href="plfa.Lists.html#24504" class="Bound">x</a> <a id="24520" href="plfa.Lists.html#24444" class="Bound Operator">⊗</a> <a id="24522" href="plfa.Lists.html#24506" class="Bound">y</a><a id="24523" class="Symbol">)</a> <a id="24525" href="plfa.Lists.html#24444" class="Bound Operator">⊗</a> <a id="24527" href="plfa.Lists.html#24508" class="Bound">z</a> <a id="24529" href="Agda.Builtin.Equality.html#125" class="Datatype Operator">≡</a> <a id="24531" href="plfa.Lists.html#24504" class="Bound">x</a> <a id="24533" href="plfa.Lists.html#24444" class="Bound Operator">⊗</a> <a id="24535" class="Symbol">(</a><a id="24536" href="plfa.Lists.html#24506" class="Bound">y</a> <a id="24538" href="plfa.Lists.html#24444" class="Bound Operator">⊗</a> <a id="24540" href="plfa.Lists.html#24508" class="Bound">z</a><a id="24541" class="Symbol">)</a>
    <a id="IsMonoid.identityˡ"></a><a id="24547" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#24547" class="Field">identityˡ</a> <a id="24557" class="Symbol">:</a> <a id="24559" class="Symbol">∀</a> <a id="24561" class="Symbol">(</a><a id="24562" href="plfa.Lists.html#24562" class="Bound">x</a> <a id="24564" class="Symbol">:</a> <a id="24566" href="plfa.Lists.html#24434" class="Bound">A</a><a id="24567" class="Symbol">)</a> <a id="24569" class="Symbol">→</a> <a id="24571" href="plfa.Lists.html#24462" class="Bound">e</a> <a id="24573" href="plfa.Lists.html#24444" class="Bound Operator">⊗</a> <a id="24575" href="plfa.Lists.html#24562" class="Bound">x</a> <a id="24577" href="Agda.Builtin.Equality.html#125" class="Datatype Operator">≡</a> <a id="24579" href="plfa.Lists.html#24562" class="Bound">x</a>
    <a id="IsMonoid.identityʳ"></a><a id="24585" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#24585" class="Field">identityʳ</a> <a id="24595" class="Symbol">:</a> <a id="24597" class="Symbol">∀</a> <a id="24599" class="Symbol">(</a><a id="24600" href="plfa.Lists.html#24600" class="Bound">x</a> <a id="24602" class="Symbol">:</a> <a id="24604" href="plfa.Lists.html#24434" class="Bound">A</a><a id="24605" class="Symbol">)</a> <a id="24607" class="Symbol">→</a> <a id="24609" href="plfa.Lists.html#24600" class="Bound">x</a> <a id="24611" href="plfa.Lists.html#24444" class="Bound Operator">⊗</a> <a id="24613" href="plfa.Lists.html#24462" class="Bound">e</a> <a id="24615" href="Agda.Builtin.Equality.html#125" class="Datatype Operator">≡</a> <a id="24617" href="plfa.Lists.html#24600" class="Bound">x</a>

<a id="24620" class="Keyword">open</a> <a id="24625" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#24424" class="Module">IsMonoid</a>
</pre>{% endraw %}
{::comment}
As examples, sum and zero, multiplication and one, and append and the empty
list, are all examples of monoids:
{:/}

举例来说，加法和零，乘法和一，附加和空列表，都是幺半群：

{% raw %}<pre class="Agda"><a id="+-monoid"></a><a id="24802" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#24802" class="Function">+-monoid</a> <a id="24811" class="Symbol">:</a> <a id="24813" href="plfa.Lists.html#24424" class="Record">IsMonoid</a> <a id="24822" href="Agda.Builtin.Nat.html#298" class="Primitive Operator">_+_</a> <a id="24826" class="Number">0</a>
<a id="24828" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#24802" class="Function">+-monoid</a> <a id="24837" class="Symbol">=</a>
  <a id="24841" class="Keyword">record</a>
    <a id="24852" class="Symbol">{</a> <a id="24854" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#24493" class="Field">assoc</a> <a id="24860" class="Symbol">=</a> <a id="24862" href="https://agda.github.io/agda-stdlib/v1.1/Data.Nat.Properties.html#11578" class="Function">+-assoc</a>
    <a id="24874" class="Symbol">;</a> <a id="24876" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#24547" class="Field">identityˡ</a> <a id="24886" class="Symbol">=</a> <a id="24888" href="https://agda.github.io/agda-stdlib/v1.1/Data.Nat.Properties.html#11679" class="Function">+-identityˡ</a>
    <a id="24904" class="Symbol">;</a> <a id="24906" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#24585" class="Field">identityʳ</a> <a id="24916" class="Symbol">=</a> <a id="24918" href="https://agda.github.io/agda-stdlib/v1.1/Data.Nat.Properties.html#11734" class="Function">+-identityʳ</a>
    <a id="24934" class="Symbol">}</a>

<a id="*-monoid"></a><a id="24937" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#24937" class="Function">*-monoid</a> <a id="24946" class="Symbol">:</a> <a id="24948" href="plfa.Lists.html#24424" class="Record">IsMonoid</a> <a id="24957" href="Agda.Builtin.Nat.html#501" class="Primitive Operator">_*_</a> <a id="24961" class="Number">1</a>
<a id="24963" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#24937" class="Function">*-monoid</a> <a id="24972" class="Symbol">=</a>
  <a id="24976" class="Keyword">record</a>
    <a id="24987" class="Symbol">{</a> <a id="24989" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#24493" class="Field">assoc</a> <a id="24995" class="Symbol">=</a> <a id="24997" href="https://agda.github.io/agda-stdlib/v1.1/Data.Nat.Properties.html#18464" class="Function">*-assoc</a>
    <a id="25009" class="Symbol">;</a> <a id="25011" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#24547" class="Field">identityˡ</a> <a id="25021" class="Symbol">=</a> <a id="25023" href="https://agda.github.io/agda-stdlib/v1.1/Data.Nat.Properties.html#17362" class="Function">*-identityˡ</a>
    <a id="25039" class="Symbol">;</a> <a id="25041" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#24585" class="Field">identityʳ</a> <a id="25051" class="Symbol">=</a> <a id="25053" href="https://agda.github.io/agda-stdlib/v1.1/Data.Nat.Properties.html#17426" class="Function">*-identityʳ</a>
    <a id="25069" class="Symbol">}</a>

<a id="++-monoid"></a><a id="25072" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#25072" class="Function">++-monoid</a> <a id="25082" class="Symbol">:</a> <a id="25084" class="Symbol">∀</a> <a id="25086" class="Symbol">{</a><a id="25087" href="plfa.Lists.html#25087" class="Bound">A</a> <a id="25089" class="Symbol">:</a> <a id="25091" class="PrimitiveType">Set</a><a id="25094" class="Symbol">}</a> <a id="25096" class="Symbol">→</a> <a id="25098" href="plfa.Lists.html#24424" class="Record">IsMonoid</a> <a id="25107" class="Symbol">{</a><a id="25108" href="plfa.Lists.html#1284" class="Datatype">List</a> <a id="25113" href="plfa.Lists.html#25087" class="Bound">A</a><a id="25114" class="Symbol">}</a> <a id="25116" href="plfa.Lists.html#4651" class="Function Operator">_++_</a> <a id="25121" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a>
<a id="25124" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#25072" class="Function">++-monoid</a> <a id="25134" class="Symbol">=</a>
  <a id="25138" class="Keyword">record</a>
    <a id="25149" class="Symbol">{</a> <a id="25151" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#24493" class="Field">assoc</a> <a id="25157" class="Symbol">=</a> <a id="25159" href="plfa.Lists.html#6032" class="Function">++-assoc</a>
    <a id="25172" class="Symbol">;</a> <a id="25174" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#24547" class="Field">identityˡ</a> <a id="25184" class="Symbol">=</a> <a id="25186" href="plfa.Lists.html#7608" class="Function">++-identityˡ</a>
    <a id="25203" class="Symbol">;</a> <a id="25205" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#24585" class="Field">identityʳ</a> <a id="25215" class="Symbol">=</a> <a id="25217" href="plfa.Lists.html#7822" class="Function">++-identityʳ</a>
    <a id="25234" class="Symbol">}</a>
</pre>{% endraw %}
{::comment}
If `_⊗_` and `e` form a monoid, then we can re-express fold on the
same operator and an arbitrary value:
{:/}

如果 `_⊗_` 和 `e` 构成一个幺半群，那么我们可以用相同的运算符和一个任意的值来表示折叠：

{% raw %}<pre class="Agda"><a id="foldr-monoid"></a><a id="25419" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#25419" class="Function">foldr-monoid</a> <a id="25432" class="Symbol">:</a> <a id="25434" class="Symbol">∀</a> <a id="25436" class="Symbol">{</a><a id="25437" href="plfa.Lists.html#25437" class="Bound">A</a> <a id="25439" class="Symbol">:</a> <a id="25441" class="PrimitiveType">Set</a><a id="25444" class="Symbol">}</a> <a id="25446" class="Symbol">(</a><a id="25447" href="plfa.Lists.html#25447" class="Bound Operator">_⊗_</a> <a id="25451" class="Symbol">:</a> <a id="25453" href="plfa.Lists.html#25437" class="Bound">A</a> <a id="25455" class="Symbol">→</a> <a id="25457" href="plfa.Lists.html#25437" class="Bound">A</a> <a id="25459" class="Symbol">→</a> <a id="25461" href="plfa.Lists.html#25437" class="Bound">A</a><a id="25462" class="Symbol">)</a> <a id="25464" class="Symbol">(</a><a id="25465" href="plfa.Lists.html#25465" class="Bound">e</a> <a id="25467" class="Symbol">:</a> <a id="25469" href="plfa.Lists.html#25437" class="Bound">A</a><a id="25470" class="Symbol">)</a> <a id="25472" class="Symbol">→</a> <a id="25474" href="plfa.Lists.html#24424" class="Record">IsMonoid</a> <a id="25483" href="plfa.Lists.html#25447" class="Bound Operator">_⊗_</a> <a id="25487" href="plfa.Lists.html#25465" class="Bound">e</a> <a id="25489" class="Symbol">→</a>
  <a id="25493" class="Symbol">∀</a> <a id="25495" class="Symbol">(</a><a id="25496" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#25496" class="Bound">xs</a> <a id="25499" class="Symbol">:</a> <a id="25501" href="plfa.Lists.html#1284" class="Datatype">List</a> <a id="25506" href="plfa.Lists.html#25437" class="Bound">A</a><a id="25507" class="Symbol">)</a> <a id="25509" class="Symbol">(</a><a id="25510" href="plfa.Lists.html#25510" class="Bound">y</a> <a id="25512" class="Symbol">:</a> <a id="25514" href="plfa.Lists.html#25437" class="Bound">A</a><a id="25515" class="Symbol">)</a> <a id="25517" class="Symbol">→</a> <a id="25519" href="plfa.Lists.html#20225" class="Function">foldr</a> <a id="25525" href="plfa.Lists.html#25447" class="Bound Operator">_⊗_</a> <a id="25529" href="plfa.Lists.html#25510" class="Bound">y</a> <a id="25531" href="plfa.Lists.html#25496" class="Bound">xs</a> <a id="25534" href="Agda.Builtin.Equality.html#125" class="Datatype Operator">≡</a> <a id="25536" href="plfa.Lists.html#20225" class="Function">foldr</a> <a id="25542" href="plfa.Lists.html#25447" class="Bound Operator">_⊗_</a> <a id="25546" href="plfa.Lists.html#25465" class="Bound">e</a> <a id="25548" href="plfa.Lists.html#25496" class="Bound">xs</a> <a id="25551" href="plfa.Lists.html#25447" class="Bound Operator">⊗</a> <a id="25553" href="plfa.Lists.html#25510" class="Bound">y</a>
<a id="25555" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#25419" class="Function">foldr-monoid</a> <a id="25568" href="plfa.Lists.html#25568" class="Bound Operator">_⊗_</a> <a id="25572" href="plfa.Lists.html#25572" class="Bound">e</a> <a id="25574" href="plfa.Lists.html#25574" class="Bound">⊗-monoid</a> <a id="25583" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a> <a id="25586" href="plfa.Lists.html#25586" class="Bound">y</a> <a id="25588" class="Symbol">=</a>
  <a id="25592" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2597" class="Function Operator">begin</a>
    <a id="25602" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#20225" class="Function">foldr</a> <a id="25608" href="plfa.Lists.html#25568" class="Bound Operator">_⊗_</a> <a id="25612" href="plfa.Lists.html#25586" class="Bound">y</a> <a id="25614" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a>
  <a id="25619" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="25627" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#25586" class="Bound">y</a>
  <a id="25631" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2714" class="Function Operator">≡⟨</a> <a id="25634" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#939" class="Function">sym</a> <a id="25638" class="Symbol">(</a><a id="25639" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#24547" class="Field">identityˡ</a> <a id="25649" href="plfa.Lists.html#25574" class="Bound">⊗-monoid</a> <a id="25658" href="plfa.Lists.html#25586" class="Bound">y</a><a id="25659" class="Symbol">)</a> <a id="25661" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2714" class="Function Operator">⟩</a>
    <a id="25667" class="Symbol">(</a><a id="25668" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#25572" class="Bound">e</a> <a id="25670" href="plfa.Lists.html#25568" class="Bound Operator">⊗</a> <a id="25672" href="plfa.Lists.html#25586" class="Bound">y</a><a id="25673" class="Symbol">)</a>
  <a id="25677" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="25685" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#20225" class="Function">foldr</a> <a id="25691" href="plfa.Lists.html#25568" class="Bound Operator">_⊗_</a> <a id="25695" href="plfa.Lists.html#25572" class="Bound">e</a> <a id="25697" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a> <a id="25700" href="plfa.Lists.html#25568" class="Bound Operator">⊗</a> <a id="25702" href="plfa.Lists.html#25586" class="Bound">y</a>
  <a id="25706" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2892" class="Function Operator">∎</a>
<a id="25708" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#25419" class="Function">foldr-monoid</a> <a id="25721" href="plfa.Lists.html#25721" class="Bound Operator">_⊗_</a> <a id="25725" href="plfa.Lists.html#25725" class="Bound">e</a> <a id="25727" href="plfa.Lists.html#25727" class="Bound">⊗-monoid</a> <a id="25736" class="Symbol">(</a><a id="25737" href="plfa.Lists.html#25737" class="Bound">x</a> <a id="25739" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="25741" href="plfa.Lists.html#25741" class="Bound">xs</a><a id="25743" class="Symbol">)</a> <a id="25745" href="plfa.Lists.html#25745" class="Bound">y</a> <a id="25747" class="Symbol">=</a>
  <a id="25751" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2597" class="Function Operator">begin</a>
    <a id="25761" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#20225" class="Function">foldr</a> <a id="25767" href="plfa.Lists.html#25721" class="Bound Operator">_⊗_</a> <a id="25771" href="plfa.Lists.html#25745" class="Bound">y</a> <a id="25773" class="Symbol">(</a><a id="25774" href="plfa.Lists.html#25737" class="Bound">x</a> <a id="25776" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="25778" href="plfa.Lists.html#25741" class="Bound">xs</a><a id="25780" class="Symbol">)</a>
  <a id="25784" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="25792" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#25737" class="Bound">x</a> <a id="25794" href="plfa.Lists.html#25721" class="Bound Operator">⊗</a> <a id="25796" class="Symbol">(</a><a id="25797" href="plfa.Lists.html#20225" class="Function">foldr</a> <a id="25803" href="plfa.Lists.html#25721" class="Bound Operator">_⊗_</a> <a id="25807" href="plfa.Lists.html#25745" class="Bound">y</a> <a id="25809" href="plfa.Lists.html#25741" class="Bound">xs</a><a id="25811" class="Symbol">)</a>
  <a id="25815" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2714" class="Function Operator">≡⟨</a> <a id="25818" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#1090" class="Function">cong</a> <a id="25823" class="Symbol">(</a><a id="25824" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#25737" class="Bound">x</a> <a id="25826" href="plfa.Lists.html#25721" class="Bound Operator">⊗_</a><a id="25828" class="Symbol">)</a> <a id="25830" class="Symbol">(</a><a id="25831" href="plfa.Lists.html#25419" class="Function">foldr-monoid</a> <a id="25844" href="plfa.Lists.html#25721" class="Bound Operator">_⊗_</a> <a id="25848" href="plfa.Lists.html#25725" class="Bound">e</a> <a id="25850" href="plfa.Lists.html#25727" class="Bound">⊗-monoid</a> <a id="25859" href="plfa.Lists.html#25741" class="Bound">xs</a> <a id="25862" href="plfa.Lists.html#25745" class="Bound">y</a><a id="25863" class="Symbol">)</a> <a id="25865" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2714" class="Function Operator">⟩</a>
    <a id="25871" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#25737" class="Bound">x</a> <a id="25873" href="plfa.Lists.html#25721" class="Bound Operator">⊗</a> <a id="25875" class="Symbol">(</a><a id="25876" href="plfa.Lists.html#20225" class="Function">foldr</a> <a id="25882" href="plfa.Lists.html#25721" class="Bound Operator">_⊗_</a> <a id="25886" href="plfa.Lists.html#25725" class="Bound">e</a> <a id="25888" href="plfa.Lists.html#25741" class="Bound">xs</a> <a id="25891" href="plfa.Lists.html#25721" class="Bound Operator">⊗</a> <a id="25893" href="plfa.Lists.html#25745" class="Bound">y</a><a id="25894" class="Symbol">)</a>
  <a id="25898" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2714" class="Function Operator">≡⟨</a> <a id="25901" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#939" class="Function">sym</a> <a id="25905" class="Symbol">(</a><a id="25906" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#24493" class="Field">assoc</a> <a id="25912" href="plfa.Lists.html#25727" class="Bound">⊗-monoid</a> <a id="25921" href="plfa.Lists.html#25737" class="Bound">x</a> <a id="25923" class="Symbol">(</a><a id="25924" href="plfa.Lists.html#20225" class="Function">foldr</a> <a id="25930" href="plfa.Lists.html#25721" class="Bound Operator">_⊗_</a> <a id="25934" href="plfa.Lists.html#25725" class="Bound">e</a> <a id="25936" href="plfa.Lists.html#25741" class="Bound">xs</a><a id="25938" class="Symbol">)</a> <a id="25940" href="plfa.Lists.html#25745" class="Bound">y</a><a id="25941" class="Symbol">)</a> <a id="25943" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2714" class="Function Operator">⟩</a>
    <a id="25949" class="Symbol">(</a><a id="25950" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#25737" class="Bound">x</a> <a id="25952" href="plfa.Lists.html#25721" class="Bound Operator">⊗</a> <a id="25954" href="plfa.Lists.html#20225" class="Function">foldr</a> <a id="25960" href="plfa.Lists.html#25721" class="Bound Operator">_⊗_</a> <a id="25964" href="plfa.Lists.html#25725" class="Bound">e</a> <a id="25966" href="plfa.Lists.html#25741" class="Bound">xs</a><a id="25968" class="Symbol">)</a> <a id="25970" href="plfa.Lists.html#25721" class="Bound Operator">⊗</a> <a id="25972" href="plfa.Lists.html#25745" class="Bound">y</a>
  <a id="25976" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">≡⟨⟩</a>
    <a id="25984" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#20225" class="Function">foldr</a> <a id="25990" href="plfa.Lists.html#25721" class="Bound Operator">_⊗_</a> <a id="25994" href="plfa.Lists.html#25725" class="Bound">e</a> <a id="25996" class="Symbol">(</a><a id="25997" href="plfa.Lists.html#25737" class="Bound">x</a> <a id="25999" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="26001" href="plfa.Lists.html#25741" class="Bound">xs</a><a id="26003" class="Symbol">)</a> <a id="26005" href="plfa.Lists.html#25721" class="Bound Operator">⊗</a> <a id="26007" href="plfa.Lists.html#25745" class="Bound">y</a>
  <a id="26011" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2892" class="Function Operator">∎</a>
</pre>{% endraw %}
{::comment}
As a consequence, using a previous exercise, we have the following:
{:/}

使用之前练习中的一个结论，我们可以得到如下：

{% raw %}<pre class="Agda"><a id="foldr-monoid-++"></a><a id="26132" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#26132" class="Function">foldr-monoid-++</a> <a id="26148" class="Symbol">:</a> <a id="26150" class="Symbol">∀</a> <a id="26152" class="Symbol">{</a><a id="26153" href="plfa.Lists.html#26153" class="Bound">A</a> <a id="26155" class="Symbol">:</a> <a id="26157" class="PrimitiveType">Set</a><a id="26160" class="Symbol">}</a> <a id="26162" class="Symbol">(</a><a id="26163" href="plfa.Lists.html#26163" class="Bound Operator">_⊗_</a> <a id="26167" class="Symbol">:</a> <a id="26169" href="plfa.Lists.html#26153" class="Bound">A</a> <a id="26171" class="Symbol">→</a> <a id="26173" href="plfa.Lists.html#26153" class="Bound">A</a> <a id="26175" class="Symbol">→</a> <a id="26177" href="plfa.Lists.html#26153" class="Bound">A</a><a id="26178" class="Symbol">)</a> <a id="26180" class="Symbol">(</a><a id="26181" href="plfa.Lists.html#26181" class="Bound">e</a> <a id="26183" class="Symbol">:</a> <a id="26185" href="plfa.Lists.html#26153" class="Bound">A</a><a id="26186" class="Symbol">)</a> <a id="26188" class="Symbol">→</a> <a id="26190" href="plfa.Lists.html#24424" class="Record">IsMonoid</a> <a id="26199" href="plfa.Lists.html#26163" class="Bound Operator">_⊗_</a> <a id="26203" href="plfa.Lists.html#26181" class="Bound">e</a> <a id="26205" class="Symbol">→</a>
  <a id="26209" class="Symbol">∀</a> <a id="26211" class="Symbol">(</a><a id="26212" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#26212" class="Bound">xs</a> <a id="26215" href="plfa.Lists.html#26215" class="Bound">ys</a> <a id="26218" class="Symbol">:</a> <a id="26220" href="plfa.Lists.html#1284" class="Datatype">List</a> <a id="26225" href="plfa.Lists.html#26153" class="Bound">A</a><a id="26226" class="Symbol">)</a> <a id="26228" class="Symbol">→</a> <a id="26230" href="plfa.Lists.html#20225" class="Function">foldr</a> <a id="26236" href="plfa.Lists.html#26163" class="Bound Operator">_⊗_</a> <a id="26240" href="plfa.Lists.html#26181" class="Bound">e</a> <a id="26242" class="Symbol">(</a><a id="26243" href="plfa.Lists.html#26212" class="Bound">xs</a> <a id="26246" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="26249" href="plfa.Lists.html#26215" class="Bound">ys</a><a id="26251" class="Symbol">)</a> <a id="26253" href="Agda.Builtin.Equality.html#125" class="Datatype Operator">≡</a> <a id="26255" href="plfa.Lists.html#20225" class="Function">foldr</a> <a id="26261" href="plfa.Lists.html#26163" class="Bound Operator">_⊗_</a> <a id="26265" href="plfa.Lists.html#26181" class="Bound">e</a> <a id="26267" href="plfa.Lists.html#26212" class="Bound">xs</a> <a id="26270" href="plfa.Lists.html#26163" class="Bound Operator">⊗</a> <a id="26272" href="plfa.Lists.html#20225" class="Function">foldr</a> <a id="26278" href="plfa.Lists.html#26163" class="Bound Operator">_⊗_</a> <a id="26282" href="plfa.Lists.html#26181" class="Bound">e</a> <a id="26284" href="plfa.Lists.html#26215" class="Bound">ys</a>
<a id="26287" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#26132" class="Function">foldr-monoid-++</a> <a id="26303" href="plfa.Lists.html#26303" class="Bound Operator">_⊗_</a> <a id="26307" href="plfa.Lists.html#26307" class="Bound">e</a> <a id="26309" href="plfa.Lists.html#26309" class="Bound">monoid-⊗</a> <a id="26318" href="plfa.Lists.html#26318" class="Bound">xs</a> <a id="26321" href="plfa.Lists.html#26321" class="Bound">ys</a> <a id="26324" class="Symbol">=</a>
  <a id="26328" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2597" class="Function Operator">begin</a>
    <a id="26338" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#20225" class="Function">foldr</a> <a id="26344" href="plfa.Lists.html#26303" class="Bound Operator">_⊗_</a> <a id="26348" href="plfa.Lists.html#26307" class="Bound">e</a> <a id="26350" class="Symbol">(</a><a id="26351" href="plfa.Lists.html#26318" class="Bound">xs</a> <a id="26354" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="26357" href="plfa.Lists.html#26321" class="Bound">ys</a><a id="26359" class="Symbol">)</a>
  <a id="26363" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2714" class="Function Operator">≡⟨</a> <a id="26366" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#22385" class="Postulate">foldr-++</a> <a id="26375" href="plfa.Lists.html#26303" class="Bound Operator">_⊗_</a> <a id="26379" href="plfa.Lists.html#26307" class="Bound">e</a> <a id="26381" href="plfa.Lists.html#26318" class="Bound">xs</a> <a id="26384" href="plfa.Lists.html#26321" class="Bound">ys</a> <a id="26387" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2714" class="Function Operator">⟩</a>
    <a id="26393" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#20225" class="Function">foldr</a> <a id="26399" href="plfa.Lists.html#26303" class="Bound Operator">_⊗_</a> <a id="26403" class="Symbol">(</a><a id="26404" href="plfa.Lists.html#20225" class="Function">foldr</a> <a id="26410" href="plfa.Lists.html#26303" class="Bound Operator">_⊗_</a> <a id="26414" href="plfa.Lists.html#26307" class="Bound">e</a> <a id="26416" href="plfa.Lists.html#26321" class="Bound">ys</a><a id="26418" class="Symbol">)</a> <a id="26420" href="plfa.Lists.html#26318" class="Bound">xs</a>
  <a id="26425" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2714" class="Function Operator">≡⟨</a> <a id="26428" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#25419" class="Function">foldr-monoid</a> <a id="26441" href="plfa.Lists.html#26303" class="Bound Operator">_⊗_</a> <a id="26445" href="plfa.Lists.html#26307" class="Bound">e</a> <a id="26447" href="plfa.Lists.html#26309" class="Bound">monoid-⊗</a> <a id="26456" href="plfa.Lists.html#26318" class="Bound">xs</a> <a id="26459" class="Symbol">(</a><a id="26460" href="plfa.Lists.html#20225" class="Function">foldr</a> <a id="26466" href="plfa.Lists.html#26303" class="Bound Operator">_⊗_</a> <a id="26470" href="plfa.Lists.html#26307" class="Bound">e</a> <a id="26472" href="plfa.Lists.html#26321" class="Bound">ys</a><a id="26474" class="Symbol">)</a> <a id="26476" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2714" class="Function Operator">⟩</a>
    <a id="26482" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#20225" class="Function">foldr</a> <a id="26488" href="plfa.Lists.html#26303" class="Bound Operator">_⊗_</a> <a id="26492" href="plfa.Lists.html#26307" class="Bound">e</a> <a id="26494" href="plfa.Lists.html#26318" class="Bound">xs</a> <a id="26497" href="plfa.Lists.html#26303" class="Bound Operator">⊗</a> <a id="26499" href="plfa.Lists.html#20225" class="Function">foldr</a> <a id="26505" href="plfa.Lists.html#26303" class="Bound Operator">_⊗_</a> <a id="26509" href="plfa.Lists.html#26307" class="Bound">e</a> <a id="26511" href="plfa.Lists.html#26321" class="Bound">ys</a>
  <a id="26516" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2892" class="Function Operator">∎</a>
</pre>{% endraw %}
{::comment}
#### Exercise `foldl`
{:/}

#### 练习 `foldl`

{::comment}
Define a function `foldl` which is analogous to `foldr`, but where
operations associate to the left rather than the right.  For example:
{:/}

定义一个函数 `foldl`，与 `foldr` 相似，但是运算符向左结合，而不是向右。例如：

    foldr _⊗_ e [ x , y , z ]  =  x ⊗ (y ⊗ (z ⊗ e))
    foldl _⊗_ e [ x , y , z ]  =  ((e ⊗ x) ⊗ y) ⊗ z

{::comment}
{% raw %}<pre class="Agda"><a id="26905" class="Comment">-- Your code goes here</a>
</pre>{% endraw %}{:/}

{% raw %}<pre class="Agda"><a id="26942" class="Comment">-- 请将代码写在此处。</a>
</pre>{% endraw %}

{::comment}
#### Exercise `foldr-monoid-foldl`
{:/}

#### 练习 `foldr-monoid-foldl`

{::comment}
Show that if `_⊗_` and `e` form a monoid, then `foldr _⊗_ e` and
`foldl _⊗_ e` always compute the same result.
{:/}

证明如果 `_⊗_` 和 `e` 构成幺半群，那么 `foldr _⊗_ e` 和 `foldl _⊗_ e` 的结果
永远是相同的。

{::comment}
{% raw %}<pre class="Agda"><a id="27258" class="Comment">-- Your code goes here</a>
</pre>{% endraw %}{:/}

{% raw %}<pre class="Agda"><a id="27295" class="Comment">-- 请将代码写在此处。</a>
</pre>{% endraw %}

{::comment}
## All {#All}
{:/}

## 所有 {#All}

{::comment}
We can also define predicates over lists. Two of the most important
are `All` and `Any`.
{:/}

我们也可以定义关于列表的谓词。最重要的两个谓词是 `All` 和 `Any`。

{::comment}
Predicate `All P` holds if predicate `P` is satisfied by every element of a list:
{:/}

谓词 `All P` 当列表里的所有元素满足 `P` 时成立：

{% raw %}<pre class="Agda"><a id="27645" class="Keyword">data</a> <a id="All"></a><a id="27650" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#27650" class="Datatype">All</a> <a id="27654" class="Symbol">{</a><a id="27655" href="plfa.Lists.html#27655" class="Bound">A</a> <a id="27657" class="Symbol">:</a> <a id="27659" class="PrimitiveType">Set</a><a id="27662" class="Symbol">}</a> <a id="27664" class="Symbol">(</a><a id="27665" href="plfa.Lists.html#27665" class="Bound">P</a> <a id="27667" class="Symbol">:</a> <a id="27669" href="plfa.Lists.html#27655" class="Bound">A</a> <a id="27671" class="Symbol">→</a> <a id="27673" class="PrimitiveType">Set</a><a id="27676" class="Symbol">)</a> <a id="27678" class="Symbol">:</a> <a id="27680" href="plfa.Lists.html#1284" class="Datatype">List</a> <a id="27685" href="plfa.Lists.html#27655" class="Bound">A</a> <a id="27687" class="Symbol">→</a> <a id="27689" class="PrimitiveType">Set</a> <a id="27693" class="Keyword">where</a>
  <a id="All.[]"></a><a id="27701" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#27701" class="InductiveConstructor">[]</a>  <a id="27705" class="Symbol">:</a> <a id="27707" href="plfa.Lists.html#27650" class="Datatype">All</a> <a id="27711" href="plfa.Lists.html#27665" class="Bound">P</a> <a id="27713" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a>
  <a id="All._∷_"></a><a id="27718" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#27718" class="InductiveConstructor Operator">_∷_</a> <a id="27722" class="Symbol">:</a> <a id="27724" class="Symbol">∀</a> <a id="27726" class="Symbol">{</a><a id="27727" href="plfa.Lists.html#27727" class="Bound">x</a> <a id="27729" class="Symbol">:</a> <a id="27731" href="plfa.Lists.html#27655" class="Bound">A</a><a id="27732" class="Symbol">}</a> <a id="27734" class="Symbol">{</a><a id="27735" href="plfa.Lists.html#27735" class="Bound">xs</a> <a id="27738" class="Symbol">:</a> <a id="27740" href="plfa.Lists.html#1284" class="Datatype">List</a> <a id="27745" href="plfa.Lists.html#27655" class="Bound">A</a><a id="27746" class="Symbol">}</a> <a id="27748" class="Symbol">→</a> <a id="27750" href="plfa.Lists.html#27665" class="Bound">P</a> <a id="27752" href="plfa.Lists.html#27727" class="Bound">x</a> <a id="27754" class="Symbol">→</a> <a id="27756" href="plfa.Lists.html#27650" class="Datatype">All</a> <a id="27760" href="plfa.Lists.html#27665" class="Bound">P</a> <a id="27762" href="plfa.Lists.html#27735" class="Bound">xs</a> <a id="27765" class="Symbol">→</a> <a id="27767" href="plfa.Lists.html#27650" class="Datatype">All</a> <a id="27771" href="plfa.Lists.html#27665" class="Bound">P</a> <a id="27773" class="Symbol">(</a><a id="27774" href="plfa.Lists.html#27727" class="Bound">x</a> <a id="27776" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="27778" href="plfa.Lists.html#27735" class="Bound">xs</a><a id="27780" class="Symbol">)</a>
</pre>{% endraw %}
{::comment}
The type has two constructors, reusing the names of the same constructors for lists.
The first asserts that `P` holds for every element of the empty list.
The second asserts that if `P` holds of the head of a list and for every
element of the tail of a list, then `P` holds for every element of the list.
Agda uses types to disambiguate whether the constructor is building
a list or evidence that `All P` holds.
{:/}

这个类型有两个构造子，使用了与列表构造子相同的名称。第一个断言了 `P` 对于空列表的任何元素成立。
第二个断言了如果 `P` 对于列表的头元素和尾列表的所有元素成立，那么 `P` 对于这个列表的任何元素成立。
Agda 使用类型来区分构造子是用于构造一个列表，还是构造 `All P` 成立的证明。

{::comment}
For example, `All (_≤ 2)` holds of a list where every element is less
than or equal to two.  Recall that `z≤n` proves `zero ≤ n` for any
`n`, and that if `m≤n` proves `m ≤ n` then `s≤s m≤n` proves `suc m ≤
suc n`, for any `m` and `n`:
{:/}

比如说，`All (_≤ 2)` 对于一个每一个元素都小于等于二的列表成立。
回忆 `z≤n` 证明了对于任意 `n`， `zero ≤ n` 成立；
对于任意 `m` 和 `n`，如果 `m≤n` 证明了 `m ≤ n`，那么 `s≤s m≤n` 证明了 `suc m ≤
suc n`:

{% raw %}<pre class="Agda"><a id="28773" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#28773" class="Function">_</a> <a id="28775" class="Symbol">:</a> <a id="28777" href="plfa.Lists.html#27650" class="Datatype">All</a> <a id="28781" class="Symbol">(</a><a id="28782" href="https://agda.github.io/agda-stdlib/v1.1/Data.Nat.Base.html#895" class="Datatype Operator">_≤</a> <a id="28785" class="Number">2</a><a id="28786" class="Symbol">)</a> <a id="28788" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">[</a> <a id="28790" class="Number">0</a> <a id="28792" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">,</a> <a id="28794" class="Number">1</a> <a id="28796" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">,</a> <a id="28798" class="Number">2</a> <a id="28800" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">]</a>
<a id="28802" class="Symbol">_</a> <a id="28804" class="Symbol">=</a> <a id="28806" href="https://agda.github.io/agda-stdlib/v1.1/Data.Nat.Base.html#918" class="InductiveConstructor">z≤n</a> <a id="28810" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#27718" class="InductiveConstructor Operator">∷</a> <a id="28812" href="https://agda.github.io/agda-stdlib/v1.1/Data.Nat.Base.html#960" class="InductiveConstructor">s≤s</a> <a id="28816" href="https://agda.github.io/agda-stdlib/v1.1/Data.Nat.Base.html#918" class="InductiveConstructor">z≤n</a> <a id="28820" href="plfa.Lists.html#27718" class="InductiveConstructor Operator">∷</a> <a id="28822" href="https://agda.github.io/agda-stdlib/v1.1/Data.Nat.Base.html#960" class="InductiveConstructor">s≤s</a> <a id="28826" class="Symbol">(</a><a id="28827" href="https://agda.github.io/agda-stdlib/v1.1/Data.Nat.Base.html#960" class="InductiveConstructor">s≤s</a> <a id="28831" href="https://agda.github.io/agda-stdlib/v1.1/Data.Nat.Base.html#918" class="InductiveConstructor">z≤n</a><a id="28834" class="Symbol">)</a> <a id="28836" href="plfa.Lists.html#27718" class="InductiveConstructor Operator">∷</a> <a id="28838" href="plfa.Lists.html#27701" class="InductiveConstructor">[]</a>
</pre>{% endraw %}
{::comment}
Here `_∷_` and `[]` are the constructors of `All P` rather than of `List A`.
The three items are proofs of `0 ≤ 2`, `1 ≤ 2`, and `2 ≤ 2`, respectively.
{:/}

这里 `_∷_` 和 `[]` 是 `All P` 的构造子，而不是 `List A` 的。
这三项分别是 `0 ≤ 2`、 `1 ≤ 2` 和 `2 ≤ 2` 的证明。

{::comment}
(One might wonder whether a pattern such as `[_,_,_]` can be used to
construct values of type `All` as well as type `List`, since both use
the same constructors. Indeed it can, so long as both types are in
scope when the pattern is declared.  That's not the case here, since
`List` is defined before `[_,_,_]`, but `All` is defined later.)
{:/}

（读者可能会思考诸如 `[_,_,_]` 的模式是否可以用于构造 `All` 类型的值，
像构造 `List` 类型的一样，因为两者使用了相同的构造子。事实上这样做是可以的，只要两个类型
在模式声明时在作用域内。然而现在不是这样的情况，因为 `List` 先于 `[_,_,_]` 定义，而 `All` 在
之后定义。）

{::comment}
## Any
{:/}

## 任意

{::comment}
Predicate `Any P` holds if predicate `P` is satisfied by some element of a list:
{:/}

谓词 `Any P` 当列表里的一些元素满足 `P` 时成立：

{% raw %}<pre class="Agda"><a id="29791" class="Keyword">data</a> <a id="Any"></a><a id="29796" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#29796" class="Datatype">Any</a> <a id="29800" class="Symbol">{</a><a id="29801" href="plfa.Lists.html#29801" class="Bound">A</a> <a id="29803" class="Symbol">:</a> <a id="29805" class="PrimitiveType">Set</a><a id="29808" class="Symbol">}</a> <a id="29810" class="Symbol">(</a><a id="29811" href="plfa.Lists.html#29811" class="Bound">P</a> <a id="29813" class="Symbol">:</a> <a id="29815" href="plfa.Lists.html#29801" class="Bound">A</a> <a id="29817" class="Symbol">→</a> <a id="29819" class="PrimitiveType">Set</a><a id="29822" class="Symbol">)</a> <a id="29824" class="Symbol">:</a> <a id="29826" href="plfa.Lists.html#1284" class="Datatype">List</a> <a id="29831" href="plfa.Lists.html#29801" class="Bound">A</a> <a id="29833" class="Symbol">→</a> <a id="29835" class="PrimitiveType">Set</a> <a id="29839" class="Keyword">where</a>
  <a id="Any.here"></a><a id="29847" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#29847" class="InductiveConstructor">here</a>  <a id="29853" class="Symbol">:</a> <a id="29855" class="Symbol">∀</a> <a id="29857" class="Symbol">{</a><a id="29858" href="plfa.Lists.html#29858" class="Bound">x</a> <a id="29860" class="Symbol">:</a> <a id="29862" href="plfa.Lists.html#29801" class="Bound">A</a><a id="29863" class="Symbol">}</a> <a id="29865" class="Symbol">{</a><a id="29866" href="plfa.Lists.html#29866" class="Bound">xs</a> <a id="29869" class="Symbol">:</a> <a id="29871" href="plfa.Lists.html#1284" class="Datatype">List</a> <a id="29876" href="plfa.Lists.html#29801" class="Bound">A</a><a id="29877" class="Symbol">}</a> <a id="29879" class="Symbol">→</a> <a id="29881" href="plfa.Lists.html#29811" class="Bound">P</a> <a id="29883" href="plfa.Lists.html#29858" class="Bound">x</a> <a id="29885" class="Symbol">→</a> <a id="29887" href="plfa.Lists.html#29796" class="Datatype">Any</a> <a id="29891" href="plfa.Lists.html#29811" class="Bound">P</a> <a id="29893" class="Symbol">(</a><a id="29894" href="plfa.Lists.html#29858" class="Bound">x</a> <a id="29896" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="29898" href="plfa.Lists.html#29866" class="Bound">xs</a><a id="29900" class="Symbol">)</a>
  <a id="Any.there"></a><a id="29904" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#29904" class="InductiveConstructor">there</a> <a id="29910" class="Symbol">:</a> <a id="29912" class="Symbol">∀</a> <a id="29914" class="Symbol">{</a><a id="29915" href="plfa.Lists.html#29915" class="Bound">x</a> <a id="29917" class="Symbol">:</a> <a id="29919" href="plfa.Lists.html#29801" class="Bound">A</a><a id="29920" class="Symbol">}</a> <a id="29922" class="Symbol">{</a><a id="29923" href="plfa.Lists.html#29923" class="Bound">xs</a> <a id="29926" class="Symbol">:</a> <a id="29928" href="plfa.Lists.html#1284" class="Datatype">List</a> <a id="29933" href="plfa.Lists.html#29801" class="Bound">A</a><a id="29934" class="Symbol">}</a> <a id="29936" class="Symbol">→</a> <a id="29938" href="plfa.Lists.html#29796" class="Datatype">Any</a> <a id="29942" href="plfa.Lists.html#29811" class="Bound">P</a> <a id="29944" href="plfa.Lists.html#29923" class="Bound">xs</a> <a id="29947" class="Symbol">→</a> <a id="29949" href="plfa.Lists.html#29796" class="Datatype">Any</a> <a id="29953" href="plfa.Lists.html#29811" class="Bound">P</a> <a id="29955" class="Symbol">(</a><a id="29956" href="plfa.Lists.html#29915" class="Bound">x</a> <a id="29958" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="29960" href="plfa.Lists.html#29923" class="Bound">xs</a><a id="29962" class="Symbol">)</a>
</pre>{% endraw %}
{::comment}
The first constructor provides evidence that the head of the list
satisfies `P`, while the second provides evidence that some element of
the tail of the list satisfies `P`.  For example, we can define list
membership as follows:
{:/}

第一个构造子证明了列表的头元素满足 `P`，第二个构造子证明的列表的尾列表中的一些元素满足 `P`。
举例来说，我们可以如下定义列表的成员关系：

{% raw %}<pre class="Agda"><a id="30294" class="Keyword">infix</a> <a id="30300" class="Number">4</a> <a id="30302" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#30311" class="Function Operator">_∈_</a> <a id="30306" href="plfa.Lists.html#30381" class="Function Operator">_∉_</a>

<a id="_∈_"></a><a id="30311" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#30311" class="Function Operator">_∈_</a> <a id="30315" class="Symbol">:</a> <a id="30317" class="Symbol">∀</a> <a id="30319" class="Symbol">{</a><a id="30320" href="plfa.Lists.html#30320" class="Bound">A</a> <a id="30322" class="Symbol">:</a> <a id="30324" class="PrimitiveType">Set</a><a id="30327" class="Symbol">}</a> <a id="30329" class="Symbol">(</a><a id="30330" href="plfa.Lists.html#30330" class="Bound">x</a> <a id="30332" class="Symbol">:</a> <a id="30334" href="plfa.Lists.html#30320" class="Bound">A</a><a id="30335" class="Symbol">)</a> <a id="30337" class="Symbol">(</a><a id="30338" href="plfa.Lists.html#30338" class="Bound">xs</a> <a id="30341" class="Symbol">:</a> <a id="30343" href="plfa.Lists.html#1284" class="Datatype">List</a> <a id="30348" href="plfa.Lists.html#30320" class="Bound">A</a><a id="30349" class="Symbol">)</a> <a id="30351" class="Symbol">→</a> <a id="30353" class="PrimitiveType">Set</a>
<a id="30357" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#30357" class="Bound">x</a> <a id="30359" href="plfa.Lists.html#30311" class="Function Operator">∈</a> <a id="30361" href="plfa.Lists.html#30361" class="Bound">xs</a> <a id="30364" class="Symbol">=</a> <a id="30366" href="plfa.Lists.html#29796" class="Datatype">Any</a> <a id="30370" class="Symbol">(</a><a id="30371" href="plfa.Lists.html#30357" class="Bound">x</a> <a id="30373" href="Agda.Builtin.Equality.html#125" class="Datatype Operator">≡_</a><a id="30375" class="Symbol">)</a> <a id="30377" href="plfa.Lists.html#30361" class="Bound">xs</a>

<a id="_∉_"></a><a id="30381" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#30381" class="Function Operator">_∉_</a> <a id="30385" class="Symbol">:</a> <a id="30387" class="Symbol">∀</a> <a id="30389" class="Symbol">{</a><a id="30390" href="plfa.Lists.html#30390" class="Bound">A</a> <a id="30392" class="Symbol">:</a> <a id="30394" class="PrimitiveType">Set</a><a id="30397" class="Symbol">}</a> <a id="30399" class="Symbol">(</a><a id="30400" href="plfa.Lists.html#30400" class="Bound">x</a> <a id="30402" class="Symbol">:</a> <a id="30404" href="plfa.Lists.html#30390" class="Bound">A</a><a id="30405" class="Symbol">)</a> <a id="30407" class="Symbol">(</a><a id="30408" href="plfa.Lists.html#30408" class="Bound">xs</a> <a id="30411" class="Symbol">:</a> <a id="30413" href="plfa.Lists.html#1284" class="Datatype">List</a> <a id="30418" href="plfa.Lists.html#30390" class="Bound">A</a><a id="30419" class="Symbol">)</a> <a id="30421" class="Symbol">→</a> <a id="30423" class="PrimitiveType">Set</a>
<a id="30427" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#30427" class="Bound">x</a> <a id="30429" href="plfa.Lists.html#30381" class="Function Operator">∉</a> <a id="30431" href="plfa.Lists.html#30431" class="Bound">xs</a> <a id="30434" class="Symbol">=</a> <a id="30436" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.html#535" class="Function Operator">¬</a> <a id="30438" class="Symbol">(</a><a id="30439" href="plfa.Lists.html#30427" class="Bound">x</a> <a id="30441" href="plfa.Lists.html#30311" class="Function Operator">∈</a> <a id="30443" href="plfa.Lists.html#30431" class="Bound">xs</a><a id="30445" class="Symbol">)</a>
</pre>{% endraw %}
{::comment}
For example, zero is an element of the list `[ 0 , 1 , 0 , 2 ]`.  Indeed, we can demonstrate
this fact in two different ways, corresponding to the two different
occurrences of zero in the list, as the first element and as the third element:
{:/}

比如说，零是列表 `[ 0 , 1 , 0 , 2 ]` 中的一个元素。
我们可以用两种方法来展示这个事实，对应零在列表中出现了两次：第一个元素和第三个元素：

{% raw %}<pre class="Agda"><a id="30796" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#30796" class="Function">_</a> <a id="30798" class="Symbol">:</a> <a id="30800" class="Number">0</a> <a id="30802" href="plfa.Lists.html#30311" class="Function Operator">∈</a> <a id="30804" href="plfa.Lists.html#3911" class="InductiveConstructor Operator">[</a> <a id="30806" class="Number">0</a> <a id="30808" href="plfa.Lists.html#3911" class="InductiveConstructor Operator">,</a> <a id="30810" class="Number">1</a> <a id="30812" href="plfa.Lists.html#3911" class="InductiveConstructor Operator">,</a> <a id="30814" class="Number">0</a> <a id="30816" href="plfa.Lists.html#3911" class="InductiveConstructor Operator">,</a> <a id="30818" class="Number">2</a> <a id="30820" href="plfa.Lists.html#3911" class="InductiveConstructor Operator">]</a>
<a id="30822" class="Symbol">_</a> <a id="30824" class="Symbol">=</a> <a id="30826" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#29847" class="InductiveConstructor">here</a> <a id="30831" href="Agda.Builtin.Equality.html#182" class="InductiveConstructor">refl</a>

<a id="30837" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#30837" class="Function">_</a> <a id="30839" class="Symbol">:</a> <a id="30841" class="Number">0</a> <a id="30843" href="plfa.Lists.html#30311" class="Function Operator">∈</a> <a id="30845" href="plfa.Lists.html#3911" class="InductiveConstructor Operator">[</a> <a id="30847" class="Number">0</a> <a id="30849" href="plfa.Lists.html#3911" class="InductiveConstructor Operator">,</a> <a id="30851" class="Number">1</a> <a id="30853" href="plfa.Lists.html#3911" class="InductiveConstructor Operator">,</a> <a id="30855" class="Number">0</a> <a id="30857" href="plfa.Lists.html#3911" class="InductiveConstructor Operator">,</a> <a id="30859" class="Number">2</a> <a id="30861" href="plfa.Lists.html#3911" class="InductiveConstructor Operator">]</a>
<a id="30863" class="Symbol">_</a> <a id="30865" class="Symbol">=</a> <a id="30867" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#29904" class="InductiveConstructor">there</a> <a id="30873" class="Symbol">(</a><a id="30874" href="plfa.Lists.html#29904" class="InductiveConstructor">there</a> <a id="30880" class="Symbol">(</a><a id="30881" href="plfa.Lists.html#29847" class="InductiveConstructor">here</a> <a id="30886" href="Agda.Builtin.Equality.html#182" class="InductiveConstructor">refl</a><a id="30890" class="Symbol">))</a>
</pre>{% endraw %}
{::comment}
Further, we can demonstrate that three is not in the list, because
any possible proof that it is in the list leads to contradiction:
{:/}

除此之外，我们可以展示三不在列表之中，因为任何它在列表中的证明会推导出矛盾：

{% raw %}<pre class="Agda"><a id="not-in"></a><a id="31093" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#31093" class="Function">not-in</a> <a id="31100" class="Symbol">:</a> <a id="31102" class="Number">3</a> <a id="31104" href="plfa.Lists.html#30381" class="Function Operator">∉</a> <a id="31106" href="plfa.Lists.html#3911" class="InductiveConstructor Operator">[</a> <a id="31108" class="Number">0</a> <a id="31110" href="plfa.Lists.html#3911" class="InductiveConstructor Operator">,</a> <a id="31112" class="Number">1</a> <a id="31114" href="plfa.Lists.html#3911" class="InductiveConstructor Operator">,</a> <a id="31116" class="Number">0</a> <a id="31118" href="plfa.Lists.html#3911" class="InductiveConstructor Operator">,</a> <a id="31120" class="Number">2</a> <a id="31122" href="plfa.Lists.html#3911" class="InductiveConstructor Operator">]</a>
<a id="31124" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#31093" class="Function">not-in</a> <a id="31131" class="Symbol">(</a><a id="31132" href="plfa.Lists.html#29847" class="InductiveConstructor">here</a> <a id="31137" class="Symbol">())</a>
<a id="31141" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#31093" class="Function">not-in</a> <a id="31148" class="Symbol">(</a><a id="31149" href="plfa.Lists.html#29904" class="InductiveConstructor">there</a> <a id="31155" class="Symbol">(</a><a id="31156" href="plfa.Lists.html#29847" class="InductiveConstructor">here</a> <a id="31161" class="Symbol">()))</a>
<a id="31166" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#31093" class="Function">not-in</a> <a id="31173" class="Symbol">(</a><a id="31174" href="plfa.Lists.html#29904" class="InductiveConstructor">there</a> <a id="31180" class="Symbol">(</a><a id="31181" href="plfa.Lists.html#29904" class="InductiveConstructor">there</a> <a id="31187" class="Symbol">(</a><a id="31188" href="plfa.Lists.html#29847" class="InductiveConstructor">here</a> <a id="31193" class="Symbol">())))</a>
<a id="31199" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#31093" class="Function">not-in</a> <a id="31206" class="Symbol">(</a><a id="31207" href="plfa.Lists.html#29904" class="InductiveConstructor">there</a> <a id="31213" class="Symbol">(</a><a id="31214" href="plfa.Lists.html#29904" class="InductiveConstructor">there</a> <a id="31220" class="Symbol">(</a><a id="31221" href="plfa.Lists.html#29904" class="InductiveConstructor">there</a> <a id="31227" class="Symbol">(</a><a id="31228" href="plfa.Lists.html#29847" class="InductiveConstructor">here</a> <a id="31233" class="Symbol">()))))</a>
<a id="31240" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#31093" class="Function">not-in</a> <a id="31247" class="Symbol">(</a><a id="31248" href="plfa.Lists.html#29904" class="InductiveConstructor">there</a> <a id="31254" class="Symbol">(</a><a id="31255" href="plfa.Lists.html#29904" class="InductiveConstructor">there</a> <a id="31261" class="Symbol">(</a><a id="31262" href="plfa.Lists.html#29904" class="InductiveConstructor">there</a> <a id="31268" class="Symbol">(</a><a id="31269" href="plfa.Lists.html#29904" class="InductiveConstructor">there</a> <a id="31275" class="Symbol">()))))</a>
</pre>{% endraw %}
{::comment}
The five occurrences of `()` attest to the fact that there is no
possible evidence for `3 ≡ 0`, `3 ≡ 1`, `3 ≡ 0`, `3 ≡ 2`, and
`3 ∈ []`, respectively.
{:/}

`()` 出现了五次，分别表示没有 `3 ≡ 0`、 `3 ≡ 1`、 `3 ≡ 0`、 `3 ≡ 2` 和
`3 ∈ []` 的证明。

{::comment}
## All and append
{:/}

## 所有和附加

{::comment}
A predicate holds for every element of one list appended to another if and
only if it holds for every element of both lists:
{:/}

一个谓词对两个附加在一起的列表的每个元素都成立，当且仅当这个谓词对两个列表的每个元素都成立：

{% raw %}<pre class="Agda"><a id="All-++-⇔"></a><a id="31767" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#31767" class="Function">All-++-⇔</a> <a id="31776" class="Symbol">:</a> <a id="31778" class="Symbol">∀</a> <a id="31780" class="Symbol">{</a><a id="31781" href="plfa.Lists.html#31781" class="Bound">A</a> <a id="31783" class="Symbol">:</a> <a id="31785" class="PrimitiveType">Set</a><a id="31788" class="Symbol">}</a> <a id="31790" class="Symbol">{</a><a id="31791" href="plfa.Lists.html#31791" class="Bound">P</a> <a id="31793" class="Symbol">:</a> <a id="31795" href="plfa.Lists.html#31781" class="Bound">A</a> <a id="31797" class="Symbol">→</a> <a id="31799" class="PrimitiveType">Set</a><a id="31802" class="Symbol">}</a> <a id="31804" class="Symbol">(</a><a id="31805" href="plfa.Lists.html#31805" class="Bound">xs</a> <a id="31808" href="plfa.Lists.html#31808" class="Bound">ys</a> <a id="31811" class="Symbol">:</a> <a id="31813" href="plfa.Lists.html#1284" class="Datatype">List</a> <a id="31818" href="plfa.Lists.html#31781" class="Bound">A</a><a id="31819" class="Symbol">)</a> <a id="31821" class="Symbol">→</a>
  <a id="31825" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#27650" class="Datatype">All</a> <a id="31829" href="plfa.Lists.html#31791" class="Bound">P</a> <a id="31831" class="Symbol">(</a><a id="31832" href="plfa.Lists.html#31805" class="Bound">xs</a> <a id="31835" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="31838" href="plfa.Lists.html#31808" class="Bound">ys</a><a id="31840" class="Symbol">)</a> <a id="31842" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Isomorphism.md %}{% raw %}#15256" class="Record Operator">⇔</a> <a id="31844" class="Symbol">(</a><a id="31845" href="plfa.Lists.html#27650" class="Datatype">All</a> <a id="31849" href="plfa.Lists.html#31791" class="Bound">P</a> <a id="31851" href="plfa.Lists.html#31805" class="Bound">xs</a> <a id="31854" href="https://agda.github.io/agda-stdlib/v1.1/Data.Product.html#1162" class="Function Operator">×</a> <a id="31856" href="plfa.Lists.html#27650" class="Datatype">All</a> <a id="31860" href="plfa.Lists.html#31791" class="Bound">P</a> <a id="31862" href="plfa.Lists.html#31808" class="Bound">ys</a><a id="31864" class="Symbol">)</a>
<a id="31866" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#31767" class="Function">All-++-⇔</a> <a id="31875" href="plfa.Lists.html#31875" class="Bound">xs</a> <a id="31878" href="plfa.Lists.html#31878" class="Bound">ys</a> <a id="31881" class="Symbol">=</a>
  <a id="31885" class="Keyword">record</a>
    <a id="31896" class="Symbol">{</a> <a id="31898" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Isomorphism.md %}{% raw %}#15296" class="Field">to</a>       <a id="31907" class="Symbol">=</a>  <a id="31910" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#31965" class="Function">to</a> <a id="31913" href="plfa.Lists.html#31875" class="Bound">xs</a> <a id="31916" href="plfa.Lists.html#31878" class="Bound">ys</a>
    <a id="31923" class="Symbol">;</a> <a id="31925" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Isomorphism.md %}{% raw %}#15313" class="Field">from</a>     <a id="31934" class="Symbol">=</a>  <a id="31937" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#32190" class="Function">from</a> <a id="31942" href="plfa.Lists.html#31875" class="Bound">xs</a> <a id="31945" href="plfa.Lists.html#31878" class="Bound">ys</a>
    <a id="31952" class="Symbol">}</a>
  <a id="31956" class="Keyword">where</a>

  <a id="31965" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#31965" class="Function">to</a> <a id="31968" class="Symbol">:</a> <a id="31970" class="Symbol">∀</a> <a id="31972" class="Symbol">{</a><a id="31973" href="plfa.Lists.html#31973" class="Bound">A</a> <a id="31975" class="Symbol">:</a> <a id="31977" class="PrimitiveType">Set</a><a id="31980" class="Symbol">}</a> <a id="31982" class="Symbol">{</a><a id="31983" href="plfa.Lists.html#31983" class="Bound">P</a> <a id="31985" class="Symbol">:</a> <a id="31987" href="plfa.Lists.html#31973" class="Bound">A</a> <a id="31989" class="Symbol">→</a> <a id="31991" class="PrimitiveType">Set</a><a id="31994" class="Symbol">}</a> <a id="31996" class="Symbol">(</a><a id="31997" href="plfa.Lists.html#31997" class="Bound">xs</a> <a id="32000" href="plfa.Lists.html#32000" class="Bound">ys</a> <a id="32003" class="Symbol">:</a> <a id="32005" href="plfa.Lists.html#1284" class="Datatype">List</a> <a id="32010" href="plfa.Lists.html#31973" class="Bound">A</a><a id="32011" class="Symbol">)</a> <a id="32013" class="Symbol">→</a>
    <a id="32019" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#27650" class="Datatype">All</a> <a id="32023" href="plfa.Lists.html#31983" class="Bound">P</a> <a id="32025" class="Symbol">(</a><a id="32026" href="plfa.Lists.html#31997" class="Bound">xs</a> <a id="32029" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="32032" href="plfa.Lists.html#32000" class="Bound">ys</a><a id="32034" class="Symbol">)</a> <a id="32036" class="Symbol">→</a> <a id="32038" class="Symbol">(</a><a id="32039" href="plfa.Lists.html#27650" class="Datatype">All</a> <a id="32043" href="plfa.Lists.html#31983" class="Bound">P</a> <a id="32045" href="plfa.Lists.html#31997" class="Bound">xs</a> <a id="32048" href="https://agda.github.io/agda-stdlib/v1.1/Data.Product.html#1162" class="Function Operator">×</a> <a id="32050" href="plfa.Lists.html#27650" class="Datatype">All</a> <a id="32054" href="plfa.Lists.html#31983" class="Bound">P</a> <a id="32056" href="plfa.Lists.html#32000" class="Bound">ys</a><a id="32058" class="Symbol">)</a>
  <a id="32062" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#31965" class="Function">to</a> <a id="32065" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a> <a id="32068" href="plfa.Lists.html#32068" class="Bound">ys</a> <a id="32071" href="plfa.Lists.html#32071" class="Bound">Pys</a> <a id="32075" class="Symbol">=</a> <a id="32077" href="Agda.Builtin.Sigma.html#209" class="InductiveConstructor Operator">⟨</a> <a id="32079" href="plfa.Lists.html#27701" class="InductiveConstructor">[]</a> <a id="32082" href="Agda.Builtin.Sigma.html#209" class="InductiveConstructor Operator">,</a> <a id="32084" href="plfa.Lists.html#32071" class="Bound">Pys</a> <a id="32088" href="Agda.Builtin.Sigma.html#209" class="InductiveConstructor Operator">⟩</a>
  <a id="32092" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#31965" class="Function">to</a> <a id="32095" class="Symbol">(</a><a id="32096" href="plfa.Lists.html#32096" class="Bound">x</a> <a id="32098" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="32100" href="plfa.Lists.html#32100" class="Bound">xs</a><a id="32102" class="Symbol">)</a> <a id="32104" href="plfa.Lists.html#32104" class="Bound">ys</a> <a id="32107" class="Symbol">(</a><a id="32108" href="plfa.Lists.html#32108" class="Bound">Px</a> <a id="32111" href="plfa.Lists.html#27718" class="InductiveConstructor Operator">∷</a> <a id="32113" href="plfa.Lists.html#32113" class="Bound">Pxs++ys</a><a id="32120" class="Symbol">)</a> <a id="32122" class="Keyword">with</a> <a id="32127" href="plfa.Lists.html#31965" class="Function">to</a> <a id="32130" href="plfa.Lists.html#32100" class="Bound">xs</a> <a id="32133" href="plfa.Lists.html#32104" class="Bound">ys</a> <a id="32136" href="plfa.Lists.html#32113" class="Bound">Pxs++ys</a>
  <a id="32146" class="Symbol">...</a> <a id="32150" class="Symbol">|</a> <a id="32152" href="Agda.Builtin.Sigma.html#209" class="InductiveConstructor Operator">⟨</a> <a id="32154" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#32154" class="Bound">Pxs</a> <a id="32158" href="Agda.Builtin.Sigma.html#209" class="InductiveConstructor Operator">,</a> <a id="32160" href="plfa.Lists.html#32160" class="Bound">Pys</a> <a id="32164" href="Agda.Builtin.Sigma.html#209" class="InductiveConstructor Operator">⟩</a> <a id="32166" class="Symbol">=</a> <a id="32168" href="Agda.Builtin.Sigma.html#209" class="InductiveConstructor Operator">⟨</a> <a id="32170" class="Bound">Px</a> <a id="32173" href="plfa.Lists.html#27718" class="InductiveConstructor Operator">∷</a> <a id="32175" href="plfa.Lists.html#32154" class="Bound">Pxs</a> <a id="32179" href="Agda.Builtin.Sigma.html#209" class="InductiveConstructor Operator">,</a> <a id="32181" href="plfa.Lists.html#32160" class="Bound">Pys</a> <a id="32185" href="Agda.Builtin.Sigma.html#209" class="InductiveConstructor Operator">⟩</a>

  <a id="32190" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#32190" class="Function">from</a> <a id="32195" class="Symbol">:</a> <a id="32197" class="Symbol">∀</a> <a id="32199" class="Symbol">{</a> <a id="32201" href="plfa.Lists.html#32201" class="Bound">A</a> <a id="32203" class="Symbol">:</a> <a id="32205" class="PrimitiveType">Set</a><a id="32208" class="Symbol">}</a> <a id="32210" class="Symbol">{</a><a id="32211" href="plfa.Lists.html#32211" class="Bound">P</a> <a id="32213" class="Symbol">:</a> <a id="32215" href="plfa.Lists.html#32201" class="Bound">A</a> <a id="32217" class="Symbol">→</a> <a id="32219" class="PrimitiveType">Set</a><a id="32222" class="Symbol">}</a> <a id="32224" class="Symbol">(</a><a id="32225" href="plfa.Lists.html#32225" class="Bound">xs</a> <a id="32228" href="plfa.Lists.html#32228" class="Bound">ys</a> <a id="32231" class="Symbol">:</a> <a id="32233" href="plfa.Lists.html#1284" class="Datatype">List</a> <a id="32238" href="plfa.Lists.html#32201" class="Bound">A</a><a id="32239" class="Symbol">)</a> <a id="32241" class="Symbol">→</a>
    <a id="32247" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#27650" class="Datatype">All</a> <a id="32251" href="plfa.Lists.html#32211" class="Bound">P</a> <a id="32253" href="plfa.Lists.html#32225" class="Bound">xs</a> <a id="32256" href="https://agda.github.io/agda-stdlib/v1.1/Data.Product.html#1162" class="Function Operator">×</a> <a id="32258" href="plfa.Lists.html#27650" class="Datatype">All</a> <a id="32262" href="plfa.Lists.html#32211" class="Bound">P</a> <a id="32264" href="plfa.Lists.html#32228" class="Bound">ys</a> <a id="32267" class="Symbol">→</a> <a id="32269" href="plfa.Lists.html#27650" class="Datatype">All</a> <a id="32273" href="plfa.Lists.html#32211" class="Bound">P</a> <a id="32275" class="Symbol">(</a><a id="32276" href="plfa.Lists.html#32225" class="Bound">xs</a> <a id="32279" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="32282" href="plfa.Lists.html#32228" class="Bound">ys</a><a id="32284" class="Symbol">)</a>
  <a id="32288" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#32190" class="Function">from</a> <a id="32293" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a> <a id="32296" href="plfa.Lists.html#32296" class="Bound">ys</a> <a id="32299" href="Agda.Builtin.Sigma.html#209" class="InductiveConstructor Operator">⟨</a> <a id="32301" href="plfa.Lists.html#27701" class="InductiveConstructor">[]</a> <a id="32304" href="Agda.Builtin.Sigma.html#209" class="InductiveConstructor Operator">,</a> <a id="32306" href="plfa.Lists.html#32306" class="Bound">Pys</a> <a id="32310" href="Agda.Builtin.Sigma.html#209" class="InductiveConstructor Operator">⟩</a> <a id="32312" class="Symbol">=</a> <a id="32314" href="plfa.Lists.html#32306" class="Bound">Pys</a>
  <a id="32320" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#32190" class="Function">from</a> <a id="32325" class="Symbol">(</a><a id="32326" href="plfa.Lists.html#32326" class="Bound">x</a> <a id="32328" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="32330" href="plfa.Lists.html#32330" class="Bound">xs</a><a id="32332" class="Symbol">)</a> <a id="32334" href="plfa.Lists.html#32334" class="Bound">ys</a> <a id="32337" href="Agda.Builtin.Sigma.html#209" class="InductiveConstructor Operator">⟨</a> <a id="32339" href="plfa.Lists.html#32339" class="Bound">Px</a> <a id="32342" href="plfa.Lists.html#27718" class="InductiveConstructor Operator">∷</a> <a id="32344" href="plfa.Lists.html#32344" class="Bound">Pxs</a> <a id="32348" href="Agda.Builtin.Sigma.html#209" class="InductiveConstructor Operator">,</a> <a id="32350" href="plfa.Lists.html#32350" class="Bound">Pys</a> <a id="32354" href="Agda.Builtin.Sigma.html#209" class="InductiveConstructor Operator">⟩</a> <a id="32356" class="Symbol">=</a>  <a id="32359" href="plfa.Lists.html#32339" class="Bound">Px</a> <a id="32362" href="plfa.Lists.html#27718" class="InductiveConstructor Operator">∷</a> <a id="32364" href="plfa.Lists.html#32190" class="Function">from</a> <a id="32369" href="plfa.Lists.html#32330" class="Bound">xs</a> <a id="32372" href="plfa.Lists.html#32334" class="Bound">ys</a> <a id="32375" href="Agda.Builtin.Sigma.html#209" class="InductiveConstructor Operator">⟨</a> <a id="32377" href="plfa.Lists.html#32344" class="Bound">Pxs</a> <a id="32381" href="Agda.Builtin.Sigma.html#209" class="InductiveConstructor Operator">,</a> <a id="32383" href="plfa.Lists.html#32350" class="Bound">Pys</a> <a id="32387" href="Agda.Builtin.Sigma.html#209" class="InductiveConstructor Operator">⟩</a>
</pre>{% endraw %}
{::comment}
#### Exercise `Any-++-⇔` (recommended)
{:/}

#### 练习 `Any-++-⇔` （推荐）

{::comment}
Prove a result similar to `All-++-⇔`, but with `Any` in place of `All`, and a suitable
replacement for `_×_`.  As a consequence, demonstrate an equivalence relating
`_∈_` and `_++_`.
{:/}
使用 `Any` 代替 `All` 与一个合适的 `_×_` 的替代，证明一个类似于 `All-++-⇔` 的结果。
作为结论，展示关联 `_∈_` 和 `_++_` 的一个等价关系。

{::comment}
{% raw %}<pre class="Agda"><a id="32786" class="Comment">-- Your code goes here</a>
</pre>{% endraw %}{:/}

{% raw %}<pre class="Agda"><a id="32823" class="Comment">-- 请将代码写在此处。</a>
</pre>{% endraw %}
{::comment}
#### Exercise `All-++-≃` (stretch)
{:/}

#### 练习 `All-++-≃` （延伸）

{::comment}
Show that the equivalence `All-++-⇔` can be extended to an isomorphism.
{:/}

证明 `All-++-⇔` 的等价关系可以被扩展至一个同构关系。

{::comment}
{% raw %}<pre class="Agda"><a id="33059" class="Comment">-- Your code goes here</a>
</pre>{% endraw %}{:/}

{% raw %}<pre class="Agda"><a id="33096" class="Comment">-- 请将代码写在此处。</a>
</pre>{% endraw %}
{::comment}
#### Exercise `¬Any≃All¬` (stretch)
{:/}

#### 练习 `¬Any≃All¬` （拓展）

{::comment}
First generalise composition to arbitrary levels, using
[universe polymorphism]({{ site.baseurl }}/Equality/#unipoly):
{:/}

首先我们将函数组合使用[全体多态]({{ site.baseurl }}/Equality/#unipoly)推广到任意等级：
{% raw %}<pre class="Agda"><a id="_∘′_"></a><a id="33399" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#33399" class="Function Operator">_∘′_</a> <a id="33404" class="Symbol">:</a> <a id="33406" class="Symbol">∀</a> <a id="33408" class="Symbol">{</a><a id="33409" href="plfa.Lists.html#33409" class="Bound">ℓ₁</a> <a id="33412" href="plfa.Lists.html#33412" class="Bound">ℓ₂</a> <a id="33415" href="plfa.Lists.html#33415" class="Bound">ℓ₃</a> <a id="33418" class="Symbol">:</a> <a id="33420" href="Agda.Primitive.html#408" class="Postulate">Level</a><a id="33425" class="Symbol">}</a> <a id="33427" class="Symbol">{</a><a id="33428" href="plfa.Lists.html#33428" class="Bound">A</a> <a id="33430" class="Symbol">:</a> <a id="33432" class="PrimitiveType">Set</a> <a id="33436" href="plfa.Lists.html#33409" class="Bound">ℓ₁</a><a id="33438" class="Symbol">}</a> <a id="33440" class="Symbol">{</a><a id="33441" href="plfa.Lists.html#33441" class="Bound">B</a> <a id="33443" class="Symbol">:</a> <a id="33445" class="PrimitiveType">Set</a> <a id="33449" href="plfa.Lists.html#33412" class="Bound">ℓ₂</a><a id="33451" class="Symbol">}</a> <a id="33453" class="Symbol">{</a><a id="33454" href="plfa.Lists.html#33454" class="Bound">C</a> <a id="33456" class="Symbol">:</a> <a id="33458" class="PrimitiveType">Set</a> <a id="33462" href="plfa.Lists.html#33415" class="Bound">ℓ₃</a><a id="33464" class="Symbol">}</a>
  <a id="33468" class="Symbol">→</a> <a id="33470" class="Symbol">(</a><a id="33471" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#33441" class="Bound">B</a> <a id="33473" class="Symbol">→</a> <a id="33475" href="plfa.Lists.html#33454" class="Bound">C</a><a id="33476" class="Symbol">)</a> <a id="33478" class="Symbol">→</a> <a id="33480" class="Symbol">(</a><a id="33481" href="plfa.Lists.html#33428" class="Bound">A</a> <a id="33483" class="Symbol">→</a> <a id="33485" href="plfa.Lists.html#33441" class="Bound">B</a><a id="33486" class="Symbol">)</a> <a id="33488" class="Symbol">→</a> <a id="33490" href="plfa.Lists.html#33428" class="Bound">A</a> <a id="33492" class="Symbol">→</a> <a id="33494" href="plfa.Lists.html#33454" class="Bound">C</a>
<a id="33496" class="Symbol">(</a><a id="33497" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#33497" class="Bound">g</a> <a id="33499" href="plfa.Lists.html#33399" class="Function Operator">∘′</a> <a id="33502" href="plfa.Lists.html#33502" class="Bound">f</a><a id="33503" class="Symbol">)</a> <a id="33505" href="plfa.Lists.html#33505" class="Bound">x</a>  <a id="33508" class="Symbol">=</a>  <a id="33511" href="plfa.Lists.html#33497" class="Bound">g</a> <a id="33513" class="Symbol">(</a><a id="33514" href="plfa.Lists.html#33502" class="Bound">f</a> <a id="33516" href="plfa.Lists.html#33505" class="Bound">x</a><a id="33517" class="Symbol">)</a>
</pre>{% endraw %}
{::comment}
Show that `Any` and `All` satisfy a version of De Morgan's Law:
{:/}

证明 `Any` 和 `All` 满足某个形式的德摩根定律：

{% raw %}<pre class="Agda"><a id="33642" class="Keyword">postulate</a>
  <a id="¬Any≃All¬"></a><a id="33654" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#33654" class="Postulate">¬Any≃All¬</a> <a id="33664" class="Symbol">:</a> <a id="33666" class="Symbol">∀</a> <a id="33668" class="Symbol">{</a><a id="33669" href="plfa.Lists.html#33669" class="Bound">A</a> <a id="33671" class="Symbol">:</a> <a id="33673" class="PrimitiveType">Set</a><a id="33676" class="Symbol">}</a> <a id="33678" class="Symbol">(</a><a id="33679" href="plfa.Lists.html#33679" class="Bound">P</a> <a id="33681" class="Symbol">:</a> <a id="33683" href="plfa.Lists.html#33669" class="Bound">A</a> <a id="33685" class="Symbol">→</a> <a id="33687" class="PrimitiveType">Set</a><a id="33690" class="Symbol">)</a> <a id="33692" class="Symbol">(</a><a id="33693" href="plfa.Lists.html#33693" class="Bound">xs</a> <a id="33696" class="Symbol">:</a> <a id="33698" href="plfa.Lists.html#1284" class="Datatype">List</a> <a id="33703" href="plfa.Lists.html#33669" class="Bound">A</a><a id="33704" class="Symbol">)</a>
    <a id="33710" class="Symbol">→</a> <a id="33712" class="Symbol">(</a><a id="33713" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.html#535" class="Function Operator">¬_</a> <a id="33716" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#33399" class="Function Operator">∘′</a> <a id="33719" href="plfa.Lists.html#29796" class="Datatype">Any</a> <a id="33723" href="plfa.Lists.html#33679" class="Bound">P</a><a id="33724" class="Symbol">)</a> <a id="33726" href="plfa.Lists.html#33693" class="Bound">xs</a> <a id="33729" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Isomorphism.md %}{% raw %}#5843" class="Record Operator">≃</a> <a id="33731" href="plfa.Lists.html#27650" class="Datatype">All</a> <a id="33735" class="Symbol">(</a><a id="33736" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.html#535" class="Function Operator">¬_</a> <a id="33739" href="plfa.Lists.html#33399" class="Function Operator">∘′</a> <a id="33742" href="plfa.Lists.html#33679" class="Bound">P</a><a id="33743" class="Symbol">)</a> <a id="33745" href="plfa.Lists.html#33693" class="Bound">xs</a>
</pre>{% endraw %}
{::comment}
Do we also have the following?
{:/}

下列命题是否成立？

{% raw %}<pre class="Agda"><a id="33817" class="Keyword">postulate</a>
  <a id="¬All≃Any¬"></a><a id="33829" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#33829" class="Postulate">¬All≃Any¬</a> <a id="33839" class="Symbol">:</a> <a id="33841" class="Symbol">∀</a> <a id="33843" class="Symbol">{</a><a id="33844" href="plfa.Lists.html#33844" class="Bound">A</a> <a id="33846" class="Symbol">:</a> <a id="33848" class="PrimitiveType">Set</a><a id="33851" class="Symbol">}</a> <a id="33853" class="Symbol">(</a><a id="33854" href="plfa.Lists.html#33854" class="Bound">P</a> <a id="33856" class="Symbol">:</a> <a id="33858" href="plfa.Lists.html#33844" class="Bound">A</a> <a id="33860" class="Symbol">→</a> <a id="33862" class="PrimitiveType">Set</a><a id="33865" class="Symbol">)</a> <a id="33867" class="Symbol">(</a><a id="33868" href="plfa.Lists.html#33868" class="Bound">xs</a> <a id="33871" class="Symbol">:</a> <a id="33873" href="plfa.Lists.html#1284" class="Datatype">List</a> <a id="33878" href="plfa.Lists.html#33844" class="Bound">A</a><a id="33879" class="Symbol">)</a>
    <a id="33885" class="Symbol">→</a> <a id="33887" class="Symbol">(</a><a id="33888" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.html#535" class="Function Operator">¬_</a> <a id="33891" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#33399" class="Function Operator">∘′</a> <a id="33894" href="plfa.Lists.html#27650" class="Datatype">All</a> <a id="33898" href="plfa.Lists.html#33854" class="Bound">P</a><a id="33899" class="Symbol">)</a> <a id="33901" href="plfa.Lists.html#33868" class="Bound">xs</a> <a id="33904" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Isomorphism.md %}{% raw %}#5843" class="Record Operator">≃</a> <a id="33906" href="plfa.Lists.html#29796" class="Datatype">Any</a> <a id="33910" class="Symbol">(</a><a id="33911" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.html#535" class="Function Operator">¬_</a> <a id="33914" href="plfa.Lists.html#33399" class="Function Operator">∘′</a> <a id="33917" href="plfa.Lists.html#33854" class="Bound">P</a><a id="33918" class="Symbol">)</a> <a id="33920" href="plfa.Lists.html#33868" class="Bound">xs</a>
</pre>{% endraw %}
{::comment}
If so, prove; if not, explain why.
{:/}

如果成立，请证明；如果不成立，请解释原因。


{::comment}
## Decidability of All
{:/}

## 所有的可判定性

{::comment}
If we consider a predicate as a function that yields a boolean,
it is easy to define an analogue of `All`, which returns true if
a given predicate returns true for every element of a list:
{:/}

如果我们将一个谓词看作一个返回布尔值的函数，那么我们可以简单的定义一个类似于 `All`
的函数，其当给定谓词对于列表每个元素返回真时返回真：

{% raw %}<pre class="Agda"><a id="all"></a><a id="34342" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#34342" class="Function">all</a> <a id="34346" class="Symbol">:</a> <a id="34348" class="Symbol">∀</a> <a id="34350" class="Symbol">{</a><a id="34351" href="plfa.Lists.html#34351" class="Bound">A</a> <a id="34353" class="Symbol">:</a> <a id="34355" class="PrimitiveType">Set</a><a id="34358" class="Symbol">}</a> <a id="34360" class="Symbol">→</a> <a id="34362" class="Symbol">(</a><a id="34363" href="plfa.Lists.html#34351" class="Bound">A</a> <a id="34365" class="Symbol">→</a> <a id="34367" href="Agda.Builtin.Bool.html#135" class="Datatype">Bool</a><a id="34371" class="Symbol">)</a> <a id="34373" class="Symbol">→</a> <a id="34375" href="plfa.Lists.html#1284" class="Datatype">List</a> <a id="34380" href="plfa.Lists.html#34351" class="Bound">A</a> <a id="34382" class="Symbol">→</a> <a id="34384" href="Agda.Builtin.Bool.html#135" class="Datatype">Bool</a>
<a id="34389" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#34342" class="Function">all</a> <a id="34393" href="plfa.Lists.html#34393" class="Bound">p</a>  <a id="34396" class="Symbol">=</a>  <a id="34399" href="plfa.Lists.html#20225" class="Function">foldr</a> <a id="34405" href="https://agda.github.io/agda-stdlib/v1.1/Data.Bool.Base.html#1015" class="Function Operator">_∧_</a> <a id="34409" href="Agda.Builtin.Bool.html#160" class="InductiveConstructor">true</a> <a id="34414" href="https://agda.github.io/agda-stdlib/v1.1/Function.html#1099" class="Function Operator">∘</a> <a id="34416" href="plfa.Lists.html#16959" class="Function">map</a> <a id="34420" href="plfa.Lists.html#34393" class="Bound">p</a>
</pre>{% endraw %}
{::comment}
The function can be written in a particularly compact style by
using the higher-order functions `map` and `foldr`.
{:/}

我们可以使用高阶函数 `map` 和 `foldr` 来简洁地写出这个函数。

{::comment}
As one would hope, if we replace booleans by decidables there is again
an analogue of `All`.  First, return to the notion of a predicate `P` as
a function of type `A → Set`, taking a value `x` of type `A` into evidence
`P x` that a property holds for `x`.  Say that a predicate `P` is _decidable_
if we have a function that for a given `x` can decide `P x`:
{:/}

正如所希望的那样，如果我们将布尔值替换成可判定值，这与 `All` 是相似的。首先，回到将 `P`
当作一个类型为 `A → Set` 的函数的概念，将一个类型为 `A` 的值 `x` 转换成 `P x` 对 `x` 成立
的证明。我们成 `P` 为**可判定的**（Decidable），如果我们有一个函数，其在给定 `x` 时能够判定 `P x`：

{% raw %}<pre class="Agda"><a id="Decidable"></a><a id="35159" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#35159" class="Function">Decidable</a> <a id="35169" class="Symbol">:</a> <a id="35171" class="Symbol">∀</a> <a id="35173" class="Symbol">{</a><a id="35174" href="plfa.Lists.html#35174" class="Bound">A</a> <a id="35176" class="Symbol">:</a> <a id="35178" class="PrimitiveType">Set</a><a id="35181" class="Symbol">}</a> <a id="35183" class="Symbol">→</a> <a id="35185" class="Symbol">(</a><a id="35186" href="plfa.Lists.html#35174" class="Bound">A</a> <a id="35188" class="Symbol">→</a> <a id="35190" class="PrimitiveType">Set</a><a id="35193" class="Symbol">)</a> <a id="35195" class="Symbol">→</a> <a id="35197" class="PrimitiveType">Set</a>
<a id="35201" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#35159" class="Function">Decidable</a> <a id="35211" class="Symbol">{</a><a id="35212" href="plfa.Lists.html#35212" class="Bound">A</a><a id="35213" class="Symbol">}</a> <a id="35215" href="plfa.Lists.html#35215" class="Bound">P</a>  <a id="35218" class="Symbol">=</a>  <a id="35221" class="Symbol">∀</a> <a id="35223" class="Symbol">(</a><a id="35224" href="plfa.Lists.html#35224" class="Bound">x</a> <a id="35226" class="Symbol">:</a> <a id="35228" href="plfa.Lists.html#35212" class="Bound">A</a><a id="35229" class="Symbol">)</a> <a id="35231" class="Symbol">→</a> <a id="35233" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.html#605" class="Datatype">Dec</a> <a id="35237" class="Symbol">(</a><a id="35238" href="plfa.Lists.html#35215" class="Bound">P</a> <a id="35240" href="plfa.Lists.html#35224" class="Bound">x</a><a id="35241" class="Symbol">)</a>
</pre>{% endraw %}
{::comment}
Then if predicate `P` is decidable, it is also decidable whether every
element of a list satisfies the predicate:
{:/}

那么当谓词 `P` 可判定时，我们亦可判定列表中的每一个元素是否满足这个谓词：
{% raw %}<pre class="Agda"><a id="All?"></a><a id="35424" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#35424" class="Function">All?</a> <a id="35429" class="Symbol">:</a> <a id="35431" class="Symbol">∀</a> <a id="35433" class="Symbol">{</a><a id="35434" href="plfa.Lists.html#35434" class="Bound">A</a> <a id="35436" class="Symbol">:</a> <a id="35438" class="PrimitiveType">Set</a><a id="35441" class="Symbol">}</a> <a id="35443" class="Symbol">{</a><a id="35444" href="plfa.Lists.html#35444" class="Bound">P</a> <a id="35446" class="Symbol">:</a> <a id="35448" href="plfa.Lists.html#35434" class="Bound">A</a> <a id="35450" class="Symbol">→</a> <a id="35452" class="PrimitiveType">Set</a><a id="35455" class="Symbol">}</a> <a id="35457" class="Symbol">→</a> <a id="35459" href="plfa.Lists.html#35159" class="Function">Decidable</a> <a id="35469" href="plfa.Lists.html#35444" class="Bound">P</a> <a id="35471" class="Symbol">→</a> <a id="35473" href="plfa.Lists.html#35159" class="Function">Decidable</a> <a id="35483" class="Symbol">(</a><a id="35484" href="plfa.Lists.html#27650" class="Datatype">All</a> <a id="35488" href="plfa.Lists.html#35444" class="Bound">P</a><a id="35489" class="Symbol">)</a>
<a id="35491" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#35424" class="Function">All?</a> <a id="35496" href="plfa.Lists.html#35496" class="Bound">P?</a> <a id="35499" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a>                                 <a id="35534" class="Symbol">=</a>  <a id="35537" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.html#641" class="InductiveConstructor">yes</a> <a id="35541" href="plfa.Lists.html#27701" class="InductiveConstructor">[]</a>
<a id="35544" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#35424" class="Function">All?</a> <a id="35549" href="plfa.Lists.html#35549" class="Bound">P?</a> <a id="35552" class="Symbol">(</a><a id="35553" href="plfa.Lists.html#35553" class="Bound">x</a> <a id="35555" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="35557" href="plfa.Lists.html#35557" class="Bound">xs</a><a id="35559" class="Symbol">)</a> <a id="35561" class="Keyword">with</a> <a id="35566" href="plfa.Lists.html#35549" class="Bound">P?</a> <a id="35569" href="plfa.Lists.html#35553" class="Bound">x</a>   <a id="35573" class="Symbol">|</a> <a id="35575" href="plfa.Lists.html#35424" class="Function">All?</a> <a id="35580" href="plfa.Lists.html#35549" class="Bound">P?</a> <a id="35583" href="plfa.Lists.html#35557" class="Bound">xs</a>
<a id="35586" class="Symbol">...</a>                 <a id="35606" class="Symbol">|</a> <a id="35608" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.html#641" class="InductiveConstructor">yes</a> <a id="35612" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#35612" class="Bound">Px</a> <a id="35615" class="Symbol">|</a> <a id="35617" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.html#641" class="InductiveConstructor">yes</a> <a id="35621" href="plfa.Lists.html#35621" class="Bound">Pxs</a>     <a id="35629" class="Symbol">=</a>  <a id="35632" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.html#641" class="InductiveConstructor">yes</a> <a id="35636" class="Symbol">(</a><a id="35637" href="plfa.Lists.html#35612" class="Bound">Px</a> <a id="35640" href="plfa.Lists.html#27718" class="InductiveConstructor Operator">∷</a> <a id="35642" href="plfa.Lists.html#35621" class="Bound">Pxs</a><a id="35645" class="Symbol">)</a>
<a id="35647" class="Symbol">...</a>                 <a id="35667" class="Symbol">|</a> <a id="35669" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.html#668" class="InductiveConstructor">no</a> <a id="35672" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#35672" class="Bound">¬Px</a> <a id="35676" class="Symbol">|</a> <a id="35678" class="Symbol">_</a>           <a id="35690" class="Symbol">=</a>  <a id="35693" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.html#668" class="InductiveConstructor">no</a> <a id="35696" class="Symbol">λ{</a> <a id="35699" class="Symbol">(</a><a id="35700" href="plfa.Lists.html#35700" class="Bound">Px</a> <a id="35703" href="plfa.Lists.html#27718" class="InductiveConstructor Operator">∷</a> <a id="35705" href="plfa.Lists.html#35705" class="Bound">Pxs</a><a id="35708" class="Symbol">)</a> <a id="35710" class="Symbol">→</a> <a id="35712" href="plfa.Lists.html#35672" class="Bound">¬Px</a> <a id="35716" href="plfa.Lists.html#35700" class="Bound">Px</a>   <a id="35721" class="Symbol">}</a>
<a id="35723" class="CatchallClause Symbol">...</a><a id="35726" class="CatchallClause">                 </a><a id="35743" class="CatchallClause Symbol">|</a><a id="35744" class="CatchallClause"> </a><a id="35745" class="CatchallClause Symbol">_</a><a id="35746" class="CatchallClause">      </a><a id="35752" class="CatchallClause Symbol">|</a><a id="35753" class="CatchallClause"> </a><a id="35754" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.html#668" class="CatchallClause InductiveConstructor">no</a><a id="35756" class="CatchallClause"> </a><a id="35757" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#35757" class="CatchallClause Bound">¬Pxs</a>     <a id="35766" class="Symbol">=</a>  <a id="35769" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.html#668" class="InductiveConstructor">no</a> <a id="35772" class="Symbol">λ{</a> <a id="35775" class="Symbol">(</a><a id="35776" href="plfa.Lists.html#35776" class="Bound">Px</a> <a id="35779" href="plfa.Lists.html#27718" class="InductiveConstructor Operator">∷</a> <a id="35781" href="plfa.Lists.html#35781" class="Bound">Pxs</a><a id="35784" class="Symbol">)</a> <a id="35786" class="Symbol">→</a> <a id="35788" href="plfa.Lists.html#35757" class="Bound">¬Pxs</a> <a id="35793" href="plfa.Lists.html#35781" class="Bound">Pxs</a> <a id="35797" class="Symbol">}</a>
</pre>{% endraw %}
{::comment}
If the list is empty, then trivially `P` holds for every element of
the list.  Otherwise, the structure of the proof is similar to that
showing that the conjunction of two decidable propositions is itself
decidable, using `_∷_` rather than `⟨_,_⟩` to combine the evidence for
the head and tail of the list.
{:/}

如果列表为空，那么 `P` 显然对列表的每个元素成立。
否则，证明的结构与两个可判定的命题是可判定的证明相似，不过我们使用 `_∷_` 而不是 `⟨_,_⟩`
来整合头元素和尾列表的证明。


{::comment}
#### Exercise `any?` (stretch)
{:/}

#### 练习 `any?` （延伸）

{::comment}
Just as `All` has analogues `all` and `All?` which determine whether a
predicate holds for every element of a list, so does `Any` have
analogues `any` and `Any?` which determine whether a predicate holds
for some element of a list.  Give their definitions.
{:/}

正如 `All` 有类似的 `all` 和 `All?` 形式，来判断列表的每个元素是否满足给定的谓词，
那么 `Any` 也有类似的 `any` 和 `Any?` 形式，来判断列表的一些元素是否满足给定的谓词。
给出它们的定义。

{::comment}
{% raw %}<pre class="Agda"><a id="36704" class="Comment">-- Your code goes here</a>
</pre>{% endraw %}{:/}

{% raw %}<pre class="Agda"><a id="36741" class="Comment">-- 请将代码写在此处。</a>
</pre>{% endraw %}

{::comment}
#### Exercise `All-∀`
{:/}

#### 练习 `All-∀`

{::comment}
Show that `All P xs` is isomorphic to `∀ {x} → x ∈ xs → P x`.
{:/}

证明 `All P xs` 与 `∀ {x} → x ∈ xs → P x` 同构。

{::comment}
{% raw %}<pre class="Agda"><a id="36957" class="Comment">-- You code goes here</a>
</pre>{% endraw %}{:/}

{% raw %}<pre class="Agda"><a id="36993" class="Comment">-- 请将代码写在此处。</a>
</pre>{% endraw %}

{::comment}
#### Exercise `Any-∃`
{:/}

#### 练习 `Any-∃`

{::comment}
Show that `Any P xs` is isomorphic to `∃[ x ∈ xs ] P x`.
{:/}

证明 `Any P xs` 与 `∃[ x ∈ xs ] P x` 同构。

{::comment}
{% raw %}<pre class="Agda"><a id="37199" class="Comment">-- You code goes here</a>
</pre>{% endraw %}{:/}

{% raw %}<pre class="Agda"><a id="37235" class="Comment">-- 请将代码写在此处。</a>
</pre>{% endraw %}

{::comment}
#### Exercise `filter?` (stretch)
{:/}

#### 练习 `filter?` （延伸）

{::comment}
Define the following variant of the traditional `filter` function on lists,
which given a decidable predicate and a list returns all elements of the
list satisfying the predicate:
{:/}

定义下面给出的列表 `filter` 函数的变种，给定一个可判定的谓词和一个列表，返回列表中所有满足
谓词的元素：

{% raw %}<pre class="Agda"><a id="37592" class="Keyword">postulate</a>
  <a id="filter?"></a><a id="37604" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#37604" class="Postulate">filter?</a> <a id="37612" class="Symbol">:</a> <a id="37614" class="Symbol">∀</a> <a id="37616" class="Symbol">{</a><a id="37617" href="plfa.Lists.html#37617" class="Bound">A</a> <a id="37619" class="Symbol">:</a> <a id="37621" class="PrimitiveType">Set</a><a id="37624" class="Symbol">}</a> <a id="37626" class="Symbol">{</a><a id="37627" href="plfa.Lists.html#37627" class="Bound">P</a> <a id="37629" class="Symbol">:</a> <a id="37631" href="plfa.Lists.html#37617" class="Bound">A</a> <a id="37633" class="Symbol">→</a> <a id="37635" class="PrimitiveType">Set</a><a id="37638" class="Symbol">}</a>
    <a id="37644" class="Symbol">→</a> <a id="37646" class="Symbol">(</a><a id="37647" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#37647" class="Bound">P?</a> <a id="37650" class="Symbol">:</a> <a id="37652" href="plfa.Lists.html#35159" class="Function">Decidable</a> <a id="37662" href="plfa.Lists.html#37627" class="Bound">P</a><a id="37663" class="Symbol">)</a> <a id="37665" class="Symbol">→</a> <a id="37667" href="plfa.Lists.html#1284" class="Datatype">List</a> <a id="37672" href="plfa.Lists.html#37617" class="Bound">A</a> <a id="37674" class="Symbol">→</a> <a id="37676" href="https://agda.github.io/agda-stdlib/v1.1/Data.Product.html#1783" class="Function">∃[</a> <a id="37679" href="plfa.Lists.html#37679" class="Bound">ys</a> <a id="37682" href="https://agda.github.io/agda-stdlib/v1.1/Data.Product.html#1783" class="Function">]</a><a id="37683" class="Symbol">(</a> <a id="37685" href="plfa.Lists.html#27650" class="Datatype">All</a> <a id="37689" href="plfa.Lists.html#37627" class="Bound">P</a> <a id="37691" href="plfa.Lists.html#37679" class="Bound">ys</a> <a id="37694" class="Symbol">)</a>
</pre>{% endraw %}

{::comment}
## Standard Library
{:/}

## 标准库

{::comment}
Definitions similar to those in this chapter can be found in the standard library:
{:/}

标准库中可以找到与本章节中相似的定义：

{% raw %}<pre class="Agda"><a id="37874" class="Keyword">import</a> <a id="37881" href="https://agda.github.io/agda-stdlib/v1.1/Data.List.html" class="Module">Data.List</a> <a id="37891" class="Keyword">using</a> <a id="37897" class="Symbol">(</a><a id="37898" href="Agda.Builtin.List.html#121" class="Datatype">List</a><a id="37902" class="Symbol">;</a> <a id="37904" href="https://agda.github.io/agda-stdlib/v1.1/Data.List.Base.html#1570" class="Function Operator">_++_</a><a id="37908" class="Symbol">;</a> <a id="37910" href="https://agda.github.io/agda-stdlib/v1.1/Data.List.Base.html#4104" class="Function">length</a><a id="37916" class="Symbol">;</a> <a id="37918" href="https://agda.github.io/agda-stdlib/v1.1/Data.List.Base.html#8564" class="Function">reverse</a><a id="37925" class="Symbol">;</a> <a id="37927" href="https://agda.github.io/agda-stdlib/v1.1/Data.List.Base.html#1304" class="Function">map</a><a id="37930" class="Symbol">;</a> <a id="37932" href="https://agda.github.io/agda-stdlib/v1.1/Data.List.Base.html#3432" class="Function">foldr</a><a id="37937" class="Symbol">;</a> <a id="37939" href="https://agda.github.io/agda-stdlib/v1.1/Data.List.Base.html#5510" class="Function">downFrom</a><a id="37947" class="Symbol">)</a>
<a id="37949" class="Keyword">import</a> <a id="37956" href="https://agda.github.io/agda-stdlib/v1.1/Data.List.All.html" class="Module">Data.List.All</a> <a id="37970" class="Keyword">using</a> <a id="37976" class="Symbol">(</a><a id="37977" href="https://agda.github.io/agda-stdlib/v1.1/Data.List.Relation.Unary.All.html#1176" class="Datatype">All</a><a id="37980" class="Symbol">;</a> <a id="37982" href="https://agda.github.io/agda-stdlib/v1.1/Data.List.Relation.Unary.All.html#1239" class="InductiveConstructor">[]</a><a id="37984" class="Symbol">;</a> <a id="37986" href="https://agda.github.io/agda-stdlib/v1.1/Data.List.Relation.Unary.All.html#1256" class="InductiveConstructor Operator">_∷_</a><a id="37989" class="Symbol">)</a>
<a id="37991" class="Keyword">import</a> <a id="37998" href="https://agda.github.io/agda-stdlib/v1.1/Data.List.Any.html" class="Module">Data.List.Any</a> <a id="38012" class="Keyword">using</a> <a id="38018" class="Symbol">(</a><a id="38019" href="https://agda.github.io/agda-stdlib/v1.1/Data.List.Relation.Unary.Any.html#1052" class="Datatype">Any</a><a id="38022" class="Symbol">;</a> <a id="38024" href="https://agda.github.io/agda-stdlib/v1.1/Data.List.Relation.Unary.Any.html#1115" class="InductiveConstructor">here</a><a id="38028" class="Symbol">;</a> <a id="38030" href="https://agda.github.io/agda-stdlib/v1.1/Data.List.Relation.Unary.Any.html#1168" class="InductiveConstructor">there</a><a id="38035" class="Symbol">)</a>
<a id="38037" class="Keyword">import</a> <a id="38044" href="https://agda.github.io/agda-stdlib/v1.1/Data.List.Membership.Propositional.html" class="Module">Data.List.Membership.Propositional</a> <a id="38079" class="Keyword">using</a> <a id="38085" class="Symbol">(</a><a id="38086" href="https://agda.github.io/agda-stdlib/v1.1/Data.List.Membership.Setoid.html#877" class="Function Operator">_∈_</a><a id="38089" class="Symbol">)</a>
<a id="38091" class="Keyword">import</a> <a id="38098" href="https://agda.github.io/agda-stdlib/v1.1/Data.List.Properties.html" class="Module">Data.List.Properties</a>
  <a id="38121" class="Keyword">using</a> <a id="38127" class="Symbol">(</a><a id="38128" href="https://agda.github.io/agda-stdlib/v1.1/Data.List.Properties.html#28549" class="Function">reverse-++-commute</a><a id="38146" class="Symbol">;</a> <a id="38148" href="https://agda.github.io/agda-stdlib/v1.1/Data.List.Properties.html#3412" class="Function">map-compose</a><a id="38159" class="Symbol">;</a> <a id="38161" href="https://agda.github.io/agda-stdlib/v1.1/Data.List.Properties.html#2721" class="Function">map-++-commute</a><a id="38175" class="Symbol">;</a> <a id="38177" href="https://agda.github.io/agda-stdlib/v1.1/Data.List.Properties.html#15835" class="Function">foldr-++</a><a id="38185" class="Symbol">)</a>
  <a id="38189" class="Keyword">renaming</a> <a id="38198" class="Symbol">(</a><a id="38199" href="https://agda.github.io/agda-stdlib/v1.1/Data.List.Properties.html#35768" class="Function">mapIsFold</a> <a id="38209" class="Symbol">to</a> <a id="38212" href="https://agda.github.io/agda-stdlib/v1.1/Data.List.Properties.html#35768" class="Function">map-is-foldr</a><a id="38224" class="Symbol">)</a>
<a id="38226" class="Keyword">import</a> <a id="38233" href="https://agda.github.io/agda-stdlib/v1.1/Algebra.Structures.html" class="Module">Algebra.Structures</a> <a id="38252" class="Keyword">using</a> <a id="38258" class="Symbol">(</a><a id="38259" href="https://agda.github.io/agda-stdlib/v1.1/Algebra.Structures.html#2215" class="Record">IsMonoid</a><a id="38267" class="Symbol">)</a>
<a id="38269" class="Keyword">import</a> <a id="38276" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Unary.html" class="Module">Relation.Unary</a> <a id="38291" class="Keyword">using</a> <a id="38297" class="Symbol">(</a><a id="38298" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Unary.html#3474" class="Function">Decidable</a><a id="38307" class="Symbol">)</a>
<a id="38309" class="Keyword">import</a> <a id="38316" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.html" class="Module">Relation.Binary</a> <a id="38332" class="Keyword">using</a> <a id="38338" class="Symbol">(</a><a id="38339" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.Core.html#5557" class="Function">Decidable</a><a id="38348" class="Symbol">)</a>
</pre>{% endraw %}
{::comment}
The standard library version of `IsMonoid` differs from the
one given here, in that it is also parameterised on an equivalence relation.
{:/}

标准库中的 `IsMonoid` 与给出的定义不同，因为它可以针对特定的等价关系参数化。

{::comment}
Both `Relation.Unary` and `Relation.Binary` define a version of `Decidable`,
one for unary relations (as used in this chapter where `P` ranges over
unary predicates) and one for binary relations (as used earlier, where `_≤_`
ranges over a binary relation).
{:/}

`Relation.Unary` 和 `Relation.Binary` 都定义了 `Decidable` 的某个版本，一个
用于单元关系（正如本章中的单元谓词 `P`），一个用于二元关系（正如之前使用的 `_≤_`）。

## Unicode

{::comment}
This chapter uses the following unicode:
{:/}

本章节使用下列 Unicode：

{::comment}
    ∷  U+2237  PROPORTION  (\::)
    ⊗  U+2297  CIRCLED TIMES  (\otimes, \ox)
    ∈  U+2208  ELEMENT OF  (\in)
    ∉  U+2209  NOT AN ELEMENT OF  (\inn, \notin)

{:/}

    ∷  U+2237  比例  (\::)
    ⊗  U+2297  带圈的乘号  (\otimes, \ox)
    ∈  U+2208  是……的元素  (\in)
    ∉  U+2209  不是……的元素  (\inn, \notin)
