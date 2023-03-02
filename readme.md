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
	#ilk 3 soruyu join kullanmadan yazın. 
	
	1) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.

	SELECT
	ogrenci.ograd, ogrenci.ogrsoyad, islem.atarih
	FROM ogrenci, islem
	WHERE
	ogrenci.ogrno = islem.ogrno;
	
	
	 2) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.

    	UPDATE tur SET turadi=trim(turadi);


    	SELECT tur.turadi, kitap.kitapadi
    	FROM tur, kitap
    	WHERE
    	tur.turno = kitap.turno AND
    	(tur.turadi = "Fıkra" OR tur.turadi= "Hikaye");

    3) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları listeleyin.

    	UPDATE ogrenci SET sinif = trim(sinif);

    	SELECT ogrenci.sinif, ogrenci.ograd, kitap.kitapadi
    	FROM ogrenci, kitap, islem
    	WHERE
    	islem.ogrno = ogrenci.ogrno
    	AND
    	(ogrenci.sinif = "10B" OR ogrenci.sinif="10C") ;

    #join ile yazın
    4) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.

    	SELECT ograd, ogrsoyad, atarih
    	FROM Ogrenci
    	INNER JOIN islem
    	ON ogrenci.ogrno = islem.ogrno;

    5) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.

    	SELECT turadi, kitapadi
    	FROM tur INNER JOIN kitap
    	ON tur.turno = kitap.turno
    	AND (tur.turadi = "Fıkra" OR tur.turadi= "Hikaye");


    6) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları, öğrenci adına göre listeleyin.

    	SELECT sinif, ograd, kitapadi
    	FROM ogrenci INNER JOIN kitap
    	INNER JOIN islem
    	ON islem.ogrno = ogrenci.ogrno
    	WHERE
    	(ogrenci.sinif = "10B" OR ogrenci.sinif="10C") ;


    7) Kitap alan öğrencinin adı, soyadı, kitap aldığı tarih listelensin. Kitap almayan öğrencilerinde listede görünsün.

    	Select ograd, ogrsoyad, atarih
    	From ogrenci
    	FULL OUTER JOIN islem
    	ON ogrenci.ogrno = islem.ogrno;

    8) Kitap almayan öğrencileri listeleyin.

    	Select ograd, ogrsoyad, atarih
    	From ogrenci
    	FULL OUTER JOIN islem
    	ON ogrenci.ogrno = islem.ogrno
    	WHERE atarih IS NULL;

    9) Alınan kitapların kitap numarasını, adını ve kaç defa alındığını kitap numaralarına göre artan sırada listeleyiniz.

    	Select kitapno, COUNT(islemno), kitapadi
    	FROM kitap INNER JOIN islem
    	ON kitap.kitapno = islem.kitapno
    	GROUP BY kitapadi
    	ORDER BY kitap.kitapno;


    10) Alınan kitapların kitap numarasını, adını kaç defa alındığını (alınmayan kitapların yanında 0 olsun) listeleyin.

		Select kitapno, COUNT(islemno), kitapadi 
		FROM kitap LEFT JOIN islem 
		ON kitap.kitapno = islem.kitapno 
		GROUP BY kitapadi 
		ORDER BY kitap.kitapno;


    11) Öğrencilerin adı soyadı ve aldıkları kitabın adı listelensin.

		SELECT ograd, ogrsoyad, kitapadi
    	FROM ogrenci INNER JOIN kitap
    	INNER JOIN islem
    	ON islem.ogrno = ogrenci.ogrno

    12) Her öğrencinin adı, soyadı, kitabın adı, yazarın adı soyad ve kitabın türünü ve kitabın alındığı tarihi listeleyiniz. Kitap almayan öğrenciler de listede görünsün.

		select ogrenci.ograd,ogrenci.ogrsoyad, kitap.kitapadi, yazar.yazarad, yazar.yazarsoyad, islem.atarih, tur.turadi from ogrenci
			left join islem on  islem.ogrno=ogrenci.ogrno
			left join kitap on islem.kitapno=kitap.kitapno
			left join yazar on kitap.yazarno=yazar.yazarno
			left join tur on kitap.turno=tur.turno;



    13) 10A veya 10B sınıfındaki öğrencilerin adı soyadı ve okuduğu kitap sayısını getirin.

		
		SELECT sinif, ograd, COUNT(islem.islemno) AS okunanKitap
    	FROM ogrenci INNER JOIN islem
    	ON islem.ogrno = ogrenci.ogrno
    	WHERE
    	(ogrenci.sinif = "10A" OR ogrenci.sinif="10B") 
             GROUP BY ograd;   
 ;

    14) Tüm kitapların ortalama sayfa sayısını bulunuz.
    #AVG

		SELECT AVG(sayfasayisi) AS AvgPageNumber From kitap

    15) Sayfa sayısı ortalama sayfanın üzerindeki kitapları listeleyin.

		SELECT kitapadi, sayfasayisi 
		From kitap WHERE sayfasayisi > 
		(Select AVG(sayfasayisi) From kitap)  ;


    16) Öğrenci tablosundaki öğrenci sayısını gösterin
	
		Select COUNT(ograd)  From ogrenci;


    17) Öğrenci tablosundaki toplam öğrenci sayısını toplam sayı takma(alias kullanımı) adı ile listeleyin.

		Select COUNT(ograd) AS "Toplam Sayı" From ogrenci;

    18) Öğrenci tablosunda kaç farklı isimde öğrenci olduğunu listeleyiniz.

		SELECT COUNT(distinct ograd) from ogrenci;


    19) En fazla sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.

		SELECT MAX(sayfasayisi) FROM kitap

    20) En fazla sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.

		Select kitapadi, sayfasayisi from kitap where sayfasayisi = (select max(sayfasayisi) from kitap)


    21) En az sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.

		SELECT min(sayfasayisi) from kitap


    22) En az sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.
	
		Select kitapadi, sayfasayisi From kitap Where sayfasayisi = (Select Min(sayfasayisi) From kitap);


    23) Dram türündeki en fazla sayfası olan kitabın sayfa sayısını bulunuz.

		SELECT MAX(sayfasayisi) from kitap
		INNER JOIN tur ON kitap.turno = tur.turno
		WHERE tur.turadi = "Dram" 

    24) numarası 15 olan öğrencinin okuduğu toplam sayfa sayısını bulunuz.

		SELECT sum(sayfasayisi), ogrenci.ogrno from ogrenci
		left join islem on ogrenci.ogrno=islem.ogrno
		left join kitap on kitap.kitapno=islem.kitapno
		where ogrenci.ogrno=15


    25) İsme göre öğrenci sayılarının adedini bulunuz.(Örn: ali 5 tane, ahmet 8 tane )

		select ograd, count(ograd) from ogrenci group by ograd


    26) Her sınıftaki öğrenci sayısını bulunuz.

		select sinif, count(*) from ogrenci group by sinif


    27) Her sınıftaki erkek ve kız öğrenci sayısını bulunuz.

		SELECT COUNT(*),cinsiyet FROM ogrenci group by cinsiyet


    28) Her öğrencinin adını, soyadını ve okuduğu toplam sayfa sayısını büyükten küçüğe doğru listeleyiniz.

	Select ograd, ogrsoyad, SUM(sayfasayisi) AS toplamSayfaSayisi From ogrenci 
    	INNER JOIN islem ON islem.ogrno = ogrenci.ogrno
    	INNER JOIN kitap ON kitap.kitapno = islem.kitapno
	GROUP By ogrenci.ogrno
	ORDER BY toplamSayfaSayisi;

    29) Her öğrencinin okuduğu kitap sayısını getiriniz.

		Select ograd, ogrsoyad, COUNT(kitapadi) AS toplamkitap From ogrenci
			INNER JOIN islem ON islem.ogrno = ogrenci.ogrno
			INNER JOIN kitap ON kitap.kitapno = islem.kitapno
		GROUP By ogrenci.ogrno
		ORDER BY ograd;