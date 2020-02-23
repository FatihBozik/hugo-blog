---
title: "Java'da Depolanan Veriler Nerede Saklanır?"
date: 2016-03-09T00:53:39+03:00
draft: false
author: "Fatih Bozik"
authorLink: "https://fatihbozik.github.io/"
description: "Lorem Ipsum Dolor Si Amet"
license: ""

tags: ['java', 'heap', 'stack']
categories: ['java']
hiddenFromHomePage: false

featuredImage: ""
featuredImagePreview: ""

toc: false
autoCollapseToc: true
math: false
comment: true
---

Java'da **primitif** ve **referans** değişkeni olmak üzere 2 tip değişken vardır.<!--more-->

## Primitif Değişkenler
Primitif değişkenler değerlerini kendi üzerlerinde taşırlar. Bunlar 8 tipten biri olabilir.

| Veri tipleri    | Java dilinde karşılıkları      | 
| --------------- | -------------------------------|
| Tam sayılar     | byte, short, int, long         |
| Kesirli sayılar | float, double                  |
| Karakter        | char                           |
| Mantıksal       | boolean                        |

## Referans Değişkeni
Referans değişkeni bir nesneyi işaret eder (C/C++ programcıları bunları C/C++'daki **pointer**lara benzetebilir). Referans değişkenlerine örnek olarak herhangi bir sınıf tipindeki değişkenleri verebiliriz. **Örneğin:** `Date date`, `String str`.

Primitif değişkenler ve referans değişkenleri şu durumlarda olabilirler.

* **class variable :** Sınıfa ait değişken,
* **instance variable :** Nesneye ait değişken,
* **local variable** : Metodun içinde tanımlanan değişken (metod parametreleri de dahil).

## Veriler Nerede Saklanır?

* Tüm nesneler **heap** alanında depolanır. Örnek değişkenler(instance variables) de nesneye ait olduklarından **heap** alanında depolanır.

* Tüm local değişkenler **yığın (stack)** da depolanır.

* Sınıf değişkenleri (yani static tanımlanmış değişkenler) Java 7'de heap bölgesi içinde yer alan **PermGen (Permanent Generation)** adı verilen özel bir alanda depolanıyor. 

  Java 8 ile birlikte PermGen alanı kaldırıldı. Oracle HotSpot JVM, artık sınıf metadatalarını Java Process Heap(Native Memory) alanında yer alan **Metaspace** alanında depoluyor. 
  
  Özetlersek static tanımlanmış değişkenler Java 8 de native memory içinde **Metaspace** alanında, Java 7'de heap bölgesi içerisinde yer alan **PermGen** alanında depolanıyor.

     
Oracle HotSpot Heap Structure | JDK 1.8 Oracle HotSpot Heap Structure
:-------------------------:|:-------------------------:
![](/images/permgen.png)  |  ![](/images/metaspace.png)
    
## Analiz

Örnek bir kod üzerinden anlatmak gerekirse,

<script src="https://gist.github.com/FatihBozik/8ddab996978a20b24219c28a1ec6e775.js"></script>

* `g`, `i`, `myClass2` değişkenleri instance variable olduklarından bu değişkenler oluşturulmaz (programımızda hiç Test nesnesi oluşturmadık). Hiç bir yerde depolanmaz. Dolayısıyla initialization(ilk değer atamaları) da yapılmaz. `myClass2 referans değişkeninin göstereceği MyClass nesnesi` de oluşturulmaz. `i referans değişkeninin göstereceği Integer nesnesi` de oluşturulmaz.

* `h`, `j`, `myClass3` değişkenleri sınıf değişkeni olduklarından native memorydeki **metaspace** alanında depolanır (Java 8 için). `myClass3 referans değişkeninin gösterdiği MyClass nesnesi` **heap**te depolanır. Aynı şekilde `j nesnesinin gösterdiği Integer nesnesi` **heap**te depolanır.

* `a`, `b` ve `myClass` local değişken olduklarından **stack**te depolanır.

* `b referans değişkeninin gösterdiği Double nesnesi` **heap**te depolanır.

* `myClass referans değişkeninin gösterdiği MyClass nesnesi` **heap**te depolanır.

* `age`, `myDate`, `name` değişkenleri instance variable olduklarından nesne ile birlikte **heap**te depolanır. `myDate referans değişkeninin gösterdiği Date nesnesi` **heap**te depolanır. Aynı şekilde name referans değişkeninin göstereceği nesne **heap**te depolanır (new ile oluşturulmadığından dolayı heap içinde **string constant pool** denilen alanda bulunur).

* `number`, `str` sınıf değişkenleri native memorydeki **metaspace** alanında depolanır. `str referans değişkeninin gösterdiği String nesnesi` **heap**te depolanır. (heap içinde **string constant pool** denilen alanda)

* `param` local değişken olduğundan **stack**te depolanır.

* `args` local değişkeni **stack**te depolanır.

* setName methoduna gönderilen argümanda `"fatih"` bir nesne olduğundan heap alanında depolanır. (**string constant pool** denilen alanda)
