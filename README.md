# aleostube-widget
aleo-autotranslate.js
(async function(){
  const FEED_URL='https://aleos-tube-daily-vlog-anak-rantau.blogspot.com/feeds/posts/default?alt=json&max-results=6';
  const CHANNEL_ID='UCTHGlP7T12oHv2QHDuw0C3g';
  const LIBRE_ENDPOINT='https://libretranslate.de/translate';
  const SERVER_ENDPOINT='https://your-vercel-endpoint.example.com/api/share';
  const POLL=600000;
  const MUSIC={
    ID:'https://example.com/music/indonesia-sample.mp3',
    US:'https://example.com/music/us-sample.mp3',
    DEFAULT:'https://example.com/music/global-sample.mp3'
  };

  function log(...x){console.log('[AleoAuto]',...x);}
  function lang(){return (navigator.language||'en').split('-')[0];}
  function country(){const l=(navigator.language||'').split('-');return l[1]||'DEFAULT';}
  const userLang=lang(),userCountry=country();

  // musik latar otomatis
  const url=MUSIC[userCountry]||MUSIC.DEFAULT;
  let audio=new Audio(url);audio.loop=true;audio.volume=0.5;
  audio.play().catch(()=>log('user gesture required'));
  
  // pause musik saat video diputar
  window.onYouTubeIframeAPIReady=function(){
    new YT.Player('player',{events:{onStateChange:e=>{
      if(e.data===1)audio.pause();
      if(e.data===2||e.data===0)audio.play();
    }}});
  };

  // translate otomatis isi posting
  async function translateAll(){
    try{
      const nodes=[...document.body.querySelectorAll('*')]
        .filter(e=>e.childNodes.length===1&&e.childNodes[0].nodeType===3&&e.innerText.trim());
      for(const n of nodes){
        const q=n.innerText;
        const r=await fetch(LIBRE_ENDPOINT,{method:'POST',headers:{'Content-Type':'application/json'},
          body:JSON.stringify({q,source:'auto',target:userLang})});
        const j=await r.json();
        if(j.translatedText)n.innerText=j.translatedText;
      }
    }catch(e){log('translate error',e);}
  }

  // auto detect post baru + share
  async function autoShare(){
    try{
      const r=await fetch(FEED_URL);
      const j=await r.json();
      const posts=j.feed.entry||[];
      const last=posts[0];
      const id=last.id.$t;
      const seen=localStorage.getItem('aleoshare')||'';
      if(seen!==id){
        const data={title:last.title.$t,url:last.link.find(l=>l.rel==='alternate').href};
        await fetch(SERVER_ENDPOINT,{method:'POST',headers:{'Content-Type':'application/json'},body:JSON.stringify(data)});
        localStorage.setItem('aleoshare',id);
      }
    }catch(e){log('share fail',e);}
  }

  // tampilkan video channel
  async function showYT(){
    const res=await fetch(`https://www.youtube.com/feeds/videos.xml?channel_id=${CHANNEL_ID}`);
    const t=await res.text();
    const v=t.match(/<yt:videoId>(.*?)<\\/yt:videoId>/);
    if(v){
      const c=document.createElement('div');
      c.innerHTML=`<iframe id=\"player\" width=\"300\" height=\"170\" src=\"https://www.youtube.com/embed/${v[1]}?autoplay=0\" allow=\"autoplay;encrypted-media\" allowfullscreen></iframe>`;
      c.style.position='fixed';c.style.bottom='10px';c.style.right='10px';c.style.zIndex='9999';
      document.body.appendChild(c);
    }
  }

  await translateAll();
  await showYT();
  await autoShare();
  setInterval(autoShare,POLL);
})();
