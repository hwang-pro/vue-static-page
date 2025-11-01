<!-- src/App.vue -->
<template>
  <main class="wrap">
    <!-- 배경 장식 -->
    <div class="sky">
      <svg class="moon" viewBox="0 0 120 120" aria-hidden="true">
        <defs>
          <radialGradient id="g" cx="50%" cy="50%">
            <stop offset="0%" stop-color="#fffceb" />
            <stop offset="70%" stop-color="#ffe9a8" />
            <stop offset="100%" stop-color="#ffd36e" />
          </radialGradient>
        </defs>
        <circle cx="60" cy="60" r="42" fill="url(#g)" />
        <!-- 토끼 실루엣 살짝 -->
        <path
            d="M60 40c6 0 9 4 9 8 0 5-4 7-4 10 0 2 3 3 3 7 0 6-5 10-11 10s-11-4-11-10c0-4 3-5 3-7 0-3-4-5-4-10 0-4 3-8 9-8 3 0 4 1 6 1s3-1 6-1Z"
            fill="rgba(0,0,0,.08)"
        />
      </svg>

      <!-- 연등 -->
      <div class="lantern l1" aria-hidden="true">🏮</div>
      <div class="lantern l2" aria-hidden="true">🏮</div>
      <div class="lantern l3" aria-hidden="true">🏮</div>

      <!-- 송편/별 -->
      <div class="floaty f1" aria-hidden="true">🌕</div>
      <div class="floaty f2" aria-hidden="true">✨</div>
      <div class="floaty f3" aria-hidden="true">🥟</div>
      <div class="floaty f4" aria-hidden="true">✨</div>
      <div class="floaty f5" aria-hidden="true">🥟</div>
    </div>

    <!-- 콘텐츠 카드 -->
    <section class="card" role="region" aria-label="한가위 인사">
      <header class="hero">
        <h1>
          한가위<span class="accent"> 보름달</span>처럼
          <span class="wave">환하게</span> 빛나는 하루 되세요
        </h1>
        <p class="subtitle">풍요와 안녕이 가득하길 기원합니다 🍂</p>
      </header>

      <article class="message">
        <p class="kkot">
          <span class="badge" aria-hidden="true">🎑 덕담</span>
          <span class="fade-in" :key="activeIndex">{{ activeBlessing }}</span>
        </p>
        <div class="actions">
          <button class="btn" @click="copyBlessing" :disabled="copied">
            <span v-if="!copied">복 나누기 (복사)</span>
            <span v-else>복사 완료! 💌</span>
          </button>
          <button class="ghost" @click="shuffle">다른 덕담 보기</button>
        </div>
      </article>

      <footer class="tips" role="list">
        <div class="tip" role="listitem">
          <strong>보름달 빌기</strong>
          <small>올해 목표 1가지를 크게 떠올리고 마음속으로 세 번! 🌝</small>
        </div>
        <div class="tip" role="listitem">
          <strong>연락 한 통</strong>
          <small>오래 못 본 친구/가족에게 안부 인사 남기기 ☎️</small>
        </div>
        <div class="tip" role="listitem">
          <strong>나눔 실천</strong>
          <small>작은 디저트라도 함께 나누면 복도 커집니다 🍪</small>
        </div>
      </footer>
    </section>

    <a
        class="signature"
        href="https://vitejs.dev/"
        target="_blank"
        rel="noopener noreferrer"
        aria-label="Vite 사이트 열기"
    >Built with Vue + TS + Vite</a
    >
  </main>
</template>

<script setup lang="ts">
import { onMounted, onUnmounted, ref, computed } from 'vue'

const blessings: string[] = [
  '한가위 달빛처럼 당신의 앞날도 환하게 빛나길 바랍니다.',
  '올가을, 수고한 만큼 풍성한 결실이 함께하길 기원해요.',
  '가족의 웃음과 건강이 언제나 곁을 지키길 바랍니다.',
  '하는 일마다 술술 풀리고, 고민은 바람처럼 흩어지길!',
  '마음 따뜻한 인연과 기회가 연등처럼 이어지기를 바랍니다.',
  '지금의 진심이 내일의 행운으로 돌아오길 바랍니다.',
  '달처럼 차오르는 성장과 성취가 가득하길 기도합니다.'
]

const activeIndex = ref<number>(0)
const copied = ref<boolean>(false)
let timer: ReturnType<typeof setInterval> | null = null

const activeBlessing = computed<string>(() => blessings[activeIndex.value])

function shuffle(): void {
  let next = Math.floor(Math.random() * blessings.length)
  if (next === activeIndex.value) next = (next + 1) % blessings.length
  activeIndex.value = next
}

async function copyBlessing(): Promise<void> {
  try {
    await navigator.clipboard.writeText(activeBlessing.value)
    copied.value = true
    setTimeout(() => (copied.value = false), 1500)
  } catch {
    // Fallback: 선택+복사 안내 (모바일/권한 거부 등)
    alert('복사 권한이 없어 덕담을 선택 후 직접 복사해 주세요.')
  }
}

onMounted(() => {
  timer = setInterval(shuffle, 5000)
})

onUnmounted(() => {
  if (timer !== null) {
    clearInterval(timer)
  }
})
</script>

<style scoped>
/* 색 변수 (라이트/다크 자동 전환) */
:root {
  --bg: linear-gradient(160deg, #0b1026 0%, #141a33 50%, #1b2447 100%);
  --card: rgba(255, 255, 255, 0.08);
  --text: #f6f7fb;
  --muted: #c7c9d9;
  --accent: #ffd36e;
  --accent-strong: #ffb703;
  --glass: rgba(255, 255, 255, 0.18);
  --shadow: 0 20px 40px rgba(0, 0, 0, 0.35);
}

@media (prefers-color-scheme: light) {
  :root {
    --bg: linear-gradient(160deg, #fff8e7 0%, #f8efe2 60%, #f2e6d8 100%);
    --card: rgba(255, 255, 255, 0.65);
    --text: #2a2a2a;
    --muted: #5a5d6b;
    --accent: #c46a00;
    --accent-strong: #a65700;
    --glass: rgba(255, 255, 255, 0.7);
    --shadow: 0 12px 24px rgba(180, 140, 90, 0.25);
  }
}

*,
*::before,
*::after {
  box-sizing: border-box;
}

.wrap {
  min-height: 100svh;
  display: grid;
  place-items: center;
  background: var(--bg);
  color: var(--text);
  position: relative;
  overflow: hidden;
  padding: 4rem 1.25rem 3.5rem;
}

/* 하늘/달/장식 */
.sky {
  position: absolute;
  inset: -10% -5% -5% -5%;
  pointer-events: none;
  z-index: 0;
  filter: saturate(1.05);
}

.moon {
  position: absolute;
  top: 6vh;
  right: 8vw;
  width: clamp(100px, 16vw, 220px);
  filter: drop-shadow(0 8px 30px rgba(255, 211, 110, 0.45));
  opacity: 0.96;
}

.lantern {
  position: absolute;
  font-size: clamp(22px, 3vw, 34px);
  animation: floatY 9s ease-in-out infinite;
  filter: drop-shadow(0 4px 10px rgba(0, 0, 0, 0.25));
  opacity: 0.9;
}
.l1 { left: 6vw; top: 12vh; animation-delay: 0.2s; }
.l2 { left: 14vw; top: 24vh; animation-delay: 1.1s; }
.l3 { left: 22vw; top: 8vh;  animation-delay: 2s; }

.floaty {
  position: absolute;
  font-size: clamp(16px, 2.6vw, 28px);
  animation: drift 12s linear infinite;
  opacity: 0.8;
}
.f1 { left: 10vw; bottom: 10vh; animation-delay: 0s; }
.f2 { left: 30vw; bottom: 16vh; animation-delay: 1s; }
.f3 { left: 50vw; bottom: 14vh; animation-delay: 2s; }
.f4 { left: 70vw; bottom: 12vh; animation-delay: 3s; }
.f5 { left: 85vw; bottom: 20vh; animation-delay: 4s; }

@keyframes drift {
  0%   { transform: translateY(0) translateX(0) rotate(0deg); }
  50%  { transform: translateY(-6vh) translateX(1vw) rotate(6deg); }
  100% { transform: translateY(0) translateX(0) rotate(0deg); }
}

@keyframes floatY {
  0%, 100% { transform: translateY(-8px); }
  50%      { transform: translateY(8px); }
}

/* 카드 */
.card {
  position: relative;
  z-index: 1;
  width: min(920px, 100%);
  background: var(--card);
  backdrop-filter: blur(10px);
  border-radius: 20px;
  box-shadow: var(--shadow);
  padding: clamp(20px, 4vw, 40px);
  border: 1px solid rgba(255, 255, 255, 0.12);
}

.hero h1 {
  line-height: 1.2;
  font-size: clamp(26px, 4vw, 42px);
  letter-spacing: 0.2px;
  margin: 0;
}
.accent { color: var(--accent); }
.wave { position: relative; }
.wave::after {
  content: '';
  position: absolute;
  left: 0; right: 0; bottom: -6px;
  height: 6px;
  background: linear-gradient(90deg, var(--accent), var(--accent-strong));
  border-radius: 999px;
  opacity: .85;
}

.subtitle {
  margin-top: .75rem;
  color: var(--muted);
  font-size: clamp(14px, 1.8vw, 16px);
}

/* 덕담 구역 */
.message {
  margin-top: 1.5rem;
}
.badge {
  display: inline-flex;
  align-items: center;
  gap: .5rem;
  padding: .25rem .6rem;
  border-radius: 999px;
  background: var(--glass);
  font-size: .9rem;
  margin-right: .5rem;
}
.kkot {
  font-size: clamp(18px, 2.4vw, 22px);
  line-height: 1.6;
}

.fade-in {
  display: inline-block;
  animation: fade 450ms ease;
}
@keyframes fade {
  from { opacity: 0; transform: translateY(6px); }
  to   { opacity: 1; transform: translateY(0); }
}

.actions {
  display: flex;
  flex-wrap: wrap;
  gap: .75rem;
  margin-top: 1rem;
}
.btn, .ghost {
  appearance: none;
  border: none;
  cursor: pointer;
  padding: .8rem 1.1rem;
  border-radius: 12px;
  font-weight: 600;
  transition: transform .05s ease, box-shadow .2s ease, background .2s ease, color .2s ease;
}
.btn {
  background: linear-gradient(90deg, var(--accent), var(--accent-strong));
  color: #1b1b1b;
  box-shadow: 0 10px 18px rgba(255, 179, 0, .25);
}
.btn:active { transform: translateY(1px) scale(.99); }

.ghost {
  background: transparent;
  color: var(--text);
  border: 1px solid rgba(255,255,255,.25);
}
.ghost:hover { background: rgba(255,255,255,.06); }

/* 팁 리스트 */
.tips {
  display: grid;
  grid-template-columns: repeat(3, minmax(0,1fr));
  gap: .8rem;
  margin-top: 1.5rem;
}
.tip {
  background: rgba(0,0,0,.12);
  border: 1px solid rgba(255,255,255,.12);
  padding: .9rem 1rem;
  border-radius: 14px;
}
.tip strong { display: block; margin-bottom: .25rem; }
.tip small { color: var(--muted); }

@media (prefers-color-scheme: light) {
  .tip {
    background: rgba(255,255,255,.6);
    border-color: rgba(0,0,0,.06);
  }
}

@media (max-width: 720px) {
  .tips { grid-template-columns: 1fr; }
}

/* 시그니처 */
.signature {
  position: absolute;
  bottom: 14px;
  right: 16px;
  font-size: 12px;
  color: rgba(255,255,255,.7);
  text-decoration: none;
}
@media (prefers-color-scheme: light) {
  .signature { color: rgba(40,40,40,.6); }
}
</style>