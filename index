<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8" />
<title>뽀모도로 타이머</title>
<style>
  :root {
    --main-bg: #f4f6fa;
    --primary: #6b8eef;
    --accent: #f99ca9;
    --border: #dde5ee;
    --text-dark: #222f3e;
    --text-light: #797d8c;
    --shadow: 0 4px 16px 0 #aeccfa50;
  }
  body {
    background: var(--main-bg);
    min-height: 100vh;
    margin: 0;
    display: flex; flex-direction: column; align-items: center; justify-content: center;
    font-family: "Pretendard", "Apple SD Gothic Neo", Arial, sans-serif;
  }
  .timer-container {
    background: #fff;
    padding: 48px 36px 36px 36px;
    border-radius: 24px;
    box-shadow: var(--shadow);
    display: inline-block;
    min-width: 375px;
    border: 1px solid var(--border);
    text-align: center;
  }
  .inputs {
    margin-bottom: 24px;
  }
  label {
    font-size: 15px;
    color: var(--text-light);
    margin: 0 7px;
  }
  input[type="number"] {
    padding: 6px;
    width: 50px;
    font-size: 15px;
    border: 1px solid var(--border);
    border-radius: 8px;
    text-align: center;
    outline: none;
    color: var(--primary);
    background: #f2f7fa;
  }
  #timer {
    font-size: 70px;
    color: var(--primary);
    margin: 18px 0;
    font-weight: 700;
    letter-spacing: 3px;
  }
  #status {
    font-size: 22px;
    color: var(--text-dark);
    margin-bottom: 8px;
  }
  .btn-group {
    margin-top: 12px;
    display: flex; justify-content: center; gap: 12px;
  }
  button {
    font-size: 17px;
    padding: 10px 28px;
    background: var(--primary);
    color: #fff;
    border-radius: 12px;
    border: none;
    margin: 0;
    cursor: pointer;
    transition: background .12s, box-shadow .12s;
    box-shadow: 0 2px 6px 0 #93a6e220;
  }
  button:disabled {
    background: #c8d2e8;
    cursor: not-allowed;
  }
  #pauseBtn.paused {
    background: var(--accent) !important;
    color: #fff;
  }
  /* 플래시 효과 */
  .flash {
    animation: flash 0.4s linear 0s 1;
  }
  @keyframes flash {
    0%   { background: #fff; }
    30%  { background: #fefdcd; }
    50%  { background: #ffe4ef; }
    80%  { background: #fffbea; }
    100% { background: #fff; }
  }
</style>
</head>
<body>
<div class="timer-container" id="mainContainer">

  <div class="inputs">
    <label>공부
      <input type="number" id="workInput" value="25" min="1" max="180">분
    </label>
    <label>휴식
      <input type="number" id="breakInput" value="5" min="1" max="60">분
    </label>
    <label>사이클
      <input type="number" id="cyclesInput" value="4" min="1" max="20">회
    </label>
  </div>

  <div id="status">준비하세요!</div>
  <div id="timer">25:00</div>

  <div class="btn-group">
    <button id="startBtn">시작</button>
    <button id="pauseBtn" disabled>일시정지</button>
    <button id="stopBtn" disabled>중지</button>
  </div>

</div>
<audio id="alarmSound" src="https://actions.google.com/sounds/v1/alarms/alarm_clock.ogg" preload="auto"></audio>
<script>
  let workMinutes, breakMinutes, cycles;
  let currentCycle = 0;
  let isWork = true;
  let timer, totalSeconds, running = false, paused = false;

  const statusEl = document.getElementById('status');
  const timerEl = document.getElementById('timer');
  const startBtn = document.getElementById('startBtn');
  const pauseBtn = document.getElementById('pauseBtn');
  const stopBtn = document.getElementById('stopBtn');
  const alarmSound = document.getElementById('alarmSound');
  const mainContainer = document.getElementById('mainContainer');

  const workInput = document.getElementById('workInput');
  const breakInput = document.getElementById('breakInput');
  const cyclesInput = document.getElementById('cyclesInput');

  function updateTimer() {
    const minutes = Math.floor(totalSeconds / 60);
    const seconds = totalSeconds % 60;
    timerEl.textContent = `${minutes.toString().padStart(2,'0')}:${seconds.toString().padStart(2,'0')}`;
  }

  function flashScreen(times=3) {
    let count = 0;
    const flashFn = () => {
      mainContainer.classList.add('flash');
      setTimeout(()=> mainContainer.classList.remove('flash'), 290);
      if (++count < times) setTimeout(flashFn, 400);
    };
    flashFn();
  }

  function startTimer() {
    if (currentCycle >= cycles) {
      statusEl.textContent = '뽀모도로 완료! 수고했어요!';
      timerEl.textContent = '';
      startBtn.disabled = false;
      pauseBtn.disabled = true;
      stopBtn.disabled = true;
      running = false;
      return;
    }
    if (isWork) {
      statusEl.textContent = `[${currentCycle + 1}번째 뽀모도로] 공부 시간!`;
      totalSeconds = workMinutes * 60;
    } else {
      statusEl.textContent = `[${currentCycle + 1}번째 뽀모도로] 휴식 시간!`;
      totalSeconds = breakMinutes * 60;
    }
    updateTimer();
    running = true; paused = false; pauseBtn.disabled = false;
    pauseBtn.textContent = '일시정지'; pauseBtn.classList.remove('paused');
    timer = setInterval(() => {
      if (!paused) {
        totalSeconds--;
        updateTimer();
        if (totalSeconds <= 0) {
          clearInterval(timer);
          alarmSound.currentTime = 0;
          alarmSound.play();
          flashScreen(3);
          setTimeout(() => {
            if (!isWork) currentCycle++; // 휴식 끝냈을 때만 증가
            isWork = !isWork;
            startTimer();
          }, 1000);
        }
      }
    }, 1000);
  }

  function stopTimer() {
    clearInterval(timer);
    running = false;
    paused = false;
    alarmSound.pause();
    alarmSound.currentTime = 0;
    statusEl.textContent = '타이머 중지됨';
    timerEl.textContent = '';
    startBtn.disabled = false;
    pauseBtn.disabled = true; pauseBtn.textContent = '일시정지'; pauseBtn.classList.remove('paused');
    stopBtn.disabled = true;
  }

  function pauseTimer() {
    if (running && !paused) {
      paused = true;
      pauseBtn.textContent = '재개';
      pauseBtn.classList.add('paused');
      statusEl.textContent += ' (일시정지됨)';
    } else if (running && paused) {
      paused = false;
      pauseBtn.textContent = '일시정지';
      pauseBtn.classList.remove('paused');
      statusEl.textContent = statusEl.textContent.replace(' (일시정지됨)', '');
    }
  }

  startBtn.addEventListener('click', () => {
    workMinutes = parseInt(workInput.value, 10);
    breakMinutes = parseInt(breakInput.value, 10);
    cycles = parseInt(cyclesInput.value, 10);
    if (isNaN(workMinutes) || workMinutes <= 0 || isNaN(breakMinutes) || breakMinutes <= 0 || isNaN(cycles) || cycles <= 0) {
      alert('모든 입력값은 양의 정수여야 합니다.');
      return;
    }
    startBtn.disabled = true;
    pauseBtn.disabled = false;
    stopBtn.disabled = false;
    currentCycle = 0;
    isWork = true;
    startTimer();
  });

  stopBtn.addEventListener('click', stopTimer);

  pauseBtn.addEventListener('click', pauseTimer);
</script>
</body>
</html>
