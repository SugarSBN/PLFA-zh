---
src       : "courses/puc/2019/Assignment2.lagda.md"
title     : "Assignment2: PUC Assignment 2"
layout    : page
permalink : /PUC/2019/Assignment2/
---

{% raw %}<pre class="Agda"><a id="110" class="Keyword">module</a> <a id="117" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}" class="Module">Assignment2</a> <a id="129" class="Keyword">where</a>
</pre>{% endraw %}
## YOUR NAME AND EMAIL GOES HERE

## Introduction

You must do _all_ the exercises labelled "(recommended)".

Exercises labelled "(stretch)" are there to provide an extra challenge.
You don't need to do all of these, but should attempt at least a few.

Exercises without a label are optional, and may be done if you want
some extra practice.

Please ensure your files execute correctly under Agda!

## Imports

{% raw %}<pre class="Agda"><a id="555" class="Keyword">import</a> <a id="562" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.html" class="Module">Relation.Binary.PropositionalEquality</a> <a id="600" class="Symbol">as</a> <a id="603" class="Module">Eq</a>
<a id="606" class="Keyword">open</a> <a id="611" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.html" class="Module">Eq</a> <a id="614" class="Keyword">using</a> <a id="620" class="Symbol">(</a><a id="621" href="Agda.Builtin.Equality.html#125" class="Datatype Operator">_≡_</a><a id="624" class="Symbol">;</a> <a id="626" href="Agda.Builtin.Equality.html#182" class="InductiveConstructor">refl</a><a id="630" class="Symbol">;</a> <a id="632" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#1090" class="Function">cong</a><a id="636" class="Symbol">;</a> <a id="638" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#939" class="Function">sym</a><a id="641" class="Symbol">)</a>
<a id="643" class="Keyword">open</a> <a id="648" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2499" class="Module">Eq.≡-Reasoning</a> <a id="663" class="Keyword">using</a> <a id="669" class="Symbol">(</a><a id="670" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2597" class="Function Operator">begin_</a><a id="676" class="Symbol">;</a> <a id="678" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2655" class="Function Operator">_≡⟨⟩_</a><a id="683" class="Symbol">;</a> <a id="685" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2714" class="Function Operator">_≡⟨_⟩_</a><a id="691" class="Symbol">;</a> <a id="693" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.Core.html#2892" class="Function Operator">_∎</a><a id="695" class="Symbol">)</a>
<a id="697" class="Keyword">open</a> <a id="702" class="Keyword">import</a> <a id="709" href="https://agda.github.io/agda-stdlib/v1.1/Data.Nat.html" class="Module">Data.Nat</a> <a id="718" class="Keyword">using</a> <a id="724" class="Symbol">(</a><a id="725" href="Agda.Builtin.Nat.html#165" class="Datatype">ℕ</a><a id="726" class="Symbol">;</a> <a id="728" href="Agda.Builtin.Nat.html#183" class="InductiveConstructor">zero</a><a id="732" class="Symbol">;</a> <a id="734" href="Agda.Builtin.Nat.html#196" class="InductiveConstructor">suc</a><a id="737" class="Symbol">;</a> <a id="739" href="Agda.Builtin.Nat.html#298" class="Primitive Operator">_+_</a><a id="742" class="Symbol">;</a> <a id="744" href="Agda.Builtin.Nat.html#501" class="Primitive Operator">_*_</a><a id="747" class="Symbol">;</a> <a id="749" href="Agda.Builtin.Nat.html#388" class="Primitive Operator">_∸_</a><a id="752" class="Symbol">;</a> <a id="754" href="https://agda.github.io/agda-stdlib/v1.1/Data.Nat.Base.html#895" class="Datatype Operator">_≤_</a><a id="757" class="Symbol">;</a> <a id="759" href="https://agda.github.io/agda-stdlib/v1.1/Data.Nat.Base.html#918" class="InductiveConstructor">z≤n</a><a id="762" class="Symbol">;</a> <a id="764" href="https://agda.github.io/agda-stdlib/v1.1/Data.Nat.Base.html#960" class="InductiveConstructor">s≤s</a><a id="767" class="Symbol">)</a>
<a id="769" class="Keyword">open</a> <a id="774" class="Keyword">import</a> <a id="781" href="https://agda.github.io/agda-stdlib/v1.1/Data.Nat.Properties.html" class="Module">Data.Nat.Properties</a> <a id="801" class="Keyword">using</a> <a id="807" class="Symbol">(</a><a id="808" href="https://agda.github.io/agda-stdlib/v1.1/Data.Nat.Properties.html#11578" class="Function">+-assoc</a><a id="815" class="Symbol">;</a> <a id="817" href="https://agda.github.io/agda-stdlib/v1.1/Data.Nat.Properties.html#11734" class="Function">+-identityʳ</a><a id="828" class="Symbol">;</a> <a id="830" href="https://agda.github.io/agda-stdlib/v1.1/Data.Nat.Properties.html#11370" class="Function">+-suc</a><a id="835" class="Symbol">;</a> <a id="837" href="https://agda.github.io/agda-stdlib/v1.1/Data.Nat.Properties.html#11911" class="Function">+-comm</a><a id="843" class="Symbol">;</a>
  <a id="847" href="https://agda.github.io/agda-stdlib/v1.1/Data.Nat.Properties.html#3632" class="Function">≤-refl</a><a id="853" class="Symbol">;</a> <a id="855" href="https://agda.github.io/agda-stdlib/v1.1/Data.Nat.Properties.html#3815" class="Function">≤-trans</a><a id="862" class="Symbol">;</a> <a id="864" href="https://agda.github.io/agda-stdlib/v1.1/Data.Nat.Properties.html#3682" class="Function">≤-antisym</a><a id="873" class="Symbol">;</a> <a id="875" href="https://agda.github.io/agda-stdlib/v1.1/Data.Nat.Properties.html#3927" class="Function">≤-total</a><a id="882" class="Symbol">;</a> <a id="884" href="https://agda.github.io/agda-stdlib/v1.1/Data.Nat.Properties.html#15619" class="Function">+-monoʳ-≤</a><a id="893" class="Symbol">;</a> <a id="895" href="https://agda.github.io/agda-stdlib/v1.1/Data.Nat.Properties.html#15529" class="Function">+-monoˡ-≤</a><a id="904" class="Symbol">;</a> <a id="906" href="https://agda.github.io/agda-stdlib/v1.1/Data.Nat.Properties.html#15373" class="Function">+-mono-≤</a><a id="914" class="Symbol">)</a>
<a id="916" class="Keyword">open</a> <a id="921" class="Keyword">import</a> <a id="928" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}" class="Module">plfa.Relations</a> <a id="943" class="Keyword">using</a> <a id="949" class="Symbol">(</a><a id="950" href="plfa.Relations.html#26522" class="Datatype Operator">_&lt;_</a><a id="953" class="Symbol">;</a> <a id="955" href="plfa.Relations.html#26549" class="InductiveConstructor">z&lt;s</a><a id="958" class="Symbol">;</a> <a id="960" href="plfa.Relations.html#26606" class="InductiveConstructor">s&lt;s</a><a id="963" class="Symbol">)</a>
<a id="965" class="Keyword">open</a> <a id="970" class="Keyword">import</a> <a id="977" href="https://agda.github.io/agda-stdlib/v1.1/Data.Product.html" class="Module">Data.Product</a> <a id="990" class="Keyword">using</a> <a id="996" class="Symbol">(</a><a id="997" href="https://agda.github.io/agda-stdlib/v1.1/Data.Product.html#1162" class="Function Operator">_×_</a><a id="1000" class="Symbol">;</a> <a id="1002" href="Agda.Builtin.Sigma.html#225" class="Field">proj₁</a><a id="1007" class="Symbol">;</a> <a id="1009" href="Agda.Builtin.Sigma.html#237" class="Field">proj₂</a><a id="1014" class="Symbol">)</a> <a id="1016" class="Keyword">renaming</a> <a id="1025" class="Symbol">(</a><a id="1026" href="Agda.Builtin.Sigma.html#209" class="InductiveConstructor Operator">_,_</a> <a id="1030" class="Symbol">to</a> <a id="1033" href="Agda.Builtin.Sigma.html#209" class="InductiveConstructor Operator">⟨_,_⟩</a><a id="1038" class="Symbol">)</a>
<a id="1040" class="Keyword">open</a> <a id="1045" class="Keyword">import</a> <a id="1052" href="https://agda.github.io/agda-stdlib/v1.1/Data.Product.html" class="Module">Data.Product</a> <a id="1065" class="Keyword">using</a> <a id="1071" class="Symbol">(</a><a id="1072" href="Agda.Builtin.Sigma.html#139" class="Record">Σ</a><a id="1073" class="Symbol">;</a> <a id="1075" href="https://agda.github.io/agda-stdlib/v1.1/Data.Product.html#1364" class="Function">∃</a><a id="1076" class="Symbol">;</a> <a id="1078" href="https://agda.github.io/agda-stdlib/v1.1/Data.Product.html#911" class="Function">Σ-syntax</a><a id="1086" class="Symbol">;</a> <a id="1088" href="https://agda.github.io/agda-stdlib/v1.1/Data.Product.html#1783" class="Function">∃-syntax</a><a id="1096" class="Symbol">)</a>
<a id="1098" class="Keyword">open</a> <a id="1103" class="Keyword">import</a> <a id="1110" href="https://agda.github.io/agda-stdlib/v1.1/Data.Unit.html" class="Module">Data.Unit</a> <a id="1120" class="Keyword">using</a> <a id="1126" class="Symbol">(</a><a id="1127" href="Agda.Builtin.Unit.html#137" class="Record">⊤</a><a id="1128" class="Symbol">;</a> <a id="1130" href="Agda.Builtin.Unit.html#174" class="InductiveConstructor">tt</a><a id="1132" class="Symbol">)</a>
<a id="1134" class="Keyword">open</a> <a id="1139" class="Keyword">import</a> <a id="1146" href="https://agda.github.io/agda-stdlib/v1.1/Data.Sum.html" class="Module">Data.Sum</a> <a id="1155" class="Keyword">using</a> <a id="1161" class="Symbol">(</a><a id="1162" href="https://agda.github.io/agda-stdlib/v1.1/Data.Sum.Base.html#612" class="Datatype Operator">_⊎_</a><a id="1165" class="Symbol">;</a> <a id="1167" href="https://agda.github.io/agda-stdlib/v1.1/Data.Sum.Base.html#662" class="InductiveConstructor">inj₁</a><a id="1171" class="Symbol">;</a> <a id="1173" href="https://agda.github.io/agda-stdlib/v1.1/Data.Sum.Base.html#687" class="InductiveConstructor">inj₂</a><a id="1177" class="Symbol">)</a> <a id="1179" class="Keyword">renaming</a> <a id="1188" class="Symbol">(</a><a id="1189" href="https://agda.github.io/agda-stdlib/v1.1/Data.Sum.Base.html#798" class="Function Operator">[_,_]</a> <a id="1195" class="Symbol">to</a> <a id="1198" href="https://agda.github.io/agda-stdlib/v1.1/Data.Sum.Base.html#798" class="Function Operator">case-⊎</a><a id="1204" class="Symbol">)</a>
<a id="1206" class="Keyword">open</a> <a id="1211" class="Keyword">import</a> <a id="1218" href="https://agda.github.io/agda-stdlib/v1.1/Data.Empty.html" class="Module">Data.Empty</a> <a id="1229" class="Keyword">using</a> <a id="1235" class="Symbol">(</a><a id="1236" href="https://agda.github.io/agda-stdlib/v1.1/Data.Empty.html#279" class="Datatype">⊥</a><a id="1237" class="Symbol">;</a> <a id="1239" href="https://agda.github.io/agda-stdlib/v1.1/Data.Empty.html#294" class="Function">⊥-elim</a><a id="1245" class="Symbol">)</a>
<a id="1247" class="Keyword">open</a> <a id="1252" class="Keyword">import</a> <a id="1259" href="https://agda.github.io/agda-stdlib/v1.1/Data.Bool.Base.html" class="Module">Data.Bool.Base</a> <a id="1274" class="Keyword">using</a> <a id="1280" class="Symbol">(</a><a id="1281" href="Agda.Builtin.Bool.html#135" class="Datatype">Bool</a><a id="1285" class="Symbol">;</a> <a id="1287" href="Agda.Builtin.Bool.html#160" class="InductiveConstructor">true</a><a id="1291" class="Symbol">;</a> <a id="1293" href="Agda.Builtin.Bool.html#154" class="InductiveConstructor">false</a><a id="1298" class="Symbol">;</a> <a id="1300" href="https://agda.github.io/agda-stdlib/v1.1/Data.Bool.Base.html#1480" class="Function">T</a><a id="1301" class="Symbol">;</a> <a id="1303" href="https://agda.github.io/agda-stdlib/v1.1/Data.Bool.Base.html#1015" class="Function Operator">_∧_</a><a id="1306" class="Symbol">;</a> <a id="1308" href="https://agda.github.io/agda-stdlib/v1.1/Data.Bool.Base.html#1073" class="Function Operator">_∨_</a><a id="1311" class="Symbol">;</a> <a id="1313" href="https://agda.github.io/agda-stdlib/v1.1/Data.Bool.Base.html#961" class="Function">not</a><a id="1316" class="Symbol">)</a>
<a id="1318" class="Keyword">open</a> <a id="1323" class="Keyword">import</a> <a id="1330" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.html" class="Module">Relation.Nullary</a> <a id="1347" class="Keyword">using</a> <a id="1353" class="Symbol">(</a><a id="1354" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.html#535" class="Function Operator">¬_</a><a id="1356" class="Symbol">;</a> <a id="1358" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.html#605" class="Datatype">Dec</a><a id="1361" class="Symbol">;</a> <a id="1363" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.html#641" class="InductiveConstructor">yes</a><a id="1366" class="Symbol">;</a> <a id="1368" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.html#668" class="InductiveConstructor">no</a><a id="1370" class="Symbol">)</a>
<a id="1372" class="Keyword">open</a> <a id="1377" class="Keyword">import</a> <a id="1384" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.Decidable.html" class="Module">Relation.Nullary.Decidable</a> <a id="1411" class="Keyword">using</a> <a id="1417" class="Symbol">(</a><a id="1418" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.Decidable.Core.html#753" class="Function Operator">⌊_⌋</a><a id="1421" class="Symbol">;</a> <a id="1423" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.Decidable.Core.html#926" class="Function">toWitness</a><a id="1432" class="Symbol">;</a> <a id="1434" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.Decidable.Core.html#1062" class="Function">fromWitness</a><a id="1445" class="Symbol">)</a>
<a id="1447" class="Keyword">open</a> <a id="1452" class="Keyword">import</a> <a id="1459" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.Negation.html" class="Module">Relation.Nullary.Negation</a> <a id="1485" class="Keyword">using</a> <a id="1491" class="Symbol">(</a><a id="1492" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.Negation.html#1115" class="Function">¬?</a><a id="1494" class="Symbol">)</a>
<a id="1496" class="Keyword">open</a> <a id="1501" class="Keyword">import</a> <a id="1508" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.Product.html" class="Module">Relation.Nullary.Product</a> <a id="1533" class="Keyword">using</a> <a id="1539" class="Symbol">(</a><a id="1540" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.Product.html#585" class="Function Operator">_×-dec_</a><a id="1547" class="Symbol">)</a>
<a id="1549" class="Keyword">open</a> <a id="1554" class="Keyword">import</a> <a id="1561" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.Sum.html" class="Module">Relation.Nullary.Sum</a> <a id="1582" class="Keyword">using</a> <a id="1588" class="Symbol">(</a><a id="1589" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.Sum.html#668" class="Function Operator">_⊎-dec_</a><a id="1596" class="Symbol">)</a>
<a id="1598" class="Keyword">open</a> <a id="1603" class="Keyword">import</a> <a id="1610" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.Negation.html" class="Module">Relation.Nullary.Negation</a> <a id="1636" class="Keyword">using</a> <a id="1642" class="Symbol">(</a><a id="1643" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.Negation.html#880" class="Function">contraposition</a><a id="1657" class="Symbol">)</a>
<a id="1659" class="Keyword">open</a> <a id="1664" class="Keyword">import</a> <a id="1671" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Unary.html" class="Module">Relation.Unary</a> <a id="1686" class="Keyword">using</a> <a id="1692" class="Symbol">(</a><a id="1693" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Unary.html#3474" class="Function">Decidable</a><a id="1702" class="Symbol">)</a>
<a id="1704" class="Keyword">open</a> <a id="1709" class="Keyword">import</a> <a id="1716" href="https://agda.github.io/agda-stdlib/v1.1/Function.html" class="Module">Function</a> <a id="1725" class="Keyword">using</a> <a id="1731" class="Symbol">(</a><a id="1732" href="https://agda.github.io/agda-stdlib/v1.1/Function.html#1099" class="Function Operator">_∘_</a><a id="1735" class="Symbol">)</a>
<a id="1737" class="Keyword">open</a> <a id="1742" class="Keyword">import</a> <a id="1749" href="https://agda.github.io/agda-stdlib/v1.1/Level.html" class="Module">Level</a> <a id="1755" class="Keyword">using</a> <a id="1761" class="Symbol">(</a><a id="1762" href="Agda.Primitive.html#408" class="Postulate">Level</a><a id="1767" class="Symbol">)</a>
<a id="1769" class="Keyword">open</a> <a id="1774" class="Keyword">import</a> <a id="1781" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}" class="Module">plfa.Relations</a> <a id="1796" class="Keyword">using</a> <a id="1802" class="Symbol">(</a><a id="1803" href="plfa.Relations.html#26522" class="Datatype Operator">_&lt;_</a><a id="1806" class="Symbol">;</a> <a id="1808" href="plfa.Relations.html#26549" class="InductiveConstructor">z&lt;s</a><a id="1811" class="Symbol">;</a> <a id="1813" href="plfa.Relations.html#26606" class="InductiveConstructor">s&lt;s</a><a id="1816" class="Symbol">)</a>
<a id="1818" class="Keyword">open</a> <a id="1823" class="Keyword">import</a> <a id="1830" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Isomorphism.md %}{% raw %}" class="Module">plfa.Isomorphism</a> <a id="1847" class="Keyword">using</a> <a id="1853" class="Symbol">(</a><a id="1854" href="plfa.Isomorphism.html#5843" class="Record Operator">_≃_</a><a id="1857" class="Symbol">;</a> <a id="1859" href="plfa.Isomorphism.html#9378" class="Function">≃-sym</a><a id="1864" class="Symbol">;</a> <a id="1866" href="plfa.Isomorphism.html#9775" class="Function">≃-trans</a><a id="1873" class="Symbol">;</a> <a id="1875" href="plfa.Isomorphism.html#11919" class="Record Operator">_≲_</a><a id="1878" class="Symbol">;</a> <a id="1880" href="plfa.Isomorphism.html#3758" class="Postulate">extensionality</a><a id="1894" class="Symbol">)</a>
<a id="1896" class="Keyword">open</a> <a id="1901" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Isomorphism.md %}{% raw %}#10987" class="Module">plfa.Isomorphism.≃-Reasoning</a>
<a id="1930" class="Keyword">open</a> <a id="1935" class="Keyword">import</a> <a id="1942" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}" class="Module">plfa.Lists</a> <a id="1953" class="Keyword">using</a> <a id="1959" class="Symbol">(</a><a id="1960" href="plfa.Lists.html#1284" class="Datatype">List</a><a id="1964" class="Symbol">;</a> <a id="1966" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a><a id="1968" class="Symbol">;</a> <a id="1970" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">_∷_</a><a id="1973" class="Symbol">;</a> <a id="1975" href="plfa.Lists.html#3818" class="Operator">[_]</a><a id="1978" class="Symbol">;</a> <a id="1980" href="plfa.Lists.html#3841" class="Operator">[_,_]</a><a id="1985" class="Symbol">;</a> <a id="1987" href="plfa.Lists.html#3872" class="Operator">[_,_,_]</a><a id="1994" class="Symbol">;</a> <a id="1996" href="plfa.Lists.html#3911" class="Operator">[_,_,_,_]</a><a id="2005" class="Symbol">;</a>
  <a id="2009" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#4651" class="Function Operator">_++_</a><a id="2013" class="Symbol">;</a> <a id="2015" href="plfa.Lists.html#10957" class="Function">reverse</a><a id="2022" class="Symbol">;</a> <a id="2024" href="plfa.Lists.html#16959" class="Function">map</a><a id="2027" class="Symbol">;</a> <a id="2029" href="plfa.Lists.html#20225" class="Function">foldr</a><a id="2034" class="Symbol">;</a> <a id="2036" href="plfa.Lists.html#21337" class="Function">sum</a><a id="2039" class="Symbol">;</a> <a id="2041" href="plfa.Lists.html#27650" class="Datatype">All</a><a id="2044" class="Symbol">;</a> <a id="2046" href="plfa.Lists.html#29796" class="Datatype">Any</a><a id="2049" class="Symbol">;</a> <a id="2051" href="plfa.Lists.html#29847" class="InductiveConstructor">here</a><a id="2055" class="Symbol">;</a> <a id="2057" href="plfa.Lists.html#29904" class="InductiveConstructor">there</a><a id="2062" class="Symbol">;</a> <a id="2064" href="plfa.Lists.html#30311" class="Function Operator">_∈_</a><a id="2067" class="Symbol">)</a>
</pre>{% endraw %}
## Equality

#### Exercise `≤-reasoning` (stretch)

The proof of monotonicity from
Chapter [Relations][plfa.Relations]
can be written in a more readable form by using an analogue of our
notation for `≡-reasoning`.  Define `≤-reasoning` analogously, and use
it to write out an alternative proof that addition is monotonic with
regard to inequality.  Rewrite both `+-monoˡ-≤` and `+-mono-≤`.



## Isomorphism

#### Exercise `≃-implies-≲`

Show that every isomorphism implies an embedding.
{% raw %}<pre class="Agda"><a id="2566" class="Keyword">postulate</a>
  <a id="≃-implies-≲"></a><a id="2578" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#2578" class="Postulate">≃-implies-≲</a> <a id="2590" class="Symbol">:</a> <a id="2592" class="Symbol">∀</a> <a id="2594" class="Symbol">{</a><a id="2595" href="Assignment2.html#2595" class="Bound">A</a> <a id="2597" href="Assignment2.html#2597" class="Bound">B</a> <a id="2599" class="Symbol">:</a> <a id="2601" class="PrimitiveType">Set</a><a id="2604" class="Symbol">}</a>
    <a id="2610" class="Symbol">→</a> <a id="2612" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#2595" class="Bound">A</a> <a id="2614" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Isomorphism.md %}{% raw %}#5843" class="Record Operator">≃</a> <a id="2616" href="Assignment2.html#2597" class="Bound">B</a>
      <a id="2624" class="Comment">-----</a>
    <a id="2634" class="Symbol">→</a> <a id="2636" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#2595" class="Bound">A</a> <a id="2638" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Isomorphism.md %}{% raw %}#11919" class="Record Operator">≲</a> <a id="2640" href="Assignment2.html#2597" class="Bound">B</a>
</pre>{% endraw %}
#### Exercise `_⇔_` (recommended) {#iff}

Define equivalence of propositions (also known as "if and only if") as follows.
{% raw %}<pre class="Agda"><a id="2773" class="Keyword">record</a> <a id="_⇔_"></a><a id="2780" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#2780" class="Record Operator">_⇔_</a> <a id="2784" class="Symbol">(</a><a id="2785" href="Assignment2.html#2785" class="Bound">A</a> <a id="2787" href="Assignment2.html#2787" class="Bound">B</a> <a id="2789" class="Symbol">:</a> <a id="2791" class="PrimitiveType">Set</a><a id="2794" class="Symbol">)</a> <a id="2796" class="Symbol">:</a> <a id="2798" class="PrimitiveType">Set</a> <a id="2802" class="Keyword">where</a>
  <a id="2810" class="Keyword">field</a>
    <a id="_⇔_.to"></a><a id="2820" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#2820" class="Field">to</a>   <a id="2825" class="Symbol">:</a> <a id="2827" href="Assignment2.html#2785" class="Bound">A</a> <a id="2829" class="Symbol">→</a> <a id="2831" href="Assignment2.html#2787" class="Bound">B</a>
    <a id="_⇔_.from"></a><a id="2837" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#2837" class="Field">from</a> <a id="2842" class="Symbol">:</a> <a id="2844" href="Assignment2.html#2787" class="Bound">B</a> <a id="2846" class="Symbol">→</a> <a id="2848" href="Assignment2.html#2785" class="Bound">A</a>

<a id="2851" class="Keyword">open</a> <a id="2856" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#2780" class="Module Operator">_⇔_</a>
</pre>{% endraw %}Show that equivalence is reflexive, symmetric, and transitive.

#### Exercise `Bin-embedding` (stretch) {#Bin-embedding}

Recall that Exercises
[Bin][plfa.Naturals#Bin] and
[Bin-laws][plfa.Induction#Bin-laws]
define a datatype of bitstrings representing natural numbers.
{% raw %}<pre class="Agda"><a id="3139" class="Keyword">data</a> <a id="Bin"></a><a id="3144" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#3144" class="Datatype">Bin</a> <a id="3148" class="Symbol">:</a> <a id="3150" class="PrimitiveType">Set</a> <a id="3154" class="Keyword">where</a>
  <a id="Bin.nil"></a><a id="3162" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#3162" class="InductiveConstructor">nil</a> <a id="3166" class="Symbol">:</a> <a id="3168" href="Assignment2.html#3144" class="Datatype">Bin</a>
  <a id="Bin.x0_"></a><a id="3174" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#3174" class="InductiveConstructor Operator">x0_</a> <a id="3178" class="Symbol">:</a> <a id="3180" href="Assignment2.html#3144" class="Datatype">Bin</a> <a id="3184" class="Symbol">→</a> <a id="3186" href="Assignment2.html#3144" class="Datatype">Bin</a>
  <a id="Bin.x1_"></a><a id="3192" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#3192" class="InductiveConstructor Operator">x1_</a> <a id="3196" class="Symbol">:</a> <a id="3198" href="Assignment2.html#3144" class="Datatype">Bin</a> <a id="3202" class="Symbol">→</a> <a id="3204" href="Assignment2.html#3144" class="Datatype">Bin</a>
</pre>{% endraw %}And ask you to define the following functions:

    to : ℕ → Bin
    from : Bin → ℕ

which satisfy the following property:

    from (to n) ≡ n

Using the above, establish that there is an embedding of `ℕ` into `Bin`.
Why is there not an isomorphism?


## Connectives

#### Exercise `⇔≃×` (recommended)

Show that `A ⇔ B` as defined [earlier][plfa.Isomorphism#iff]
is isomorphic to `(A → B) × (B → A)`.

#### Exercise `⊎-comm` (recommended)

Show sum is commutative up to isomorphism.

#### Exercise `⊎-assoc`

Show sum is associative up to isomorphism.

#### Exercise `⊥-identityˡ` (recommended)

Show zero is the left identity of addition.

#### Exercise `⊥-identityʳ`

Show zero is the right identity of addition.

#### Exercise `⊎-weak-×` (recommended)

Show that the following property holds.
{% raw %}<pre class="Agda"><a id="4014" class="Keyword">postulate</a>
  <a id="⊎-weak-×"></a><a id="4026" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#4026" class="Postulate">⊎-weak-×</a> <a id="4035" class="Symbol">:</a> <a id="4037" class="Symbol">∀</a> <a id="4039" class="Symbol">{</a><a id="4040" href="Assignment2.html#4040" class="Bound">A</a> <a id="4042" href="Assignment2.html#4042" class="Bound">B</a> <a id="4044" href="Assignment2.html#4044" class="Bound">C</a> <a id="4046" class="Symbol">:</a> <a id="4048" class="PrimitiveType">Set</a><a id="4051" class="Symbol">}</a> <a id="4053" class="Symbol">→</a> <a id="4055" class="Symbol">(</a><a id="4056" href="Assignment2.html#4040" class="Bound">A</a> <a id="4058" href="https://agda.github.io/agda-stdlib/v1.1/Data.Sum.Base.html#612" class="Datatype Operator">⊎</a> <a id="4060" href="Assignment2.html#4042" class="Bound">B</a><a id="4061" class="Symbol">)</a> <a id="4063" href="https://agda.github.io/agda-stdlib/v1.1/Data.Product.html#1162" class="Function Operator">×</a> <a id="4065" href="Assignment2.html#4044" class="Bound">C</a> <a id="4067" class="Symbol">→</a> <a id="4069" href="Assignment2.html#4040" class="Bound">A</a> <a id="4071" href="https://agda.github.io/agda-stdlib/v1.1/Data.Sum.Base.html#612" class="Datatype Operator">⊎</a> <a id="4073" class="Symbol">(</a><a id="4074" href="Assignment2.html#4042" class="Bound">B</a> <a id="4076" href="https://agda.github.io/agda-stdlib/v1.1/Data.Product.html#1162" class="Function Operator">×</a> <a id="4078" href="Assignment2.html#4044" class="Bound">C</a><a id="4079" class="Symbol">)</a>
</pre>{% endraw %}This is called a _weak distributive law_. Give the corresponding
distributive law, and explain how it relates to the weak version.

#### Exercise `⊎×-implies-×⊎`

Show that a disjunct of conjuncts implies a conjunct of disjuncts.
{% raw %}<pre class="Agda"><a id="4319" class="Keyword">postulate</a>
  <a id="⊎×-implies-×⊎"></a><a id="4331" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#4331" class="Postulate">⊎×-implies-×⊎</a> <a id="4345" class="Symbol">:</a> <a id="4347" class="Symbol">∀</a> <a id="4349" class="Symbol">{</a><a id="4350" href="Assignment2.html#4350" class="Bound">A</a> <a id="4352" href="Assignment2.html#4352" class="Bound">B</a> <a id="4354" href="Assignment2.html#4354" class="Bound">C</a> <a id="4356" href="Assignment2.html#4356" class="Bound">D</a> <a id="4358" class="Symbol">:</a> <a id="4360" class="PrimitiveType">Set</a><a id="4363" class="Symbol">}</a> <a id="4365" class="Symbol">→</a> <a id="4367" class="Symbol">(</a><a id="4368" href="Assignment2.html#4350" class="Bound">A</a> <a id="4370" href="https://agda.github.io/agda-stdlib/v1.1/Data.Product.html#1162" class="Function Operator">×</a> <a id="4372" href="Assignment2.html#4352" class="Bound">B</a><a id="4373" class="Symbol">)</a> <a id="4375" href="https://agda.github.io/agda-stdlib/v1.1/Data.Sum.Base.html#612" class="Datatype Operator">⊎</a> <a id="4377" class="Symbol">(</a><a id="4378" href="Assignment2.html#4354" class="Bound">C</a> <a id="4380" href="https://agda.github.io/agda-stdlib/v1.1/Data.Product.html#1162" class="Function Operator">×</a> <a id="4382" href="Assignment2.html#4356" class="Bound">D</a><a id="4383" class="Symbol">)</a> <a id="4385" class="Symbol">→</a> <a id="4387" class="Symbol">(</a><a id="4388" href="Assignment2.html#4350" class="Bound">A</a> <a id="4390" href="https://agda.github.io/agda-stdlib/v1.1/Data.Sum.Base.html#612" class="Datatype Operator">⊎</a> <a id="4392" href="Assignment2.html#4354" class="Bound">C</a><a id="4393" class="Symbol">)</a> <a id="4395" href="https://agda.github.io/agda-stdlib/v1.1/Data.Product.html#1162" class="Function Operator">×</a> <a id="4397" class="Symbol">(</a><a id="4398" href="Assignment2.html#4352" class="Bound">B</a> <a id="4400" href="https://agda.github.io/agda-stdlib/v1.1/Data.Sum.Base.html#612" class="Datatype Operator">⊎</a> <a id="4402" href="Assignment2.html#4356" class="Bound">D</a><a id="4403" class="Symbol">)</a>
</pre>{% endraw %}Does the converse hold? If so, prove; if not, give a counterexample.


## Negation

#### Exercise `<-irreflexive` (recommended)

Using negation, show that
[strict inequality][plfa.Relations#strict-inequality]
is irreflexive, that is, `n < n` holds for no `n`.


#### Exercise `trichotomy`

Show that strict inequality satisfies
[trichotomy][plfa.Relations#trichotomy],
that is, for any naturals `m` and `n` exactly one of the following holds:

* `m < n`
* `m ≡ n`
* `m > n`

Here "exactly one" means that one of the three must hold, and each implies the
negation of the other two.


#### Exercise `⊎-dual-×` (recommended)

Show that conjunction, disjunction, and negation are related by a
version of De Morgan's Law.

    ¬ (A ⊎ B) ≃ (¬ A) × (¬ B)

This result is an easy consequence of something we've proved previously.

Do we also have the following?

    ¬ (A × B) ≃ (¬ A) ⊎ (¬ B)

If so, prove; if not, give a variant that does hold.


#### Exercise `Classical` (stretch)

Consider the following principles.

  * Excluded Middle: `A ⊎ ¬ A`, for all `A`
  * Double Negation Elimination: `¬ ¬ A → A`, for all `A`
  * Peirce's Law: `((A → B) → A) → A`, for all `A` and `B`.
  * Implication as disjunction: `(A → B) → ¬ A ⊎ B`, for all `A` and `B`.
  * De Morgan: `¬ (¬ A × ¬ B) → A ⊎ B`, for all `A` and `B`.

Show that each of these implies all the others.


#### Exercise `Stable` (stretch)

Say that a formula is _stable_ if double negation elimination holds for it.
{% raw %}<pre class="Agda"><a id="Stable"></a><a id="5885" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#5885" class="Function">Stable</a> <a id="5892" class="Symbol">:</a> <a id="5894" class="PrimitiveType">Set</a> <a id="5898" class="Symbol">→</a> <a id="5900" class="PrimitiveType">Set</a>
<a id="5904" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#5885" class="Function">Stable</a> <a id="5911" href="Assignment2.html#5911" class="Bound">A</a> <a id="5913" class="Symbol">=</a> <a id="5915" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.html#535" class="Function Operator">¬</a> <a id="5917" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.html#535" class="Function Operator">¬</a> <a id="5919" href="Assignment2.html#5911" class="Bound">A</a> <a id="5921" class="Symbol">→</a> <a id="5923" href="Assignment2.html#5911" class="Bound">A</a>
</pre>{% endraw %}Show that any negated formula is stable, and that the conjunction
of two stable formulas is stable.


## Quantifiers

#### Exercise `∀-distrib-×` (recommended)

Show that universals distribute over conjunction.
{% raw %}<pre class="Agda"><a id="6144" class="Keyword">postulate</a>
  <a id="∀-distrib-×"></a><a id="6156" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#6156" class="Postulate">∀-distrib-×</a> <a id="6168" class="Symbol">:</a> <a id="6170" class="Symbol">∀</a> <a id="6172" class="Symbol">{</a><a id="6173" href="Assignment2.html#6173" class="Bound">A</a> <a id="6175" class="Symbol">:</a> <a id="6177" class="PrimitiveType">Set</a><a id="6180" class="Symbol">}</a> <a id="6182" class="Symbol">{</a><a id="6183" href="Assignment2.html#6183" class="Bound">B</a> <a id="6185" href="Assignment2.html#6185" class="Bound">C</a> <a id="6187" class="Symbol">:</a> <a id="6189" href="Assignment2.html#6173" class="Bound">A</a> <a id="6191" class="Symbol">→</a> <a id="6193" class="PrimitiveType">Set</a><a id="6196" class="Symbol">}</a> <a id="6198" class="Symbol">→</a>
    <a id="6204" class="Symbol">(∀</a> <a id="6207" class="Symbol">(</a><a id="6208" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#6208" class="Bound">x</a> <a id="6210" class="Symbol">:</a> <a id="6212" href="Assignment2.html#6173" class="Bound">A</a><a id="6213" class="Symbol">)</a> <a id="6215" class="Symbol">→</a> <a id="6217" href="Assignment2.html#6183" class="Bound">B</a> <a id="6219" href="Assignment2.html#6208" class="Bound">x</a> <a id="6221" href="https://agda.github.io/agda-stdlib/v1.1/Data.Product.html#1162" class="Function Operator">×</a> <a id="6223" href="Assignment2.html#6185" class="Bound">C</a> <a id="6225" href="Assignment2.html#6208" class="Bound">x</a><a id="6226" class="Symbol">)</a> <a id="6228" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Isomorphism.md %}{% raw %}#5843" class="Record Operator">≃</a> <a id="6230" class="Symbol">(∀</a> <a id="6233" class="Symbol">(</a><a id="6234" href="Assignment2.html#6234" class="Bound">x</a> <a id="6236" class="Symbol">:</a> <a id="6238" href="Assignment2.html#6173" class="Bound">A</a><a id="6239" class="Symbol">)</a> <a id="6241" class="Symbol">→</a> <a id="6243" href="Assignment2.html#6183" class="Bound">B</a> <a id="6245" href="Assignment2.html#6234" class="Bound">x</a><a id="6246" class="Symbol">)</a> <a id="6248" href="https://agda.github.io/agda-stdlib/v1.1/Data.Product.html#1162" class="Function Operator">×</a> <a id="6250" class="Symbol">(∀</a> <a id="6253" class="Symbol">(</a><a id="6254" href="Assignment2.html#6254" class="Bound">x</a> <a id="6256" class="Symbol">:</a> <a id="6258" href="Assignment2.html#6173" class="Bound">A</a><a id="6259" class="Symbol">)</a> <a id="6261" class="Symbol">→</a> <a id="6263" href="Assignment2.html#6185" class="Bound">C</a> <a id="6265" href="Assignment2.html#6254" class="Bound">x</a><a id="6266" class="Symbol">)</a>
</pre>{% endraw %}Compare this with the result (`→-distrib-×`) in
Chapter [Connectives][plfa.Connectives].

#### Exercise `⊎∀-implies-∀⊎`

Show that a disjunction of universals implies a universal of disjunctions.
{% raw %}<pre class="Agda"><a id="6472" class="Keyword">postulate</a>
  <a id="⊎∀-implies-∀⊎"></a><a id="6484" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#6484" class="Postulate">⊎∀-implies-∀⊎</a> <a id="6498" class="Symbol">:</a> <a id="6500" class="Symbol">∀</a> <a id="6502" class="Symbol">{</a><a id="6503" href="Assignment2.html#6503" class="Bound">A</a> <a id="6505" class="Symbol">:</a> <a id="6507" class="PrimitiveType">Set</a><a id="6510" class="Symbol">}</a> <a id="6512" class="Symbol">{</a> <a id="6514" href="Assignment2.html#6514" class="Bound">B</a> <a id="6516" href="Assignment2.html#6516" class="Bound">C</a> <a id="6518" class="Symbol">:</a> <a id="6520" href="Assignment2.html#6503" class="Bound">A</a> <a id="6522" class="Symbol">→</a> <a id="6524" class="PrimitiveType">Set</a> <a id="6528" class="Symbol">}</a> <a id="6530" class="Symbol">→</a>
    <a id="6536" class="Symbol">(∀</a> <a id="6539" class="Symbol">(</a><a id="6540" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#6540" class="Bound">x</a> <a id="6542" class="Symbol">:</a> <a id="6544" href="Assignment2.html#6503" class="Bound">A</a><a id="6545" class="Symbol">)</a> <a id="6547" class="Symbol">→</a> <a id="6549" href="Assignment2.html#6514" class="Bound">B</a> <a id="6551" href="Assignment2.html#6540" class="Bound">x</a><a id="6552" class="Symbol">)</a> <a id="6554" href="https://agda.github.io/agda-stdlib/v1.1/Data.Sum.Base.html#612" class="Datatype Operator">⊎</a> <a id="6556" class="Symbol">(∀</a> <a id="6559" class="Symbol">(</a><a id="6560" href="Assignment2.html#6560" class="Bound">x</a> <a id="6562" class="Symbol">:</a> <a id="6564" href="Assignment2.html#6503" class="Bound">A</a><a id="6565" class="Symbol">)</a> <a id="6567" class="Symbol">→</a> <a id="6569" href="Assignment2.html#6516" class="Bound">C</a> <a id="6571" href="Assignment2.html#6560" class="Bound">x</a><a id="6572" class="Symbol">)</a>  <a id="6575" class="Symbol">→</a>  <a id="6578" class="Symbol">∀</a> <a id="6580" class="Symbol">(</a><a id="6581" href="Assignment2.html#6581" class="Bound">x</a> <a id="6583" class="Symbol">:</a> <a id="6585" href="Assignment2.html#6503" class="Bound">A</a><a id="6586" class="Symbol">)</a> <a id="6588" class="Symbol">→</a> <a id="6590" href="Assignment2.html#6514" class="Bound">B</a> <a id="6592" href="Assignment2.html#6581" class="Bound">x</a> <a id="6594" href="https://agda.github.io/agda-stdlib/v1.1/Data.Sum.Base.html#612" class="Datatype Operator">⊎</a> <a id="6596" href="Assignment2.html#6516" class="Bound">C</a> <a id="6598" href="Assignment2.html#6581" class="Bound">x</a>
</pre>{% endraw %}Does the converse hold? If so, prove; if not, explain why.

#### Exercise `∃-distrib-⊎` (recommended)

Show that existentials distribute over disjunction.
{% raw %}<pre class="Agda"><a id="6763" class="Keyword">postulate</a>
  <a id="∃-distrib-⊎"></a><a id="6775" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#6775" class="Postulate">∃-distrib-⊎</a> <a id="6787" class="Symbol">:</a> <a id="6789" class="Symbol">∀</a> <a id="6791" class="Symbol">{</a><a id="6792" href="Assignment2.html#6792" class="Bound">A</a> <a id="6794" class="Symbol">:</a> <a id="6796" class="PrimitiveType">Set</a><a id="6799" class="Symbol">}</a> <a id="6801" class="Symbol">{</a><a id="6802" href="Assignment2.html#6802" class="Bound">B</a> <a id="6804" href="Assignment2.html#6804" class="Bound">C</a> <a id="6806" class="Symbol">:</a> <a id="6808" href="Assignment2.html#6792" class="Bound">A</a> <a id="6810" class="Symbol">→</a> <a id="6812" class="PrimitiveType">Set</a><a id="6815" class="Symbol">}</a> <a id="6817" class="Symbol">→</a>
    <a id="6823" href="https://agda.github.io/agda-stdlib/v1.1/Data.Product.html#1783" class="Function">∃[</a> <a id="6826" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#6826" class="Bound">x</a> <a id="6828" href="https://agda.github.io/agda-stdlib/v1.1/Data.Product.html#1783" class="Function">]</a> <a id="6830" class="Symbol">(</a><a id="6831" href="Assignment2.html#6802" class="Bound">B</a> <a id="6833" href="Assignment2.html#6826" class="Bound">x</a> <a id="6835" href="https://agda.github.io/agda-stdlib/v1.1/Data.Sum.Base.html#612" class="Datatype Operator">⊎</a> <a id="6837" href="Assignment2.html#6804" class="Bound">C</a> <a id="6839" href="Assignment2.html#6826" class="Bound">x</a><a id="6840" class="Symbol">)</a> <a id="6842" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Isomorphism.md %}{% raw %}#5843" class="Record Operator">≃</a> <a id="6844" class="Symbol">(</a><a id="6845" href="https://agda.github.io/agda-stdlib/v1.1/Data.Product.html#1783" class="Function">∃[</a> <a id="6848" href="Assignment2.html#6848" class="Bound">x</a> <a id="6850" href="https://agda.github.io/agda-stdlib/v1.1/Data.Product.html#1783" class="Function">]</a> <a id="6852" href="Assignment2.html#6802" class="Bound">B</a> <a id="6854" href="Assignment2.html#6848" class="Bound">x</a><a id="6855" class="Symbol">)</a> <a id="6857" href="https://agda.github.io/agda-stdlib/v1.1/Data.Sum.Base.html#612" class="Datatype Operator">⊎</a> <a id="6859" class="Symbol">(</a><a id="6860" href="https://agda.github.io/agda-stdlib/v1.1/Data.Product.html#1783" class="Function">∃[</a> <a id="6863" href="Assignment2.html#6863" class="Bound">x</a> <a id="6865" href="https://agda.github.io/agda-stdlib/v1.1/Data.Product.html#1783" class="Function">]</a> <a id="6867" href="Assignment2.html#6804" class="Bound">C</a> <a id="6869" href="Assignment2.html#6863" class="Bound">x</a><a id="6870" class="Symbol">)</a>
</pre>{% endraw %}
#### Exercise `∃×-implies-×∃`

Show that an existential of conjunctions implies a conjunction of existentials.
{% raw %}<pre class="Agda"><a id="6992" class="Keyword">postulate</a>
  <a id="∃×-implies-×∃"></a><a id="7004" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#7004" class="Postulate">∃×-implies-×∃</a> <a id="7018" class="Symbol">:</a> <a id="7020" class="Symbol">∀</a> <a id="7022" class="Symbol">{</a><a id="7023" href="Assignment2.html#7023" class="Bound">A</a> <a id="7025" class="Symbol">:</a> <a id="7027" class="PrimitiveType">Set</a><a id="7030" class="Symbol">}</a> <a id="7032" class="Symbol">{</a> <a id="7034" href="Assignment2.html#7034" class="Bound">B</a> <a id="7036" href="Assignment2.html#7036" class="Bound">C</a> <a id="7038" class="Symbol">:</a> <a id="7040" href="Assignment2.html#7023" class="Bound">A</a> <a id="7042" class="Symbol">→</a> <a id="7044" class="PrimitiveType">Set</a> <a id="7048" class="Symbol">}</a> <a id="7050" class="Symbol">→</a>
    <a id="7056" href="https://agda.github.io/agda-stdlib/v1.1/Data.Product.html#1783" class="Function">∃[</a> <a id="7059" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#7059" class="Bound">x</a> <a id="7061" href="https://agda.github.io/agda-stdlib/v1.1/Data.Product.html#1783" class="Function">]</a> <a id="7063" class="Symbol">(</a><a id="7064" href="Assignment2.html#7034" class="Bound">B</a> <a id="7066" href="Assignment2.html#7059" class="Bound">x</a> <a id="7068" href="https://agda.github.io/agda-stdlib/v1.1/Data.Product.html#1162" class="Function Operator">×</a> <a id="7070" href="Assignment2.html#7036" class="Bound">C</a> <a id="7072" href="Assignment2.html#7059" class="Bound">x</a><a id="7073" class="Symbol">)</a> <a id="7075" class="Symbol">→</a> <a id="7077" class="Symbol">(</a><a id="7078" href="https://agda.github.io/agda-stdlib/v1.1/Data.Product.html#1783" class="Function">∃[</a> <a id="7081" href="Assignment2.html#7081" class="Bound">x</a> <a id="7083" href="https://agda.github.io/agda-stdlib/v1.1/Data.Product.html#1783" class="Function">]</a> <a id="7085" href="Assignment2.html#7034" class="Bound">B</a> <a id="7087" href="Assignment2.html#7081" class="Bound">x</a><a id="7088" class="Symbol">)</a> <a id="7090" href="https://agda.github.io/agda-stdlib/v1.1/Data.Product.html#1162" class="Function Operator">×</a> <a id="7092" class="Symbol">(</a><a id="7093" href="https://agda.github.io/agda-stdlib/v1.1/Data.Product.html#1783" class="Function">∃[</a> <a id="7096" href="Assignment2.html#7096" class="Bound">x</a> <a id="7098" href="https://agda.github.io/agda-stdlib/v1.1/Data.Product.html#1783" class="Function">]</a> <a id="7100" href="Assignment2.html#7036" class="Bound">C</a> <a id="7102" href="Assignment2.html#7096" class="Bound">x</a><a id="7103" class="Symbol">)</a>
</pre>{% endraw %}Does the converse hold? If so, prove; if not, explain why.

#### Exercise `∃-even-odd`

How do the proofs become more difficult if we replace `m * 2` and `1 + m * 2`
by `2 * m` and `2 * m + 1`?  Rewrite the proofs of `∃-even` and `∃-odd` when
restated in this way.

#### Exercise `∃-|-≤`

Show that `y ≤ z` holds if and only if there exists a `x` such that
`x + y ≡ z`.

#### Exercise `∃¬-implies-¬∀` (recommended)

Show that existential of a negation implies negation of a universal.
{% raw %}<pre class="Agda"><a id="7598" class="Keyword">postulate</a>
  <a id="∃¬-implies-¬∀"></a><a id="7610" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#7610" class="Postulate">∃¬-implies-¬∀</a> <a id="7624" class="Symbol">:</a> <a id="7626" class="Symbol">∀</a> <a id="7628" class="Symbol">{</a><a id="7629" href="Assignment2.html#7629" class="Bound">A</a> <a id="7631" class="Symbol">:</a> <a id="7633" class="PrimitiveType">Set</a><a id="7636" class="Symbol">}</a> <a id="7638" class="Symbol">{</a><a id="7639" href="Assignment2.html#7639" class="Bound">B</a> <a id="7641" class="Symbol">:</a> <a id="7643" href="Assignment2.html#7629" class="Bound">A</a> <a id="7645" class="Symbol">→</a> <a id="7647" class="PrimitiveType">Set</a><a id="7650" class="Symbol">}</a>
    <a id="7656" class="Symbol">→</a> <a id="7658" href="https://agda.github.io/agda-stdlib/v1.1/Data.Product.html#1783" class="Function">∃[</a> <a id="7661" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#7661" class="Bound">x</a> <a id="7663" href="https://agda.github.io/agda-stdlib/v1.1/Data.Product.html#1783" class="Function">]</a> <a id="7665" class="Symbol">(</a><a id="7666" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.html#535" class="Function Operator">¬</a> <a id="7668" href="Assignment2.html#7639" class="Bound">B</a> <a id="7670" href="Assignment2.html#7661" class="Bound">x</a><a id="7671" class="Symbol">)</a>
      <a id="7679" class="Comment">--------------</a>
    <a id="7698" class="Symbol">→</a> <a id="7700" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.html#535" class="Function Operator">¬</a> <a id="7702" class="Symbol">(∀</a> <a id="7705" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#7705" class="Bound">x</a> <a id="7707" class="Symbol">→</a> <a id="7709" href="Assignment2.html#7639" class="Bound">B</a> <a id="7711" href="Assignment2.html#7705" class="Bound">x</a><a id="7712" class="Symbol">)</a>
</pre>{% endraw %}Does the converse hold? If so, prove; if not, explain why.


#### Exercise `Bin-isomorphism` (stretch) {#Bin-isomorphism}

Recall that Exercises
[Bin][plfa.Naturals#Bin],
[Bin-laws][plfa.Induction#Bin-laws], and
[Bin-predicates][plfa.Relations#Bin-predicates]
define a datatype of bitstrings representing natural numbers.

    data Bin : Set where
      nil : Bin
      x0_ : Bin → Bin
      x1_ : Bin → Bin

And ask you to define the following functions and predicates.

    to   : ℕ → Bin
    from : Bin → ℕ
    Can  : Bin → Set

And to establish the following properties.

    from (to n) ≡ n

    ----------
    Can (to n)

    Can x
    ---------------
    to (from x) ≡ x

Using the above, establish that there is an isomorphism between `ℕ` and
`∃[ x ](Can x)`.


## Decidable

#### Exercise `_<?_` (recommended)

Analogous to the function above, define a function to decide strict inequality.
{% raw %}<pre class="Agda"><a id="8622" class="Keyword">postulate</a>
  <a id="_&lt;?_"></a><a id="8634" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#8634" class="Postulate Operator">_&lt;?_</a> <a id="8639" class="Symbol">:</a> <a id="8641" class="Symbol">∀</a> <a id="8643" class="Symbol">(</a><a id="8644" href="Assignment2.html#8644" class="Bound">m</a> <a id="8646" href="Assignment2.html#8646" class="Bound">n</a> <a id="8648" class="Symbol">:</a> <a id="8650" href="Agda.Builtin.Nat.html#165" class="Datatype">ℕ</a><a id="8651" class="Symbol">)</a> <a id="8653" class="Symbol">→</a> <a id="8655" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.html#605" class="Datatype">Dec</a> <a id="8659" class="Symbol">(</a><a id="8660" href="Assignment2.html#8644" class="Bound">m</a> <a id="8662" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#26522" class="Datatype Operator">&lt;</a> <a id="8664" href="Assignment2.html#8646" class="Bound">n</a><a id="8665" class="Symbol">)</a>
</pre>{% endraw %}
#### Exercise `_≡ℕ?_`

Define a function to decide whether two naturals are equal.
{% raw %}<pre class="Agda"><a id="8759" class="Keyword">postulate</a>
  <a id="_≡ℕ?_"></a><a id="8771" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#8771" class="Postulate Operator">_≡ℕ?_</a> <a id="8777" class="Symbol">:</a> <a id="8779" class="Symbol">∀</a> <a id="8781" class="Symbol">(</a><a id="8782" href="Assignment2.html#8782" class="Bound">m</a> <a id="8784" href="Assignment2.html#8784" class="Bound">n</a> <a id="8786" class="Symbol">:</a> <a id="8788" href="Agda.Builtin.Nat.html#165" class="Datatype">ℕ</a><a id="8789" class="Symbol">)</a> <a id="8791" class="Symbol">→</a> <a id="8793" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.html#605" class="Datatype">Dec</a> <a id="8797" class="Symbol">(</a><a id="8798" href="Assignment2.html#8782" class="Bound">m</a> <a id="8800" href="Agda.Builtin.Equality.html#125" class="Datatype Operator">≡</a> <a id="8802" href="Assignment2.html#8784" class="Bound">n</a><a id="8803" class="Symbol">)</a>
</pre>{% endraw %}

#### Exercise `erasure`

Show that erasure relates corresponding boolean and decidable operations.
{% raw %}<pre class="Agda"><a id="8914" class="Keyword">postulate</a>
  <a id="∧-×"></a><a id="8926" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#8926" class="Postulate">∧-×</a> <a id="8930" class="Symbol">:</a> <a id="8932" class="Symbol">∀</a> <a id="8934" class="Symbol">{</a><a id="8935" href="Assignment2.html#8935" class="Bound">A</a> <a id="8937" href="Assignment2.html#8937" class="Bound">B</a> <a id="8939" class="Symbol">:</a> <a id="8941" class="PrimitiveType">Set</a><a id="8944" class="Symbol">}</a> <a id="8946" class="Symbol">(</a><a id="8947" href="Assignment2.html#8947" class="Bound">x</a> <a id="8949" class="Symbol">:</a> <a id="8951" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.html#605" class="Datatype">Dec</a> <a id="8955" href="Assignment2.html#8935" class="Bound">A</a><a id="8956" class="Symbol">)</a> <a id="8958" class="Symbol">(</a><a id="8959" href="Assignment2.html#8959" class="Bound">y</a> <a id="8961" class="Symbol">:</a> <a id="8963" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.html#605" class="Datatype">Dec</a> <a id="8967" href="Assignment2.html#8937" class="Bound">B</a><a id="8968" class="Symbol">)</a> <a id="8970" class="Symbol">→</a> <a id="8972" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.Decidable.Core.html#753" class="Function Operator">⌊</a> <a id="8974" href="Assignment2.html#8947" class="Bound">x</a> <a id="8976" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.Decidable.Core.html#753" class="Function Operator">⌋</a> <a id="8978" href="https://agda.github.io/agda-stdlib/v1.1/Data.Bool.Base.html#1015" class="Function Operator">∧</a> <a id="8980" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.Decidable.Core.html#753" class="Function Operator">⌊</a> <a id="8982" href="Assignment2.html#8959" class="Bound">y</a> <a id="8984" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.Decidable.Core.html#753" class="Function Operator">⌋</a> <a id="8986" href="Agda.Builtin.Equality.html#125" class="Datatype Operator">≡</a> <a id="8988" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.Decidable.Core.html#753" class="Function Operator">⌊</a> <a id="8990" href="Assignment2.html#8947" class="Bound">x</a> <a id="8992" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.Product.html#585" class="Function Operator">×-dec</a> <a id="8998" href="Assignment2.html#8959" class="Bound">y</a> <a id="9000" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.Decidable.Core.html#753" class="Function Operator">⌋</a>
  <a id="∨-×"></a><a id="9004" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#9004" class="Postulate">∨-×</a> <a id="9008" class="Symbol">:</a> <a id="9010" class="Symbol">∀</a> <a id="9012" class="Symbol">{</a><a id="9013" href="Assignment2.html#9013" class="Bound">A</a> <a id="9015" href="Assignment2.html#9015" class="Bound">B</a> <a id="9017" class="Symbol">:</a> <a id="9019" class="PrimitiveType">Set</a><a id="9022" class="Symbol">}</a> <a id="9024" class="Symbol">(</a><a id="9025" href="Assignment2.html#9025" class="Bound">x</a> <a id="9027" class="Symbol">:</a> <a id="9029" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.html#605" class="Datatype">Dec</a> <a id="9033" href="Assignment2.html#9013" class="Bound">A</a><a id="9034" class="Symbol">)</a> <a id="9036" class="Symbol">(</a><a id="9037" href="Assignment2.html#9037" class="Bound">y</a> <a id="9039" class="Symbol">:</a> <a id="9041" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.html#605" class="Datatype">Dec</a> <a id="9045" href="Assignment2.html#9015" class="Bound">B</a><a id="9046" class="Symbol">)</a> <a id="9048" class="Symbol">→</a> <a id="9050" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.Decidable.Core.html#753" class="Function Operator">⌊</a> <a id="9052" href="Assignment2.html#9025" class="Bound">x</a> <a id="9054" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.Decidable.Core.html#753" class="Function Operator">⌋</a> <a id="9056" href="https://agda.github.io/agda-stdlib/v1.1/Data.Bool.Base.html#1073" class="Function Operator">∨</a> <a id="9058" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.Decidable.Core.html#753" class="Function Operator">⌊</a> <a id="9060" href="Assignment2.html#9037" class="Bound">y</a> <a id="9062" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.Decidable.Core.html#753" class="Function Operator">⌋</a> <a id="9064" href="Agda.Builtin.Equality.html#125" class="Datatype Operator">≡</a> <a id="9066" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.Decidable.Core.html#753" class="Function Operator">⌊</a> <a id="9068" href="Assignment2.html#9025" class="Bound">x</a> <a id="9070" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.Sum.html#668" class="Function Operator">⊎-dec</a> <a id="9076" href="Assignment2.html#9037" class="Bound">y</a> <a id="9078" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.Decidable.Core.html#753" class="Function Operator">⌋</a>
  <a id="not-¬"></a><a id="9082" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#9082" class="Postulate">not-¬</a> <a id="9088" class="Symbol">:</a> <a id="9090" class="Symbol">∀</a> <a id="9092" class="Symbol">{</a><a id="9093" href="Assignment2.html#9093" class="Bound">A</a> <a id="9095" class="Symbol">:</a> <a id="9097" class="PrimitiveType">Set</a><a id="9100" class="Symbol">}</a> <a id="9102" class="Symbol">(</a><a id="9103" href="Assignment2.html#9103" class="Bound">x</a> <a id="9105" class="Symbol">:</a> <a id="9107" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.html#605" class="Datatype">Dec</a> <a id="9111" href="Assignment2.html#9093" class="Bound">A</a><a id="9112" class="Symbol">)</a> <a id="9114" class="Symbol">→</a> <a id="9116" href="https://agda.github.io/agda-stdlib/v1.1/Data.Bool.Base.html#961" class="Function">not</a> <a id="9120" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.Decidable.Core.html#753" class="Function Operator">⌊</a> <a id="9122" href="Assignment2.html#9103" class="Bound">x</a> <a id="9124" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.Decidable.Core.html#753" class="Function Operator">⌋</a> <a id="9126" href="Agda.Builtin.Equality.html#125" class="Datatype Operator">≡</a> <a id="9128" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.Decidable.Core.html#753" class="Function Operator">⌊</a> <a id="9130" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.Negation.html#1115" class="Function">¬?</a> <a id="9133" href="Assignment2.html#9103" class="Bound">x</a> <a id="9135" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.Decidable.Core.html#753" class="Function Operator">⌋</a>
</pre>{% endraw %}
#### Exercise `iff-erasure` (recommended)

Give analogues of the `_⇔_` operation from
Chapter [Isomorphism][plfa.Isomorphism#iff],
operation on booleans and decidables, and also show the corresponding erasure.
{% raw %}<pre class="Agda"><a id="9356" class="Keyword">postulate</a>
  <a id="_iff_"></a><a id="9368" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#9368" class="Postulate Operator">_iff_</a> <a id="9374" class="Symbol">:</a> <a id="9376" href="Agda.Builtin.Bool.html#135" class="Datatype">Bool</a> <a id="9381" class="Symbol">→</a> <a id="9383" href="Agda.Builtin.Bool.html#135" class="Datatype">Bool</a> <a id="9388" class="Symbol">→</a> <a id="9390" href="Agda.Builtin.Bool.html#135" class="Datatype">Bool</a>
  <a id="_⇔-dec_"></a><a id="9397" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#9397" class="Postulate Operator">_⇔-dec_</a> <a id="9405" class="Symbol">:</a> <a id="9407" class="Symbol">∀</a> <a id="9409" class="Symbol">{</a><a id="9410" href="Assignment2.html#9410" class="Bound">A</a> <a id="9412" href="Assignment2.html#9412" class="Bound">B</a> <a id="9414" class="Symbol">:</a> <a id="9416" class="PrimitiveType">Set</a><a id="9419" class="Symbol">}</a> <a id="9421" class="Symbol">→</a> <a id="9423" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.html#605" class="Datatype">Dec</a> <a id="9427" href="Assignment2.html#9410" class="Bound">A</a> <a id="9429" class="Symbol">→</a> <a id="9431" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.html#605" class="Datatype">Dec</a> <a id="9435" href="Assignment2.html#9412" class="Bound">B</a> <a id="9437" class="Symbol">→</a> <a id="9439" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.html#605" class="Datatype">Dec</a> <a id="9443" class="Symbol">(</a><a id="9444" href="Assignment2.html#9410" class="Bound">A</a> <a id="9446" href="Assignment2.html#2780" class="Record Operator">⇔</a> <a id="9448" href="Assignment2.html#9412" class="Bound">B</a><a id="9449" class="Symbol">)</a>
  <a id="iff-⇔"></a><a id="9453" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#9453" class="Postulate">iff-⇔</a> <a id="9459" class="Symbol">:</a> <a id="9461" class="Symbol">∀</a> <a id="9463" class="Symbol">{</a><a id="9464" href="Assignment2.html#9464" class="Bound">A</a> <a id="9466" href="Assignment2.html#9466" class="Bound">B</a> <a id="9468" class="Symbol">:</a> <a id="9470" class="PrimitiveType">Set</a><a id="9473" class="Symbol">}</a> <a id="9475" class="Symbol">(</a><a id="9476" href="Assignment2.html#9476" class="Bound">x</a> <a id="9478" class="Symbol">:</a> <a id="9480" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.html#605" class="Datatype">Dec</a> <a id="9484" href="Assignment2.html#9464" class="Bound">A</a><a id="9485" class="Symbol">)</a> <a id="9487" class="Symbol">(</a><a id="9488" href="Assignment2.html#9488" class="Bound">y</a> <a id="9490" class="Symbol">:</a> <a id="9492" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.html#605" class="Datatype">Dec</a> <a id="9496" href="Assignment2.html#9466" class="Bound">B</a><a id="9497" class="Symbol">)</a> <a id="9499" class="Symbol">→</a> <a id="9501" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.Decidable.Core.html#753" class="Function Operator">⌊</a> <a id="9503" href="Assignment2.html#9476" class="Bound">x</a> <a id="9505" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.Decidable.Core.html#753" class="Function Operator">⌋</a> <a id="9507" href="Assignment2.html#9368" class="Postulate Operator">iff</a> <a id="9511" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.Decidable.Core.html#753" class="Function Operator">⌊</a> <a id="9513" href="Assignment2.html#9488" class="Bound">y</a> <a id="9515" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.Decidable.Core.html#753" class="Function Operator">⌋</a> <a id="9517" href="Agda.Builtin.Equality.html#125" class="Datatype Operator">≡</a> <a id="9519" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.Decidable.Core.html#753" class="Function Operator">⌊</a> <a id="9521" href="Assignment2.html#9476" class="Bound">x</a> <a id="9523" href="Assignment2.html#9397" class="Postulate Operator">⇔-dec</a> <a id="9529" href="Assignment2.html#9488" class="Bound">y</a> <a id="9531" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.Decidable.Core.html#753" class="Function Operator">⌋</a>
</pre>{% endraw %}
## Lists

#### Exercise `reverse-++-commute` (recommended)

Show that the reverse of one list appended to another is the
reverse of the second appended to the reverse of the first.
{% raw %}<pre class="Agda"><a id="9723" class="Keyword">postulate</a>
  <a id="reverse-++-commute"></a><a id="9735" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#9735" class="Postulate">reverse-++-commute</a> <a id="9754" class="Symbol">:</a> <a id="9756" class="Symbol">∀</a> <a id="9758" class="Symbol">{</a><a id="9759" href="Assignment2.html#9759" class="Bound">A</a> <a id="9761" class="Symbol">:</a> <a id="9763" class="PrimitiveType">Set</a><a id="9766" class="Symbol">}</a> <a id="9768" class="Symbol">{</a><a id="9769" href="Assignment2.html#9769" class="Bound">xs</a> <a id="9772" href="Assignment2.html#9772" class="Bound">ys</a> <a id="9775" class="Symbol">:</a> <a id="9777" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#1284" class="Datatype">List</a> <a id="9782" href="Assignment2.html#9759" class="Bound">A</a><a id="9783" class="Symbol">}</a>
    <a id="9789" class="Symbol">→</a> <a id="9791" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#10957" class="Function">reverse</a> <a id="9799" class="Symbol">(</a><a id="9800" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#9769" class="Bound">xs</a> <a id="9803" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="9806" href="Assignment2.html#9772" class="Bound">ys</a><a id="9808" class="Symbol">)</a> <a id="9810" href="Agda.Builtin.Equality.html#125" class="Datatype Operator">≡</a> <a id="9812" href="plfa.Lists.html#10957" class="Function">reverse</a> <a id="9820" href="Assignment2.html#9772" class="Bound">ys</a> <a id="9823" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="9826" href="plfa.Lists.html#10957" class="Function">reverse</a> <a id="9834" href="Assignment2.html#9769" class="Bound">xs</a>
</pre>{% endraw %}
#### Exercise `reverse-involutive` (recommended)

A function is an _involution_ if when applied twice it acts
as the identity function.  Show that reverse is an involution.
{% raw %}<pre class="Agda"><a id="10019" class="Keyword">postulate</a>
  <a id="reverse-involutive"></a><a id="10031" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#10031" class="Postulate">reverse-involutive</a> <a id="10050" class="Symbol">:</a> <a id="10052" class="Symbol">∀</a> <a id="10054" class="Symbol">{</a><a id="10055" href="Assignment2.html#10055" class="Bound">A</a> <a id="10057" class="Symbol">:</a> <a id="10059" class="PrimitiveType">Set</a><a id="10062" class="Symbol">}</a> <a id="10064" class="Symbol">{</a><a id="10065" href="Assignment2.html#10065" class="Bound">xs</a> <a id="10068" class="Symbol">:</a> <a id="10070" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#1284" class="Datatype">List</a> <a id="10075" href="Assignment2.html#10055" class="Bound">A</a><a id="10076" class="Symbol">}</a>
    <a id="10082" class="Symbol">→</a> <a id="10084" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#10957" class="Function">reverse</a> <a id="10092" class="Symbol">(</a><a id="10093" href="plfa.Lists.html#10957" class="Function">reverse</a> <a id="10101" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#10065" class="Bound">xs</a><a id="10103" class="Symbol">)</a> <a id="10105" href="Agda.Builtin.Equality.html#125" class="Datatype Operator">≡</a> <a id="10107" href="Assignment2.html#10065" class="Bound">xs</a>
</pre>{% endraw %}
#### Exercise `map-compose`

Prove that the map of a composition is equal to the composition of two maps.
{% raw %}<pre class="Agda"><a id="10225" class="Keyword">postulate</a>
  <a id="map-compose"></a><a id="10237" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#10237" class="Postulate">map-compose</a> <a id="10249" class="Symbol">:</a> <a id="10251" class="Symbol">∀</a> <a id="10253" class="Symbol">{</a><a id="10254" href="Assignment2.html#10254" class="Bound">A</a> <a id="10256" href="Assignment2.html#10256" class="Bound">B</a> <a id="10258" href="Assignment2.html#10258" class="Bound">C</a> <a id="10260" class="Symbol">:</a> <a id="10262" class="PrimitiveType">Set</a><a id="10265" class="Symbol">}</a> <a id="10267" class="Symbol">{</a><a id="10268" href="Assignment2.html#10268" class="Bound">f</a> <a id="10270" class="Symbol">:</a> <a id="10272" href="Assignment2.html#10254" class="Bound">A</a> <a id="10274" class="Symbol">→</a> <a id="10276" href="Assignment2.html#10256" class="Bound">B</a><a id="10277" class="Symbol">}</a> <a id="10279" class="Symbol">{</a><a id="10280" href="Assignment2.html#10280" class="Bound">g</a> <a id="10282" class="Symbol">:</a> <a id="10284" href="Assignment2.html#10256" class="Bound">B</a> <a id="10286" class="Symbol">→</a> <a id="10288" href="Assignment2.html#10258" class="Bound">C</a><a id="10289" class="Symbol">}</a>
    <a id="10295" class="Symbol">→</a> <a id="10297" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#16959" class="Function">map</a> <a id="10301" class="Symbol">(</a><a id="10302" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#10280" class="Bound">g</a> <a id="10304" href="https://agda.github.io/agda-stdlib/v1.1/Function.html#1099" class="Function Operator">∘</a> <a id="10306" href="Assignment2.html#10268" class="Bound">f</a><a id="10307" class="Symbol">)</a> <a id="10309" href="Agda.Builtin.Equality.html#125" class="Datatype Operator">≡</a> <a id="10311" href="plfa.Lists.html#16959" class="Function">map</a> <a id="10315" href="Assignment2.html#10280" class="Bound">g</a> <a id="10317" href="https://agda.github.io/agda-stdlib/v1.1/Function.html#1099" class="Function Operator">∘</a> <a id="10319" href="plfa.Lists.html#16959" class="Function">map</a> <a id="10323" href="Assignment2.html#10268" class="Bound">f</a>
</pre>{% endraw %}The last step of the proof requires extensionality.

#### Exercise `map-++-commute`

Prove the following relationship between map and append.
{% raw %}<pre class="Agda"><a id="10475" class="Keyword">postulate</a>
  <a id="map-++-commute"></a><a id="10487" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#10487" class="Postulate">map-++-commute</a> <a id="10502" class="Symbol">:</a> <a id="10504" class="Symbol">∀</a> <a id="10506" class="Symbol">{</a><a id="10507" href="Assignment2.html#10507" class="Bound">A</a> <a id="10509" href="Assignment2.html#10509" class="Bound">B</a> <a id="10511" class="Symbol">:</a> <a id="10513" class="PrimitiveType">Set</a><a id="10516" class="Symbol">}</a> <a id="10518" class="Symbol">{</a><a id="10519" href="Assignment2.html#10519" class="Bound">f</a> <a id="10521" class="Symbol">:</a> <a id="10523" href="Assignment2.html#10507" class="Bound">A</a> <a id="10525" class="Symbol">→</a> <a id="10527" href="Assignment2.html#10509" class="Bound">B</a><a id="10528" class="Symbol">}</a> <a id="10530" class="Symbol">{</a><a id="10531" href="Assignment2.html#10531" class="Bound">xs</a> <a id="10534" href="Assignment2.html#10534" class="Bound">ys</a> <a id="10537" class="Symbol">:</a> <a id="10539" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#1284" class="Datatype">List</a> <a id="10544" href="Assignment2.html#10507" class="Bound">A</a><a id="10545" class="Symbol">}</a>
   <a id="10550" class="Symbol">→</a>  <a id="10553" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#16959" class="Function">map</a> <a id="10557" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#10519" class="Bound">f</a> <a id="10559" class="Symbol">(</a><a id="10560" href="Assignment2.html#10531" class="Bound">xs</a> <a id="10563" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="10566" href="Assignment2.html#10534" class="Bound">ys</a><a id="10568" class="Symbol">)</a> <a id="10570" href="Agda.Builtin.Equality.html#125" class="Datatype Operator">≡</a> <a id="10572" href="plfa.Lists.html#16959" class="Function">map</a> <a id="10576" href="Assignment2.html#10519" class="Bound">f</a> <a id="10578" href="Assignment2.html#10531" class="Bound">xs</a> <a id="10581" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="10584" href="plfa.Lists.html#16959" class="Function">map</a> <a id="10588" href="Assignment2.html#10519" class="Bound">f</a> <a id="10590" href="Assignment2.html#10534" class="Bound">ys</a>
</pre>{% endraw %}
#### Exercise `map-Tree`

Define a type of trees with leaves of type `A` and internal
nodes of type `B`.
{% raw %}<pre class="Agda"><a id="10707" class="Keyword">data</a> <a id="Tree"></a><a id="10712" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#10712" class="Datatype">Tree</a> <a id="10717" class="Symbol">(</a><a id="10718" href="Assignment2.html#10718" class="Bound">A</a> <a id="10720" href="Assignment2.html#10720" class="Bound">B</a> <a id="10722" class="Symbol">:</a> <a id="10724" class="PrimitiveType">Set</a><a id="10727" class="Symbol">)</a> <a id="10729" class="Symbol">:</a> <a id="10731" class="PrimitiveType">Set</a> <a id="10735" class="Keyword">where</a>
  <a id="Tree.leaf"></a><a id="10743" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#10743" class="InductiveConstructor">leaf</a> <a id="10748" class="Symbol">:</a> <a id="10750" href="Assignment2.html#10718" class="Bound">A</a> <a id="10752" class="Symbol">→</a> <a id="10754" href="Assignment2.html#10712" class="Datatype">Tree</a> <a id="10759" href="Assignment2.html#10718" class="Bound">A</a> <a id="10761" href="Assignment2.html#10720" class="Bound">B</a>
  <a id="Tree.node"></a><a id="10765" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#10765" class="InductiveConstructor">node</a> <a id="10770" class="Symbol">:</a> <a id="10772" href="Assignment2.html#10712" class="Datatype">Tree</a> <a id="10777" href="Assignment2.html#10718" class="Bound">A</a> <a id="10779" href="Assignment2.html#10720" class="Bound">B</a> <a id="10781" class="Symbol">→</a> <a id="10783" href="Assignment2.html#10720" class="Bound">B</a> <a id="10785" class="Symbol">→</a> <a id="10787" href="Assignment2.html#10712" class="Datatype">Tree</a> <a id="10792" href="Assignment2.html#10718" class="Bound">A</a> <a id="10794" href="Assignment2.html#10720" class="Bound">B</a> <a id="10796" class="Symbol">→</a> <a id="10798" href="Assignment2.html#10712" class="Datatype">Tree</a> <a id="10803" href="Assignment2.html#10718" class="Bound">A</a> <a id="10805" href="Assignment2.html#10720" class="Bound">B</a>
</pre>{% endraw %}Define a suitable map operator over trees.
{% raw %}<pre class="Agda"><a id="10858" class="Keyword">postulate</a>
  <a id="map-Tree"></a><a id="10870" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#10870" class="Postulate">map-Tree</a> <a id="10879" class="Symbol">:</a> <a id="10881" class="Symbol">∀</a> <a id="10883" class="Symbol">{</a><a id="10884" href="Assignment2.html#10884" class="Bound">A</a> <a id="10886" href="Assignment2.html#10886" class="Bound">B</a> <a id="10888" href="Assignment2.html#10888" class="Bound">C</a> <a id="10890" href="Assignment2.html#10890" class="Bound">D</a> <a id="10892" class="Symbol">:</a> <a id="10894" class="PrimitiveType">Set</a><a id="10897" class="Symbol">}</a>
    <a id="10903" class="Symbol">→</a> <a id="10905" class="Symbol">(</a><a id="10906" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#10884" class="Bound">A</a> <a id="10908" class="Symbol">→</a> <a id="10910" href="Assignment2.html#10888" class="Bound">C</a><a id="10911" class="Symbol">)</a> <a id="10913" class="Symbol">→</a> <a id="10915" class="Symbol">(</a><a id="10916" href="Assignment2.html#10886" class="Bound">B</a> <a id="10918" class="Symbol">→</a> <a id="10920" href="Assignment2.html#10890" class="Bound">D</a><a id="10921" class="Symbol">)</a> <a id="10923" class="Symbol">→</a> <a id="10925" href="Assignment2.html#10712" class="Datatype">Tree</a> <a id="10930" href="Assignment2.html#10884" class="Bound">A</a> <a id="10932" href="Assignment2.html#10886" class="Bound">B</a> <a id="10934" class="Symbol">→</a> <a id="10936" href="Assignment2.html#10712" class="Datatype">Tree</a> <a id="10941" href="Assignment2.html#10888" class="Bound">C</a> <a id="10943" href="Assignment2.html#10890" class="Bound">D</a>
</pre>{% endraw %}
#### Exercise `product` (recommended)

Use fold to define a function to find the product of a list of numbers.
For example,

    product [ 1 , 2 , 3 , 4 ] ≡ 24

#### Exercise `foldr-++` (recommended)

Show that fold and append are related as follows.
{% raw %}<pre class="Agda"><a id="11205" class="Keyword">postulate</a>
  <a id="foldr-++"></a><a id="11217" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#11217" class="Postulate">foldr-++</a> <a id="11226" class="Symbol">:</a> <a id="11228" class="Symbol">∀</a> <a id="11230" class="Symbol">{</a><a id="11231" href="Assignment2.html#11231" class="Bound">A</a> <a id="11233" href="Assignment2.html#11233" class="Bound">B</a> <a id="11235" class="Symbol">:</a> <a id="11237" class="PrimitiveType">Set</a><a id="11240" class="Symbol">}</a> <a id="11242" class="Symbol">(</a><a id="11243" href="Assignment2.html#11243" class="Bound Operator">_⊗_</a> <a id="11247" class="Symbol">:</a> <a id="11249" href="Assignment2.html#11231" class="Bound">A</a> <a id="11251" class="Symbol">→</a> <a id="11253" href="Assignment2.html#11233" class="Bound">B</a> <a id="11255" class="Symbol">→</a> <a id="11257" href="Assignment2.html#11233" class="Bound">B</a><a id="11258" class="Symbol">)</a> <a id="11260" class="Symbol">(</a><a id="11261" href="Assignment2.html#11261" class="Bound">e</a> <a id="11263" class="Symbol">:</a> <a id="11265" href="Assignment2.html#11233" class="Bound">B</a><a id="11266" class="Symbol">)</a> <a id="11268" class="Symbol">(</a><a id="11269" href="Assignment2.html#11269" class="Bound">xs</a> <a id="11272" href="Assignment2.html#11272" class="Bound">ys</a> <a id="11275" class="Symbol">:</a> <a id="11277" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#1284" class="Datatype">List</a> <a id="11282" href="Assignment2.html#11231" class="Bound">A</a><a id="11283" class="Symbol">)</a> <a id="11285" class="Symbol">→</a>
    <a id="11291" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#20225" class="Function">foldr</a> <a id="11297" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#11243" class="Bound Operator">_⊗_</a> <a id="11301" href="Assignment2.html#11261" class="Bound">e</a> <a id="11303" class="Symbol">(</a><a id="11304" href="Assignment2.html#11269" class="Bound">xs</a> <a id="11307" href="plfa.Lists.html#4651" class="Function Operator">++</a> <a id="11310" href="Assignment2.html#11272" class="Bound">ys</a><a id="11312" class="Symbol">)</a> <a id="11314" href="Agda.Builtin.Equality.html#125" class="Datatype Operator">≡</a> <a id="11316" href="plfa.Lists.html#20225" class="Function">foldr</a> <a id="11322" href="Assignment2.html#11243" class="Bound Operator">_⊗_</a> <a id="11326" class="Symbol">(</a><a id="11327" href="plfa.Lists.html#20225" class="Function">foldr</a> <a id="11333" href="Assignment2.html#11243" class="Bound Operator">_⊗_</a> <a id="11337" href="Assignment2.html#11261" class="Bound">e</a> <a id="11339" href="Assignment2.html#11272" class="Bound">ys</a><a id="11341" class="Symbol">)</a> <a id="11343" href="Assignment2.html#11269" class="Bound">xs</a>
</pre>{% endraw %}

#### Exercise `map-is-foldr`

Show that map can be defined using fold.
{% raw %}<pre class="Agda"><a id="11427" class="Keyword">postulate</a>
  <a id="map-is-foldr"></a><a id="11439" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#11439" class="Postulate">map-is-foldr</a> <a id="11452" class="Symbol">:</a> <a id="11454" class="Symbol">∀</a> <a id="11456" class="Symbol">{</a><a id="11457" href="Assignment2.html#11457" class="Bound">A</a> <a id="11459" href="Assignment2.html#11459" class="Bound">B</a> <a id="11461" class="Symbol">:</a> <a id="11463" class="PrimitiveType">Set</a><a id="11466" class="Symbol">}</a> <a id="11468" class="Symbol">{</a><a id="11469" href="Assignment2.html#11469" class="Bound">f</a> <a id="11471" class="Symbol">:</a> <a id="11473" href="Assignment2.html#11457" class="Bound">A</a> <a id="11475" class="Symbol">→</a> <a id="11477" href="Assignment2.html#11459" class="Bound">B</a><a id="11478" class="Symbol">}</a> <a id="11480" class="Symbol">→</a>
    <a id="11486" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#16959" class="Function">map</a> <a id="11490" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#11469" class="Bound">f</a> <a id="11492" href="Agda.Builtin.Equality.html#125" class="Datatype Operator">≡</a> <a id="11494" href="plfa.Lists.html#20225" class="Function">foldr</a> <a id="11500" class="Symbol">(λ</a> <a id="11503" href="Assignment2.html#11503" class="Bound">x</a> <a id="11505" href="Assignment2.html#11505" class="Bound">xs</a> <a id="11508" class="Symbol">→</a> <a id="11510" href="Assignment2.html#11469" class="Bound">f</a> <a id="11512" href="Assignment2.html#11503" class="Bound">x</a> <a id="11514" href="plfa.Lists.html#1328" class="InductiveConstructor Operator">∷</a> <a id="11516" href="Assignment2.html#11505" class="Bound">xs</a><a id="11518" class="Symbol">)</a> <a id="11520" href="plfa.Lists.html#1313" class="InductiveConstructor">[]</a>
</pre>{% endraw %}This requires extensionality.

#### Exercise `fold-Tree`

Define a suitable fold function for the type of trees given earlier.
{% raw %}<pre class="Agda"><a id="11658" class="Keyword">postulate</a>
  <a id="fold-Tree"></a><a id="11670" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#11670" class="Postulate">fold-Tree</a> <a id="11680" class="Symbol">:</a> <a id="11682" class="Symbol">∀</a> <a id="11684" class="Symbol">{</a><a id="11685" href="Assignment2.html#11685" class="Bound">A</a> <a id="11687" href="Assignment2.html#11687" class="Bound">B</a> <a id="11689" href="Assignment2.html#11689" class="Bound">C</a> <a id="11691" class="Symbol">:</a> <a id="11693" class="PrimitiveType">Set</a><a id="11696" class="Symbol">}</a>
    <a id="11702" class="Symbol">→</a> <a id="11704" class="Symbol">(</a><a id="11705" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#11685" class="Bound">A</a> <a id="11707" class="Symbol">→</a> <a id="11709" href="Assignment2.html#11689" class="Bound">C</a><a id="11710" class="Symbol">)</a> <a id="11712" class="Symbol">→</a> <a id="11714" class="Symbol">(</a><a id="11715" href="Assignment2.html#11689" class="Bound">C</a> <a id="11717" class="Symbol">→</a> <a id="11719" href="Assignment2.html#11687" class="Bound">B</a> <a id="11721" class="Symbol">→</a> <a id="11723" href="Assignment2.html#11689" class="Bound">C</a> <a id="11725" class="Symbol">→</a> <a id="11727" href="Assignment2.html#11689" class="Bound">C</a><a id="11728" class="Symbol">)</a> <a id="11730" class="Symbol">→</a> <a id="11732" href="Assignment2.html#10712" class="Datatype">Tree</a> <a id="11737" href="Assignment2.html#11685" class="Bound">A</a> <a id="11739" href="Assignment2.html#11687" class="Bound">B</a> <a id="11741" class="Symbol">→</a> <a id="11743" href="Assignment2.html#11689" class="Bound">C</a>
</pre>{% endraw %}
#### Exercise `map-is-fold-Tree`

Demonstrate an analogue of `map-is-foldr` for the type of trees.

#### Exercise `sum-downFrom` (stretch)

Define a function that counts down as follows.
{% raw %}<pre class="Agda"><a id="downFrom"></a><a id="11941" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#11941" class="Function">downFrom</a> <a id="11950" class="Symbol">:</a> <a id="11952" href="Agda.Builtin.Nat.html#165" class="Datatype">ℕ</a> <a id="11954" class="Symbol">→</a> <a id="11956" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#1284" class="Datatype">List</a> <a id="11961" href="Agda.Builtin.Nat.html#165" class="Datatype">ℕ</a>
<a id="11963" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#11941" class="Function">downFrom</a> <a id="11972" href="Agda.Builtin.Nat.html#183" class="InductiveConstructor">zero</a>     <a id="11981" class="Symbol">=</a>  <a id="11984" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#1313" class="InductiveConstructor">[]</a>
<a id="11987" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#11941" class="Function">downFrom</a> <a id="11996" class="Symbol">(</a><a id="11997" href="Agda.Builtin.Nat.html#196" class="InductiveConstructor">suc</a> <a id="12001" href="Assignment2.html#12001" class="Bound">n</a><a id="12002" class="Symbol">)</a>  <a id="12005" class="Symbol">=</a>  <a id="12008" href="Assignment2.html#12001" class="Bound">n</a> <a id="12010" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#1328" class="InductiveConstructor Operator">∷</a> <a id="12012" href="Assignment2.html#11941" class="Function">downFrom</a> <a id="12021" href="Assignment2.html#12001" class="Bound">n</a>
</pre>{% endraw %}For example,
{% raw %}<pre class="Agda"><a id="12044" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#12044" class="Function">_</a> <a id="12046" class="Symbol">:</a> <a id="12048" href="Assignment2.html#11941" class="Function">downFrom</a> <a id="12057" class="Number">3</a> <a id="12059" href="Agda.Builtin.Equality.html#125" class="Datatype Operator">≡</a> <a id="12061" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#3872" class="InductiveConstructor Operator">[</a> <a id="12063" class="Number">2</a> <a id="12065" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">,</a> <a id="12067" class="Number">1</a> <a id="12069" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">,</a> <a id="12071" class="Number">0</a> <a id="12073" href="plfa.Lists.html#3872" class="InductiveConstructor Operator">]</a>
<a id="12075" class="Symbol">_</a> <a id="12077" class="Symbol">=</a> <a id="12079" href="Agda.Builtin.Equality.html#182" class="InductiveConstructor">refl</a>
</pre>{% endraw %}Prove that the sum of the numbers `(n - 1) + ⋯ + 0` is
equal to `n * (n ∸ 1) / 2`.
{% raw %}<pre class="Agda"><a id="12175" class="Keyword">postulate</a>
  <a id="sum-downFrom"></a><a id="12187" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#12187" class="Postulate">sum-downFrom</a> <a id="12200" class="Symbol">:</a> <a id="12202" class="Symbol">∀</a> <a id="12204" class="Symbol">(</a><a id="12205" href="Assignment2.html#12205" class="Bound">n</a> <a id="12207" class="Symbol">:</a> <a id="12209" href="Agda.Builtin.Nat.html#165" class="Datatype">ℕ</a><a id="12210" class="Symbol">)</a>
    <a id="12216" class="Symbol">→</a> <a id="12218" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#21337" class="Function">sum</a> <a id="12222" class="Symbol">(</a><a id="12223" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#11941" class="Function">downFrom</a> <a id="12232" href="Assignment2.html#12205" class="Bound">n</a><a id="12233" class="Symbol">)</a> <a id="12235" href="Agda.Builtin.Nat.html#501" class="Primitive Operator">*</a> <a id="12237" class="Number">2</a> <a id="12239" href="Agda.Builtin.Equality.html#125" class="Datatype Operator">≡</a> <a id="12241" href="Assignment2.html#12205" class="Bound">n</a> <a id="12243" href="Agda.Builtin.Nat.html#501" class="Primitive Operator">*</a> <a id="12245" class="Symbol">(</a><a id="12246" href="Assignment2.html#12205" class="Bound">n</a> <a id="12248" href="Agda.Builtin.Nat.html#388" class="Primitive Operator">∸</a> <a id="12250" class="Number">1</a><a id="12251" class="Symbol">)</a>
</pre>{% endraw %}

#### Exercise `foldl`

Define a function `foldl` which is analogous to `foldr`, but where
operations associate to the left rather than the right.  For example,

    foldr _⊗_ e [ x , y , z ]  =  x ⊗ (y ⊗ (z ⊗ e))
    foldl _⊗_ e [ x , y , z ]  =  ((e ⊗ x) ⊗ y) ⊗ z


#### Exercise `foldr-monoid-foldl`

Show that if `_⊕_` and `e` form a monoid, then `foldr _⊗_ e` and
`foldl _⊗_ e` always compute the same result.


#### Exercise `Any-++-⇔` (recommended)

Prove a result similar to `All-++-⇔`, but with `Any` in place of `All`, and a suitable
replacement for `_×_`.  As a consequence, demonstrate an equivalence relating
`_∈_` and `_++_`.


#### Exercise `All-++-≃` (stretch)

Show that the equivalence `All-++-⇔` can be extended to an isomorphism.


#### Exercise `¬Any≃All¬` (stretch)

First generalise composition to arbitrary levels, using
[universe polymorphism][plfa.Equality#unipoly].
{% raw %}<pre class="Agda"><a id="_∘′_"></a><a id="13155" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#13155" class="Function Operator">_∘′_</a> <a id="13160" class="Symbol">:</a> <a id="13162" class="Symbol">∀</a> <a id="13164" class="Symbol">{</a><a id="13165" href="Assignment2.html#13165" class="Bound">ℓ₁</a> <a id="13168" href="Assignment2.html#13168" class="Bound">ℓ₂</a> <a id="13171" href="Assignment2.html#13171" class="Bound">ℓ₃</a> <a id="13174" class="Symbol">:</a> <a id="13176" href="Agda.Primitive.html#408" class="Postulate">Level</a><a id="13181" class="Symbol">}</a> <a id="13183" class="Symbol">{</a><a id="13184" href="Assignment2.html#13184" class="Bound">A</a> <a id="13186" class="Symbol">:</a> <a id="13188" class="PrimitiveType">Set</a> <a id="13192" href="Assignment2.html#13165" class="Bound">ℓ₁</a><a id="13194" class="Symbol">}</a> <a id="13196" class="Symbol">{</a><a id="13197" href="Assignment2.html#13197" class="Bound">B</a> <a id="13199" class="Symbol">:</a> <a id="13201" class="PrimitiveType">Set</a> <a id="13205" href="Assignment2.html#13168" class="Bound">ℓ₂</a><a id="13207" class="Symbol">}</a> <a id="13209" class="Symbol">{</a><a id="13210" href="Assignment2.html#13210" class="Bound">C</a> <a id="13212" class="Symbol">:</a> <a id="13214" class="PrimitiveType">Set</a> <a id="13218" href="Assignment2.html#13171" class="Bound">ℓ₃</a><a id="13220" class="Symbol">}</a>
  <a id="13224" class="Symbol">→</a> <a id="13226" class="Symbol">(</a><a id="13227" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#13197" class="Bound">B</a> <a id="13229" class="Symbol">→</a> <a id="13231" href="Assignment2.html#13210" class="Bound">C</a><a id="13232" class="Symbol">)</a> <a id="13234" class="Symbol">→</a> <a id="13236" class="Symbol">(</a><a id="13237" href="Assignment2.html#13184" class="Bound">A</a> <a id="13239" class="Symbol">→</a> <a id="13241" href="Assignment2.html#13197" class="Bound">B</a><a id="13242" class="Symbol">)</a> <a id="13244" class="Symbol">→</a> <a id="13246" href="Assignment2.html#13184" class="Bound">A</a> <a id="13248" class="Symbol">→</a> <a id="13250" href="Assignment2.html#13210" class="Bound">C</a>
<a id="13252" class="Symbol">(</a><a id="13253" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#13253" class="Bound">g</a> <a id="13255" href="Assignment2.html#13155" class="Function Operator">∘′</a> <a id="13258" href="Assignment2.html#13258" class="Bound">f</a><a id="13259" class="Symbol">)</a> <a id="13261" href="Assignment2.html#13261" class="Bound">x</a>  <a id="13264" class="Symbol">=</a>  <a id="13267" href="Assignment2.html#13253" class="Bound">g</a> <a id="13269" class="Symbol">(</a><a id="13270" href="Assignment2.html#13258" class="Bound">f</a> <a id="13272" href="Assignment2.html#13261" class="Bound">x</a><a id="13273" class="Symbol">)</a>
</pre>{% endraw %}
Show that `Any` and `All` satisfy a version of De Morgan's Law.
{% raw %}<pre class="Agda"><a id="13348" class="Keyword">postulate</a>
  <a id="¬Any≃All¬"></a><a id="13360" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#13360" class="Postulate">¬Any≃All¬</a> <a id="13370" class="Symbol">:</a> <a id="13372" class="Symbol">∀</a> <a id="13374" class="Symbol">{</a><a id="13375" href="Assignment2.html#13375" class="Bound">A</a> <a id="13377" class="Symbol">:</a> <a id="13379" class="PrimitiveType">Set</a><a id="13382" class="Symbol">}</a> <a id="13384" class="Symbol">(</a><a id="13385" href="Assignment2.html#13385" class="Bound">P</a> <a id="13387" class="Symbol">:</a> <a id="13389" href="Assignment2.html#13375" class="Bound">A</a> <a id="13391" class="Symbol">→</a> <a id="13393" class="PrimitiveType">Set</a><a id="13396" class="Symbol">)</a> <a id="13398" class="Symbol">(</a><a id="13399" href="Assignment2.html#13399" class="Bound">xs</a> <a id="13402" class="Symbol">:</a> <a id="13404" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#1284" class="Datatype">List</a> <a id="13409" href="Assignment2.html#13375" class="Bound">A</a><a id="13410" class="Symbol">)</a>
    <a id="13416" class="Symbol">→</a> <a id="13418" class="Symbol">(</a><a id="13419" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.html#535" class="Function Operator">¬_</a> <a id="13422" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#13155" class="Function Operator">∘′</a> <a id="13425" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#29796" class="Datatype">Any</a> <a id="13429" href="Assignment2.html#13385" class="Bound">P</a><a id="13430" class="Symbol">)</a> <a id="13432" href="Assignment2.html#13399" class="Bound">xs</a> <a id="13435" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Isomorphism.md %}{% raw %}#5843" class="Record Operator">≃</a> <a id="13437" href="plfa.Lists.html#27650" class="Datatype">All</a> <a id="13441" class="Symbol">(</a><a id="13442" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.html#535" class="Function Operator">¬_</a> <a id="13445" href="Assignment2.html#13155" class="Function Operator">∘′</a> <a id="13448" href="Assignment2.html#13385" class="Bound">P</a><a id="13449" class="Symbol">)</a> <a id="13451" href="Assignment2.html#13399" class="Bound">xs</a>
</pre>{% endraw %}
Do we also have the following?
{% raw %}<pre class="Agda"><a id="13494" class="Keyword">postulate</a>
  <a id="¬All≃Any¬"></a><a id="13506" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#13506" class="Postulate">¬All≃Any¬</a> <a id="13516" class="Symbol">:</a> <a id="13518" class="Symbol">∀</a> <a id="13520" class="Symbol">{</a><a id="13521" href="Assignment2.html#13521" class="Bound">A</a> <a id="13523" class="Symbol">:</a> <a id="13525" class="PrimitiveType">Set</a><a id="13528" class="Symbol">}</a> <a id="13530" class="Symbol">(</a><a id="13531" href="Assignment2.html#13531" class="Bound">P</a> <a id="13533" class="Symbol">:</a> <a id="13535" href="Assignment2.html#13521" class="Bound">A</a> <a id="13537" class="Symbol">→</a> <a id="13539" class="PrimitiveType">Set</a><a id="13542" class="Symbol">)</a> <a id="13544" class="Symbol">(</a><a id="13545" href="Assignment2.html#13545" class="Bound">xs</a> <a id="13548" class="Symbol">:</a> <a id="13550" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#1284" class="Datatype">List</a> <a id="13555" href="Assignment2.html#13521" class="Bound">A</a><a id="13556" class="Symbol">)</a>
    <a id="13562" class="Symbol">→</a> <a id="13564" class="Symbol">(</a><a id="13565" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.html#535" class="Function Operator">¬_</a> <a id="13568" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#13155" class="Function Operator">∘′</a> <a id="13571" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#27650" class="Datatype">All</a> <a id="13575" href="Assignment2.html#13531" class="Bound">P</a><a id="13576" class="Symbol">)</a> <a id="13578" href="Assignment2.html#13545" class="Bound">xs</a> <a id="13581" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Isomorphism.md %}{% raw %}#5843" class="Record Operator">≃</a> <a id="13583" href="plfa.Lists.html#29796" class="Datatype">Any</a> <a id="13587" class="Symbol">(</a><a id="13588" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.html#535" class="Function Operator">¬_</a> <a id="13591" href="Assignment2.html#13155" class="Function Operator">∘′</a> <a id="13594" href="Assignment2.html#13531" class="Bound">P</a><a id="13595" class="Symbol">)</a> <a id="13597" href="Assignment2.html#13545" class="Bound">xs</a>
</pre>{% endraw %}If so, prove; if not, explain why.


#### Exercise `any?` (stretch)

Just as `All` has analogues `all` and `all?` which determine whether a
predicate holds for every element of a list, so does `Any` have
analogues `any` and `any?` which determine whether a predicates holds
for some element of a list.  Give their definitions.


#### Exercise `filter?` (stretch)

Define the following variant of the traditional `filter` function on lists,
which given a list and a decidable predicate returns all elements of the
list satisfying the predicate.
{% raw %}<pre class="Agda"><a id="14152" class="Keyword">postulate</a>
  <a id="filter?"></a><a id="14164" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#14164" class="Postulate">filter?</a> <a id="14172" class="Symbol">:</a> <a id="14174" class="Symbol">∀</a> <a id="14176" class="Symbol">{</a><a id="14177" href="Assignment2.html#14177" class="Bound">A</a> <a id="14179" class="Symbol">:</a> <a id="14181" class="PrimitiveType">Set</a><a id="14184" class="Symbol">}</a> <a id="14186" class="Symbol">{</a><a id="14187" href="Assignment2.html#14187" class="Bound">P</a> <a id="14189" class="Symbol">:</a> <a id="14191" href="Assignment2.html#14177" class="Bound">A</a> <a id="14193" class="Symbol">→</a> <a id="14195" class="PrimitiveType">Set</a><a id="14198" class="Symbol">}</a>
    <a id="14204" class="Symbol">→</a> <a id="14206" class="Symbol">(</a><a id="14207" href="{% endraw %}{{ site.baseurl }}{% link out/puc/2019/Assignment2.md %}{% raw %}#14207" class="Bound">P?</a> <a id="14210" class="Symbol">:</a> <a id="14212" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Unary.html#3474" class="Function">Decidable</a> <a id="14222" href="Assignment2.html#14187" class="Bound">P</a><a id="14223" class="Symbol">)</a> <a id="14225" class="Symbol">→</a> <a id="14227" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Lists.md %}{% raw %}#1284" class="Datatype">List</a> <a id="14232" href="Assignment2.html#14177" class="Bound">A</a> <a id="14234" class="Symbol">→</a> <a id="14236" href="https://agda.github.io/agda-stdlib/v1.1/Data.Product.html#1783" class="Function">∃[</a> <a id="14239" href="Assignment2.html#14239" class="Bound">ys</a> <a id="14242" href="https://agda.github.io/agda-stdlib/v1.1/Data.Product.html#1783" class="Function">]</a><a id="14243" class="Symbol">(</a> <a id="14245" href="plfa.Lists.html#27650" class="Datatype">All</a> <a id="14249" href="Assignment2.html#14187" class="Bound">P</a> <a id="14251" href="Assignment2.html#14239" class="Bound">ys</a> <a id="14254" class="Symbol">)</a>
</pre>{% endraw %}
