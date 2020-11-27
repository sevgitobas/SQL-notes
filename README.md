# SQL-notes
**KISA KISA SQL NOTLARI**

Oracle'da _pl-sql_, Windows'da _transact-sql_ kullanılır. _Use_ deyip _master_ kısmını daha önceden isim verdiğimiz etrade ile kullanıp çalıştırmaya başlıyoruz.

_Delete_ siliyor gibi görünüyor ama aynı id'den devam ediyor. _Truncate_ ise tamamen silip 1den başlatıyor tablo ilk boş haline dönüyor. 
“`insert into tabloadı (kolon,...) values (değer,...) `
`update customers set age = datediff(year, birthdate, getdate()) `
`where city in('istanbul','ankara')` “

Kolonlarda karakter aralığını artırmak istediğinde _tools-options-designers_ kısmından _prevent saving changes that require table re-creation_ kısmının tikini kaldırıyorum. satırı yapmak için -- iki tire işareti kullan. 

_District_ birkaç tane olan seçeneği tek sefer gösterir mesela “`select distinct city, district from customers where city='istanbul'`” dediğinde istanbul'daki ilçelerin isimlerini verir tekrarlamaz.

_Asc = küçükten büyüğe_, a'dan z'ye, default ayar budur. _Desc = büyükten küçüğe_, z'den a'ya. _Order by 1_ dersek ilk kolon, 5 dersek beşinci kolona göre sırala olur. İlla kolon adını yazmaya gerek yok.

`select top 100` dersen ilk yüzü verir 5 dersen ilk beşi verir. `select top 10 percent` dersen ilk yüzde onu verir.

_Where_'in içinde aggregate functionları kullanamazsın bu yüzden _having_ ile yazmalısın.


**Sayısal veri tipleri**; bigint 8 byte, int 4 byte, smallint 2 byte, tinyint 1 byte, bit ise duruma göre 1 ya da 2 byte yer kaplar. Bit 0 ya da 1 değerini alır. True-False olan durumlarda kullanılır.

Metin karakterlerinde her harf 1 byte'lık yer kaplar. _Varchar_'da max ifadeyi seçip ona göre alan ayarlarken _char_'da bu alanı biz belirliyoruz. başındaki n ifadeside 255 karakterin dışında karakter girmemizi sağlıyor. Başına n yazdığında 1 byte'lık yer kaplayan karakterler artık 2 bytle'lık yer kaplar.
 

**RDMS ilişkisel veri tabanı sistemi**

_Primary key_ ile bağlantı kuruluyor.

_int identitiy(1,1) :_ birer birer artan birden başlayan

Master detay ilişki ile RDMS bağlanır.

RDMS ile ekstra id belirlediğimiz tablolar oluşturursak verinin kapladığı alanı azaltabilir, sorgu performansını artırabiliriz.

_Foreign key:_ Primary key'in başka tabloda ilişkilendirme için kullanılması.

Relation kurduğumuz tabloda insert and update spesification kısmındaki _delete rule_'u default yani _no action_ bırakırsan silmek istediğin verinin başka bir tabloda ilişkisi olduğu olduğu için hata verir. Ama eğer delete rule kısmını _cascade_ olarak ayarlarsan bu sefer hem silmek istediğin veriyi istediğin tabloda siler hem de aynı anda ilişkisi bulunan tablodaki verileri de siler. Bu dikkat edilmediği takdirde tehlikelidir. Bu ayarı set null yaparsak silmek istediğin veriyi siler, ilişkisi olan tablodaki kısmını null yapar ama ilişkili veriler kalır. Set default seçersek de set null ile aynı olur.

_Data definition language_ ile excelden hazırladığımız tabloları script kullanarak sql'e gönderebiliriz.

Toplu tablo silmek istediğimizde o tablonun üstü tıklı iken view, object explorer details'e tıklayıp tamamını seçip sağ tıklayıp delete diyoruz.
 
Tablomuza excelden yeni veri eklerken daha önceden veri eklenip silinmemiş olduğunu bilmediğimizi düşünüp "`truncate table TabloAdı`" diyoruz ve tabloyu ilk create edildiği hale getiriyoruz. SQL server management studio arayüzünde istenilen tabloyu edit yapıp sağ üst sıralardaki _SQL_ butonuna tıklayıp select kısmından sonraki top(..) kısmını silip ekleyeceğimiz kolonun adlarını bırakıyoruz.

Alt kısımdaki tabloya verileri sağ click ile ekliyoruz. Bu işlem uzun sürüyor. Ama eğer daha hızlı işlem yapmak istiyorsak Excel'de insert komutu ile yeni komutlar oluşturup tekrardan management studio’dan ekleyebiliriz. 

İki ayrı tabloyu aynı anda sorgulayıp ekrana yazdırmak için örnek;
 	"` select users.*, address.addresstext from users, address `
`where users.id=address.userid and users.id=1` " 
bu örnekte user id numarası 1 olan kişinin users tablosundaki tüm bilgileri ile adres tablosunda ona ait olan adres bilgini sorgulatmış olduk.

Ama bu yöntemin yerine _join_ ifadesini kullanıyoruz. Örneğin: 
" `select u.*, a.addresstext `
`from users u `
`join address a on u.id=a.userid` " 
diyoruz. Join ifadesi _inner join_ ile aynı.

_Left(outer) join_ ise soldaki kümeyi tamamen al, diğerinin ise kesişimini al demek.
_Full join_ ile de her iki kümeyi kapsayan bir sorgulama yapmış oluyoruz.
_Date time_’lı kolonu sadece date’e çevirmek için `convert(date, istediğin_kolon)` yapıp çevir. Ya da bunu sadece ay olarak kullanmak istiyorsan `datepart(month, istediğin_kolon)` yapıp al. Yılı istiyorsan datepart kullanarak month yerine year yaz kullan. Eğer ayları isimlerine göre yazdırmak istiyorsan “`case when datepart(month, istediğin_kolon)=1 then ‘ocak’` ” şeklinde her ay için ayrı ayrı yazıp sonuna “end as ayadı” yazdırabilirsin.

İki tarih arasındaki farkı bulmak için “`datediff(date, başlangıç_tarihi, bitiş_tarihi)`” yaz. Eğer gün bazındaysa date, yıl bazındaysa year, saat bazındaysa hour, dakika bazında minute yaz.

Eğer sorgu yaparken hangi tablodan ne kadar veri çekilip okunduğu bilgisini almak istiyorsak sorgunun başına “`set statistics io on`” yazıyoruz.

_SubQuery_ sorgusunda Join sorguya göre daha çok performans harcanır. Okunan her page’de 8byte’lık harcama yapılır.
SubQuery ile son adres bilgisi çekmek için : 
	“`select u.*,`
		`(select top 1 addresstext from address where userid= u.id order by id desc) sonadres`
	 `from users u`”
burada önce parantez içinde adresleri sıraladık, sonra ilk adresi çekip kullandık. Böylece son adres bilgisi gelmiş oldu. İlk adres bilgisi için ise desc yerine asc yazabilir ya da boş bırakabilirsin.

Transact SQL’de string işlemler yapılabilir. En önemli avantajı da budur.


**String İşlemler:**

_ASCII_ : Bir karakterin kendindeki karşılığını verir. Örneğin “ `select ascii (‘1’)` ” dediğimizde 49 değerini verir.

_CHAR_ : ASCII’nin tersidir. Yani eğer “ select char (49) ” dersek 1 değerini verir.

_SUBSTRING_ : “ `select substring ( ‘ömer faruk’, 1, 4)` ” dersek ömer sonucunu verir. Birinci karakterden başla dört karakter al, anlamına gelir.

_CHARINDEX_ : “ `select charindex(‘f’, ‘ömer faruk’, 1)` ” dersek ömer faruk’un içinde 1’inci karakterden itibaren ‘f’ harfini ara demiş oluyoruz. Eğer bulamazsa sonucu sıfır verir. ‘f’ harfinin yanı sıra ‘faruk’ diyerek aramasını da isteyebiliriz.

_CONCAT_ : String ifadeleri birleştirerek yazdırır. Örneğin; 
“ `select`
`Username_ + ‘ ’+ password_ + ‘ ’ + namesurname,`
`Concat(Username_ , ‘ ’, password_ , ‘ ’ , namesurname),`
`Concat_ws(‘ ‘ ,Username_ , password_ , namesurname)`
`From user `“ 	şeklinde yanyana üç string ifadeyi aralarında boşlu bırakarak aynı şekilde yazdırabiliriz.

_GETDATE() _: Bugünün tarihini getirir.

_FORMAT _: Örneğin; 
“ `select format(getdate(), ‘d’, ‘en-us’)` “ dediğimizde bugünün tarihini 10/18/2018 formatında verir.
“ `select format(getdate(), ‘D’, ‘en-us’)` “ dediğimizde ise Thursday, October, 18, 2018 formatında verir.

_LEFT, RIGHT, LEN_ : Örneğin;
“ `select left(‘ömer çolakoğlu’, 4)` “ soldan ilk 4 karakter ömer 
“ `select right(‘ömer çolakoğlu’, 4) `“ sağdan ilk 4 karakter oğlu
“ `select len(‘1234567890’)` ” dediğimizde ise 10 sonucunu verir.

_TRIM, LTRIM, RTRIM _: Trim baştaki ve sondaki boşlukları siler. Ltrim soldaki, Rtrim ise sağdaki boşlukları siler.

_LOWER, UPPER, REVERSE, REPLİCATE_ : Sırasıyla küçük harf, büyük harf, ters yaz, birkaç kere yazdır komutlarıdır. Eğer sırano’yu 8 haneli yazdırmak istiyorsak, başında sıfırlar olarak;
“ `update test set sirano2 = replicate(‘0’, 8 – len(sirano)) + convert(varchar, sirano) `“ diyoruz. Örnek sonuç 00001845 gibi oluyor.

_REPLACE_ : Örneğin; “ `DECLARE @CUMLE AS VARCHAR(MAX)` “ dediğimizde cumle adında değişken belirleyip türünü varchar yapıyoruz. “ `SET @CUMLE = ……`” diyerekte değişkene değer atıyoruz. “`SET @CUMLE = REPLACE(@CUMLE, ‘değişsin istediğin değer’, ‘istenilen değer’)` diyerekte değer değişimi yapıyoruz.

