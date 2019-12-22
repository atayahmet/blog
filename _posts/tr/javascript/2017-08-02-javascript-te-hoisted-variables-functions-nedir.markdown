---
layout: post
title: Javascript'te Hoisted Variables & Functions nedir?
date: 2017-08-02 10:00:00 +0300
description: Javascript’de yeni olanların ilk karşılaştıklarında kafalarını karıştırabilecek bir konu olan hoisted variables & functions konusu anlatmaya çalışacağım.
img: js-1.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [Javascript, Fundamental]
categories: [tr, javascript]
---

Javascript’de yeni olanların ilk karşılaştıklarında kafalarını karıştırabilecek bir konu olan hoisted variables & functions konusu anlatmaya çalışacağım. Hosited nedir peki? Ne işe yarar?

## Hoisted Variables:

Javascript’te değişkenlerimizi `var`, `let` ve `const` gibi türlerle tanımlarız (declaration). Sonrasında bir değer atayarak değişkeni kullanıma hazır hale getiriz. İşte tam da burada değişkeni nerede tanımladığımızın çok büyük önemi vardır. Bir çok yazılım dillerinde değişken tanımlama işlemi kod akışında en başta tanımlamayı gerektirir. Yada kullanımından önce değerinin atanmış olması gerekir.

**Örnek verelim:**

```javascript
var a = 'Merhaba';
console.log(a); // Merhaba
```

Normal şartlarda her zaman bir değişkeni yukarıdaki gibi tanımlayıp kullanırız.

**Başka bir örnek ile devam edelim:**

```javascript
var a = 'Merhaba';
function x() {
  var a = 'Selam';
}
console.log(a); // Merhaba
```

İki tane **a** değişkeni oluşturduk fakat bir tanesi global kapsamda yani window nesnesine kaydedildi. Diğerini ise **x** fonksiyonu içinde tanımlandık. Çıktının Merhaba olduğunu görebiliyoruz. Tanımlanan değişkenlerin sadece kendi scope’larından erişilebildiğini, scope dışında kalan alanlarda değişkenin aslında hiç oluşturulmadığını görürüz.

Tanımlanmayan (undeclared) bir değişkeni kullanmak istediğimizde **ReferenceError** istisnasının fırlatıldığını hatırlatalım.

```javascript
console.log(c) // throws ReferenceError
```

Bir değişken tanımlanıp fakat bir değer atanmadığında varsayılan değeri `undefined` olur.

**Örnek:**

```javascript
var b;
console.log(b); // undefined
b = 'Istanbul';
console.log(b); // Istanbul
```

Bir diğer ayrıntı ise değişken tanımlarken türünü belirtmezsek eğer global kampasamda tanımlanmış olur. Yani tür tanımlanmayan bir değişken, fonksiyon içinde tanımlanmış dahi olsa kodumuzun her hangi bir yerinden erişebiliriz.

**Örnek:**

```javascript
function y() {
  name = 'Ahmet';
  var surname = 'ATAY';
}
y(); // call
console.log(name); // Ahmet
console.log(surname); // throws ReferenceError
```

## Hoisted Functions

Fonksiyon tanımlarken farklı yöntemler kullanılabilir. Javascript’in bizi şaşırtan bir takım özellikleri burada ortaya çıkıyor.

**Bir örnek verelim:**

```javascript
merhaba(); // Merhaba

// function declaration
function merhaba() {
  console.log('Merhaba');
}

gunaydin(); // ReferenceError: gunaydin is not defined

selam(); // TypeError undefined is not a function

// function express
var selam = function() {
  console.log('Selam');
}
```

Yukarıdaki kullanımları sırasıyla açıklayalım;

İlk olarak merhaba() fonksiyonunu çağırdık ve beklediğimiz çıktıyı aldık. Burada şu soruyu soruyor olabilirsiniz; fonksiyon tanımlandığı yerden önce çalıştırıldı. Evet ilk bakışta kafa karıştıcı geliyor. Function hoisting tanımlanmış olan fonksiyonları scope’ta üste taşıma işlemi anlmaına gelmketedir. Böylelikle tanımlama işlemi çalıştırma işleminden önce gerçekleşmiş olur.

Hala daha kafanızda bu konu ile ilgili soru işaretleri var ise şöyle bir açıklama yapabilirim. Tarayıcılardaki Javascript yorumlayıcıları tüm kodları çalışma anında (Runtime) bazı düzenlemelerden sonra çalıştırır. Hoisting işlemide bu düzenleme kapsamında uygulanmaktadır.

Pratikte değişken tanımlamaları her zaman akışa göre üste olması, kod içinde oluşabilecek declaration kargaşasının önüne geçmek için doğru bir yol olacaktır.

Yazının faydalı olması dileğiyle.