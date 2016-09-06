---
layout: post
title: "Sekilas Perjalanan Javascript"
date: 2016-08-7 18:11:25 +0700
categories: Javascript
---

Mungkin ada yang penasaran, kenapa hanya javascript yang bisa digunakan untuk melakukan client-side coding? kenapa tidak Python, Java, atau bahasa lainnya?

Sebenarnya kita bisa menggunakan bahasa pemograman lain untuk coding client-side, dan jika merujuk ke spesifikasi, [DOM] (Document Object Model) didesain agar dapat digunakan dengan bahasa pemograman apapun. Namun, faktanya saat ini Javascript merupakan bahasa *de facto* di web, untuk menjawab pertanyaan ini, kita perlu kembali ke masa kejayaan browser [Netscape] sekitar tahun 1995. 

Awalnya pembuat javascript, Brendan Eich dijanjikan untuk menerapkan bahasa [Scheme] pada browser oleh perusahaan Netscape, tetapi itu hanya umpan agar Brendan Eich bergabung dengan Netscape. Belum sempat dimulai, Netscape menjalin kerjasama dengan Sun untuk membuat Navigator menyertakan bahasa pemograman yang lebih static yaitu, Java didalam browser. Hal ini menyebabkan perdebatan di Netscape, kenapa harus ada dua bahasa dalam Navigator? Alasannya adalah untuk memberikan kemudahan web page desainer (menuliskan script langsung didalam file html) dan `component author`(pengguna bahasa C++ atau Java), selain itu para petinggi di Netscape menghendaki agar bahasa tersebut harus mirip Java. Secara otomatis hal tersebut menghilangkan Python, Perl, Tcl, dan Scheme dari pilihan bahasa yang akan digunakan.

Kemudian dalam waktu 10 hari (Mei 1995) lahirlah Javascript saat itu dikenal dengan sebutan Mocha dan di dalam browser Netscape Navigator versi 2.0. Kenapa hanya 10 hari? karena bagian `client engineering management` perlu prototype (proof of concept) untuk mempertahankan ide diatas dari prososal lainnya. 

Dipihak lain, Microsoft tidak mau ketinggalan menyertakan Jscript (Javascriptnya microsoft, dinamakan demikian karena masalah trademark) ke dalam IE, vendor lainnya pun mulai mengadopsi Javascript kedalam browsernya. 

Browser Netscape kalah pangsa pasar dengan IE tapi penggunaan Javascript sudah mengakar di web dan browser, sehingga tidak mungkin digantikan dengan bahasa lain (walaupun DOM tidak terikat dengan bahasa tertentu, membuat browser dengan bahasa pemograman lain akan mengakibatkan web developer melakukan pemograman dalam dua bahasa yang berbeda).


[https://brendaneich.com/2008/04/popularity/]
[http://www.2ality.com/2011/03/javascript-how-it-all-began.html]
[http://speakingjs.com/es5/ch04.html]
[http://programmers.stackexchange.com/a/190282]
[https://www.w3.org/community/webed/wiki/A_Short_History_of_JavaScript]
[DOM]: [https://www.w3.org/TR/DOM-Level-2-Core/introduction.html]
