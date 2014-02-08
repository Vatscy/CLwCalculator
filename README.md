# CLwCalculator

組み合わせ論理(combinatory logic)の体系 **CLw** におけるCL−項の正規化を行います。

# 使い方

Javaアプリケーションです。  
`javac`コマンドでコンパイル後、classファイルを`java`コマンドで起動します。  

CL-項を入力するとw-正規形が出力されます。

`exit`と入力することで終了します。

# 体系CLwの定義

## 基本記号

**変数** : `x`, `y`, `z`... (英小文字1字で表現)

**定数** : `K`, `S`

## CL-項

1. 変数と定数はCL-項 【原始】
2. `M`と`N`が共にCL-項ならば、`(MN)`もCL-項 【適用】

### (例)

`x`, `K`, `(xy)`, `(K(*xy*))`, `(((S(*xy*))*z*)(K(*xy*)))` は全てCL-項

## 括弧の省略

括弧は、**左から順に省略**することができます。

```
(((S(xy))z)(K(xy))) → ((S(xy))z)(K(xy))  
→ (S(xy))z(K(xy)) → S(xy)z(K(xy)) ※これ以上は省略できない
```

括弧を補う場合も左から順に付けていきます。

```
Sxyz → (Sx)yz → ((Sx)y)z → (((Sx)y)z)
```

## 定数の拡張

基本定数`S`, `K`以外にも、以下のような定数があります。  
全て`S`と`K`で表現し直すことができます。

```
【I】 I ≡ SKK
【B】 B ≡ S(KS)K
【C】 C ≡ S(BBS)(KK)
【W】 W ≡ SS(KI)
```

## 式

`M`と`N`がCL-項の時、下記3つの表現をCLwの**式**という。

```
M |>1 N
M |> N
M = N
```

## 公理型

**公理**とは、常に「正しい」と評価される式のことです。  
算数でいうと、`1 = 1`は正しいので公理ですが、`1 = 2`は正しくないので公理ではありません。

公理の型を**公理型**といいます。  
CLwの公理型は以下の3つです。`M`, `N`, `R`はCL−項とします。

```
【K】 KMN |>1 M
【S】 SMNR |>1 MR(NR)
【ρ】 M |> M
```

## weak変形

公理型を用いて、左辺のCL-項を右辺のCL-項に変形させることを**weak変形**といいます。  
例えば、`Kxy`に公理型【K】を適用すると、`Kxy |>1 x`となります。  
すなわち、`Kxy`をweak変形させると`x`になります。

weak変形はCL-項の一部分に適用させることもできます。  
例えば、`Kxyz`の`Kxy`の部分に公理型【K】を適用し、`x`に変形します。  
残った`z`をくっつけると、`xz`になります。  
すなわち、`Kxyz`をweak変形させると`xz`になります。

ただし、一部分に適用させる場合は、注意が必要です。  
例えば、`xKyz`の`Kyz`には、**公理型【K】は適用できません。**  
なぜなら、`xKyz`に括弧を補うと`((xK)y)z)`となり、`Kyz`は同じ固まりではないからです。

## 正規化

weak変形を繰り返し、これ以上変形できない形(**w-正規形**)まで変形することを**正規化**といいます。

### (例)

```
S(Kx)yz |>1 Kxz(yz) |>1 x(yz)
```
1回目の変形では公理型【S】を適用しています。2回目の変形では公理型【K】を適用しています。  
これはこれ以上変形できないため、`S(Kx)yz`のw-正規形は`x(yz)`となります。
