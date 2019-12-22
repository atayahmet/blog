---
layout: post
title: State Management Pattern nedir? Neden Kullanılır?
date: 2018-02-16 10:00:00 +0300
description: Geniş çaplı uygulamaların karmaşıklık seviyesi, projenin büyüme hızıyla doğru orantılıdır.
img: state-management-pattern-diagram.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [Javascript, Design Pattern]
categories: [tr, javascript]
---

Geniş çaplı uygulamaların karmaşıklık seviyesi, projenin büyüme hızıyla doğru orantılıdır. Özellikle proje javascript gibi bir dil ile geliştiriliyorsa `state management` gibi design pattern‘ların kullanımı zorunluluk hale geliyor.

UI `(user interface)` için kullanabileceğimiz bir çok framework’ün (`Angular`, `VueJS`, `React`, `React Native`) piyasaya çıkmasıyla yeni yöntemlerin uygulanmasına bağlı olarak bir takım sorunlar da beraberinde geldi. Bunlardan en önemlisi nesneler (objects), diziler (arrays) ve ilkel (primitive) türler dediğimiz `string`, `boolean` ve `number` türlerinin değişmezlik `(immutable)` yönetimleridir.

Kod performansını yüksek tutmak için mutable/immutable konusu önemli bir konudur. Bir `<x-component>`‘i içinde kullandığınız değişken `<y-component>`‘i için de değişme (mutate) riski taşıyacağından bu da beraberinde bir takım bug’ları getiriyor olacaktır. Bu tür sorunların debugging işlemi bir hayli sancılı olmaktadır.

Bir değişkeni reference olarak değil yeni bir kopyasını (clone) oluşturarak farklı yerlerde kullanılması çok daha doğru bir adım olacaktır. Immutable kavramının tam olarak tanımı için; “Herhangi bir türe sahip değişkenin değerini asla değiştirme. Yeni bir kopyasını oluştur ve kullan.” şeklinde açıklayabiliriz.

Neden peki değişkenleri istediğimiz yerde istediğimiz şekilde kullanamıyoruz yada değiştiremiyoruz? Aslında bu sorunun cevabı kullandığımız UI framework’lerin `data-bind` mimarisi üzerine kurulu olmasıyla ilgili bir durum. Özetle component içinde bir değişken ile html elementini birbirine bağlayarak (binding) arada bir ilişki kuruluyor. Bu iki uç nokta sürekli olarak birbirleriyle etkileşim içinde oluyorlar.

> Not: Data-Binding iki yönlü (two-way) ya da tek yönlü (one-way) olabilir. Kullanılan teknolojiye göre farklılık gösterebilir. İki yönlü binding mimarisi, bind edilen değişken mevcut değerinden farklı bir değer ile değiştirilirse bu değişim direkt olarak UI’a yansıyacaktır. Aynı şekilde UI tarafında bir input yada vb. form element ile bind edilmiş ise bu elemente girilecek olan her değer direkt olarak değişkeni etkileyecektir.

Bind edilmiş bir değişken ile element her hangi bir değişime karşı birbirlerini sürekli dinliyor olacaklardır. Bir değişkenin değeri her değiştiğinde direkt olarak componentin kendini tekrar render etmesini sağlayacaktır. Peki ama aynı değişken bir den fazla component için de aynı referans ile kullanıldığında olacakları bir düşünün? Tek bir değişkenin mutate olması bir çok componenti tekrar tekrar render olmaya zorlayacaktır. Bu durum performans sorunlarını beraberinde getirecektir.

Yukarıdaki hata senaryolarının çözümü state management kullanımında yatmaktadır. Neyseki bir çok framework bu pattern’ı kendi bünyelerine entegre ederek kullanımını teşvik etmektedir. Aksi halde frensiz bir arabayla yolculuk yapmak gibi riskli bir geliştirme süreci kaçınılmaz olacaktır.

## Temel özellikleri

State management temel de 3 farklı bileşini vardır. Bunlar;

- Store
- Actions
- Reducers

## Store
Store tüm değişkenlerin muhafaza edileceği (in-memory) kısım. Veriler buraya kaydedilip kullanılmak istenildiğin de tekrar buradan elde edilecektir.

## Actions
Bir değişken/sabit yada class olarak kullanılabilmektedir. Store ile veri alışverişi yapılırken her işlemi temsil edecek olan bir kimlik olarak düşünebiliriz. Genel de açıklayıcı niteliği taşıyan değerler atanır.

**Örnek:** 
```javascript
const REMOVE_ITEM_ACTION = '[Todo] Remove item'
```

## Reducers
Reducer aşaması store’daki veriyi işleme görevini yapacak olan kısımdır. Action’lar ile iletişim kurar ve mutate işlemlerini gerçekleştirir. Daha sonra veriyi tekrar store’a yazar ve yeni bir kopyasını kullanılmak üzere geri döndürür.

## Avantajları
En büyük avantajı hiç şüphesiz in-memory veri yığınının daha tutarlı kullanımına imkan sağlaması. Yazının girişin de bahsettiğimiz gibi büyük ölçekli projeler de en büyük risk immutable yönetemidir.

Bir diğer avantaj cache imkanı sunuyor olması. Browser ortamın da çalışan sistemler de store’a eklenen verilerin tekrar API sorgusuna gerek kalmadan kullanılabilir olmasıdır.

## Dezavantajları
Avantaj olarak gördüğümüz cache’leme kolaylığı iyi yönetilemezse dezavantaj olarak önümüze geri gelecektir. Çok fazla kaynak tüketimi (memory leak) olacağından tarayıcı ortamında (özellikle mobil tarayıcılar da) ciddi performans sorunlarına yol açabilir.

## Sonuç
Sonuç olarak client (özellikle SPA) projeler de kullanmamız faydalı olacaktır. Avantajları ve dezavantajlarının göz önünde bulundurulması gerektiğini unutmamalıyız.

Günümüz tarayıcılarının imkanları geniş ve modern teknolojilere hızlıca destek veriyor olsalar da sınırsız olmadığını unutmamak lazım.

Aşağıda her hangi bir framework’e bağımlı kalmadan kullanabileceğiniz bir kaç State Management library repo’ları paylaştım.

[https://redux.js.org/](https://redux.js.org/)

[https://facebook.github.io/immutable-js/](https://facebook.github.io/immutable-js/)

[https://github.com/mobxjs/mobx](https://github.com/mobxjs/mobx)