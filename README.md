# Sinha
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Llox — Private Orbital Travel</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@400;500;600;700&family=Inter:wght@400;500;600&family=JetBrains+Mono:wght@400;500&display=swap');

  :root{
    --void: #080A12;
    --void-2: #0E1220;
    --void-3: #121729;
    --ion: #EDEBFF;
    --muted: rgba(237,235,255,0.56);
    --muted-2: rgba(237,235,255,0.34);
    --violet: #8D7CFF;
    --violet-soft: rgba(141,124,255,0.5);
    --coral: #FF8B6B;
    --glass-fill: rgba(237,235,255,0.045);
    --glass-fill-strong: rgba(237,235,255,0.08);
    --glass-border: rgba(237,235,255,0.14);
    --ease-spring: cubic-bezier(.16,1,.3,1);
  }

  *{ margin:0; padding:0; box-sizing:border-box; }

  html{ scroll-behavior:smooth; }

  body{
    background:var(--void);
    color:var(--ion);
    font-family:'Inter', sans-serif;
    overflow-x:hidden;
  }

  ::selection{ background:var(--violet); color:var(--void); }

  .mono{
    font-family:'JetBrains Mono', monospace;
    font-size:11px;
    letter-spacing:0.06em;
    text-transform:uppercase;
    color:var(--muted);
  }

  h1,h2,h3{
    font-family:'Space Grotesk', sans-serif;
    font-weight:600;
    letter-spacing:-0.02em;
    line-height:1.05;
  }

  p{ color:var(--muted); line-height:1.65; font-size:15.5px; }

  a{ color:inherit; text-decoration:none; }

  /* ---------- Starfield ---------- */
  #starfield{
    position:fixed;
    inset:0;
    z-index:0;
    pointer-events:none;
  }
  .star{
    position:absolute;
    background:var(--ion);
    border-radius:50%;
    opacity:0.6;
    animation:twinkle 4s ease-in-out infinite;
  }
  @keyframes twinkle{
    0%,100%{ opacity:0.15; }
    50%{ opacity:0.85; }
  }

  .nebula-glow{
    position:fixed;
    border-radius:50%;
    filter:blur(90px);
    pointer-events:none;
    z-index:0;
    opacity:0.35;
  }
  .nebula-glow.g1{
    width:600px; height:600px;
    top:-10%; left:-10%;
    background:radial-gradient(circle, var(--violet), transparent 70%);
  }
  .nebula-glow.g2{
    width:500px; height:500px;
    bottom:5%; right:-10%;
    background:radial-gradient(circle, var(--coral), transparent 70%);
  }

  /* ---------- Nav ---------- */
  .topnav{
    position:fixed;
    top:0; left:0; right:0;
    z-index:50;
    display:flex;
    align-items:center;
    justify-content:space-between;
    padding:26px 48px;
    mix-blend-mode:difference;
  }
  .logo{
    font-family:'Space Grotesk', sans-serif;
    font-weight:700;
    font-size:20px;
    letter-spacing:0.02em;
  }
  .nav-cta{
    font-size:12.5px;
    font-weight:600;
    padding:9px 18px;
    border:1px solid var(--glass-border);
    border-radius:100px;
  }

  .dotnav{
    position:fixed;
    right:32px;
    top:50%;
    transform:translateY(-50%);
    z-index:50;
    display:flex;
    flex-direction:column;
    gap:16px;
  }
  .dotnav button{
    width:8px; height:8px;
    border-radius:50%;
    background:var(--muted-2);
    border:none;
    cursor:pointer;
    transition:all .4s var(--ease-spring);
    padding:0;
  }
  .dotnav button.active{
    background:var(--ion);
    height:22px;
    border-radius:4px;
  }

  section{
    position:relative;
    z-index:1;
    min-height:100vh;
    scroll-snap-align:start;
    display:flex;
    flex-direction:column;
    justify-content:center;
    padding:120px 8vw;
  }

  html, body{ scroll-snap-type: y proximity; }

  /* ---------- Reveal engine ---------- */
  [data-reveal]{
    opacity:0;
    transform:translateY(28px);
    transition:opacity .9s var(--ease-spring), transform .9s var(--ease-spring);
    transition-delay:calc(var(--stagger, 0) * 90ms);
  }
  [data-reveal].in-view{
    opacity:1;
    transform:translateY(0);
  }
  [data-reveal="scale"]{ transform:translateY(0) scale(0.92); }
  [data-reveal="scale"].in-view{ transform:translateY(0) scale(1); }
  [data-reveal="left"]{ transform:translateX(-36px); }
  [data-reveal="left"].in-view{ transform:translateX(0); }
  [data-reveal="right"]{ transform:translateX(36px); }
  [data-reveal="right"].in-view{ transform:translateX(0); }

  /* ---------- Liquid glass system ---------- */
  .glass{
    background:var(--glass-fill);
    border:1px solid var(--glass-border);
    backdrop-filter:blur(20px) saturate(140%);
    -webkit-backdrop-filter:blur(20px) saturate(140%);
    border-radius:22px;
    position:relative;
    overflow:hidden;
  }
  .glass::before{
    content:'';
    position:absolute;
    inset:0;
    background:linear-gradient(135deg, rgba(255,255,255,0.10), transparent 40%);
    pointer-events:none;
  }
  .glass:hover{
    background:var(--glass-fill-strong);
    border-color:rgba(237,235,255,0.24);
    transition:all .5s var(--ease-spring);
  }

  .eyebrow{
    display:inline-flex;
    align-items:center;
    gap:8px;
    margin-bottom:18px;
  }
  .eyebrow .dot{
    width:6px; height:6px;
    border-radius:50%;
    background:var(--coral);
  }

  .btn-primary{
    display:inline-flex;
    align-items:center;
    gap:10px;
    background:var(--ion);
    color:var(--void);
    font-weight:600;
    font-size:14px;
    padding:15px 26px;
    border-radius:100px;
    transition:transform .4s var(--ease-spring), box-shadow .4s var(--ease-spring);
    box-shadow:0 0 0 rgba(141,124,255,0);
  }
  .btn-primary:hover{
    transform:translateY(-3px);
    box-shadow:0 12px 30px rgba(141,124,255,0.35);
  }
  .btn-ghost{
    display:inline-flex;
    align-items:center;
    gap:10px;
    padding:15px 24px;
    border:1px solid var(--glass-border);
    border-radius:100px;
    font-size:14px;
    font-weight:500;
    transition:all .4s var(--ease-spring);
  }
  .btn-ghost:hover{
    border-color:var(--ion);
    background:var(--glass-fill);
  }
</style>
</head>
<body>

<div id="starfield"></div>
<div class="nebula-glow g1"></div>
<div class="nebula-glow g2"></div>

<nav class="topnav">
  <div class="logo">llox</div>
  <a href="#reserve" class="nav-cta">Reserve a seat</a>
</nav>

<div class="dotnav" id="dotnav"></div>

<!-- sections get inserted below -->
<div id="sections-root">

<!-- 1. HERO -->
<section id="s1" style="align-items:center; text-align:center; padding-top:0;">
  <div class="mono" data-reveal style="margin-bottom:22px;">LLOX AEROSPACE · EST. 2031 · SUBORBITAL & LUNAR CHARTER</div>
  <h1 data-reveal style="font-size:clamp(46px,8vw,108px); max-width:1100px;">
    Leave orbit.<br><span style="color:var(--muted-2)">Arrive different.</span>
  </h1>
  <p data-reveal style="max-width:520px; margin:26px auto 0; font-size:17px;" data-stagger="1">
    Llox designs and flies private spacecraft for people who have already done everything else. Six seats. One planet behind you.
  </p>
  <div data-reveal data-stagger="2" style="display:flex; gap:14px; justify-content:center; margin-top:38px;">
    <a href="#reserve" class="btn-primary">Reserve your seat →</a>
    <a href="#s4" class="btn-ghost">See the spacecraft</a>
  </div>

  <div id="orbit-wrap" data-reveal data-stagger="3" style="margin-top:64px; position:relative; width:100%; max-width:560px; margin-left:auto; margin-right:auto; height:150px;">
    <svg viewBox="0 0 560 150" width="100%" height="150">
      <ellipse cx="280" cy="75" rx="240" ry="34" fill="none" stroke="rgba(237,235,255,0.16)" stroke-width="1"/>
      <ellipse cx="280" cy="75" rx="150" ry="20" fill="none" stroke="rgba(237,235,255,0.10)" stroke-width="1"/>
      <circle id="orbit-dot" cx="40" cy="75" r="4.5" fill="#FF8B6B"/>
    </svg>
  </div>
</section>

<!-- 2. MANIFESTO -->
<section id="s2">
  <div style="max-width:900px;">
    <div class="eyebrow mono" data-reveal><span class="dot"></span>01 · MANIFESTO</div>
    <h2 data-reveal style="font-size:clamp(30px,4.2vw,54px); max-width:820px;" data-stagger="1">
      Space travel stopped being about the rocket a long time ago. It's about who you become when the door seals shut.
    </h2>
    <p data-reveal data-stagger="2" style="max-width:600px; margin-top:28px; font-size:16.5px;">
      Every Llox flight is built around a single idea: the overview effect shouldn't be rationed to professional astronauts.
      We handle the eighteen months of engineering, certification, and training so that your only job, on the day, is to look out the window.
    </p>
  </div>
  <div style="display:grid; grid-template-columns:repeat(3,1fr); gap:20px; margin-top:64px; max-width:1000px;">
    <div class="glass" data-reveal data-stagger="1" style="padding:26px;">
      <div class="mono" style="color:var(--coral); margin-bottom:10px;">FOUNDED</div>
      <div style="font-family:'Space Grotesk'; font-size:28px;">2031</div>
    </div>
    <div class="glass" data-reveal data-stagger="2" style="padding:26px;">
      <div class="mono" style="color:var(--coral); margin-bottom:10px;">FLIGHTS TO DATE</div>
      <div style="font-family:'Space Grotesk'; font-size:28px;" class="counter" data-target="214">0</div>
    </div>
    <div class="glass" data-reveal data-stagger="3" style="padding:26px;">
      <div class="mono" style="color:var(--coral); margin-bottom:10px;">SAFETY RECORD</div>
      <div style="font-family:'Space Grotesk'; font-size:28px;">100%</div>
    </div>
  </div>
</section>

<!-- 3. DESTINATIONS -->
<section id="s3">
  <div class="eyebrow mono" data-reveal><span class="dot"></span>02 · DESTINATIONS</div>
  <h2 data-reveal style="font-size:clamp(30px,4vw,50px); max-width:640px;" data-stagger="1">Three ways to leave the ground</h2>
  <div style="display:grid; grid-template-columns:repeat(3,1fr); gap:22px; margin-top:52px;">
    <div class="glass" data-reveal="scale" data-stagger="1" style="padding:32px; display:flex; flex-direction:column; min-height:340px;">
      <div class="mono" style="color:var(--violet);">ALTITUDE 100KM</div>
      <h3 style="font-size:26px; margin-top:14px;">Karman Hop</h3>
      <p style="margin-top:12px; flex:1;">A 90-minute suborbital arc across the Karman line. Four minutes of weightlessness, the curvature of Earth, home before lunch.</p>
      <div class="mono" style="margin-top:18px;">FROM $450,000</div>
    </div>
    <div class="glass" data-reveal="scale" data-stagger="2" style="padding:32px; display:flex; flex-direction:column; min-height:340px;">
      <div class="mono" style="color:var(--violet);">ALTITUDE 400KM</div>
      <h3 style="font-size:26px; margin-top:14px;">Orbital Stay</h3>
      <p style="margin-top:12px; flex:1;">Three nights aboard Llox Aurora, our private orbital module. Sixteen sunrises a day, a window the size of a dining table.</p>
      <div class="mono" style="margin-top:18px;">FROM $2.8M</div>
    </div>
    <div class="glass" data-reveal="scale" data-stagger="3" style="padding:32px; display:flex; flex-direction:column; min-height:340px;">
      <div class="mono" style="color:var(--violet);">DISTANCE 384,400KM</div>
      <h3 style="font-size:26px; margin-top:14px;">Lunar Flyby</h3>
      <p style="margin-top:12px; flex:1;">A free-return trajectory around the far side of the Moon. Eight days, no landing, the most remote a human can travel and come home.</p>
      <div class="mono" style="margin-top:18px;">FROM $9.4M</div>
    </div>
  </div>
</section>

<!-- 4. SPACECRAFT -->
<section id="s4">
  <div style="display:grid; grid-template-columns:1fr 1fr; gap:60px; align-items:center;">
    <div>
      <div class="eyebrow mono" data-reveal><span class="dot"></span>03 · THE SPACECRAFT</div>
      <h2 data-reveal style="font-size:clamp(30px,4vw,50px);" data-stagger="1">Meet Vesper-2</h2>
      <p data-reveal data-stagger="2" style="margin-top:18px; max-width:440px;">
        A six-seat reusable spaceplane built around a full-carbon primary structure and a triple-redundant abort system. Every Vesper flies over 40 missions before retirement.
      </p>
      <div style="display:grid; grid-template-columns:1fr 1fr; gap:16px; margin-top:32px; max-width:440px;">
        <div data-reveal data-stagger="3">
          <div class="mono">MAX ALTITUDE</div>
          <div style="font-family:'Space Grotesk'; font-size:22px; margin-top:6px;">108 km</div>
        </div>
        <div data-reveal data-stagger="3">
          <div class="mono">CREW + PILOTS</div>
          <div style="font-family:'Space Grotesk'; font-size:22px; margin-top:6px;">6 + 2</div>
        </div>
        <div data-reveal data-stagger="4">
          <div class="mono">BURN TIME</div>
          <div style="font-family:'Space Grotesk'; font-size:22px; margin-top:6px;">142 sec</div>
        </div>
        <div data-reveal data-stagger="4">
          <div class="mono">G-FORCE PEAK</div>
          <div style="font-family:'Space Grotesk'; font-size:22px; margin-top:6px;">3.2 g</div>
        </div>
      </div>
    </div>
    <div class="glass" data-reveal="right" style="padding:40px; aspect-ratio:1/1; display:flex; align-items:center; justify-content:center;">
      <svg viewBox="0 0 240 240" width="80%" height="80%">
        <ellipse cx="120" cy="120" rx="90" ry="18" fill="none" stroke="rgba(237,235,255,0.15)" stroke-dasharray="2 6"/>
        <path d="M120 40 L150 150 L120 135 L90 150 Z" fill="none" stroke="#8D7CFF" stroke-width="1.6"/>
        <circle cx="120" cy="60" r="3" fill="#FF8B6B"/>
        <line x1="120" y1="40" x2="120" y2="200" stroke="rgba(237,235,255,0.12)" stroke-dasharray="3 5"/>
      </svg>
    </div>
  </div>
</section>

<!-- 5. EXPERIENCE TIMELINE -->
<section id="s5">
  <div class="eyebrow mono" data-reveal><span class="dot"></span>04 · THE EXPERIENCE</div>
  <h2 data-reveal style="font-size:clamp(30px,4vw,50px); max-width:640px;" data-stagger="1">From training to touchdown</h2>
  <div style="margin-top:56px; max-width:760px;">
    <div style="display:flex; gap:24px; padding:22px 0; border-top:1px solid var(--glass-border);" data-reveal="left" data-stagger="1">
      <div class="mono" style="width:90px; flex-shrink:0; color:var(--coral);">DAY -14</div>
      <div>
        <h3 style="font-size:19px;">Flight readiness training</h3>
        <p style="margin-top:6px;">Centrifuge sessions, egress drills, and a full mission briefing at the Llox campus.</p>
      </div>
    </div>
    <div style="display:flex; gap:24px; padding:22px 0; border-top:1px solid var(--glass-border);" data-reveal="left" data-stagger="2">
      <div class="mono" style="width:90px; flex-shrink:0; color:var(--coral);">DAY 0 · T-3H</div>
      <div>
        <h3 style="font-size:19px;">Suit-up & final systems check</h3>
        <p style="margin-top:6px;">Your flight suit is fitted to you alone. Vesper completes 4,000 automated pre-flight checks.</p>
      </div>
    </div>
    <div style="display:flex; gap:24px; padding:22px 0; border-top:1px solid var(--glass-border);" data-reveal="left" data-stagger="3">
      <div class="mono" style="width:90px; flex-shrink:0; color:var(--coral);">T+0</div>
      <div>
        <h3 style="font-size:19px;">Ignition</h3>
        <p style="margin-top:6px;">Main engine burn to apogee. Weightlessness begins the moment the engine cuts.</p>
      </div>
    </div>
    <div style="display:flex; gap:24px; padding:22px 0; border-top:1px solid var(--glass-border); border-bottom:1px solid var(--glass-border);" data-reveal="left" data-stagger="4">
      <div class="mono" style="width:90px; flex-shrink:0; color:var(--coral);">T+90MIN</div>
      <div>
        <h3 style="font-size:19px;">Touchdown</h3>
        <p style="margin-top:6px;">Vesper glides back to a runway landing. A welcome team and your mission footage are waiting.</p>
      </div>
    </div>
  </div>
</section>

<!-- 6. SAFETY & ENGINEERING -->
<section id="s6">
  <div style="display:grid; grid-template-columns:1fr 1fr; gap:60px; align-items:center;">
    <div class="glass" data-reveal="left" style="padding:36px;">
      <div class="mono" style="color:var(--violet); margin-bottom:16px;">REDUNDANCY MAP</div>
      <div style="display:flex; flex-direction:column; gap:14px;">
        <div style="display:flex; justify-content:space-between; align-items:center; padding-bottom:12px; border-bottom:1px solid var(--glass-border);">
          <span style="font-size:14.5px;">Launch abort system</span>
          <span class="mono" style="color:var(--coral);">TRIPLE</span>
        </div>
        <div style="display:flex; justify-content:space-between; align-items:center; padding-bottom:12px; border-bottom:1px solid var(--glass-border);">
          <span style="font-size:14.5px;">Flight computers</span>
          <span class="mono" style="color:var(--coral);">QUAD</span>
        </div>
        <div style="display:flex; justify-content:space-between; align-items:center; padding-bottom:12px; border-bottom:1px solid var(--glass-border);">
          <span style="font-size:14.5px;">Life support loops</span>
          <span class="mono" style="color:var(--coral);">DUAL</span>
        </div>
        <div style="display:flex; justify-content:space-between; align-items:center;">
          <span style="font-size:14.5px;">Pressure hull margin</span>
          <span class="mono" style="color:var(--coral);">2.5x</span>
        </div>
      </div>
    </div>
    <div>
      <div class="eyebrow mono" data-reveal><span class="dot"></span>05 · SAFETY & ENGINEERING</div>
      <h2 data-reveal style="font-size:clamp(30px,4vw,50px);" data-stagger="1">Certified the hard way</h2>
      <p data-reveal data-stagger="2" style="margin-top:18px; max-width:440px;">
        Vesper-2 holds full commercial spaceflight certification. Every subsystem that can fail twice, does — safely, and in simulation, thousands of times before it ever carries a passenger.
      </p>
      <div data-reveal data-stagger="3" style="margin-top:26px; display:flex; gap:10px; flex-wrap:wrap;">
        <span class="mono glass" style="padding:8px 14px; border-radius:100px;">FAA-AST CERTIFIED</span>
        <span class="mono glass" style="padding:8px 14px; border-radius:100px;">ISO 14620</span>
        <span class="mono glass" style="padding:8px 14px; border-radius:100px;">AS9100D</span>
      </div>
    </div>
  </div>
</section>

<!-- 7. MEMBERSHIP -->
<section id="s7">
  <div class="eyebrow mono" data-reveal><span class="dot"></span>06 · MEMBERSHIP</div>
  <h2 data-reveal style="font-size:clamp(30px,4vw,50px); max-width:640px;" data-stagger="1">Fly once, or fly for life</h2>
  <div style="display:grid; grid-template-columns:repeat(3,1fr); gap:22px; margin-top:52px;">
    <div class="glass" data-reveal data-stagger="1" style="padding:30px;">
      <div class="mono">ASCENT</div>
      <div style="font-family:'Space Grotesk'; font-size:32px; margin-top:12px;">$450K</div>
      <p style="margin-top:14px;">One Karman Hop, flight training included, lifetime mission footage archive.</p>
      <a href="#reserve" class="btn-ghost" style="margin-top:22px; width:100%; justify-content:center;">Select Ascent</a>
    </div>
    <div class="glass" data-reveal data-stagger="2" style="padding:30px; border-color:var(--violet-soft);">
      <div class="mono" style="color:var(--violet);">APEX — MOST BOOKED</div>
      <div style="font-family:'Space Grotesk'; font-size:32px; margin-top:12px;">$2.8M</div>
      <p style="margin-top:14px;">A full Orbital Stay aboard Aurora, priority scheduling, and two guest Karman Hops per year.</p>
      <a href="#reserve" class="btn-primary" style="margin-top:22px; width:100%; justify-content:center;">Select Apex</a>
    </div>
    <div class="glass" data-reveal data-stagger="3" style="padding:30px;">
      <div class="mono">ZENITH</div>
      <div style="font-family:'Space Grotesk'; font-size:32px; margin-top:12px;">$9.4M</div>
      <p style="margin-top:14px;">One Lunar Flyby, a named seat on the Vesper fleet, and unlimited Karman Hops for you and a guest.</p>
      <a href="#reserve" class="btn-ghost" style="margin-top:22px; width:100%; justify-content:center;">Select Zenith</a>
    </div>
  </div>
</section>

<!-- 8. FINAL CTA + FOOTER -->
<section id="s8" style="justify-content:space-between;">
  <div id="reserve" style="text-align:center; max-width:720px; margin:0 auto;">
    <div class="mono" data-reveal style="margin-bottom:18px;">07 · RESERVE</div>
    <h2 data-reveal data-stagger="1" style="font-size:clamp(32px,5vw,64px);">Your seat is waiting at the edge of the atmosphere.</h2>
    <p data-reveal data-stagger="2" style="margin-top:20px; max-width:480px; margin-left:auto; margin-right:auto;">
      Speak with a mission advisor to check availability for 2027 launch windows.
    </p>
    <div data-reveal data-stagger="3" style="margin-top:32px; display:flex; gap:14px; justify-content:center;">
      <a href="#" class="btn-primary">Book a mission call →</a>
      <a href="#" class="btn-ghost">Download the prospectus</a>
    </div>
  </div>

  <footer style="margin-top:100px; padding-top:32px; border-top:1px solid var(--glass-border); display:flex; justify-content:space-between; align-items:center; flex-wrap:wrap; gap:16px;">
    <div class="logo" style="font-size:16px;">llox</div>
    <div class="mono">© 2031 LLOX AEROSPACE, INC. · MOJAVE LAUNCH COMPLEX 4</div>
    <div style="display:flex; gap:18px;">
      <a href="#" class="mono">INSTAGRAM</a>
      <a href="#" class="mono">X</a>
      <a href="#" class="mono">PRESS</a>
    </div>
  </footer>
</section>

</div>

<script>
  // ---------- Starfield ----------
  const starfield = document.getElementById('starfield');
  const STAR_COUNT = 140;
  for (let i = 0; i < STAR_COUNT; i++) {
    const s = document.createElement('div');
    s.className = 'star';
    const size = Math.random() * 2 + 0.5;
    s.style.width = size + 'px';
    s.style.height = size + 'px';
    s.style.left = Math.random() * 100 + 'vw';
    s.style.top = Math.random() * 100 + 'vh';
    s.style.animationDelay = (Math.random() * 4) + 's';
    s.style.animationDuration = (3 + Math.random() * 3) + 's';
    starfield.appendChild(s);
  }

  // parallax drift on scroll
  window.addEventListener('scroll', () => {
    const y = window.scrollY;
    starfield.style.transform = `translateY(${y * 0.08}px)`;
  }, { passive: true });

  // ---------- Stagger delay assignment ----------
  document.querySelectorAll('[data-reveal]').forEach(el => {
    const stagger = el.getAttribute('data-stagger');
    if (stagger) el.style.setProperty('--stagger', stagger);
  });

  // ---------- Entrance reveal (IntersectionObserver mimicking Framer Motion viewport-triggered animations) ----------
  const revealObserver = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        entry.target.classList.add('in-view');
      }
    });
  }, { threshold: 0.18, rootMargin: '0px 0px -8% 0px' });

  document.querySelectorAll('[data-reveal]').forEach(el => revealObserver.observe(el));

  // ---------- Counter animation ----------
  const counters = document.querySelectorAll('.counter');
  const counterObserver = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        const el = entry.target;
        const target = parseInt(el.getAttribute('data-target'), 10);
        let current = 0;
        const duration = 1400;
        const startTime = performance.now();
        function tick(now) {
          const progress = Math.min((now - startTime) / duration, 1);
          const eased = 1 - Math.pow(1 - progress, 3);
          current = Math.round(eased * target);
          el.textContent = current;
          if (progress < 1) requestAnimationFrame(tick);
        }
        requestAnimationFrame(tick);
        counterObserver.unobserve(el);
      }
    });
  }, { threshold: 0.5 });
  counters.forEach(c => counterObserver.observe(c));

  // ---------- Orbit dot animation ----------
  const orbitDot = document.getElementById('orbit-dot');
  let orbitAngle = 0;
  function animateOrbit() {
    orbitAngle += 0.008;
    const rx = 240, ry = 34, cx = 280, cy = 75;
    const x = cx + rx * Math.cos(orbitAngle);
    const y = cy + ry * Math.sin(orbitAngle);
    if (orbitDot) {
      orbitDot.setAttribute('cx', x);
      orbitDot.setAttribute('cy', y);
    }
    requestAnimationFrame(animateOrbit);
  }
  animateOrbit();

  // ---------- Dot nav ----------
  const sectionIds = ['s1','s2','s3','s4','s5','s6','s7','s8'];
  const dotnav = document.getElementById('dotnav');
  sectionIds.forEach((id, i) => {
    const btn = document.createElement('button');
    if (i === 0) btn.classList.add('active');
    btn.addEventListener('click', () => {
      document.getElementById(id).scrollIntoView({ behavior: 'smooth' });
    });
    btn.setAttribute('aria-label', 'Go to section ' + (i + 1));
    dotnav.appendChild(btn);
  });
  const dotButtons = dotnav.querySelectorAll('button');

  const navObserver = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        const idx = sectionIds.indexOf(entry.target.id);
        dotButtons.forEach(b => b.classList.remove('active'));
        if (idx > -1) dotButtons[idx].classList.add('active');
      }
    });
  }, { threshold: 0.5 });
  sectionIds.forEach(id => {
    const el = document.getElementById(id);
    if (el) navObserver.observe(el);
  });

  // ---------- Magnetic glass hover highlight ----------
  document.querySelectorAll('.glass').forEach(card => {
    card.addEventListener('mousemove', (e) => {
      const rect = card.getBoundingClientRect();
      const x = ((e.clientX - rect.left) / rect.width) * 100;
      const y = ((e.clientY - rect.top) / rect.height) * 100;
      card.style.background = `radial-gradient(circle at ${x}% ${y}%, rgba(141,124,255,0.14), var(--glass-fill) 60%)`;
    });
    card.addEventListener('mouseleave', () => {
      card.style.background = 'var(--glass-fill)';
    });
  });
</script>

</body>
</html>
