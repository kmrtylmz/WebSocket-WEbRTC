## ScreenShots
<img src="https://github.com/kmrtylmz/socket-io-webrtc/blob/master/assets/tests/Test-1.jpg" width="90%" />
<img src="https://github.com/kmrtylmz/socket-io-webrtc/blob/master/assets/tests/Test-2.jpg" width="90%" />




## Geliştirme Ortamı
### WEbRTC & WebSocket
P2PCONNECTION [Peer To Peer] teknolojisini destekleyen protokol. Web ve mobil platformlarında desteklenen , tarayıcılar tarafından gömülü, hiç bir eklenti kurulmasına gerek kalmadan real-time ses ve data aktarımı iletilmesini sağlayan yapı. Symetric NATlar harici STUN Serverlar aracılığı ile NAT'ı ve Firewall'ı delerek iletişimde kullanılacak Public Ip adreslerinin ve Streamlerin metadatalarının eşleştirilmesiyle iletişim sağlanır.WebRTC open-source ve Google Chrome ekibi tarafından yürütülmektedir. Ayrıca sadece HTTPS bağlantılarda geçerlidir. Eşler arası bağlantıda arada sunucu yoktur. Eşlerin birbirlerini tanıması için "Sinyal Server" gereksinimi vardır ve bu sinyal serverın ne olacağının kararı tamamen bağımsızdır [JSEP]. Bunun için en ideal yöntemin real-time sunucu bazlı [HTTP nın TCP bağlantısı üzerinde Upgrade olmuş hali] Websocket teknolojisi yer alır.[Kanaatim] Websocketler tarayıcıda da bir API olarak yer alır ancak gerçek WEbsocket deneyimi katmaz. Bu yüzden WEbsocket için gelişmiş bir kütüphaneye ihtiyaç duyulur. µWebSockets , Ws , ClusterWS gibi tercihlerin yanı sıra daha populer olan socket.io'yu kullanmak mantıklıdır.

Herhangi bir dil ile etkileşime geçebilirsiniz.PHP tarafında çalışırken elephant.io tarzı kütüphanelerle calısılabilir. Ben de PHP ile çalışırken test aşamasında farklı yöntemlere yönelmek zorunda kalmıştım. O yüzden bodoslama dalmanızdan ziyade NodeJS tarafını başta ele alın derim. HTTPS bağlantıları için OpenSSL ile bu durumu aşacağımı düşünerek bir sunucu kiralanması yada WAN ayarlarını değiştirmekten öte localhostu Ngrok ile [proxy] yayınladım. 1 sunucu hakkım oldugu ıcın Apache tarafında da PHP ile işlemin çok olmamasından dolayı [Socket.io da NodeJS tarafında calıstıgından] NodeJS tarafında Kendinden imzalı sertifikalarla HTTPS webServer kurup Express Frameworkunu bu servera bağlayıp Socket Serverıda bu webserver üzerinden 443 portundan çalışssın dedim ve Ngrok'u 443 portundan yayınladım :).

Sistem çalışmıştı tabi bunu test için yapmıştım. Socket kullanmamın avantajı,  haberleşme real-time oldugu ıcın aradaki etkileşimin etki-tepki süresi daha çok kısalacaktır bu da biraz performans demektir. WEBRTC nin harika bir şey olduğundan öte birkaç sorulara cevapta vermek istiyorum. <br/>
Misal :<br/>
VPN leri aşar mı ? Evet.  Bunun meşhur VPN lerde sızıntı durumu olduğu size bildirilir.<br/>

Yada iletim güvenlimi ? Evet. DTLS ve SCTP protokolleri ile bu güvenlilik sağlanır. Ayrıca bu yapının genel koruyucuları Browserlardır. Bir bug oldugunda müdahale edilecektir.<br/>

NAT'ı delemiyor veya Firewall'ı aşamıyorsa ? İletişimi sağlayacak ara sunucular yani TURN serverların adreslerine yönlendirilir. Tabiki TURN serverlar sizin serverlarınızdır. Data burda toplanır direk diğer Peer Objesine iletilir.<br/> 

Stun Serverım olmalı mı ? Olabilir veya ücretsiz kullanabileceğiniz Google ve Mozilla gibi şirketlerin sağladığı serverlar vardır. Unutmayın ki bu serverlarda Private ve Public iplerin belirlenmesınden dolayı gizliliği ihmal edebilecek durumlar vardır. Dikkatlı kullanmak gerekiyor. <br/>
...<br/>
..<br/>
.<br/>
vb.

#### Sonuç :
İşlem basit gibi olsa da çok farklı yerlerde umulmadık hatalar çıkabiliyor.
Misal : <br/>
DataChannellar üzerinde gönderilecek maksimum byte sayısından tutun ekran paylaşımları ya da anlık kamera değişime kadar neler olabileceği hakkında testler üretmek gerekiyor. <br/>

Pooling [XHR] yapmadıgınızı farklı protokollerle calıstıgını farketmeniz gerekiyor. [RTP]. Farklı API lara IndexedDb - Data Typelar Blob Arraybuffer gibi kavramlara hakim olmak gerekiyor. <br/> 

Daha önceki projem 1 haftamı almışken 3 Haftamı bu projeye vermek zorunda kaldım. WebSocket kavramına hakim değildim ve WebRTC ile birleştirerek harika bir projenin ufak bir prototipini ortaya koydum. Aynı zamanda facebook üzerinden arkadaşla denediğimiz sesli görüşmede bu teknolojinin kullanıldıgına şahit olduk. <br/>  [chrome://webrtc-internal]  Chrome için arama yaptıgınızda diğer sekmede akışı görebilirsiniz.

Tabiki yapıyı anlamak en çok zaman alan kısım..  Bununla ilgili pek iç açıcı türkçe bilginin olmaması bazen sizi RFC dökümanlarına kadar baktırıp "hıım" dedirtebiliyor..  Eğer derin bir öğrenme gerçekleşecekse RFC , MDN bu işin olmazsa olmazları.

**DipNot** : Her ne kadar bununla alakalı kütüphaneler ortalarda dolassa da Simple-Peer gibi. yöntemler hep değişiyor. Eğer ki değişiklikler bu kütüphanelere yansıtılmazsa olayın nasıl calıstıgını anlamanız gerekiyor. Issuelarını takip edebilirsiniz hem kodunuzu daha iyi bir hale getirecektir. En büyük referans alacağınız kişinin : muaz-khan oldugunu buraya ekliyorum. Aratmanızda göz gezdirmenizde fayda var. Çok eski yazdıklarına dikkat ederseniz buglar hep var. Tarayıcılarda oturmak üzere olan bir teknoloji ve gerisi sizde bol şans.

>Yaptığım projeyi public edemiyorum geliştirmeyi düşünüyorum. Ama bu yazıyı anlamanız ve referans almanız açısından yayınlama ihtiyacı hissettim. Umarım 1 kişide olsa anlattıklarım kafasında canlanmaya yetebilir..

