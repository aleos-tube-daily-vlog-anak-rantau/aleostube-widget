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
