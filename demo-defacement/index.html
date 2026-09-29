<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>HACKED by N4z0k and $01z!c.sh</title>
<style>
  :root { --green:#00ff41; --red:#ff2a2a; --bg:#000; }
  * { box-sizing: border-box; margin: 0; padding: 0; }
  html, body { height: 100%; background: var(--bg); color: var(--green); font-family: "Courier New", monospace; overflow: hidden; }
  #matrix { position: fixed; inset: 0; z-index: 0; opacity: .35; }
  .scanlines { position: fixed; inset: 0; z-index: 3; pointer-events: none;
    background: repeating-linear-gradient(to bottom, rgba(0,0,0,0) 0, rgba(0,0,0,0) 2px, rgba(0,0,0,.25) 3px);
    animation: flicker 4s infinite; }
  @keyframes flicker { 0%,100%{opacity:.8} 50%{opacity:1} 52%{opacity:.6} 54%{opacity:1} }
  main { position: relative; z-index: 2; height: 100%; display: flex; flex-direction: column;
    align-items: center; justify-content: center; text-align: center; padding: 1rem; gap: 1.2rem; }
  .skull { font-size: clamp(3rem, 10vw, 6rem); filter: drop-shadow(0 0 15px var(--red)); animation: pulse 1.5s infinite; }
  @keyframes pulse { 50% { transform: scale(1.12); } }
  h1 { font-size: clamp(1.8rem, 7vw, 4.5rem); color: var(--red); letter-spacing: .1em; text-transform: uppercase;
    position: relative; text-shadow: 0 0 10px var(--red), 0 0 30px var(--red); }
  h1 .name { color: var(--green); text-shadow: 0 0 10px var(--green), 0 0 30px var(--green); }
  /* glitch effect */
  .glitch { position: relative; display: inline-block; }
  .glitch::before, .glitch::after { content: attr(data-text); position: absolute; left: 0; top: 0; width: 100%; overflow: hidden; }
  .glitch::before { color: #0ff; left: 3px; clip-path: inset(0 0 60% 0); animation: g1 2.2s infinite linear alternate-reverse; }
  .glitch::after  { color: #f0f; left: -3px; clip-path: inset(60% 0 0 0); animation: g2 1.7s infinite linear alternate-reverse; }
  @keyframes g1 { 0%{clip-path:inset(5% 0 80% 0)} 25%{clip-path:inset(40% 0 40% 0)} 50%{clip-path:inset(70% 0 5% 0)} 75%{clip-path:inset(20% 0 60% 0)} 100%{clip-path:inset(50% 0 30% 0)} }
  @keyframes g2 { 0%{clip-path:inset(80% 0 5% 0)} 25%{clip-path:inset(10% 0 70% 0)} 50%{clip-path:inset(45% 0 35% 0)} 75%{clip-path:inset(60% 0 15% 0)} 100%{clip-path:inset(0 0 90% 0)} }
  #terminal { min-height: 9em; width: min(680px, 92vw); text-align: left; font-size: clamp(.8rem, 2.2vw, 1rem);
    background: rgba(0,20,0,.75); border: 1px solid var(--green); border-radius: 6px; padding: 1rem;
    box-shadow: 0 0 20px rgba(0,255,65,.35); white-space: pre-wrap; }
  #terminal .cursor { display: inline-block; width: .6em; background: var(--green); animation: blink 1s steps(1) infinite; }
  @keyframes blink { 50% { opacity: 0; } }
  .counter { color: var(--red); font-size: 1.1rem; }
  .dedicace { position: fixed; bottom: 3.2rem; left: 0; right: 0; z-index: 2; text-align: center;
    font-size: .75rem; color: var(--green); opacity: .7; letter-spacing: .15em; text-shadow: 0 0 6px var(--green); }
  .edu { position: fixed; bottom: 0; left: 0; right: 0; z-index: 4; background: #ffd400; color: #000;
    font-family: system-ui, sans-serif; font-size: .9rem; padding: .6rem 1rem; text-align: center; font-weight: 600; }
  .edu button { margin-left: .8rem; cursor: pointer; border: 2px solid #000; background: #000; color: #ffd400;
    padding: .2rem .7rem; font-weight: 700; border-radius: 4px; }
  body.revealed main, body.revealed #matrix { filter: blur(2px) brightness(.4); }
  #lesson { display: none; position: fixed; inset: 0; z-index: 5; background: rgba(0,0,0,.94); color: #fff;
    font-family: system-ui, sans-serif; overflow: auto; padding: 2rem 1rem 5rem; }
  #lesson.show { display: block; }
  #lesson .box { max-width: 720px; margin: 0 auto; line-height: 1.55; }
  #lesson h2 { color: #ffd400; margin-bottom: 1rem; }
  #lesson ul { margin: .5rem 0 1rem 1.3rem; }
  #lesson li { margin-bottom: .4rem; }
  #lesson ol.chain { margin: .8rem 0 1rem 1.3rem; }
  #lesson ol.chain li { margin-bottom: .6rem; }
  #lesson code { background: #222; color: #00ff41; padding: 0 .3em; border-radius: 3px; font-size: .9em; }
  #lesson button { margin-top: 1rem; padding: .5rem 1rem; cursor: pointer; font-weight: 700; border-radius: 4px; border: 0; background: #ffd400; }
</style>
</head>
<body>
<canvas id="matrix"></canvas>
<div class="scanlines"></div>

<main>
  <div class="skull">💀</div>
  <h1><span class="glitch" data-text="YOU HAVE BEEN HACKED">YOU HAVE BEEN HACKED</span><br>
      by <span class="name glitch" data-text="N4z0k and $01z!c.sh">N4z0k and $01z!c.sh</span></h1>
  <div id="terminal"></div>
  <div class="counter">Données « chiffrées » dans : <span id="timer">00:59:59</span></div>
</main>

<div class="dedicace">// dédicace : Thierry Peyre //</div>

<div class="edu">
  DÉCOUVERTE DU PENTESTING — Pentest d'un serveur HTTP · BUT R&amp;T · SAE3Cyber04
  <button id="reveal">Que faire ?</button>
</div>

<div id="lesson">
  <div class="box">
    <h2>🛡️ Ce que vous venez de voir : un défacement</h2>
    <p>Un attaquant remplace le contenu d'un site par son propre message, souvent pour la visibilité, l'idéologie ou pour prouver qu'il a eu accès au serveur.</p>

    <h2 style="margin-top:1.5rem">🎯 Le scénario de la SAE</h2>
    <p>Cible : une machine volontairement vulnérable qui héberge un site WordPress. Rien de sophistiqué : une seule faille non corrigée a suffi à tout faire tomber.</p>
    <ol class="chain">
      <li><b>Reconnaissance</b> : un scan des services exposés révèle un serveur <b>ProFTPD 1.3.3c</b>, une version connue pour être vulnérable.</li>
      <li><b>Porte dérobée (backdoor)</b> : cette version distribuée en 2010 avait été piégée (CVE-2010-20103, module Metasploit <code>proftpd_133c_backdoor</code>, score 9.8/10). Elle permet d'exécuter des commandes sans authentification.</li>
      <li><b>Reverse shell</b> : l'attaquant obtient un accès en ligne de commande sur le serveur.</li>
      <li><b>Accès au webroot</b> : les fichiers du site WordPress sont lisibles, dont <code>wp-config.php</code> qui contient les identifiants de la base de données en clair.</li>
      <li><b>Base MySQL</b> : avec ces identifiants, connexion à la base et modification du mot de passe de l'administrateur WordPress.</li>
      <li><b>Prise de contrôle</b> : connexion au tableau de bord <code>/wp-admin</code> en tant qu'admin, puis modification de la page d'accueil.</li>
      <li><b>Défacement</b> : la page que vous venez de voir. 💀</li>
    </ol>
    <p><b>À retenir :</b> l'attaque ne vient pas de WordPress lui-même, mais d'un service annexe oublié (FTP). Une seule faille, puis un effet domino.</p>

    <h2 style="margin-top:1.5rem">🔧 Comment l'éviter, étape par étape</h2>
    <ul>
      <li><b>Mettre à jour tous les services</b>, pas seulement le CMS : FTP, serveur web, système. ProFTPD 1.3.3c a été corrigé depuis 2010.</li>
      <li><b>Télécharger les logiciels depuis les sources officielles</b> et vérifier les signatures / empreintes (c'est un dépôt compromis qui a distribué cette backdoor).</li>
      <li><b>Réduire la surface d'attaque</b> : désactiver les services inutiles, remplacer FTP par SFTP, filtrer les ports avec un pare-feu.</li>
      <li><b>Scanner régulièrement</b> ses propres serveurs (Nmap, OpenVAS, Nessus) pour repérer les versions vulnérables avant les attaquants.</li>
      <li><b>Segmenter et limiter les droits</b> : le service FTP ne doit pas tourner en root ni pouvoir lire les fichiers de configuration du site.</li>
      <li><b>Protéger la base de données</b> : compte MySQL dédié aux droits minimaux, base non accessible depuis l'extérieur, <code>wp-config.php</code> avec des permissions strictes.</li>
      <li><b>Sécuriser l'admin WordPress</b> : mot de passe fort, double authentification (2FA), limitation des tentatives, alerte en cas de changement de mot de passe.</li>
      <li><b>Surveiller</b> : journaux, détection d'intrusion, contrôle d'intégrité des fichiers du site.</li>
      <li><b>Sauvegarder</b> : sauvegardes régulières, testées et hors ligne, pour restaurer rapidement.</li>
    </ul>

    <h2>🚨 Si ça vous arrive</h2>
    <ul>
      <li>Ne pas payer / ne pas répondre à l'attaquant</li>
      <li>Mettre le site hors ligne, prévenir l'hébergeur</li>
      <li>Considérer que <b>tout le serveur est compromis</b> : changer tous les mots de passe (admin, base de données, FTP, SSH), restaurer une sauvegarde saine, corriger la faille d'origine</li>
      <li>Déposer plainte et signaler sur cybermalveillance.gouv.fr</li>
    </ul>
    <p style="margin-top:1.5rem;opacity:.7;font-size:.9rem">by Alfeze &amp; Soizic ;)</p>
    <button id="close">Retour à la page</button>
  </div>
</div>

<script>
// --- Matrix rain ---
const canvas = document.getElementById('matrix'), ctx = canvas.getContext('2d');
let cols, drops;
const chars = 'アイウエオカキクケコ01N4Z0K#$%<>/{}'.split('');
function resize() {
  canvas.width = innerWidth; canvas.height = innerHeight;
  cols = Math.floor(canvas.width / 16);
  drops = Array(cols).fill(1);
}
addEventListener('resize', resize); resize();
setInterval(() => {
  ctx.fillStyle = 'rgba(0,0,0,.08)';
  ctx.fillRect(0, 0, canvas.width, canvas.height);
  ctx.fillStyle = '#00ff41'; ctx.font = '16px monospace';
  drops.forEach((y, i) => {
    ctx.fillText(chars[Math.floor(Math.random() * chars.length)], i * 16, y * 16);
    if (y * 16 > canvas.height && Math.random() > .975) drops[i] = 0;
    drops[i]++;
  });
}, 45);

// --- Terminal typing ---
const lines = [
  '> Scan des ports... FTP ouvert (ProFTPD 1.3.3c)',
  '> Exploitation de la backdoor... OK',
  '> Reverse shell obtenu... OK',
  '> Lecture de wp-config.php... OK',
  '> Connexion MySQL, reset du mot de passe admin... OK',
  '> Connexion à /wp-admin... OK',
  '> Site défacé avec succès.',
  '',
  'Un seul service oublié a suffi.'
];
const term = document.getElementById('terminal');
let li = 0, ci = 0, out = '';
function type() {
  if (li >= lines.length) { term.innerHTML = out + '<span class="cursor">&nbsp;</span>'; return; }
  const line = lines[li];
  out += line[ci] ?? '';
  ci++;
  term.innerHTML = out + '<span class="cursor">&nbsp;</span>';
  if (ci > line.length) { out += '\n'; li++; ci = 0; setTimeout(type, 350); }
  else setTimeout(type, 30 + Math.random() * 40);
}
type();

// --- Countdown (fake) ---
let secs = 3599;
const timer = document.getElementById('timer');
setInterval(() => {
  secs = secs > 0 ? secs - 1 : 3599;
  const p = n => String(n).padStart(2, '0');
  timer.textContent = `${p(Math.floor(secs / 3600))}:${p(Math.floor(secs / 60) % 60)}:${p(secs % 60)}`;
}, 1000);

// --- Random glitch shake on title ---
const h1 = document.querySelector('h1');
setInterval(() => {
  h1.style.transform = `translate(${(Math.random() - .5) * 8}px, ${(Math.random() - .5) * 4}px)`;
  setTimeout(() => h1.style.transform = '', 80);
}, 1800);

// --- Lesson overlay ---
const lesson = document.getElementById('lesson');
document.getElementById('reveal').onclick = () => { lesson.classList.add('show'); document.body.classList.add('revealed'); };
document.getElementById('close').onclick = () => { lesson.classList.remove('show'); document.body.classList.remove('revealed'); };
</script>
</body>
</html>
