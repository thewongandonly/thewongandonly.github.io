<template>
  <div class="monitor" @click="handleBodyClick">
    <!-- Taskbar / Start Bar -->
    <div class="start">
      <div class="start__button" @click.stop="toggleStartMenu">
        <div class="start__logo"></div>
        <p class="start__text">Start</p>
      </div>
      <div class="start__time">
        <p class="start__time-text">{{ currentTime }}</p>
      </div>
    </div>

    <!-- Main Start Menu -->
    <div class="start__menu-main" :class="{ 'menu-open': isStartMenuOpen }">
      <div class="start__menu-main-side">
        <div class="start__menu-main-side-text text">
          <p class="ninefive">95</p>
          <p class="windows">Windows</p>
        </div>
      </div>
      <div class="start__menu-main-items">
        <ul class="start__menu-main-items-list1">
          <!-- Programs Item with Submenu -->
          <li class="start__menu-main-items-item programs" @click.stop="toggleProgramsMenu">
            <img class="programs-img icon" src="https://i.ya-webdesign.com/images/windows-95-icons-png-7.png" alt="Programs"/>
            <p class="start__menu-main-items-item-title programs"><span class="underline">P</span>rograms</p>
            <div class="start__menu-main-items-item-arrowright"></div>
            
            <ul class="start__menu-sub sub__programs" :class="{ 'menu-open': isProgramsMenuOpen }">
              <li class="start__menu-sub-items-item sub__programs-items">
                <img class="accessories-img icon" src="https://i.ya-webdesign.com/images/windows-95-icons-png-7.png" alt="Accessories"/>
                <p class="start__menu-sub-items-item-title accessories">Accessories</p>
                <div class="start__menu-main-items-item-arrowright"></div>
              </li>
              <li class="start__menu-sub-items-item sub__programs-items">
                <img class="startup-img icon" src="https://i.ya-webdesign.com/images/windows-95-icons-png-7.png" alt="StartUp"/>
                <p class="start__menu-sub-items-item-title startup">StartUp</p>
                <div class="start__menu-main-items-item-arrowright"></div>
              </li>
              <li class="start__menu-sub-items-item sub__programs-items">
                <img class="dos-img icon" src="https://upload.wikimedia.org/wikipedia/commons/3/3d/Msdos-icon.png" alt="DOS"/>
                <p class="start__menu-sub-items-item-title dos">MS-DOS Prompt</p>
                <div class="start__menu-main-items-item-arrowright"></div>
              </li>
            </ul>
          </li>

          <li class="start__menu-main-items-item documents">
            <img class="documents-img icon" src="https://i.ya-webdesign.com/images/windows-95-icons-png-6.png" alt="Documents"/>
            <p class="start__menu-main-items-item-title documents"><span class="underline">D</span>ocuments</p>
            <div class="start__menu-main-items-item-arrowright"></div>
          </li>
          <li class="start__menu-main-items-item settings">
            <div class="settings-img icon bg-img"></div>
            <p class="start__menu-main-items-item-title settings"><span class="underline">S</span>ettings</p>
            <div class="start__menu-main-items-item-arrowright"></div>
          </li>
          <li class="start__menu-main-items-item find">
            <div class="find-img icon bg-img"></div>
            <p class="start__menu-main-items-item-title find"><span class="underline">F</span>ind</p>
            <div class="start__menu-main-items-item-arrowright"></div>
          </li>
          <li class="start__menu-main-items-item help">
            <img class="help-img icon" src="https://expensive.toys/img/about-icon.png" alt="Help"/>
            <p class="start__menu-main-items-item-title help"><span class="underline">H</span>elp</p>
          </li>
          <li class="start__menu-main-items-item run">
            <div class="run-img icon bg-img"></div>
            <p class="start__menu-main-items-item-title run"><span class="underline">R</span>un...</p>
          </li>
        </ul>

        <ul class="start__menu-main-items-list2">
          <li class="start__menu-main-items-item shutdown">
            <div class="shutdown-img icon bg-img"></div>
            <p class="start__menu-main-items-item-title shutdown">Sh<span class="underline">u</span>t Down...</p>
          </li>
        </ul>
      </div>
    </div>

    <!-- Desktop Icons (Draggable via Directive or Mousedown Events) -->
    <div 
      class="computer desktop-icon"
      :style="{ top: computerPos.top + 'px', left: computerPos.left + 'px' }"
      @mousedown="startDrag($event, 'computer')"
    >
      <div class="computer__icon desktop-icon__icon"></div>
      <p class="computer__text desktop-icon__text">My Computer</p>
    </div>

    <div 
      class="recycle desktop-icon"
      :style="{ top: recyclePos.top + 'px', left: recyclePos.left + 'px' }"
      @mousedown="startDrag($event, 'recycle')"
    >
      <div class="recycle__icon desktop-icon__icon"></div>
      <p class="recycle__text desktop-icon__text">Recycle Bin</p>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive, onMounted, onUnmounted } from 'vue'

// --- Time State & Logic ---
const currentTime = ref('')
let timer = null

const updateTime = () => {
  const today = new Date()
  const hours24 = today.getHours()
  const minutes = today.getMinutes()
  
  const suffix = hours24 >= 12 ? ' PM' : ' AM'
  const hours12 = hours24 % 12 || 12
  const formattedMinutes = minutes < 10 ? '0' + minutes : minutes

  currentTime.value = `${hours12}:${formattedMinutes}${suffix}`
}

onMounted(() => {
  updateTime()
  timer = setInterval(updateTime, 1000)
})

onUnmounted(() => {
  clearInterval(timer)
})

// --- Start Menu Controls ---
const isStartMenuOpen = ref(false)
const isProgramsMenuOpen = ref(false)

const toggleStartMenu = () => {
  isStartMenuOpen.value = !isStartMenuOpen.value
  if (!isStartMenuOpen.value) {
    isProgramsMenuOpen.value = false
  }
}

const toggleProgramsMenu = () => {
  isProgramsMenuOpen.value = !isProgramsMenuOpen.value
}

const handleBodyClick = (e) => {
  // If click is outside elements starting with 'start__', close the start menu
  const isStartElement = e.target.closest('[class*="start__"]')
  if (!isStartElement) {
    isStartMenuOpen.value = false
    isProgramsMenuOpen.value = false
  }
}

// --- Draggable Icons Logic ---
const computerPos = reactive({ top: 20, left: 10 })
const recyclePos = reactive({ top: 120, left: 10 })

const startDrag = (e, iconKey) => {
  e.preventDefault()
  let startX = e.clientX
  let startY = e.clientY

  const targetPos = iconKey === 'computer' ? computerPos : recyclePos

  const onMouseMove = (moveEvent) => {
    moveEvent.preventDefault()
    const dx = startX - moveEvent.clientX
    const dy = startY - moveEvent.clientY
    startX = moveEvent.clientX
    startY = moveEvent.clientY

    targetPos.top -= dy
    targetPos.left -= dx
  }

  const onMouseUp = () => {
    document.removeEventListener('mousemove', onMouseMove)
    document.removeEventListener('mouseup', onMouseUp)
  }

  document.addEventListener('mousemove', onMouseMove)
  document.addEventListener('mouseup', onMouseUp)
}
</script>

<style scoped>
 
/* Scope background styles to the component layout wrapper */
.monitor {
  position: relative;
  width: 800px;
  height: 600px;
  background: #008080;
  margin: 15vh auto 0 auto;
  font-family: Tahoma, sans-serif;
  user-select: none;
  overflow: hidden;
}

.start {
  position: absolute;
  bottom: 0;
  width: 100%;
  height: 35px;
  background: #c3c6cd;
  border-top: 3px solid #deebf3;
  display: flex;
  justify-content: space-between;
  z-index: 100;
}
.start__button {
  width: 80px;
  height: 72%;
  background: #c3c6cd;
  border-top: 2px solid #deebf3;
  border-left: 2px solid #deebf3;
  border-right: 2px solid black;
  border-bottom: 2px solid black;
  margin: 3px;
  display: flex;
  align-items: center;
}
.start__button:active {
  border-top: 2px solid black;
  border-left: 2px solid black;
  border-right: 2px solid #deebf3;
  border-bottom: 2px solid #deebf3;
}
.start__button:hover {
  cursor: pointer;
}
.start__logo {
  width: 35px;
  height: 18px;
  position: relative;
}
.start__logo::after {
  content: "";
  background-size: contain;
  mix-blend-mode: multiply;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  position: absolute;
}
.start__text {
  font-size: 14px;
  font-weight: bold;
}
.start__time {
  width: 80px;
  height: 72%;
  background: #c3c6cd;
  border-top: 2px solid #85888f;
  border-left: 2px solid #85888f;
  border-right: 2px solid #deebf3;
  border-bottom: 2px solid #deebf3;
  margin: 3px;
  display: flex;
  align-items: center;
}
.start__time-text {
  margin: auto;
  font-size: 12px;
}
.start__menu-main {
  background: #c3c6cd;
  border-top: 2px solid #deebf3;
  border-left: 2px solid #deebf3;
  border-right: 2px solid black;
  border-bottom: 2px solid black;
  width: 20%;
  display: none;
  position: absolute;
  bottom: 38px;
  z-index: 101;
}
.start__menu-main-side {
  background: #85888f;
  width: 15%;
}
.start__menu-main-side-text {
  position: relative;
  margin-top: 202px;
}
.start__menu-main-side-text .windows {
  font-weight: bold;
  color: #c3c6cd;
  transform: rotate(270deg);
}
.start__menu-main-side-text .ninefive {
  color: white;
  transform: rotate(270deg);
  margin-bottom: 56px;
}
.start__menu-main-items {
  padding: 0;
  width: 100%;
}
.start__menu-main-items .icon {
  width: 30px;
  height: 30px;
  padding-left: 5px;
  padding-right: 5px;
}
.start__menu-main-items-item {
  font-size: 12px;
  padding-top: 3px;
  padding-bottom: 3px;
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
}
.start__menu-main-items-item:hover {
  background: #00207d;
  color: white;
  cursor: pointer;
}
.start__menu-main-items-item:hover .start__menu-main-items-item-title {
  color: white;
}
.start__menu-main-items-item:hover .start__menu-main-items-item-arrowright {
  border-left: 4px solid white;
}
.start__menu-main-items-list1 {
  margin: 0;
  padding: 0;
  list-style: none;
  border-bottom: 1px solid #85888f;
}
.start__menu-main-items-list2 {
  margin: 0;
  padding: 0;
  list-style: none;
  border-top: 1px solid #deebf3;
}
.start__menu-sub {
  background: #c3c6cd;
  border-top: 2px solid #deebf3;
  border-left: 2px solid #deebf3;
  border-right: 2px solid black;
  border-bottom: 2px solid black;
  flex-direction: column;
  width: 180px;
  left: 156px;
  padding: 0;
  list-style: none;
  display: none;
  position: absolute;
  bottom: 226px;
}
.start__menu-sub .icon {
  width: 23px;
  height: 23px;
}
.start__menu-sub-items-item {
  display: flex;
  align-items: center;
}
.start__menu-sub-items-item:hover {
  background: #00207d;
  color: white;
}
.start__menu-sub-items-item:hover .start__menu-sub-items-item-title {
  color: white;
}
.start__menu-sub-items-item:hover .start__menu-sub-items-item-arrowright {
  border-left: 4px solid white;
}
.start__menu-sub-items-item-title {
  margin: 7px 0;
}

.desktop-icon {
  position: absolute;
  height: 100px;
  width: 100px;
  display: flex;
  flex-direction: column;
  align-items: center;
}
.desktop-icon__icon {
  height: 50px;
}
.desktop-icon__text {
  color: white;
  font-size: 12px;
  margin-top: 5px;
  margin-bottom: 0;
  text-align: center;
  text-shadow: 1px 1px black;
}
.desktop-icon:hover {
  cursor: pointer;
}

.computer {
  top: 20px;
  left: 10px;
}
.computer__icon {
  width: 55px;
  background: url("https://www.pngkey.com/png/full/44-448393_windows-95-icons-png-picture-free-stock-icon.png") no-repeat center;
  background-size: contain;
}

.recycle {
  top: 120px;
  left: 10px;
}
.recycle__icon {
  width: 53px;
  background: url("https://telegram-stickers.github.io/public/stickers/windows-95/27.png") no-repeat center;
  background-size: contain;
}

.start__menu-main-items-item-arrowright {
  width: 0;
  height: 0;
  border-top: 4px solid transparent;
  border-bottom: 4px solid transparent;
  border-left: 4px solid black;
  margin-left: auto;
  margin-right: 7px;
}

.underline {
  text-decoration: underline;
}

.menu-open {
  display: flex !important;
}

.bg-img {
  background-image: url(https://i.pinimg.com/originals/96/17/c0/9617c03faeb0986659acd2fa7ff87a26.png);
  object-fit: cover;
  width: 200px;
  height: 200px;
  mix-blend-mode: multiply;
}

.settings-img {
  background-position: 87% 21%;
}

.find-img {
  background-position: 25% 22%;
}

.run-img {
  background-position: 0% 80%;
}

.shutdown-img {
  background-position: 75% 79%;
}
</style>