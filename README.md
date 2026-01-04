# YGA Test
import React, { useState, useEffect } from 'react';
import { ChevronRight, CheckCircle2, Circle } from 'lucide-react';

const YGAInterestTest = () => {
  const [stage, setStage] = useState('welcome'); // welcome, test, ranking, results
  const [currentQuestion, setCurrentQuestion] = useState(0);
  const [answers, setAnswers] = useState({});
  const [rankingAnswers, setRankingAnswers] = useState({});
  const [scores, setScores] = useState({ R: 0, I: 0, A: 0, S: 0, E: 0, C: 0 });

  const forcedChoiceQuestions = [
    {
      q: "Bir YGA etkinliğinde hangi rolü almak daha çok zevk verir?",
      options: [
        { text: "Sahne düzenini kurmak ve teknik ekipmanları ayarlamak", type: "R" },
        { text: "Katılımcılarla sohbet edip onları rahat ettirmek", type: "S" },
        { text: "Etkinlik programını detaylı şekilde planlamak", type: "C" }
      ]
    },
    {
      q: "Boş bir cumartesi günün var. Ne yapmayı tercih edersin?",
      options: [
        { text: "Yeni bir araştırma makalesini okumak veya belgesel izlemek", type: "I" },
        { text: "Arkadaşlarınla bir şeyler organize etmek", type: "E" },
        { text: "Bir hobi projesinde yaratıcı bir şey üretmek", type: "A" }
      ]
    },
    {
      q: "Bir bilim sahasında çocuklar ilgisiz görünüyor. Ne yaparsın?",
      options: [
        { text: "Deneyi daha görsel ve eğlenceli hale getiririm", type: "A" },
        { text: "Çocuklarla birebir konuşup ne merak ettiklerini anlamaya çalışırım", type: "S" },
        { text: "Deneyin bilimsel mantığını farklı şekilde açıklarım", type: "I" }
      ]
    },
    {
      q: "YGA'da yeni bir proje başlatılacak. Hangi kısmında yer almak istersin?",
      options: [
        { text: "Projenin nasıl çalışacağını analiz edip araştırma yapmak", type: "I" },
        { text: "Projeyi paydaşlara sunmak ve destek toplamak", type: "E" },
        { text: "Proje dosyalarını düzenlemek ve takibi yapmak", type: "C" }
      ]
    },
    {
      q: "Hangi tür görevlerde kendini daha enerjik hissedersin?",
      options: [
        { text: "Fiziksel bir şey yapmak (kurulum, düzenleme, taşıma)", type: "R" },
        { text: "İnsanlarla konuşmak, dinlemek, yardım etmek", type: "S" },
        { text: "Yaratıcı içerik üretmek (yazı, görsel, video)", type: "A" }
      ]
    },
    {
      q: "Twin bilim setleriyle ilgili bir görev var. Hangisini seçersin?",
      options: [
        { text: "Setleri kurmak ve çocuklara nasıl kullanılacağını göstermek", type: "R" },
        { text: "Setlerle yapılabilecek yeni deneyler araştırmak", type: "I" },
        { text: "Setlerin envanter takibini ve lojistiğini yönetmek", type: "C" }
      ]
    },
    {
      q: "Bir webinar hazırlığı yapılıyor. Hangi görevi tercih edersin?",
      options: [
        { text: "Konuşmacıların sunumlarını tasarlamak", type: "A" },
        { text: "Katılımcı kayıtlarını organize etmek ve hatırlatmalar göndermek", type: "C" },
        { text: "Webinar akışını yönetmek ve ekibi koordine etmek", type: "E" }
      ]
    },
    {
      q: "Hangi durumda daha mutlu olursun?",
      options: [
        { text: "Karmaşık bir problemi çözdüğünde", type: "I" },
        { text: "Birine yardım edip onu mutlu ettiğinde", type: "S" },
        { text: "Orijinal bir fikir üretip hayata geçirdiğinde", type: "A" }
      ]
    },
    {
      q: "YGA için yurt dışı bir toplantıya gidiyorsun. Neler yaparsın?",
      options: [
        { text: "Sunumları hazırlamak ve temsil etmek", type: "E" },
        { text: "Toplantı notlarını tutmak ve raporlamak", type: "C" },
        { text: "Oradaki insanlarla tanışmak ve ilişki kurmak", type: "S" }
      ]
    },
    {
      q: "Bilim Seferberliği için yeni bir lokasyon araştırması yapılıyor. Hangisinde yer alırsın?",
      options: [
        { text: "Verileri analiz edip en uygun yeri bulmak", type: "I" },
        { text: "Yerel paydaşlarla görüşmeler yapmak", type: "E" },
        { text: "Lojistik planlamayı ve bütçeyi hesaplamak", type: "C" }
      ]
    },
    {
      q: "Bir kampda hangi aktivitede olmak daha zevkli?",
      options: [
        { text: "Gece ateş başında hikaye anlatmak veya oyun yönetmek", type: "A" },
        { text: "Yeni katılanlarla tanışıp onlara rehberlik etmek", type: "S" },
        { text: "Kampın günlük programını organize etmek", type: "C" }
      ]
    },
    {
      q: "Aşağıdaki çalışma ortamlarından hangisinde daha verimli olursun?",
      options: [
        { text: "Atölyede, makerspace'te, fiziksel şeyler üzerinde çalışırken", type: "R" },
        { text: "İnsanlarla dolu bir ortamda, sürekli etkileşimde", type: "S" },
        { text: "Sessiz bir yerde, düşünüp araştırma yaparken", type: "I" }
      ]
    },
    {
      q: "Twin uygulaması için yeni bir özellik geliştiriliyor. Hangi kısmında olmak istersin?",
      options: [
        { text: "Kullanıcı deneyimini (UX) tasarlamak", type: "A" },
        { text: "Teknik altyapıyı kodlamak", type: "I" },
        { text: "Kullanıcı testleri yapıp feedback toplamak", type: "S" }
      ]
    },
    {
      q: "Zirve hazırlığında hangi görevi tercih edersin?",
      options: [
        { text: "Sahne tasarımını ve dekorasyonu yapmak", type: "A" },
        { text: "Katılımcı deneyimini planlamak ve akışı yönetmek", type: "E" },
        { text: "Kayıtları ve katılımcı listelerini yönetmek", type: "C" }
      ]
    },
    {
      q: "Yeni bir gönüllüyle çalışıyorsun. Nasıl yaklaşırsın?",
      options: [
        { text: "Ona görevlerini net şekilde açıklarım ve takip ederim", type: "C" },
        { text: "Onunla oturup tanışırım, neye ihtiyacı var dinlerim", type: "S" },
        { text: "Ona zorlayıcı bir görev verip öğrenmesini izlerim", type: "E" }
      ]
    },
    {
      q: "Hangi başarı seni daha çok tatmin eder?",
      options: [
        { text: "Bir şeyi baştan sona kendin ürettiğinde", type: "R" },
        { text: "Bir sorunu analiz edip çözüm bulduğunda", type: "I" },
        { text: "Bir ekibi başarıya taşıdığında", type: "E" }
      ]
    },
    {
      q: "Content ekibi için yeni bir video çekiliyor. Nerede olmak istersin?",
      options: [
        { text: "Kamera ve ışık ayarlarını yapmak", type: "R" },
        { text: "Senaryo ve konsept geliştirmek", type: "A" },
        { text: "Çekimleri organize edip ekibi yönetmek", type: "E" }
      ]
    },
    {
      q: "Global Expansion için bir sunum hazırlanıyor. Hangisini yaparsın?",
      options: [
        { text: "Verileri araştırıp stratejik analiz yapmak", type: "I" },
        { text: "Sunumu görsel ve çekici hale getirmek", type: "A" },
        { text: "Sunumu yapmak ve paydaşları ikna etmek", type: "E" }
      ]
    },
    {
      q: "Bir bilim sahasında teknik bir sorun oluştu. Ne yaparsın?",
      options: [
        { text: "Sorunu hemen kendim çözmeye çalışırım", type: "R" },
        { text: "Ekiple birlikte beyin fırtınası yaparım", type: "S" },
        { text: "Problemi sistematik şekilde analiz edip çözüm ararım", type: "I" }
      ]
    },
    {
      q: "YGA'nın sosyal medya içerikleri için ne yapmak istersin?",
      options: [
        { text: "Görseller ve videolar tasarlamak", type: "A" },
        { text: "Analitikleri takip edip strateji geliştirmek", type: "I" },
        { text: "Takipçilerle etkileşime geçmek ve community yönetmek", type: "S" }
      ]
    },
    {
      q: "Sustainability projeleri için hangi rolde olmak istersin?",
      options: [
        { text: "Çevresel etki analizi yapmak", type: "I" },
        { text: "Paydaşlarla iş birlikleri kurmak", type: "E" },
        { text: "Projelerin düzenli raporlamasını yapmak", type: "C" }
      ]
    },
    {
      q: "Hangi tür toplantılarda daha aktif ve enerjik olursun?",
      options: [
        { text: "Beyin fırtınası ve yaratıcı çalışma toplantıları", type: "A" },
        { text: "Stratejik planlama ve karar alma toplantıları", type: "E" },
        { text: "Detaylı veri analizi ve raporlama toplantıları", type: "I" }
      ]
    },
    {
      q: "Program Development için yeni bir kamp tasarlanıyor. Nerede katkı sağlarsın?",
      options: [
        { text: "Kampın eğitim içeriğini araştırmak", type: "I" },
        { text: "Kampın interaktif aktivitelerini tasarlamak", type: "A" },
        { text: "Kampın lojistiğini ve timeline'ını planlamak", type: "C" }
      ]
    },
    {
      q: "People & Culture ekibinde hangi görevi tercih edersin?",
      options: [
        { text: "Gönüllülerle 1:1 görüşmeler yapıp onları dinlemek", type: "S" },
        { text: "Engagement anketleri ve veri analizi", type: "I" },
        { text: "Onboarding süreçlerini sistematize etmek", type: "C" }
      ]
    },
    {
      q: "Bir çocuk sahada 'Ben bunu yapamam' diyor. Ne yaparsın?",
      options: [
        { text: "Ona adım adım gösteririm, beraber yaparız", type: "S" },
        { text: "Onu farklı bir yaklaşım denemeye teşvik ederim", type: "A" },
        { text: "Ona neden yapabileceğini mantıksal olarak açıklarım", type: "I" }
      ]
    },
    {
      q: "GSC (Global Sustainability Challenge) için ne yapmak istersin?",
      options: [
        { text: "Katılımcı ekiplere mentoring yapmak", type: "S" },
        { text: "Yarışmanın organizasyonunu ve jüri sürecini yönetmek", type: "E" },
        { text: "Projlerin teknik kalitesini değerlendirmek", type: "I" }
      ]
    },
    {
      q: "Satış ekibinde hangi rolü tercih edersin?",
      options: [
        { text: "Potansiyel müşterilerle görüşmeler yapmak", type: "E" },
        { text: "Satış verilerini analiz edip strateji geliştirmek", type: "I" },
        { text: "CRM sistemini düzenlemek ve takip yapmak", type: "C" }
      ]
    },
    {
      q: "Twin AI Companion için hangi kısımda çalışmak istersin?",
      options: [
        { text: "Algoritma ve makine öğrenmesi geliştirmek", type: "I" },
        { text: "Kullanıcı arayüzünü tasarlamak", type: "A" },
        { text: "Kullanıcı testleri yapıp feedback toplamak", type: "S" }
      ]
    },
    {
      q: "Bir proje başarısız oldu. Nasıl tepki verirsin?",
      options: [
        { text: "Neyin yanlış gittiğini analiz ederim", type: "I" },
        { text: "Ekibin moralini yüksek tutmaya çalışırım", type: "S" },
        { text: "Hemen yeni bir plan yapar, harekete geçerim", type: "E" }
      ]
    },
    {
      q: "Technology ekibinde hangi rolü tercih edersin?",
      options: [
        { text: "Backend development ve database yönetimi", type: "I" },
        { text: "Frontend development ve görsel tasarım", type: "A" },
        { text: "Bug fixing ve sistem bakımı", type: "R" }
      ]
    },
    {
      q: "Ruanda Eğitim Bakanlığı ile görüşme var. Hazırlıkta nasıl katkı sağlarsın?",
      options: [
        { text: "Sunum materyallerini hazırlamak", type: "A" },
        { text: "Görüşmeyi yönetmek ve müzakere etmek", type: "E" },
        { text: "Arka planda veri ve raporlar hazırlamak", type: "I" }
      ]
    },
    {
      q: "Spokes bisiklet projesi için hangi görevi tercih edersin?",
      options: [
        { text: "Rota planlaması ve lojistik", type: "C" },
        { text: "Yol boyunca çocuklarla etkileşim", type: "S" },
        { text: "Projenin hikayesini anlatmak ve belgelemek", type: "A" }
      ]
    },
    {
      q: "Gönüllendirin (Marketing) ekibinde ne yapmak istersin?",
      options: [
        { text: "Kampanya stratejileri geliştirmek", type: "E" },
        { text: "Yaratıcı içerikler üretmek", type: "A" },
        { text: "Kampanya metriklerini takip etmek", type: "C" }
      ]
    },
    {
      q: "Bir çocuk bir deneyde beklenmedik bir sonuç buldu. Ne yaparsın?",
      options: [
        { text: "'Vay! Bunu neden oldu acaba? Araştıralım!' derim", type: "I" },
        { text: "'Harika bir keşif yaptın!' diyerek onu kutlarım", type: "S" },
        { text: "'Farklı bir şey deneyelim belki daha ilginç olur' derim", type: "A" }
      ]
    },
    {
      q: "Twin Digital Library için hangi kısımda çalışmak istersin?",
      options: [
        { text: "İçerik kataloglaması ve organizasyonu", type: "C" },
        { text: "Yeni eğitim materyalleri geliştirmek", type: "I" },
        { text: "Kullanıcı engagement stratejileri", type: "E" }
      ]
    },
    {
      q: "Bilim Seferberliği'nin etkisini nasıl ölçmek istersin?",
      options: [
        { text: "Veri toplama ve istatistiksel analiz", type: "I" },
        { text: "Çocuklarla röportajlar yapma", type: "S" },
        { text: "Raporlama ve görselleştirme", type: "A" }
      ]
    },
    {
      q: "Yeni bir gönüllü kampa geldi ve kaybolmuş görünüyor. Ne yaparsın?",
      options: [
        { text: "Ona yaklaşır, kendimi tanıtır, yardım teklif ederim", type: "S" },
        { text: "Ona kampın haritasını ve programını detaylı açıklarım", type: "C" },
        { text: "Onu gruba dahil eder, tanışmasını sağlarım", type: "E" }
      ]
    },
    {
      q: "Product ekibinde hangi görevi tercih edersin?",
      options: [
        { text: "Kullanıcı ihtiyaçlarını araştırmak", type: "I" },
        { text: "Product roadmap ve prioritization", type: "E" },
        { text: "Feature testleri ve QA", type: "C" }
      ]
    },
    {
      q: "Hangi tür görevlerde zaman nasıl geçtiğini anlamazsın?",
      options: [
        { text: "Bir şeyleri tamir ederken veya monte ederken", type: "R" },
        { text: "Araştırma yaparken veya veri analiz ederken", type: "I" },
        { text: "Yaratıcı çalışırken (yazarken, tasarlarken)", type: "A" }
      ]
    },
    {
      q: "Program Development'ta hangi kısma odaklanırsın?",
      options: [
        { text: "Gönüllü deneyimini iyileştirmek", type: "S" },
        { text: "Program etkisini ölçümlemek", type: "I" },
        { text: "Süreçleri optimize etmek", type: "C" }
      ]
    },
    {
      q: "Bir toplantıda hangi rol sana daha doğal gelir?",
      options: [
        { text: "Not alan, görevleri takip eden", type: "C" },
        { text: "Yaratıcı fikirler öneren", type: "A" },
        { text: "Toplantıyı yöneten, kararları organize eden", type: "E" }
      ]
    },
    {
      q: "Content üretimi için hangi formatı tercih edersin?",
      options: [
        { text: "Yazılı içerik (blog, makale)", type: "I" },
        { text: "Video içerik (çekim, kurgu)", type: "A" },
        { text: "İnfografik ve data visualization", type: "A" }
      ]
    },
    {
      q: "Global Expansion için yeni bir ülke araştırması yapılıyor. Nasıl katkı sağlarsın?",
      options: [
        { text: "Ülkenin eğitim sistemini analiz etmek", type: "I" },
        { text: "Yerel ortaklarla networking yapmak", type: "E" },
        { text: "Feasibility raporunu hazırlamak", type: "C" }
      ]
    },
    {
      q: "Bir çocuğun ailesi sahaya geldi ve YGA'yı merak ediyor. Ne yaparsın?",
      options: [
        { text: "Onlarla sıcak bir şekilde sohbet eder, YGA'yı anlatırım", type: "S" },
        { text: "Onlara YGA'nın etki raporlarını ve verilerini gösteririm", type: "I" },
        { text: "Onları bir bilim deneyi yapmaya davet ederim", type: "R" }
      ]
    },
    {
      q: "Technology stack kararı alınıyor. Hangi yaklaşımı tercih edersin?",
      options: [
        { text: "En yeni teknolojileri araştırıp denerim", type: "I" },
        { text: "Ekiple konuşur, ihtiyaçları dinlerim", type: "S" },
        { text: "Mevcut sistemlerle uyumlu, stabil olanı seçerim", type: "C" }
      ]
    },
    {
      q: "Sustainability projelerinde hangi kısım seni daha çok heyecanlandırır?",
      options: [
        { text: "Carbon footprint hesaplama ve analiz", type: "I" },
        { text: "Yeşil kampanyalar tasarlama", type: "A" },
        { text: "Paydaşlarla sürdürülebilirlik anlaşmaları yapmak", type: "E" }
      ]
    },
    {
      q: "People & Culture için yeni bir initiative başlatılıyor. Hangisini tercih edersin?",
      options: [
        { text: "Mentoring programı tasarlamak", type: "S" },
        { text: "Gönüllü gelişim framework'ü oluşturmak", type: "I" },
        { text: "Team building aktiviteleri organize etmek", type: "E" }
      ]
    },
    {
      q: "Bilim Seferberliği için yeni bir deney seti geliştiriliyor. Nerede olmak istersin?",
      options: [
        { text: "Deneyin bilimsel içeriğini tasarlamak", type: "I" },
        { text: "Fiziksel materyalleri kurmak ve test etmek", type: "R" },
        { text: "Deneyin anlaşılır şekilde çocuklara anlatılmasını sağlamak", type: "S" }
      ]
    },
    {
      q: "GSC için sponsorluk görüşmeleri yapılıyor. Hangi rolü tercih edersin?",
      options: [
        { text: "Sponsorluk paketlerini hazırlamak", type: "C" },
        { text: "Sponsorlarla görüşmeler yapmak", type: "E" },
        { text: "Sponsorluk etkisini ölçümlemek", type: "I" }
      ]
    },
    {
      q: "Hangi tür çalışma seni daha çok motive eder?",
      options: [
        { text: "Uzun vadeli, stratejik projeler", type: "I" },
        { text: "Hızlı, dinamik, kısa döngülü işler", type: "R" },
        { text: "Düzenli, öngörülebilir, sistemli işler", type: "C" }
      ]
    },
    {
      q: "Satış ekibi için yeni bir pitch hazırlanıyor. Nasıl katkı sağlarsın?",
      options: [
        { text: "Pitch storyline ve messaging", type: "E" },
        { text: "Müşteri segmentasyonu ve analiz", type: "I" },
        { text: "Pitch deck tasarımı", type: "A" }
      ]
    },
    {
      q: "Twin AI Companion'da bir bug var. Nasıl yaklaşırsın?",
      options: [
        { text: "Hemen kodu inceleyip düzeltmeye çalışırım", type: "I" },
        { text: "Bug'ın kullanıcıları nasıl etkilediğini anlarım", type: "S" },
        { text: "Bug tracking sistemine kaydeder, prioritize ederim", type: "C" }
      ]
    },
    {
      q: "Gönüllendirin kampanyası için yeni bir strateji gerekiyor. Ne yaparsın?",
      options: [
        { text: "Geçmiş verileri analiz edip insight çıkarırım", type: "I" },
        { text: "Yaratıcı bir kampanya konsepti tasarlarım", type: "A" },
        { text: "Kampanyayı adım adım planlarım", type: "C" }
      ]
    },
    {
      q: "Bir projede ekip içinde fikir ayrılığı var. Ne yaparsın?",
      options: [
        { text: "Herkesin görüşünü dinler, ortak zemini ararım", type: "S" },
        { text: "Verilere bakar, en mantıklı çözümü bulurum", type: "I" },
        { text: "Karar alır, ekibi yönlendiririm", type: "E" }
      ]
    }
  ];

  const rankingQuestions = [
    {
      q: "Bir YGA projesi için aşağıdaki görevleri sırala (1=En çok keyif alırım, 4=En az keyif alırım):",
      options: [
        { text: "Projenin teknik altyapısını kurmak", type: "R" },
        { text: "Projenin etkisini araştırmak ve analiz etmek", type: "I" },
        { text: "Proje için yaratıcı içerikler üretmek", type: "A" },
        { text: "Proje ekibini yönetmek ve koordine etmek", type: "E" }
      ]
    },
    {
      q: "Bir bilim sahasında aşağıdaki aktiviteleri sırala:",
      options: [
        { text: "Setleri kurmak ve teknik hazırlıkları yapmak", type: "R" },
        { text: "Çocuklarla birebir ilgilenmek ve yardım etmek", type: "S" },
        { text: "Sahanın organizasyonunu ve lojistiğini yönetmek", type: "C" },
        { text: "Deneylerin bilimsel içeriğini geliştirmek", type: "I" }
      ]
    },
    {
      q: "Bir kampanya için aşağıdaki görevleri sırala:",
      options: [
        { text: "Kampanya görsellerini ve videolarını tasarlamak", type: "A" },
        { text: "Kampanyayı sosyal medyada yürütmek ve yönetmek", type: "E" },
        { text: "Kampanya metriklerini takip etmek ve raporlamak", type: "C" },
        { text: "Hedef kitle analizi ve strateji geliştirmek", type: "I" }
      ]
    },
    {
      q: "Twin için yeni bir özellik geliştiriliyor. Aşağıdaki rolleri sırala:",
      options: [
        { text: "Kod yazmak ve backend geliştirmek", type: "I" },
        { text: "Kullanıcı arayüzünü tasarlamak", type: "A" },
        { text: "Kullanıcılarla test yapıp feedback almak", type: "S" },
        { text: "Özelliği düzenli olarak test edip bug çıkarmak", type: "C" }
      ]
    },
    {
      q: "Bir toplantıda aşağıdaki rolleri sırala:",
      options: [
        { text: "Yaratıcı fikirler üretmek ve brainstorming yönetmek", type: "A" },
        { text: "Toplantıyı yönetmek ve kararları netleştirmek", type: "E" },
        { text: "Toplantı notlarını tutmak ve görevleri dağıtmak", type: "C" },
        { text: "Veri ve analiz sunmak", type: "I" }
      ]
    },
    {
      q: "Global Expansion için aşağıdaki görevleri sırala:",
      options: [
        { text: "Yeni ülkelerin pazar araştırmasını yapmak", type: "I" },
        { text: "Uluslararası ortaklarla görüşmeler yapmak", type: "E" },
        { text: "Expansion dokümanlarını ve planlarını hazırlamak", type: "C" },
        { text: "Expansion hikayesini anlatmak için içerik üretmek", type: "A" }
      ]
    },
    {
      q: "People & Culture için aşağıdaki aktiviteleri sırala:",
      options: [
        { text: "Gönüllülerle 1:1 görüşmeler yapıp onları dinlemek", type: "S" },
        { text: "Gönüllü memnuniyet anketleri ve analiz", type: "I" },
        { text: "Onboarding süreçlerini dokümante etmek", type: "C" },
        { text: "Team building aktiviteleri tasarlamak", type: "A" }
      ]
    },
    {
      q: "Sustainability projesinde aşağıdaki rolleri sırala:",
      options: [
        { text: "Çevresel etki ölçümlemesi ve raporlama", type: "I" },
        { text: "Sürdürülebilirlik kampanyaları tasarlamak", type: "A" },
        { text: "Sustainability paydaşlarıyla iş birliği kurmak", type: "E" },
        { text: "Sustainability metriklerini düzenli takip etmek", type: "C" }
      ]
    },
    {
      q: "Content üretimi için aşağıdaki formatları sırala:",
      options: [
        { text: "Yazılı içerik (blog, makale)", type: "I" },
        { text: "Video çekimi ve kurgusu", type: "A" },
        { text: "Sosyal medya community management", type: "S" },
        { text: "İçerik takvimleri ve planlama", type: "C" }
      ]
    },
    {
      q: "GSC için aşağıdaki görevleri sırala:",
      options: [
        { text: "Katılımcı ekiplere mentoring yapmak", type: "S" },
        { text: "Yarışma süreçlerini organize etmek", type: "C" },
        { text: "Projelerin teknik kalitesini değerlendirmek", type: "I" },
        { text: "Yarışmayı tanıtmak ve sponsorlarla görüşmek", type: "E" }
      ]
    },
    {
      q: "Satış sürecinde aşağıdaki rolleri sırala:",
      options: [
        { text: "Müşterilerle görüşmeler ve ikna", type: "E" },
        { text: "Satış verilerini analiz etmek", type: "I" },
        { text: "CRM sistemini yönetmek ve takip", type: "C" },
        { text: "Satış materyallerini tasarlamak", type: "A" }
      ]
    },
    {
      q: "Bir krizde (örn: son dakika değişiklikleri) aşağıdaki yaklaşımları sırala:",
      options: [
        { text: "Hızlıca pratik çözüm bulup uygulamak", type: "R" },
        { text: "Durumu analiz edip en iyi çözümü bulmak", type: "I" },
        { text: "Ekibin moralini yüksek tutmak", type: "S" },
        { text: "Krizi fırsata çevirmek için yaratıcı düşünmek", type: "A" }
      ]
    }
  ];

  const handleAnswer = (type) => {
    const newAnswers = { ...answers, [currentQuestion]: type };
    setAnswers(newAnswers);
    
    if (currentQuestion < forcedChoiceQuestions.length - 1) {
      setTimeout(() => setCurrentQuestion(currentQuestion + 1), 200);
    } else {
      setTimeout(() => setStage('ranking'), 200);
    }
  };

  const handleRanking = (questionIndex, optionIndex, rank) => {
    setRankingAnswers(prev => ({
      ...prev,
      [questionIndex]: {
        ...prev[questionIndex],
        [optionIndex]: rank
      }
    }));
  };

  const calculateResults = () => {
    const newScores = { R: 0, I: 0, A: 0, S: 0, E: 0, C: 0 };
    
    // Forced choice scoring
    Object.values(answers).forEach(type => {
      newScores[type] += 1;
    });
    
    // Ranking scoring
    Object.entries(rankingAnswers).forEach(([qIndex, rankings]) => {
      const question = rankingQuestions[parseInt(qIndex)];
      Object.entries(rankings).forEach(([optIndex, rank]) => {
        const option = question.options[parseInt(optIndex)];
        const points = 5 - parseInt(rank); // 1st=4pts, 2nd=3pts, 3rd=2pts, 4th=1pt
        newScores[option.type] += points;
      });
    });
    
    setScores(newScores);
    setStage('results');
  };

  const getTopTypes = () => {
    const sorted = Object.entries(scores).sort((a, b) => b[1] - a[1]);
    return sorted.slice(0, 3);
  };

  const getTypeInfo = (type) => {
    const info = {
      R: { name: "Realistic", desc: "Pratik, elle yapma, teknik" },
      I: { name: "Investigative", desc: "Analitik, araştırmacı, meraklı" },
      A: { name: "Artistic", desc: "Yaratıcı, özgün, ifade odaklı" },
      S: { name: "Social", desc: "İnsancıl, yardımsever, empatik" },
      E: { name: "Enterprising", desc: "Girişimci, lider, ikna edici" },
      C: { name: "Conventional", desc: "Organize, sistemli, detaycı" }
    };
    return info[type];
  };

  const getRoleRecommendations = () => {
    const topTypes = getTopTypes();
    const primary = topTypes[0][0];
    const secondary = topTypes[1][0];
    
    const roleMap = {
      'SI': ['Bilim Seferberliği', 'Program Development', 'People & Culture'],
      'IS': ['Bilim Seferberliği', 'Program Development', 'Product'],
      'IA': ['Product', 'Technology', 'Content'],
      'AI': ['Product', 'Content', 'Technology'],
      'EA': ['Gönüllendirin (Marketing)', 'Global Expansion', 'Content'],
      'AE': ['Gönüllendirin (Marketing)', 'Content', 'Global Expansion'],
      'ES': ['People & Culture', 'Global Expansion', 'GSC'],
      'SE': ['People & Culture', 'Bilim Seferberliği', 'GSC'],
      'EI': ['Global Expansion', 'Satış', 'GSC'],
      'IE': ['Product', 'Global Expansion', 'Sustainability'],
      'IC': ['Technology', 'Product', 'Sustainability'],
      'CI': ['Technology', 'Product', 'Program Development'],
      'SC': ['People & Culture', 'Bilim Seferberliği', 'Program Development'],
      'CS': ['People & Culture', 'Program Development', 'Content'],
      'EC': ['Global Expansion', 'Satış', 'GSC'],
      'CE': ['Product', 'Program Development', 'Technology'],
      'RI': ['Technology', 'Bilim Seferberliği', 'Product'],
      'IR': ['Technology', 'Product', 'Sustainability'],
      'RA': ['Content', 'Technology', 'Bilim Seferberliği'],
      'AR': ['Content', 'Product', 'Technology'],
      'RS': ['Bilim Seferberliği', 'Technology', 'Program Development'],
      'SR': ['Bilim Seferberliği', 'People & Culture', 'Program Development'],
      'RC': ['Technology', 'Product', 'Bilim Seferberliği'],
      'CR': ['Technology', 'Product', 'Program Development'],
      'RE': ['Satış', 'Global Expansion', 'Technology'],
      'ER': ['Global Expansion', 'Satış', 'GSC'],
      'AS': ['Content', 'People & Culture', 'Gönüllendirin'],
      'SA': ['People & Culture', 'Content', 'Bilim Seferberliği'],
      'AC': ['Content', 'Product', 'Gönüllendirin'],
      'CA': ['Content', 'Product', 'Technology']
    };
    
    return roleMap[primary + secondary] || ['Product', 'Program Development', 'Content'];
  };

  const progress = stage === 'test' 
    ? (currentQuestion / forcedChoiceQuestions.length) * 100 
    : stage === 'ranking' 
    ? 100
    : 0;

  const allRankingsComplete = () => {
    return rankingQuestions.every((_, qIndex) => {
      const rankings = rankingAnswers[qIndex];
      if (!rankings) return false;
      const values = Object.values(rankings);
      return values.length === 4 && new Set(values).size === 4 && 
             values.every(v => v >= 1 && v <= 4);
    });
  };

  if (stage === 'welcome') {
    return (
      <div className="min-h-screen bg-gradient-to-br from-gray-50 to-gray-100 flex items-center justify-center p-4">
        <div className="max-w-2xl w-full bg-white rounded-3xl shadow-2xl p-12">
          <div className="text-center mb-8">
            <div className="w-20 h-20 bg-gradient-to-br from-blue-500 to-purple-600 rounded-full mx-auto mb-6 flex items-center justify-center">
              <span className="text-white text-3xl font-bold">YGA</span>
            </div>
            <h1 className="text-4xl font-bold text-gray-900 mb-4">
              Gönüllü İlgi Alanları Testi
            </h1>
            <p className="text-xl text-gray-600 mb-8">
              YGA'da hangi rollerde en çok keyif alacağını keşfet
            </p>
          </div>

          <div className="bg-gradient-to-br from-blue-50 to-purple-50 rounded-2xl p-8 mb-8">
            <h2 className="text-lg font-semibold text-gray-900 mb-4">Önemli:</h2>
            <ul className="space-y-3 text-gray-700">
              <li className="flex items-start">
                <CheckCircle2 className="w-5 h-5 text-blue-600 mr-3 mt-0.5 flex-shrink-0" />
                <span>Neyi iyi yaptığını değil, neyi yapmaktan zevk aldığını düşün</span>
              </li>
              <li className="flex items-start">
                <CheckCircle2 className="w-5 h-5 text-blue-600 mr-3 mt-0.5 flex-shrink-0" />
                <span>Başkalarının senden ne beklediğini değil, sen ne istediğini işaretle</span>
              </li>
              <li className="flex items-start">
                <CheckCircle2 className="w-5 h-5 text-blue-600 mr-3 mt-0.5 flex-shrink-0" />
                <span>'Doğru' cevap yok, sadece senin cevabın var</span>
              </li>
              <li className="flex items-start">
                <CheckCircle2 className="w-5 h-5 text-blue-600 mr-3 mt-0.5 flex-shrink-0" />
                <span>İlk içgüdünü takip et, çok düşünme</span>
              </li>
            </ul>
          </div>

          <div className="flex items-center justify-between text-sm text-gray-600 mb-8">
            <div className="flex items-center">
              <div className="w-10 h-10 rounded-full bg-blue-100 flex items-center justify-center mr-3">
                <span className="text-blue-600 font-semibold">66</span>
              </div>
              <span>Soru</span>
            </div>
            <div className="flex items-center">
              <div className="w-10 h-10 rounded-full bg-purple-100 flex items-center justify-center mr-3">
                <span className="text-purple-600 font-semibold">15</span>
              </div>
              <span>Dakika</span>
            </div>
          </div>

          <button
            onClick={() => setStage('test')}
            className="w-full bg-gradient-to-r from-blue-600 to-purple-600 text-white text-lg font-semibold py-4 rounded-2xl hover:from-blue-700 hover:to-purple-700 transition-all duration-200 shadow-lg hover:shadow-xl flex items-center justify-center group"
          >
            Teste Başla
            <ChevronRight className="ml-2 w-5 h-5 group-hover:translate-x-1 transition-transform" />
          </button>
        </div>
      </div>
    );
  }

  if (stage === 'test') {
    const question = forcedChoiceQuestions[currentQuestion];
    return (
      <div className="min-h-screen bg-gradient-to-br from-gray-50 to-gray-100 flex items-center justify-center p-4">
        <div className="max-w-3xl w-full">
          <div className="mb-8">
            <div className="flex items-center justify-between mb-3">
              <span className="text-sm font-medium text-gray-600">
                Soru {currentQuestion + 1} / {forcedChoiceQuestions.length}
              </span>
              <span className="text-sm font-medium text-blue-600">
                Bölüm 1: Seçim Soruları
              </span>
            </div>
            <div className="w-full h-2 bg-gray-200 rounded-full overflow-hidden">
              <div 
                className="h-full bg-gradient-to-r from-blue-600 to-purple-600 transition-all duration-300"
                style={{ width: `${progress}%` }}
              />
            </div>
          </div>

          <div className="bg-white rounded-3xl shadow-2xl p-8 md:p-12">
            <h2 className="text-2xl md:text-3xl font-bold text-gray-900 mb-8 leading-relaxed">
              {question.q}
            </h2>

            <div className="space-y-4">
              {question.options.map((option, idx) => (
                <button
                  key={idx}
                  onClick={() => handleAnswer(option.type)}
                  className={`w-full text-left p-6 rounded-2xl border-2 transition-all duration-200 hover:border-blue-500 hover:shadow-lg group ${
                    answers[currentQuestion] === option.type
                      ? 'border-blue-600 bg-blue-50 shadow-md'
                      : 'border-gray-200 bg-white hover:bg-gray-50'
                  }`}
                >
                  <div className="flex items-start">
                    <div className={`w-6 h-6 rounded-full border-2 flex items-center justify-center mr-4 mt-0.5 flex-shrink-0 transition-colors ${
                      answers[currentQuestion] === option.type
                        ? 'border-blue-600 bg-blue-600'
                        : 'border-gray-300 group-hover:border-blue-500'
                    }`}>
                      {answers[currentQuestion] === option.type && (
                        <div className="w-2 h-2 bg-white rounded-full" />
                      )}
                    </div>
                    <span className="text-lg text-gray-800 leading-relaxed">
                      {option.text}
                    </span>
                  </div>
                </button>
              ))}
            </div>

            {currentQuestion > 0 && (
              <button
                onClick={() => setCurrentQuestion(currentQuestion - 1)}
                className="mt-8 text-gray-600 hover:text-gray-900 font-medium transition-colors"
              >
                ← Önceki Soru
              </button>
            )}
          </div>
        </div>
      </div>
    );
  }

  if (stage === 'ranking') {
    return (
      <div className="min-h-screen bg-gradient-to-br from-gray-50 to-gray-100 p-4 py-12">
        <div className="max-w-4xl mx-auto">
          <div className="mb-8">
            <div className="flex items-center justify-between mb-3">
              <span className="text-sm font-medium text-gray-600">
                Bölüm 2: Sıralama Soruları
              </span>
              <span className="text-sm font-medium text-purple-600">
                Son Aşama
              </span>
            </div>
            <div className="w-full h-2 bg-gray-200 rounded-full overflow-hidden">
              <div className="h-full bg-gradient-to-r from-purple-600 to-pink-600 w-full" />
            </div>
          </div>

          <div className="bg-white rounded-3xl shadow-2xl p-8 md:p-12 mb-8">
            <h2 className="text-2xl md:text-3xl font-bold text-gray-900 mb-4">
              Son Aşama: Aktiviteleri Sırala
            </h2>
            <p className="text-gray-600 mb-8 text-lg">
              Her soruda aktiviteleri <span className="font-semibold text-purple-600">1-4 arası sırala</span>. 
              (1=En çok keyif alırım, 4=En az keyif alırım)
            </p>

            <div className="space-y-12">
              {rankingQuestions.map((question, qIdx) => (
                <div key={qIdx} className="border-b border-gray-200 pb-8 last:border-b-0">
                  <h3 className="text-lg font-semibold text-gray-900 mb-6">
                    {qIdx + 1}. {question.q}
                  </h3>
                  <div className="space-y-4">
                    {question.options.map((option, optIdx) => (
                      <div key={optIdx} className="bg-gray-50 rounded-xl p-5">
                        <div className="flex items-center justify-between">
                          <span className="text-gray-800 flex-1 pr-4">
                            {option.text}
                          </span>
                          <div className="flex gap-2">
                            {[1, 2, 3, 4].map(rank => (
                              <button
                                key={rank}
                                onClick={() => handleRanking(qIdx, optIdx, rank)}
                                className={`w-12 h-12 rounded-xl font-semibold transition-all duration-200 ${
                                  rankingAnswers[qIdx]?.[optIdx] === rank
                                    ? 'bg-gradient-to-br from-purple-600 to-pink-600 text-white shadow-lg scale-110'
                                    : 'bg-white text-gray-600 border-2 border-gray-300 hover:border-purple-500 hover:text-purple-600'
                                }`}
                              >
                                {rank}
                              </button>
                            ))}
                          </div>
                        </div>
                      </div>
                    ))}
                  </div>
                </div>
              ))}
            </div>
          </div>

          <button
            onClick={calculateResults}
            disabled={!allRankingsComplete()}
            className={`w-full text-lg font-semibold py-5 rounded-2xl transition-all duration-200 shadow-lg flex items-center justify-center group ${
              allRankingsComplete()
                ? 'bg-gradient-to-r from-purple-600 to-pink-600 text-white hover:from-purple-700 hover:to-pink-700 hover:shadow-xl'
                : 'bg-gray-300 text-gray-500 cursor-not-allowed'
            }`}
          >
            {allRankingsComplete() ? (
              <>
                Sonuçları Gör
                <ChevronRight className="ml-2 w-5 h-5 group-hover:translate-x-1 transition-transform" />
              </>
            ) : (
              'Lütfen tüm soruları tamamla'
            )}
          </button>
        </div>
      </div>
    );
  }

  if (stage === 'results') {
    const topTypes = getTopTypes();
    const maxScore = 102;
    const roles = getRoleRecommendations();

    return (
      <div className="min-h-screen bg-gradient-to-br from-gray-50 to-gray-100 p-4 py-12">
        <div className="max-w-4xl mx-auto">
          <div className="bg-white rounded-3xl shadow-2xl p-8 md:p-12 mb-8">
            <div className="text-center mb-12">
              <div className="w-24 h-24 bg-gradient-to-br from-green-500 to-emerald-600 rounded-full mx-auto mb-6 flex items-center justify-center">
                <CheckCircle2 className="w-12 h-12 text-white" />
              </div>
              <h1 className="text-4xl font-bold text-gray-900 mb-4">
                Sonuçların Hazır!
              </h1>
              <p className="text-xl text-gray-600">
                İşte senin YGA ilgi alanı profilin
              </p>
            </div>

            <div className="mb-12">
              <h2 className="text-2xl font-bold text-gray-900 mb-6">
                📊 RIASEC Profil Skorların
              </h2>
              <div className="space-y-4">
                {Object.entries(scores).map(([type, score]) => {
                  const info = getTypeInfo(type);
                  const percentage = Math.round((score / maxScore) * 100);
                  return (
                    <div key={type}>
                      <div className="flex items-center justify-between mb-2">
                        <div>
                          <span className="font-semibold text-gray-900 text-lg">
                            {type} - {info.name}
                          </span>
                          <span className="text-gray-600 text-sm ml-3">
                            {info.desc}
                          </span>
                        </div>
                        <span className="font-bold text-2xl text-gray-900">
                          {percentage}%
                        </span>
                      </div>
                      <div className="w-full h-4 bg-gray-200 rounded-full overflow-hidden">
                        <div 
                          className="h-full bg-gradient-to-r from-blue-600 to-purple-600 transition-all duration-1000"
                          style={{ width: `${percentage}%` }}
                        />
                      </div>
                    </div>
                  );
                })}
              </div>
            </div>

            <div className="bg-gradient-to-br from-blue-50 to-purple-50 rounded-2xl p-8 mb-12">
              <h2 className="text-2xl font-bold text-gray-900 mb-4">
                🎯 Senin Profil Tipin
              </h2>
              <div className="text-center mb-6">
                <div className="inline-block bg-gradient-to-r from-blue-600 to-purple-600 text-white px-8 py-4 rounded-2xl text-3xl font-bold mb-4">
                  {topTypes[0][0]}-{topTypes[1][0]}
                </div>
                <p className="text-xl text-gray-700 font-medium">
                  {getTypeInfo(topTypes[0][0]).name} - {getTypeInfo(topTypes[1][0]).name}
                </p>
              </div>
              <div className="grid md:grid-cols-3 gap-4 mt-6">
                {topTypes.map(([type, score], idx) => (
                  <div key={type} className="bg-white rounded-xl p-4 text-center">
                    <div className="text-sm text-gray-600 mb-1">
                      {idx === 0 ? '🥇 En Yüksek' : idx === 1 ? '🥈 İkinci' : '🥉 Üçüncü'}
                    </div>
                    <div className="text-2xl font-bold text-gray-900">
                      {type}
                    </div>
                    <div className="text-sm text-gray-600">
                      {Math.round((score / maxScore) * 100)}%
                    </div>
                  </div>
                ))}
              </div>
            </div>

            <div className="mb-12">
              <h2 className="text-2xl font-bold text-gray-900 mb-6">
                ⭐ Senin İçin En Uygun YGA Rolleri
              </h2>
              <div className="space-y-4">
                {roles.map((role, idx) => (
                  <div 
                    key={idx}
                    className="bg-gradient-to-r from-white to-gray-50 border-2 border-gray-200 rounded-2xl p-6 hover:border-blue-500 hover:shadow-lg transition-all duration-200"
                  >
                    <div className="flex items-center">
                      <div className="w-12 h-12 bg-gradient-to-br from-blue-600 to-purple-600 rounded-full flex items-center justify-center text-white font-bold text-xl mr-4 flex-shrink-0">
                        {idx + 1}
                      </div>
                      <div>
                        <h3 className="text-xl font-bold text-gray-900">
                          {role}
                        </h3>
                        <p className="text-gray-600">
                          {idx === 0 ? 'En uygun rol - Kuvvetli yönlerinle tam uyumlu' : 
                           idx === 1 ? 'Çok uygun - İlgi alanlarına çok yakın' : 
                           'Uygun - Gelişim için harika fırsat'}
                        </p>
                      </div>
                    </div>
                  </div>
                ))}
              </div>
            </div>

            <div className="bg-gradient-to-br from-green-50 to-emerald-50 rounded-2xl p-8">
              <h2 className="text-2xl font-bold text-gray-900 mb-4">
                💡 Bir Sonraki Adım
              </h2>
              <ul className="space-y-3 text-gray-700 text-lg">
                <li className="flex items-start">
                  <ChevronRight className="w-6 h-6 text-green-600 mr-3 mt-0.5 flex-shrink-0" />
                  <span>101 Kampı'nda mentorünle bu sonuçları konuş</span>
                </li>
                <li className="flex items-start">
                  <ChevronRight className="w-6 h-6 text-green-600 mr-3 mt-0.5 flex-shrink-0" />
                  <span>İlk projende <strong>{roles[0]}</strong> alanını dene</span>
                </li>
                <li className="flex items-start">
                  <ChevronRight className="w-6 h-6 text-green-600 mr-3 mt-0.5 flex-shrink-0" />
                  <span>Kendini gözlemle ve geliştir!</span>
                </li>
              </ul>
            </div>
          </div>

          <button
            onClick={() => {
              setStage('welcome');
              setCurrentQuestion(0);
              setAnswers({});
              setRankingAnswers({});
              setScores({ R: 0, I: 0, A: 0, S: 0, E: 0, C: 0 });
            }}
            className="w-full bg-gray-200 text-gray-700 text-lg font-semibold py-4 rounded-2xl hover:bg-gray-300 transition-all duration-200"
          >
            Testi Yeniden Başlat
          </button>
        </div>
      </div>
    );
  }

  return null;
};

export default YGAInterestTest;
