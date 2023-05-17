# SQL Sorgu Alıştırmaları

Bu hafta SQL sorguları üzerine çalışıyorsunuz. Bugünkü alıştırmada sizin için hazırladığımız veritabanında aşağıda istediğimiz sonuçları elde etmenize yarayan SQL sorgularını oluşturacaksınız.

## Proje Kurulumu

Projeyi forklayın ve clonlayın. Tamamladığınızda da pushlayın.

### Kütüphane Bilgi Sistemi

Bu veritabanı, bir okulun kütüphanesinden öğrencilerin aldıkları kitapların bilgisini barındırmaktadır.

#### Tablolar

`ogrenci` tablosu öğrencilerin listesini tutar.
`islem` tablosu öğrencilerin kütüphaneden aldıkları kitapların bilgilerini tutar
`kitap` tablosu kütüphanedeki kitapların bilgisini tutar
`yazar` tablosu kitapların yazarları bilgisini tutar
`tur` tablosu kitapların türlerini tutar.

Tablo ilişiklerini görmek için [ktphn.png] dosyasına göz atın.

Yazdığınız sorguları buradan test edebilirsiniz: [https://ergineer.com/assets/materials/fkg36so5-kutuphanebilgisistemi-sql/]

##### Görevler

Aşağıda istenilen sonuçlara ulaşabilmek için gerekli SQL sorgularını yazın.

MIN-MAX, COUNT-AVG-SUM, GROUP BY, JOINS (INNER, OUTER, LEFT, RIGHT
#ilk 3 soruyu join kullanmadan yazın. 1) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.
CEVAP: select ograd,ogrsoyad,atarih from ogrenci,islem where ogrenci.ogrno = islem.ogrno; 2) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.
CEVAP: select kitapadi,turadi, from kitap,tur
where kitap.turno = turno and turadi in("Fıkra","Hikaye")

    3) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları listeleyin.

    CEVAP: select ogrenci.ogrno,ograd,ogrsoyad,kitapdi from ogrenci,islem,kitap
    	where sinif in("10B","10C")
    		and ogrenci.ogrno = islem.ogrno
    		and islem.kitapno = kitap.kitapno;


    #join ile yazın
    4) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.

    CEVAP: select o.ograd,o.ogrsoyad, i.atarih from ogrenci as o
    	join islem as i
    	on o.ogrno = i.ogrno;

    5) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.

    CEVAP:	select k.kitapadi,t.turadi from kitap k
    	left join tur t on t.turno=k.turno
    	where t.turadi = "Fıkra" or t.turadi ="Hikaye"



    6) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları, öğrenci adına göre listeleyin.

    CEVAP: select o.ogrno,o.ograd,o.ogrsoyad,k.kitapadi from ogrenci o
    left join islem i on i.ogrno = o.ogrno
    left join kitap k on k.kitapno = i.kitapno
    where o.sinif = "10B" or o.sinif ="10C"
    order by o.ograd


    7) Kitap alan öğrencinin adı, soyadı, kitap aldığı tarih listelensin. Kitap almayan öğrencilerinde listede görünsün.

    CEVAP: select o.ogrno, o.ograd,o.ogrsoyad, i.atarih from ogrenci o
    inner join islem i on i.ogrno = o.ogrno
    inner join kitap k on k.kitapno = i.kitapno



    8) Kitap almayan öğrencileri listeleyin.

    CEVAP: select ograd,o.ogrno from ogrenci o
    left join islem i on i.ogrno = o.ogrno
    where i.kitapno is null



    9) Alınan kitapların kitap numarasını, adını ve kaç defa alındığını kitap numaralarına göre artan sırada listeleyiniz.

    CEVAP: select k.kitapno,k.kitapadi,count(i.kitapno) as "Alınan Kitap Sayısı" from kitap k
    left join islem i on i.kitapno = k.kitapno
    group by k.kitapno



    10) Alınan kitapların kitap numarasını, adını kaç defa alındığını (alınmayan kitapların yanında 0 olsun) listeleyin.

    CEVAP: select k.kitapno, count(i.kitapno) as "Alınan Kitap Sayısı" from kitap k
    full outer join islem i on i.kitapno=k.kitapno
    group by k.kitapno



    11) Öğrencilerin adı soyadı ve aldıkları kitabın adı listelensin.

    CEVAP: select o.ogrno, o.ograd, o.ogrsoyad, k.kitapadi from ogrenci o
    left join islem i on i.ogrno = o.ogrno
    left join kitap k on k.kitapno = i.kitapno
    order by o.ograd

    12) Her öğrencinin adı, soyadı, kitabın adı, yazarın adı soyad ve kitabın türünü ve kitabın alındığı tarihi listeleyiniz. Kitap almayan öğrenciler de listede görünsün.

    CEVAP: select o.ogrno, o.ograd , o.ogrsoyad , k.kitapadi, y.yazarad, y.yazarsoyad, i.atarih from ogrenci o
    left join islem i on i.ogrno = o.ogrno
    left join kitap k on k.kitapno = i.kitapno
    left join yazar y on y.yazarno = k.yazarno
    order by o.ograd

    13) 10A veya 10B sınıfındaki öğrencilerin adı soyadı ve okuduğu kitap sayısını getirin.

    CEVAP: select o.ogrno, o.ograd , o.ogrsoyad , count(k.kitapno) as 'Okunan Kitap Sayısı' from ogrenci o
    left join islem i on i.ogrno = o.ogrno
    left join kitap k on k.kitapno = i.kitapno
    where o.sinif = '10B' or o.sinif='10C'
    group by o.ograd
    order by o.ograd

    14) Tüm kitapların ortalama sayfa sayısını bulunuz.
    #AVG

    CEVAP: select AVG(sayfasayisi) from kitap

    15) Sayfa sayısı ortalama sayfanın üzerindeki kitapları listeleyin.

    CEVAP: select sayfasayisi from kitap
    where sayfasayisi > (Select AVG(sayfasayisi) from kitap)

    16) Öğrenci tablosundaki öğrenci sayısını gösterin

    CEVAP: select count(ogrno) as 'ogrenci sayısı' from ogrenci

    17) Öğrenci tablosundaki toplam öğrenci sayısını toplam sayı takma(alias kullanımı) adı ile listeleyin.

    CEVAP: select count(ogrno) as 'toplam sayı' from ogrenci

    18) Öğrenci tablosunda kaç farklı isimde öğrenci olduğunu listeleyiniz.

    CEVAP: select count(ograd), ograd from ogrenci group by ograd

    select distinct ograd from ogrenci

    19) En fazla sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.

    CEVAP: select max(sayfasayisi) from kitap

    20) En fazla sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.

    CEVAP: select kitapadi, sayfasayisi from kitap
    where sayfasayisi = (select max(sayfasayisi)from kitap)

    21) En az sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.

    CEVAP: select min(sayfasayisi) from kitap

    22) En az sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.

    CEVAP: select kitapadi,sayfasayisi from kitap
    where sayfasayisi = (select min(sayfasayisi) from kitap)

    23) Dram türündeki en fazla sayfası olan kitabın sayfa sayısını bulunuz.

    CEVAP: select max(k.sayfasayisi) from kitap k
    left join tur t on t.turno =k.turno
    where t.turadi ="dram"

    24) numarası 15 olan öğrencinin okuduğu toplam sayfa sayısını bulunuz.

    CEVAP:select sum(k.sayfasayisi) as "Toplam Sayfa", o.ogrno from ogrenci o
    left join islem i on i.ogrno = o.ogrno
    left join kitap k on k.kitapno = i.kitapno
    where o.ogrno = 15

    25) İsme göre öğrenci sayılarının adedini bulunuz.(Örn: ali 5 tane, ahmet 8 tane )

    CEVAP: select count(ograd),ograd from ogrenci
    	group by ograd

    26) Her sınıftaki öğrenci sayısını bulunuz.

    CEVAP: select count(ogrno) as "Öğrenci Sayısı", sinif from ogrenci
    	group by sinif

    27) Her sınıftaki erkek ve kız öğrenci sayısını bulunuz.

    CEVAP: select count(ogrno) as "ögrenci Sayısı", sinif,cinsiyet from ogrenci
    	group by sinif,cinsiyet

    28) Her öğrencinin adını, soyadını ve okuduğu toplam sayfa sayısını büyükten küçüğe doğru listeleyiniz.

    CEVAP: select o.ograd,o.ogrsoyad, sum(k.sayfasayisi) as "Öğrencinin okuduğu sayfa sayısı" from ogrenci o
    left join islem i on i.ogrno = o.ogrno
    left join kitap k on k.kitapno = i.kitapno
    	group by o.ogrno

    29) Her öğrencinin okuduğu kitap sayısını getiriniz.

    CEVAP: select o.ograd, o.ogrsoyad, count(k.sayfasayisi) as "Öğrencinin okuduğu kitap sayısı" from ogrenci o
    left join islem i on i.ogrno = o.ogrno
    left join kitap k on k.kitapno = i.kitapno
    	group by o.ogrno
