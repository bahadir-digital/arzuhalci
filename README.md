# /arzuhalci — Claude ile Dilekçe Hazırlama Skill'i

Türk hukuk sistemine uygun, mahkemenin kabul edeceği formatta dilekçe hazırlayan bir Claude skill'i. Usta bir arzuhalci gibi çalışır: derdinizi dinler, dilekçe türünü teşhis eder, eksik bilgiyi adım adım sorar ve doğru formatta belgeyi üretir.

Bu skill'in ortaya çıkış hikayesi ve detaylı anlatımı için blog yazısı: [Arzuhalcinin Sattığı Şey](https://bahadir.digital/arzuhalci-claude-skill)

## Ne yapıyor?

- **8 dilekçe türünü destekliyor:** Dava dilekçesi (HMK 119), idari dava (İYUK 3), icra takibine itiraz, savcılığa suç duyurusu, tüketici hakem heyeti başvurusu, ihtarname, cevap dilekçesi ve istinaf/temyiz.
- **Dilekçe türünü kendisi teşhis ediyor.** "Dava açmak istiyorum", "itiraz edeceğim", "şikayetçi olacağım" demeniz yeterli; hangi tür dilekçe gerektiğini anlattıklarınızdan çıkarıyor.
- **Eksik bilgiyi vatandaş diliyle soruyor.** Hukuk terimleriyle değil, kademeli ve anlaşılır sorularla. Anlattığınız hiçbir şeyi tekrar sormuyor.
- **Görevli ve yetkili mahkemeyi kendisi buluyor.** Size sormuyor, sadece teyit ettiriyor.
- **Yasal süreleri hesaplayıp uyarıyor.** İdari davada 60 gün, icra itirazında 7 gün, şikayete bağlı suçlarda 6 ay gibi hak düşürücü süreleri sizin tarihinize göre hesaplıyor ve daha ilk mesajlarda bildiriyor.
- **yargi-mcp bağlıysa gerçek emsal kararlarla güçlendiriyor.** Dilekçenin Hukuki Sebepler bölümüne Yargıtay/Danıştay kararlarından atıf ekliyor. En katı kural: emsal karar asla hafızadan yazılmıyor; esas/karar numaraları yalnızca [yargi-mcp](https://github.com/saidsurucu/yargi-mcp) üzerinden fiilen getirilen kararlardan alınıyor. Sunucu bağlı değilse dilekçe emsalsiz üretiliyor ve bu durum açıkça söyleniyor.
- **Teslim kılavuzu üretiyor.** Her dilekçenin son sayfasında: belge nereye verilecek, UYAP Vatandaş veya e-Devlet'ten gönderilebilir mi, kaç nüsha gerekiyor, harç var mı, teslimden sonra süreç nasıl takip edilecek.
- **Güncel tutarları web'den doğruluyor.** Tüketici hakem heyeti parasal sınırı, harçlar gibi her yıl değişen değerler skill'e gömülü değil; kullanım anında doğrulanıyor.
- **Kişisel verilere temkinli yaklaşıyor.** T.C. kimlik numarası gibi bilgileri sohbete yazmanızı istemiyor; bu alanları yer tutucu olarak bırakıp çıktıda kendinizin doldurmasını öneriyor.

## Ne yapmıyor?

Bu skill bir avukat değildir ve hukuki tavsiye vermez. Her çıktının sonunda belgenin bilgilendirme amaçlı olduğunu ve kesin hukuki değerlendirme için bir avukata danışılması gerektiğini belirtir. Özellikle ceza hukuku gibi geri dönüşü zor sonuçlar doğurabilecek konularda bu uyarıyı dilekçeden önce de yapar.

## Kurulum

### 1. Skill'i yükleyin

1. Bu depodaki `arzuhalci.skill` dosyasını indirin (veya Releases bölümünden alın).
2. [claude.ai](https://claude.ai) hesabınızda **Settings > Capabilities** bölümüne gidin.
3. Skills alanından dosyayı yükleyin.

### 2. yargi-mcp connector'ını ekleyin (isteğe bağlı ama önerilir)

Emsal Yargıtay/Danıştay kararı desteği için:

1. claude.ai'de **Settings > Connectors > Add custom connector** yolunu izleyin.
2. URL alanına şunu girin: `https://yargimcp.surucu.dev/mcp`
3. Bağlantıyı onaylayın.

yargi-mcp, [Said Sürücü](https://github.com/saidsurucu) tarafından geliştirilen açık kaynak bir MCP sunucusudur; Yargıtay, Danıştay, Anayasa Mahkemesi dahil pek çok Türk hukuk veritabanına erişim sağlar. Kurulum seçeneklerinin tamamı için [projenin sayfasına](https://github.com/saidsurucu/yargi-mcp) bakın.

### 3. Kullanmaya başlayın

Yeni bir sohbette derdinizi anlatmanız yeterli:

> "Kiracım 4 aydır kira ödemiyor, ne yapabilirim?"
> "İnternetten aldığım telefon bozuk çıktı, satıcı iade almıyor."
> "Trafik cezasına itiraz etmek istiyorum."

## Dosya yapısı

```
arzuhalci/
├── SKILL.md                        # Yönlendirici: teşhis → sorgu → üretim akışı
└── references/
    ├── dava-dilekcesi.md           # Hukuk mahkemesi dava dilekçesi (HMK 119)
    ├── idari-dava.md               # İdari dava (İYUK 3)
    ├── icra.md                     # İcra takibine itiraz (İİK)
    ├── ceza.md                     # Suç duyurusu / şikayet
    ├── tuketici.md                 # Tüketici hakem heyeti başvurusu
    ├── ihtarname.md                # Noter ihtarnamesi
    ├── cevap-istinaf-temyiz.md     # Cevap dilekçesi, istinaf, temyiz
    └── sureler.md                  # Hak düşürücü süreler ve zamanaşımı tablosu
```

Skill, progressive disclosure yaklaşımıyla çalışır: Claude yalnızca teşhis ettiği dilekçe türünün referans dosyasını okur, böylece bağlam gereksiz yere şişmez.

## Sorumluluk reddi

Bu skill'in ürettiği belgeler bilgilendirme amaçlıdır ve hukuki tavsiye niteliği taşımaz. Dilekçelerin hukuki sonuç doğuran belgeler olduğunu unutmayın; önemli konularda mutlaka bir avukata danışın. Skill'in ürettiği içeriklerden doğabilecek sonuçların sorumluluğu kullanıcıya aittir.

## Katkı ve geri bildirim

Hata bildirimi ve önerileriniz için issue açabilirsiniz. Türkçe Claude içerikleri ve diğer skill paketleri için:

- Blog: [bahadir.digital](https://bahadir.digital)
- Claude.ai Türkiye WhatsApp topluluğu: [katılım linki](https://chat.whatsapp.com/I2763pKIpmS9xADAHLaGPE)

## Lisans

MIT
