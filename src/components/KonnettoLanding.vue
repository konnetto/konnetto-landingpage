<template>
  <div class="konnetto-landing">
    <div class="page">
      <div class="starfield" aria-hidden="true"></div>

      <header>
        <img src="/logo.png" alt="Konnetto logo" class="logo" />
        <div class="brand-name">KONNETTO</div>
      </header>

      <main>
        <!-- LEFT: copy + form -->
        <section class="left">
          <div class="badge">
            <span class="badge-dot"></span>
            Launching Soon
          </div>

          <h1>
            The orbit of <span class="highlight">otaku culture</span>
          </h1>

          <p class="tagline">
            <strong>Konnetto</strong> is the digital home where anime fans, cosplayers,
            VTubers, and creators drift into the same orbit to connect, share, and shine.
          </p>

          <p class="subcopy">
            Scrolling, but searching for your sign? When we go online together,
            your oshi, your friends, and your stories all meet under the same
            <em>hoshizora</em>.
          </p>

          <p class="hero-lyric">
            <span>♪</span>
            <em>“Konnetto de, we go kyun-kyun ☆ In the orbit of otaku dreams…”</em>
          </p>

          <div class="pill-row">
            <span class="pill">📺 Track your anime journey & dokidoki moments</span>
            <span class="pill">🎭 Fans, Cosplayers, VTubers, & Creators in one orbit</span>
            <span class="pill">✨ Share fanart, stories, and secret memes together</span>
          </div>

          <div class="cta-row">
            <form class="waitlist-form" @submit.prevent="joinWaitlist">
              <input
                v-model="email"
                type="email"
                name="email"
                class="email-input"
                placeholder="Enter your email"
                required
              />
              <button class="btn btn-primary" type="submit">
                Notify me when it’s live
                <span>✨</span>
              </button>
            </form>
            <span class="eta-text">
              No spam. Just one gentle ping when Konnetto goes <strong>online</strong>.
            </span>
          </div>

          <!-- Lyric strip -->
          <div class="lyric-strip">
            <div class="lyric-marquee">
              キラキラ画面 light up my night · my heart go dokidoki time ·
              Konnetto de, we go kyun-kyun ☆ · under the 星空 we start ·
              キラキラ画面 light up my night · my heart go dokidoki time ·
              Konnetto de, we go kyun-kyun ☆ · under the 星空 we start ·
            </div>
          </div>
        </section>

        <!-- RIGHT: chibi -->
        <aside class="right">
          <div class="chibi-wrapper">
            <div class="chibi-glow" aria-hidden="true"></div>

            <img
              src="/konnetto-chibi.png"
              alt="Konnetto chibi character"
              class="chibi-img"
            />

            <div class="speech">
              <small>HOSHINO-CHAN</small>
              “Ah… you’re here online? ✨<br />
              Wait for me a little more, okay?<br />
              We’ll go kyun-kyun together on Konnetto.”
            </div>
          </div>
        </aside>
      </main>

      <footer>
        © {{ currentYear }} Konnetto · The orbit of otaku culture.
      </footer>
    </div>

    <!-- Background music -->
    <audio ref="bgm" src="/konnetto-theme.mp3" loop></audio>

    <!-- Floating music widget -->
    <div class="music-widget">
      <button
        class="music-btn"
        type="button"
        :aria-label="isPlaying ? 'Pause Konnetto theme' : 'Play Konnetto theme'"
        @click="toggleMusic"
      >
        {{ isPlaying ? "⏸" : "♪" }}
      </button>
      <div class="music-meta">
        <div class="music-title">Konnetto J-Pop Moe Theme</div>
        <div class="music-sub">short ver. · launch night</div>
        <div class="music-lyric">“Konnetto de, we go kyun-kyun ☆”</div>
      </div>
    </div>

    <!-- Moe success modal -->
    <div
      v-if="isModalOpen"
      class="modal-backdrop"
      @click.self="closeModal"
    >
      <div class="modal-card">
        <div class="modal-sparkle" aria-hidden="true"></div>
        <div class="modal-inner">
          <div class="modal-avatar">
            <img src="/hoshino-wave.png" alt="Hoshino-chan waving" />
          </div>
          <div class="modal-title">Yatta~! ✨</div>
          <div class="modal-sub">
            <strong>Hoshino-chan</strong> safely kept your email in her starry inbox.
          </div>
          <div class="modal-quote">
            “When Konnetto opens under the <em>hoshizora</em>, I’ll gently ping you…
            so, stay close to me, okay? (´｡• ᵕ •｡`)♡”
          </div>
          <button class="modal-btn" type="button" @click="closeModal">
            Okay, I’ll wait~ <span>🌙</span>
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const email = ref('');
const isPlaying = ref(false);
const isModalOpen = ref(false);
const currentYear = new Date().getFullYear();
const bgm = ref(null);

const joinWaitlist = () => {
  const trimmed = email.value.trim();
  if (!trimmed) return;

  // TODO: kirim ke backend / API Konnetto nanti
  console.log('Waitlist email:', trimmed);

  email.value = '';
  isModalOpen.value = true;
};

const toggleMusic = async () => {
  const audio = bgm.value;
  if (!audio) return;

  try {
    if (!isPlaying.value) {
      await audio.play();
      isPlaying.value = true;
    } else {
      audio.pause();
      isPlaying.value = false;
    }
  } catch (err) {
    console.error('Unable to play audio:', err);
  }
};

const closeModal = () => {
  isModalOpen.value = false;
};
</script>

<style>
:root {
  --bg-dark: #050816;
  --accent-purple: #7d5fff;
  --accent-green: #1db954;
  --text-main: #f5f5f5;
  --text-muted: #a0a0b8;
}

/* pastikan body nggak ada margin */
body {
  margin: 0;
  font-family: system-ui, -apple-system, BlinkMacSystemFont, "Inter", "Segoe UI",
    sans-serif;
}

.konnetto-landing {
  min-height: 100vh;
  color: var(--text-main);
  background:
    radial-gradient(circle at top left, #202067 0, transparent 55%),
    radial-gradient(circle at bottom right, #030b24 0, transparent 60%),
    #050816;
  display: flex;
  align-items: stretch;
  justify-content: center;
}

.page {
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
  padding: 24px 20px 32px;
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  position: relative;
  overflow: hidden;
}

/* Starry layer */
.starfield {
  position: absolute;
  inset: -20%;
  background-image:
    radial-gradient(1px 1px at 10% 20%, rgba(255, 255, 255, 0.4) 0, transparent 50%),
    radial-gradient(1px 1px at 80% 10%, rgba(181, 206, 255, 0.52) 0, transparent 55%),
    radial-gradient(1px 1px at 30% 80%, rgba(255, 255, 255, 0.4) 0, transparent 50%),
    radial-gradient(2px 2px at 60% 50%, rgba(125, 95, 255, 0.6) 0, transparent 60%);
  opacity: 0.5;
  pointer-events: none;
  animation: twinkle 14s linear infinite alternate;
  z-index: 0;
}

@keyframes twinkle {
  0% {
    opacity: 0.35;
    transform: translate3d(0, 0, 0);
  }
  50% {
    opacity: 0.7;
    transform: translate3d(4px, -6px, 0);
  }
  100% {
    opacity: 0.45;
    transform: translate3d(-4px, 4px, 0);
  }
}

header {
  display: flex;
  align-items: center;
  gap: 10px;
  padding-bottom: 16px;
  position: relative;
  z-index: 1;
}

.logo {
  height: 32px;
  width: auto;
  object-fit: contain;
}

.brand-name {
  font-size: 1.2rem;
  letter-spacing: 0.15em;
  text-transform: uppercase;
}

main {
  flex: 1;
  display: flex;
  align-items: center;
  gap: 40px;
  position: relative;
  z-index: 1;
}

.left {
  flex: 1.05;
  max-width: 540px;
}

.badge {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  font-size: 0.75rem;
  text-transform: uppercase;
  letter-spacing: 0.18em;
  color: var(--accent-green);
  background: rgba(7, 39, 25, 0.9);
  border-radius: 999px;
  padding: 6px 12px;
  border: 1px solid rgba(29, 185, 84, 0.5);
  margin-bottom: 14px;
}

.badge-dot {
  width: 6px;
  height: 6px;
  border-radius: 50%;
  background: var(--accent-green);
  box-shadow: 0 0 10px rgba(29, 185, 84, 0.9);
}

h1 {
  font-size: clamp(2.1rem, 4vw, 3rem);
  line-height: 1.15;
  margin-bottom: 10px;
}

h1 span.highlight {
  background: linear-gradient(120deg, var(--accent-green), var(--accent-purple));
  -webkit-background-clip: text;
  background-clip: text;
  color: transparent;
}

.tagline {
  font-size: 0.96rem;
  color: var(--text-muted);
  margin-bottom: 12px;
}

.tagline strong {
  color: #ffffff;
  font-weight: 500;
}

.subcopy {
  font-size: 0.88rem;
  color: #c9ccff;
  opacity: 0.92;
  margin-bottom: 14px;
}

.hero-lyric {
  font-size: 0.82rem;
  color: #f4f0ff;
  opacity: 0.9;
  margin-bottom: 20px;
  font-style: italic;
}

.hero-lyric span {
  color: var(--accent-green);
}

.pill-row {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  margin-bottom: 22px;
}

.pill {
  font-size: 0.76rem;
  padding: 6px 10px;
  border-radius: 999px;
  border: 1px solid rgba(160, 160, 184, 0.4);
  color: var(--text-muted);
  background: rgba(3, 9, 30, 0.8);
}

.cta-row {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  gap: 8px;
}

.waitlist-form {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  gap: 8px;
  justify-content: flex-start;
}

.email-input {
  padding: 10px 12px;
  border-radius: 999px;
  border: 1px solid rgba(160, 160, 184, 0.5);
  background: rgba(3, 9, 30, 0.9);
  color: #fff;
  min-width: 220px;
  font-size: 0.9rem;
  outline: none;
  flex: 1;
}

.email-input::placeholder {
  color: rgba(160, 160, 184, 0.8);
}

.email-input:focus {
  border-color: rgba(125, 95, 255, 0.8);
  box-shadow: 0 0 0 1px rgba(125, 95, 255, 0.4);
}

.btn {
  border: none;
  outline: none;
  cursor: pointer;
  border-radius: 999px;
  padding: 10px 20px;
  font-size: 0.9rem;
  font-weight: 500;
  letter-spacing: 0.03em;
  display: inline-flex;
  align-items: center;
  gap: 8px;
  transition: transform 0.14s ease, box-shadow 0.14s ease, background 0.18s ease;
  white-space: nowrap;
}

.btn-primary {
  background: linear-gradient(135deg, var(--accent-purple), var(--accent-green));
  color: #fff;
  box-shadow: 0 14px 35px rgba(0, 0, 0, 0.7);
}

.btn-primary:hover {
  transform: translateY(-1px);
  box-shadow: 0 18px 45px rgba(0, 0, 0, 0.85);
}

.eta-text {
  font-size: 0.8rem;
  color: var(--text-muted);
}

/* Right side */
.right {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
}

.chibi-wrapper {
  position: relative;
  max-width: 360px;
  width: 100%;
}

.chibi-glow {
  position: absolute;
  inset: 10% 5% 0 5%;
  background:
    radial-gradient(circle at 50% 0%, rgba(255, 255, 255, 0.14) 0, transparent 55%),
    radial-gradient(circle at 10% 80%, rgba(125, 95, 255, 0.4) 0, transparent 55%),
    radial-gradient(circle at 90% 80%, rgba(29, 185, 84, 0.35) 0, transparent 55%);
  opacity: 0.7;
  z-index: 0;
}

.chibi-img {
  position: relative;
  z-index: 1;
  width: 100%;
  height: auto;
  object-fit: contain;
  filter: drop-shadow(0 12px 35px rgba(0, 0, 0, 0.9));
  animation: float 5s ease-in-out infinite;
}

.speech {
  position: absolute;
  right: -6px;
  top: 10px;
  transform: translateX(50%);
  padding: 10px 14px;
  border-radius: 16px;
  background: rgba(7, 17, 40, 0.96);
  border: 1px solid rgba(160, 193, 255, 0.7);
  font-size: 0.8rem;
  max-width: 210px;
  color: #f5f5ff;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.8);
  z-index: 2;
}

.speech small {
  display: block;
  margin-bottom: 4px;
  font-size: 0.7rem;
  text-transform: uppercase;
  letter-spacing: 0.16em;
  color: var(--accent-green);
}

@keyframes float {
  0% {
    transform: translateY(0px);
  }
  50% {
    transform: translateY(-8px);
  }
  100% {
    transform: translateY(0px);
  }
}

/* Lyric strip */
.lyric-strip {
  position: relative;
  z-index: 1;
  margin-top: 10px;
  margin-bottom: 10px;
  border-radius: 999px;
  padding: 8px 14px;
  font-size: 0.75rem;
  color: #fefbff;
  background: linear-gradient(
    120deg,
    rgba(125, 95, 255, 0.25),
    rgba(29, 185, 84, 0.18)
  );
  border: 1px solid rgba(180, 196, 255, 0.5);
  overflow: hidden;
}

.lyric-marquee {
  white-space: nowrap;
  animation: scroll-lyric 22s linear infinite;
}

@keyframes scroll-lyric {
  0% {
    transform: translateX(0);
  }
  100% {
    transform: translateX(-50%);
  }
}

footer {
  font-size: 0.72rem;
  color: rgba(160, 160, 184, 0.7);
  padding-top: 14px;
  text-align: center;
  position: relative;
  z-index: 1;
}

/* MUSIC WIDGET */
.music-widget {
  position: fixed;
  right: 20px;
  bottom: 20px;
  z-index: 40;
  background: radial-gradient(
      circle at top left,
      rgba(125, 95, 255, 0.32),
      rgba(5, 8, 22, 0.98)
    );
  border-radius: 16px;
  padding: 10px 14px;
  border: 1px solid rgba(160, 193, 255, 0.7);
  box-shadow: 0 14px 40px rgba(0, 0, 0, 0.85);
  display: flex;
  align-items: center;
  gap: 10px;
  max-width: 280px;
  backdrop-filter: blur(10px);
}

.music-btn {
  width: 38px;
  height: 38px;
  border-radius: 999px;
  border: none;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  font-size: 1.2rem;
  cursor: pointer;
  background: radial-gradient(
    circle at 30% 0%,
    var(--accent-green),
    var(--accent-purple)
  );
  color: #fff;
  box-shadow: 0 8px 18px rgba(0, 0, 0, 0.7);
  transition: transform 0.14s ease, box-shadow 0.14s ease;
  flex-shrink: 0;
}

.music-btn:hover {
  transform: translateY(-1px);
  box-shadow: 0 12px 26px rgba(0, 0, 0, 0.9);
}

.music-meta {
  display: flex;
  flex-direction: column;
  gap: 2px;
  font-size: 0.72rem;
}

.music-title {
  font-weight: 600;
  letter-spacing: 0.04em;
  text-transform: uppercase;
  font-size: 0.7rem;
}

.music-sub {
  color: var(--text-muted);
  font-size: 0.7rem;
}

.music-lyric {
  font-size: 0.7rem;
  color: #f5f5ff;
  opacity: 0.9;
}

/* RESPONSIVE */
@media (max-width: 840px) {
  main {
    flex-direction: column-reverse;
    text-align: center;
    gap: 28px;
  }

  .left {
    max-width: 100%;
    background: linear-gradient(
      135deg,
      rgba(7, 15, 40, 0.96),
      rgba(5, 8, 22, 0.96)
    );
    border-radius: 24px;
    padding: 20px 18px 18px;
    box-shadow: 0 20px 55px rgba(0, 0, 0, 0.8);
    border: 1px solid rgba(143, 163, 255, 0.25);
    backdrop-filter: blur(10px);
  }

  .cta-row {
    align-items: center;
  }

  .waitlist-form {
    justify-content: center;
  }

  .speech {
    position: static;
    transform: none;
    margin: 12px auto 0;
    text-align: center;
  }

  .chibi-wrapper {
    max-width: 280px;
  }
}

@media (max-width: 640px) {
  .music-widget {
    left: 50%;
    right: auto;
    transform: translateX(-50%);
    bottom: 14px;
    max-width: 320px;
    padding: 8px 12px;
  }

  .music-title,
  .music-sub,
  .music-lyric {
    font-size: 0.68rem;
  }

  .music-btn {
    width: 34px;
    height: 34px;
    font-size: 1rem;
  }
}

@media (max-width: 600px) {
  .page {
    padding: 18px 14px 24px;
  }

  header {
    justify-content: center;
  }

  .brand-name {
    font-size: 1rem;
    letter-spacing: 0.12em;
  }

  h1 {
    font-size: 1.9rem;
  }

  .lyric-strip {
    font-size: 0.7rem;
  }

  .waitlist-form {
    flex-direction: column;
    align-items: stretch;
  }
}

/* MOE MODAL */
.modal-backdrop {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.78);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 50;
}

.modal-card {
  background: radial-gradient(
      circle at top,
      rgba(125, 95, 255, 0.4),
      rgba(5, 10, 32, 0.98)
    );
  border-radius: 24px;
  padding: 18px 20px 16px;
  max-width: 320px;
  width: 90%;
  border: 1px solid rgba(178, 197, 255, 0.7);
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.9);
  text-align: center;
  position: relative;
  overflow: hidden;
}

.modal-sparkle {
  position: absolute;
  inset: -30%;
  background-image:
    radial-gradient(2px 2px at 20% 30%, rgba(255, 255, 255, 0.7) 0, transparent 55%),
    radial-gradient(2px 2px at 80% 70%, rgba(205, 230, 255, 0.8) 0, transparent 55%);
  opacity: 0.4;
  pointer-events: none;
}

.modal-inner {
  position: relative;
  z-index: 1;
}

.modal-avatar {
  width: 52px;
  height: 52px;
  border-radius: 999px;
  margin: 0 auto 8px;
  background: radial-gradient(circle at 30% 0%, #ffffff, #f5e3ff);
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.8);
  border: 2px solid rgba(255, 255, 255, 0.85);
  overflow: hidden;
}

.modal-avatar img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: inherit;
  display: block;
}

.modal-title {
  font-size: 0.98rem;
  font-weight: 600;
  margin-bottom: 6px;
}

.modal-sub {
  font-size: 0.8rem;
  color: #e5e5ff;
  margin-bottom: 10px;
}

.modal-sub strong {
  color: #ffffff;
}

.modal-quote {
  font-size: 0.8rem;
  color: #fdfbff;
  font-style: italic;
  margin-bottom: 12px;
}

.modal-btn {
  margin-top: 4px;
  border-radius: 999px;
  border: none;
  padding: 8px 16px;
  font-size: 0.8rem;
  letter-spacing: 0.03em;
  cursor: pointer;
  background: linear-gradient(135deg, var(--accent-purple), var(--accent-green));
  color: #fff;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.85);
  display: inline-flex;
  align-items: center;
  gap: 6px;
}

.modal-btn:hover {
  transform: translateY(-1px);
}
</style>
