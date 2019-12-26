## ScreenShots
<img src="https://github.com/kmrtylmz/socket-io-webrtc/blob/master/assets/tests/Test-1.jpg" width="90%" />
<img src="https://github.com/kmrtylmz/socket-io-webrtc/blob/master/assets/tests/Test-2.jpg" width="90%" />




## Geliştirme Ortamı
### WEbRTC & WebSocket
P2PCONNECTION [Peer To Peer] teknolojisini destekleyen protokol. Web ve mobil platformlarında desteklenen , tarayıcılar tarafından gömülü, hiç bir eklenti kurulmasına gerek kalmadan real-time ses ve data aktarımı iletilmesini sağlayan yapı. Symetric NATlar harici STUN Serverlar aracılığı ile NAT'ı ve Firewall'ı delerek iletişimde kullanılacak Public Ip adreslerinin ve Streamlerin metadatalarının eşleştirilmesiyle iletişim sağlanır. Eşler arası bağlantıda arada sunucu yoktur. Eşlerin birbirlerini tanıması için "Sinyal Server" gereksinimi vardır ve bu sinyal serverın ne olacağının kararı tamamen bağımsızdır [JSEP]. Bunun için en ideal yöntemin real-time sunucu bazlı [HTTP nın TCP bağlantısı üzerinde Upgrade olmuş hali] Websocket teknolojisi yer alır. Websocketler tarayıcıda bir API olarak yer alır ancak gerçek WEbsocket deneyimi katmaz. Bu yüzden WEbsocket için gelişmiş bir kütüphaneye ihtiyaç duyulur. µWebSockets , Ws , ClusterWS gibi tercihlerin yanı sıra daha populer olan socket.io'yu kullandım.

WebRTC open-source ve Google Chrome ekibi tarafından yürütülmektedir. Ayrıca sadece HTTPS bağlantılarda geçerlidir. PHP tarafında çalışırken test aşamasında farklı yöntemlere yöneldim. OpenSSL ile bu durumu aşacağımı düşünerek bir sunucu kiralanması yada WAN ayarlarını değiştirmekten öte localhostu Ngrok ile [proxy] yayınladım. 1 sunucu hakkım oldugu ıcın Apache tarafında da PHP ile işlemin çok olmamasından dolayı NodeJS tarafında Kendinden imzalı sertifikalarla HTTPS webServer kurup Express Frameworkunu bu servera bağlayıp Socket Serverıda bu webserver üzerinden 443 portundan çalışssın dedim :).

Bu şekilde karşılıklı haberleşme real-time oldugu ıcın etki-tepki süresi daha çok kısalacaktır. Akıla takılan problemler olabiliyor VPN leri aşar mı ? Evet. Yada iletim güvenlimi ? Evet. DTLS ve SCTP protokolleri ile bu güvenlilik sağlanır. Ayrıca bu yapının genel koruyucuları Browserlardır. Bir bug oldugunda müdahale edilecektir. NAT'ı delemiyorsa ? İletişimi sağlayacak ara sunucular yani TURN serverların adreslerine yönlendirilir. Tabiki TURN serverlar sizin serverlarınızdır. Data burda toplanır direk Peer Objesine iletilir. Stun Serverım olmalı mı ? Olabilir veya ücretsiz kullanabileceğiniz Google ve Mozilla gibi şirketlerin sağladığı serverlar vardır. Unutmayın ki bu serverlarda Private ve Public iplerin belirlenmesınden dolayı gizliliği ihmal edebilecek durumlar vardır. Dikkatlı kullanmak gerekiyor. <br/>
...<br/>
..<br/>
.<br/>
vb.

#### Sonuç :
Detaylarla boğuşmak gerek o kadar sanıldığı kadar basit bir olay değil. DataChannellar üzerinde gönderilecek maksimum byte sayısından tutun ekran paylaşımları ya da anlık kamera değişime kadar neler olabileceği hakkında testler üretmek gerekiyor. Pooling [XHR] yapmadıgınızı farklı protokollerle calıstıgını farkedin [RTP]. Farklı API lara IndexedDb - Data Typelar Blob Arraybuffer gibi kavramlara hakim olmak gerekiyor. Daha önceki projem 1 haftamı almışken 3 Haftamı bu projeye vermek zorunda kaldım. WebSocket kavramına hakim değildim ve WebRTC ile birleştirerek harika bir projenin ufak bir prototipini ortaya koydum. Tabiki yapıyı anlamak en çok zaman alan kısım.. ne yapılsa yapılsın tecrübe yazılım geliştirme sürecinin en etkili rolüdür. Bununla ilgili pek iç açıcı türkçe bilginin olmaması bazen sizi RFC dökümanlarına kadar baktırıp "hıım" dedirtebiliyor..

**DipNot** : Her ne kadar bununla alakalı kütüphaneler konulsada Simple-Peer vb. yöntemler hep değişiyor. Eğer ki değişiklikler bu kütüphanelere yansıtılmazsa olayın nasıl calıstıgını anlamanız gerekiyor. En büyük referans alacağınız kişinin : muaz-khan oldugunu buraya ekliyorum.

>Yaptığım şey kendime ait bir proje olduğundan public edemiyorum. Ama bu yazıyı anlamanız ve referans almanız açısından yayınlama ihtiyacı hissettim. Umarım 1 kişide olsa anlattıklarım kafasında canlanmaya yetebilir..
