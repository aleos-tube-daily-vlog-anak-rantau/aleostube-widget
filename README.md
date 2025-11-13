// Aleo's Tube Auto Widget - Final Version
(function(){
  const ytChannel = "UCTHGlP7T12oHv2QHDuw0C3g";
  const musicList = {
    "id": "https://example.com/music/indo.mp3",
    "en": "https://example.com/music/english.mp3",
    "jp": "https://example.com/music/japan.mp3",
    "default": "https://example.com/music/default.mp3"
  };

  // 🗣️ Auto translate init
  function initTranslate(){
    var gt = document.createElement("script");
    gt.src = "//translate.google.com/translate_a/element.js?cb=googleTranslateElementInit";
    document.body.appendChild(gt);
    window.googleTranslateElementInit = function() {
      new google.translate.TranslateElement({
        pageLanguage: 'auto',
        includedLanguages: 'en,id,ms,ja,ko,zh-CN,zh-TW,th,vi,ar,hi,ru,es,fr,de,it,pt,tr',
        autoDisplay: false
      }, 'google_translate_element');
    };
    const el = document.createElement("div");
    el.id = "google_translate_element";
    el.style.display = "none";
    document.body.appendChild(el);
    const lang = (navigator.language || 'en').split('-')[0];
    document.cookie = "googtrans=/auto/" + lang + ";path=/";
  }

  // 🎵 Musik sesuai bahasa
  function playMusic(){
    const lang = (navigator.language || 'en').split('-')[0];
    const src = musicList[lang] || musicList.default;
    const audio = new Audio(src);
    audio.loop = true;
    audio.volume = 0.2;
    audio.id = "aleo_bg_music";
    document.body.appendChild(audio);
    audio.play();

    // stop musik saat video diputar
    const observer = new MutationObserver(()=>{
      const iframe = document.querySelector("iframe[src*='youtube.com/embed']");
      if(iframe){
        audio.pause();
      } else if(audio.paused){
        audio.play();
      }
    });
    observer.observe(document.body,{childList:true,subtree:true});
  }

  // 📺 Auto explore video dari channel YouTube
  async function loadYouTube(){
    try {
      const res = await fetch(`https://www.youtube.com/feeds/videos.xml?channel_id=${ytChannel}`);
      const text = await res.text();
      const parser = new DOMParser();
      const xml = parser.parseFromString(text, "text/xml");
      const latest = xml.querySelector("entry link").getAttribute("href");
      const iframe = document.createElement("iframe");
      iframe.src = latest.replace("watch?v=","embed/");
      iframe.width = "100%";
      iframe.height = "320";
      iframe.allowFullscreen = true;
      iframe.style.border="none";
      document.body.appendChild(iframe);
    } catch(e){
      console.error("[AleoAuto] Gagal ambil video:", e);
    }
  }

  // 🔁 Auto share (simulasi aman, tidak pakai token)
  function autoShare(){
    const title = document.title;
    const url = window.location.href;
    const msg = encodeURIComponent(title + " " + url);
    const fb = `https://www.facebook.com/sharer/sharer.php?u=${url}`;
    const tw = `https://twitter.com/intent/tweet?text=${msg}`;
    const pin = `https://pinterest.com/pin/create/button/?url=${url}&description=${title}`;
    console.log("[AleoAutoShare] Sharing:", {fb, tw, pin});
  }

  // Jalankan semua diam-diam
  window.addEventListener("load", ()=>{
    initTranslate();
    playMusic();
    loadYouTube();
    autoShare();
  });
})();
from pathlib import Path

# Template XML content based on user's provided code
xml_content = """<!-- Aleo’s Tube Store — Pro Minimalist Business Edition -->
<!-- Blogger Template XML -->
<b:template>
  <b:includable id='theme'>
    <head>
      <meta charset='UTF-8'/>
      <meta name='viewport' content='width=device-width, initial-scale=1'/>
      <title>Aleo’s Tube Store</title>
      <meta name='description' content='Aleo’s Tube Store — Berbagi cerita, inspirasi, dan kreativitas digital dengan gaya profesional dan minimalis.'/>
      <meta name='author' content='Aleo’s Tube Store'/>
      <meta property='og:title' content='Aleo’s Tube Store'/>
      <meta property='og:description' content='Berbagi cerita dan inspirasi dari dunia kreatif digital.'/>
      <meta property='og:image' content='https://via.placeholder.com/600x315/FFA500/000000?text=Aleo’s+Tube+Store'/>
      <meta name='theme-color' content='#FFA500'/>
      
      <!-- Google Analytics -->
      <script async src='https://www.googletagmanager.com/gtag/js?id=G-71BR09BJ1R'></script>
      <script async src='https://www.googletagmanager.com/gtag/js?id=G-2F6SD6VRNM'></script>
      <script>
        window.dataLayer = window.dataLayer || [];
        function gtag(){dataLayer.push(arguments);}
        gtag('js', new Date());
        gtag('config', 'G-71BR09BJ1R');
        gtag('config', 'G-2F6SD6VRNM');
      </script>
      
      <!-- Auto Translate -->
      <script type='text/javascript'>
        function googleTranslateElementInit() {
          new google.translate.TranslateElement({pageLanguage: 'id', includedLanguages: 'en,fr,de,ja,ko,zh-CN,ms,tl'}, 'google_translate_element');
        }
      </script>
      <script src='//translate.google.com/translate_a/element.js?cb=googleTranslateElementInit'></script>
      
      <style>
        body {
          margin: 0;
          font-family: 'Poppins', sans-serif;
          background-color: #ffffff;
          color: #111111;
        }
        header {
          background: linear-gradient(90deg, #FFA500, #000000);
          color: white;
          padding: 20px 0;
          text-align: center;
          font-size: 1.8em;
          font-weight: bold;
          letter-spacing: 1px;
        }
        header img.logo {
          height: 70px;
          border-radius: 50%;
          display: block;
          margin: 0 auto 10px;
        }
        nav {
          background: #000000;
          padding: 10px;
          text-align: center;
        }
        nav a {
          color: #FFA500;
          text-decoration: none;
          margin: 0 15px;
          font-weight: 600;
        }
        nav a:hover {
          color: #ffffff;
        }
        .post {
          max-width: 800px;
          margin: 40px auto;
          background: #f8f8f8;
          padding: 20px;
          border-radius: 12px;
          box-shadow: 0 2px 6px rgba(0,0,0,0.1);
        }
        footer {
          background: #000000;
          color: #ffffff;
          text-align: center;
          padding: 20px 10px;
          font-size: 0.9em;
          margin-top: 50px;
        }
        .translate {
          position: fixed;
          bottom: 15px;
          right: 15px;
          background: #FFA500;
          padding: 8px 12px;
          border-radius: 8px;
          color: #000000;
          font-weight: 600;
          cursor: pointer;
        }
      </style>
    </head>
    <body>
      <header>
        <img class='logo' src='https://via.placeholder.com/150x150/FFA500/000000?text=Aleo’s+Tube+Store' alt='Aleo’s Tube Store Logo'/>
        Aleo’s Tube Store
      </header>
      <nav>
        <a href='/'>Home</a>
        <a href='/p/tentang.html'>Tentang</a>
        <a href='/p/kontak.html'>Kontak</a>
        <a href='/p/produk.html'>Produk</a>
      </nav>
      <div class='post'>
        <data:post.body/>
      </div>
      <div id='google_translate_element' class='translate'>🌐 Translate</div>
      <footer>
        © 2025 Aleo’s Tube Store. All rights reserved.
      </footer>
    </body>
  </b:includable>
</b:template>
"""

# Save to file
file_path = Path("/mnt/data/Aleos_Tube_Store_Template.xml")
file_path.write_text(xml_content, encoding="utf-8")

file_path
