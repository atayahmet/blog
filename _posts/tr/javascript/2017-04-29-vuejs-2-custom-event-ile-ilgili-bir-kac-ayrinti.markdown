---
layout: post
title: VueJS 2 Custom Event ile ilgili bir kaç ayrıntı
date: 2017-04-29 10:00:00 +0300
description: VueJS 2 ile birlikte gelen yeniliklerin yanı sıra hali hazırdaki bir takım özellikler de kaldırıldı (depracated) ya da kullanım şekli değiştirildi diyebiliriz.
img: vuejs.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [Javascript, Vue JS]
categories: [tr, javascript]
---

VueJS 2 ile birlikte gelen yeniliklerin yanı sıra hali hazırdaki bir takım özellikler de kaldırıldı (depracated) ya da kullanım şekli değiştirildi diyebiliriz. Yeni versiondan etkilenen özelliklerden biri de Custom Event’lerin kullanımlarıdır.

Şöyleki; VueJS version 1’de parent instance’tan child component’te kaydedilmiş bir event yada event’leri `$broadcast` edebiliyorduk diğer bir söylemle tetikleyebiliyorduk. Şuan da version 2’de bu özellik resmi olarak kaldırılmış durumda, yani parent’tan child componentteki bir event’leri çalıştıramıyoruz.

Artık sadece child component’ten parent instance’taki event’leri çalıştırabiliyoruz. Burada yine bir kullanım dğeişikliği söz konusu, version 1’de `$dispatch` ile child component’ten parent’taki bir event tetikliyorken, version 2’de bu süreç artık tamamen event directive’leri üzerinden ilerliyor.

***Parent-Child event yönetimi ile ilgili dökümanda bulunan ayrıntı:***

> You cannot use $on to listen to events emitted by children. You must use v-on directly in the template, as in the example below. [VueJS 2 DocumentCustom Events](https://vuejs.org/v2/guide/components.html#Custom-Events)

**Örnek:**

```html
<div id="example">
  <products v-on:alert="alerts"></products>
</div>
```

**Products Component:**

```javascript
Vue.component('products', {
  methods: {
    getProducts() {
      this.$emit('alert', 'Ürün bulunamadı');
    }
  }
});
```

**Product List Component (parent):**

```javascript
new Vue({
  methods: {
    alerts(message) {
      // Ürün bulunamadı
      console.log(message);
    }
  }
}).$mount('example');
```

Sonuç itibariyle üzerinde çalıştığım projede parent instance’tan child component’teki bir event’i tetiklemem gerektiğinden bunu farklı bir yöntemle çözdüm. One-way binding yapıp child component’te tanımladığım prop’u VueJS watch ile izleyerek bir nevi event gibi kullanmış oldum.

