<template>
  <div 
    class="fullscreen-container" 
    ref="pixiContainer"
  >
    <!-- Fire GIF Overlays (Positioned over the stove) -->
    <template v-if="isFireLit">
      <img src="/fire.gif" class="fire-gif" :style="fireStyleLeft" />
      <img src="/fire.gif" class="fire-gif" :style="fireStyleCenter" />
      <img src="/fire.gif" class="fire-gif" :style="fireStyleRight" />
    </template>

    <!-- Title Bar -->
    <div class="title-bar">
      <div class="title">Become Tomyum</div>
      <div class="title-actions">
        <!-- Import Icon -->
        <button class="icon-btn" @click="triggerFileInput" title="Import Picture">
          <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"/><polyline points="17 8 12 3 7 8"/><line x1="12" y1="3" x2="12" y2="15"/></svg>
        </button>
        <!-- Share Icon -->
        <button class="icon-btn" @click="shareScreenshot" title="Share Screenshot">
          <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><circle cx="18" cy="5" r="3"/><circle cx="6" cy="12" r="3"/><circle cx="18" cy="19" r="3"/><line x1="8.59" y1="13.51" x2="15.42" y2="17.49"/><line x1="15.41" y1="6.51" x2="8.59" y2="10.49"/></svg>
        </button>
      </div>
    </div>

    <!-- Note Button -->
    <button class="note-toggle" @click="toggleNote" :title="isNoteOpen ? 'Close Notes' : 'Open Notes'">
      <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><path d="M2 3h6a4 4 0 0 1 4 4v14a3 3 0 0 0-3-3H2z"/><path d="M22 3h-6a4 4 0 0 0-4 4v14a3 3 0 0 1 3-3h7z"/></svg>
    </button>

    <!-- Help Button -->
    <button class="help-toggle" @click="toggleHelp" :title="isHelpOpen ? 'Close Help' : 'Open Help'">
      <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"/><path d="M9.09 9a3 3 0 0 1 5.83 1c0 2-3 3-3 3"/><line x1="12" y1="17" x2="12.01" y2="17"/></svg>
    </button>

    <!-- X-Ray Button -->
    <button class="xray-toggle" @click="toggleXray" :title="isXrayMode ? 'Disable X-Ray' : 'Enable X-Ray'">
      <svg v-if="isXrayMode" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z"/><circle cx="12" cy="12" r="3"/></svg>
      <svg v-else xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><path d="M17.94 17.94A10.07 10.07 0 0 1 12 20c-7 0-11-8-11-8a18.45 18.45 0 0 1 5.06-5.94M9.9 4.24A9.12 9.12 0 0 1 12 4c7 0 11 8 11 8a18.5 18.5 0 0 1-2.16 3.19m-6.72-1.07a3 3 0 1 1-4.24-4.24"/><line x1="1" y1="1" x2="23" y2="23"/></svg>
    </button>

    <!-- Ingredients Toggle Button -->
    <button class="ingredients-toggle" @click="toggleSidebar" :title="isSidebarOpen ? 'Close Ingredients' : 'Open Ingredients'">
      <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><path d="M2.27 21.73A2.5 2.5 0 0 0 5.8 22a6.9 6.9 0 0 0 5.41-2.58l.42-.51c.32-.38.7-.72 1.12-1l7.86-5.11a3.07 3.07 0 0 0 .5-4.47l-2.42-2.42a3.07 3.07 0 0 0-4.47.5L9.1 14.28c-.28.42-.62.8-1 1.12l-.51.42A6.9 6.9 0 0 0 5 21.23a2.5 2.5 0 0 0-2.73.5Z"/><path d="M12.44 9.17 14.83 6.78"/><path d="M15.5 5.5l2-2"/><path d="M18.5 8.5l2-2"/></svg>
    </button>

    <!-- Clear Button -->
    <button class="clear-btn-icon" @click="clearSprites" title="Clear All">
      <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><polyline points="3 6 5 6 21 6"/><path d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6m3 0V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2"/><line x1="10" y1="11" x2="10" y2="17"/><line x1="14" y1="11" x2="14" y2="17"/></svg>
    </button>
    
    <input type="file" ref="fileInput" @change="onFileSelected" accept="image/png, image/jpeg" style="display: none" />
    
    <!-- Left Sidebar (Notes or Help) -->
    <div :class="['left-sidebar', { 'is-open': isNoteOpen || isHelpOpen }]">
      <div class="sidebar-header">
        <h3>{{ isNoteOpen ? 'Notes' : 'How to Play' }}</h3>
        <button class="close-sidebar-btn" @click="closeLeftSidebar" title="Close Sidebar">
          <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><polyline points="15 18 9 12 15 6"/></svg>
        </button>
      </div>
      
      <!-- Notes Content -->
      <div v-if="isNoteOpen" class="sidebar-content notes-content">
        <div v-if="isLoadingNotes" class="loading-text">Loading notes...</div>
        <div v-else-if="notes.length === 0" class="empty-text">No notes found. Add .txt files to public/notes/</div>
        <div v-else class="note-item" v-for="(note, index) in notes" :key="index">
          <p>{{ note }}</p>
        </div>
      </div>
      
      <!-- Help Content -->
      <div v-if="isHelpOpen" class="sidebar-content help-content">
        <div class="help-item">
          <h4>🔥 Cooking</h4>
          <p>Click on the stove underneath the pot to turn it on and off. Wait a few seconds for the water to boil!</p>
        </div>
        <div class="help-item">
          <h4>🌿 Herbs</h4>
          <p>Open the ingredients menu on the right. Click any herb to drop it into the boiling pot.</p>
        </div>
        <div class="help-item">
          <h4>📸 Custom Pictures</h4>
          <p>Click the import button on the top right to upload your own picture. You can crop it and use Magic Die-cut to perfectly remove the background!</p>
        </div>
      </div>
    </div>

    <!-- Ingredients Sidebar -->
    <div :class="['ingredients-sidebar', { 'is-open': isSidebarOpen }]">
      <div class="sidebar-header">
        <h3>Fresh Herbs</h3>
        <button class="close-sidebar-btn" @click="toggleSidebar" title="Close Sidebar">
          <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><polyline points="9 18 15 12 9 6"/></svg>
        </button>
      </div>
      <div class="herbs-list">
        <div 
          v-for="herb in presetHerbs" 
          :key="herb.name" 
          class="herb-item"
          @click="spawnHerb(herb)"
        >
          <img :src="`/ingredients/thumbnail/${herb.name}.png`" :alt="herb.name" draggable="false" />
          <span>{{ herb.name }}</span>
        </div>
      </div>
    </div>
  </div>

  <div v-if="isAdjusting" class="adjust-overlay friendly-overlay">
    <div class="adjust-panel">
      <div class="adjust-header">
        <h3>Prepare Ingredient</h3>
        <button class="close-btn" @click="cancelAdjust">✖</button>
      </div>
      
      <div class="controls-group">
        <div class="stencil-toggle">
          <label>
            <input type="radio" value="rectangle" v-model="stencilType" /> Rectangle
          </label>
          <label>
            <input type="radio" value="circle" v-model="stencilType" /> Circle
          </label>
        </div>
        <div class="die-cut-toggle">
          <label title="Removes background and tight-crops to the subject">
            <input type="checkbox" v-model="applyDieCut" /> ✂️ Magic Auto Die-cut
          </label>
        </div>
      </div>
      
      <div class="cropper-container">
        <Cropper
          ref="cropper"
          class="cropper friendly-cropper"
          :src="uploadedImageSrc"
          :stencil-component="stencilType === 'circle' ? CircleStencil : RectangleStencil"
          background-class="cropper-background"
        />
      </div>
      
      <div class="adjust-footer">
        <button class="add-btn friendly-btn" @click="addCroppedImage" :disabled="isProcessing">
          {{ isProcessing ? 'Working...' : 'Toss into Pot!' }}
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'
import { Cropper, CircleStencil, RectangleStencil } from 'vue-advanced-cropper'
import 'vue-advanced-cropper/dist/style.css'
import { removeBackground } from '@imgly/background-removal'

const pixiContainer = ref(null)
let app = null
let PIXI = null
const sprites = []
const particles = []

const isXrayMode = ref(false)

// UI Refs
const fileInput = ref(null)
const isAdjusting = ref(false)
const uploadedImageSrc = ref(null)
const cropper = ref(null)
const stencilType = ref('rectangle')
const applyDieCut = ref(false)
const isProcessing = ref(false)

// Ingredients Sidebar
const isSidebarOpen = ref(false)
const presetHerbs = ref([
  { name: 'galangal', maxIndex: 5 },
  { name: 'lemongrass', maxIndex: 6 },
  { name: 'redonion', maxIndex: 5 }
])

// Note & Help Sidebars
const isNoteOpen = ref(false)
const isHelpOpen = ref(false)
const notes = ref([])
const isLoadingNotes = ref(false)
const noteFiles = ['note.txt', 'note1.txt', 'note2.txt', 'note3.txt', 'recipe.txt']

// Stove & Fire Mechanics
const isFireLit = ref(false)
const isBoiling = ref(false)
const fireStyleCenter = ref({})
const fireStyleLeft = ref({})
const fireStyleRight = ref({})
let boilTimer = 0
let boilTargetMs = 0

function toggleSidebar() {
  isSidebarOpen.value = !isSidebarOpen.value
}

function toggleNote() {
  isNoteOpen.value = !isNoteOpen.value
  if (isNoteOpen.value) {
    isHelpOpen.value = false
    loadNotes()
  }
}

function toggleHelp() {
  isHelpOpen.value = !isHelpOpen.value
  if (isHelpOpen.value) {
    isNoteOpen.value = false
  }
}

function closeLeftSidebar() {
  isNoteOpen.value = false
  isHelpOpen.value = false
}

async function loadNotes() {
  if (notes.value.length > 0) return // Already loaded
  isLoadingNotes.value = true
  const loadedNotes = []
  
  for (const file of noteFiles) {
    try {
      const response = await fetch(`/notes/${file}`)
      if (response.ok) {
        const contentType = response.headers.get('content-type')
        // Only load if it's actually a text file, ignoring HTML fallbacks from dev server
        if (contentType && contentType.includes('text/plain')) {
          const text = await response.text()
          if (text) loadedNotes.push(text)
        }
      }
    } catch (e) {
      console.log(`Could not load ${file}`)
    }
  }
  
  notes.value = loadedNotes
  isLoadingNotes.value = false
}

async function spawnHerb(herb) {
  const randomNum = Math.floor(Math.random() * herb.maxIndex) + 1
  const imageUrl = `/ingredients/sprite/${herb.name}${randomNum}.png`
  
  try {
    const texture = await PIXI.Assets.load(imageUrl)
    addSpriteToCanvas(texture, null, null, 0.50) // sprite spawn scale = 50%
  } catch (e) {
    console.error('Failed to load herb sprite', e)
  }
}

function toggleXray() {
  isXrayMode.value = !isXrayMode.value
  if (app) {
    drawPot()
  }
}

function triggerFileInput() {
  if (fileInput.value) {
    fileInput.value.click()
  }
}

function onFileSelected(event) {
  const file = event.target.files[0]
  if (file) {
    uploadedImageSrc.value = URL.createObjectURL(file)
    isAdjusting.value = true
    event.target.value = '' // reset
  }
}

function cancelAdjust() {
  isAdjusting.value = false
  if (uploadedImageSrc.value) {
    URL.revokeObjectURL(uploadedImageSrc.value)
  }
  uploadedImageSrc.value = null
}

async function addCroppedImage() {
  if (cropper.value && PIXI && app) {
    isProcessing.value = true
    try {
      const { canvas } = cropper.value.getResult()
      if (canvas) {
        let finalDataUrl = canvas.toDataURL('image/png')
        
        if (applyDieCut.value) {
          const blob = await new Promise(resolve => canvas.toBlob(resolve, 'image/png'))
          const bgRemovedBlob = await removeBackground(blob)
          finalDataUrl = await tightCropBlob(bgRemovedBlob)
        }
        
        const texture = await PIXI.Assets.load(finalDataUrl)
        addSpriteToCanvas(texture)
      }
    } catch (e) {
      console.error('Error during image processing:', e)
    } finally {
      isProcessing.value = false
      cancelAdjust()
    }
  }
}

async function tightCropBlob(blob) {
  return new Promise((resolve) => {
    const url = URL.createObjectURL(blob)
    const img = new Image()
    img.onload = () => {
      const c = document.createElement('canvas')
      c.width = img.width
      c.height = img.height
      const ctx = c.getContext('2d', { willReadFrequently: true })
      ctx.drawImage(img, 0, 0)
      
      const imgData = ctx.getImageData(0, 0, c.width, c.height)
      const data = imgData.data
      
      let minX = c.width, minY = c.height, maxX = 0, maxY = 0
      let hasVisiblePixels = false
      
      for (let y = 0; y < c.height; y++) {
        for (let x = 0; x < c.width; x++) {
          const alpha = data[(y * c.width + x) * 4 + 3]
          if (alpha > 10) {
            if (x < minX) minX = x
            if (x > maxX) maxX = x
            if (y < minY) minY = y
            if (y > maxY) maxY = y
            hasVisiblePixels = true
          }
        }
      }
      
      if (!hasVisiblePixels) {
        resolve(c.toDataURL('image/png'))
        return
      }
      
      const cropW = maxX - minX + 1
      const cropH = maxY - minY + 1
      
      const cropCanvas = document.createElement('canvas')
      cropCanvas.width = cropW
      cropCanvas.height = cropH
      const cropCtx = cropCanvas.getContext('2d')
      
      cropCtx.drawImage(c, minX, minY, cropW, cropH, 0, 0, cropW, cropH)
      
      URL.revokeObjectURL(url)
      resolve(cropCanvas.toDataURL('image/png'))
    }
    img.src = url
  })
}

function clearSprites() {
  sprites.forEach(s => {
    app.stage.removeChild(s.sprite)
    s.sprite.destroy(true)
  })
  sprites.splice(0, sprites.length)
}

async function shareScreenshot() {
  if (!app) return
  try {
    const dataUrl = await app.renderer.extract.base64({ target: app.stage })
    if (navigator.share) {
      const blob = await (await fetch(dataUrl)).blob()
      const file = new File([blob], 'become-tomyum.png', { type: 'image/png' })
      await navigator.share({
        title: 'Become Tomyum',
        text: 'Look at what I put in my Tomyum pot!',
        files: [file]
      })
    } else {
      downloadScreenshot(dataUrl)
    }
  } catch (e) {
    console.error('Error sharing screenshot:', e)
  }
}

function downloadScreenshot(dataUrl) {
  const link = document.createElement('a')
  link.href = dataUrl
  link.download = 'become-tomyum.png'
  document.body.appendChild(link)
  link.click()
  document.body.removeChild(link)
}

// Pot objects and properties
let bgSprite = null
let stove = null
let potBack = null
let water = null
let potFront = null

let potX = 0
let potY = 0
let potWidth = 0
let potHeight = 0
let potHalfW = 0
let potHalfH = 0
let stoveHeight = 0

function resizeBackground() {
  if (!bgSprite || !app) return
  const scaleX = app.screen.width / bgSprite.texture.width
  const scaleY = app.screen.height / bgSprite.texture.height
  const scale = Math.max(scaleX, scaleY)
  bgSprite.scale.set(scale)
  bgSprite.x = app.screen.width / 2
  bgSprite.y = app.screen.height / 2
  bgSprite.anchor.set(0.5)
}

function fitSpriteToScreen(targetSprite, scaleFactor = 1.0) {
  if (!targetSprite || !targetSprite.texture || !app) return
  const screenMin = Math.min(app.screen.width, app.screen.height)
  const targetSize = Math.max(120, Math.min(220, screenMin * 0.25)) * scaleFactor
  const maxTextureDim = Math.max(targetSprite.texture.width, targetSprite.texture.height)
  if (maxTextureDim > 0) {
    const scale = targetSize / maxTextureDim
    targetSprite.scale.set(scale)
  }
}

function addSpriteToCanvas(texture, dropX = null, dropY = null, scaleFactor = 1.0) {
  if (!app || !PIXI) return

  const sprite = new PIXI.Sprite(texture)
  sprite.anchor.set(0.5)
  
  if (dropX !== null && dropY !== null) {
    sprite.x = dropX
    sprite.y = dropY
  } else {
    sprite.x = app.screen.width / 2 + (Math.random() - 0.5) * 40
    sprite.y = app.screen.height / 2 - 150
  }
  
  fitSpriteToScreen(sprite, scaleFactor)

  const potFrontIndex = app.stage.getChildIndex(potFront)
  app.stage.addChildAt(sprite, potFrontIndex)

  const spriteObj = {
    sprite,
    velocity: { x: 0, y: 0 },
    isInPot: false,
    isDragging: false,
    lastPos: { x: 0, y: 0 },
    dragOffset: { x: 0, y: 0 }
  }

  sprite.eventMode = 'static'
  sprite.cursor = 'grab'

  sprite.on('pointerdown', (e) => {
    spriteObj.isDragging = true
    sprite.cursor = 'grabbing'
    spriteObj.lastPos = { x: e.global.x, y: e.global.y }
    spriteObj.dragOffset = { 
      x: sprite.x - e.global.x, 
      y: sprite.y - e.global.y 
    }
    spriteObj.velocity = { x: 0, y: 0 } 
  })

  sprite.on('globalpointermove', (e) => {
    if (spriteObj.isDragging) {
      const newPos = { x: e.global.x, y: e.global.y }
      spriteObj.velocity.x = newPos.x - spriteObj.lastPos.x
      spriteObj.velocity.y = newPos.y - spriteObj.lastPos.y
      sprite.x = newPos.x + spriteObj.dragOffset.x
      sprite.y = newPos.y + spriteObj.dragOffset.y
      applyPhysicsConstraints(spriteObj, true)
      spriteObj.lastPos = newPos
    }
  })

  const stopDrag = () => {
    spriteObj.isDragging = false
    sprite.cursor = 'grab'
  }
  sprite.on('pointerup', stopDrag)
  sprite.on('pointerupoutside', stopDrag)
  sprite.on('globalpointerup', stopDrag)

  sprites.push(spriteObj)
}

let handleResize = null

function drawPot() {
  if (!app) return
  
  potWidth = Math.min(app.screen.width * 0.8, 600)
  potHeight = Math.min(app.screen.height * 0.4, 300)
  potHalfW = potWidth / 2
  potHalfH = potHalfW * 0.3 
  stoveHeight = potHalfH * 0.8
  
  // Shift pot up to make room for stove
  potX = app.screen.width / 2
  potY = app.screen.height - potHeight - potHalfH - 120 // moved up by ~60px more
  
  const kappa = 0.5522848
  const ox = potHalfW * kappa 
  const oy = potHalfH * kappa 

  // 0. Stove (Under pot)
  const stoveY = potY + potHeight + potHalfH - 10
  const stoveHalfW = potHalfW * 0.8
  const stoveHalfH = stoveHeight
  
  stove.clear()
  stove.ellipse(potX, stoveY, stoveHalfW, stoveHalfH)
  stove.fill({ color: 0x222222 })
  stove.stroke({ width: 4, color: 0x444444 })

  // Position DOM Fire Overlays over the stove area
  const fireW = stoveHalfW * 0.55 // Center fire is 25% of the original big size
  const fireH = fireW * 0.6
  
  const fireBaseTop = stoveY - fireH * 0.7 - 10
  
  fireStyleCenter.value = {
    position: 'absolute',
    left: `${potX - fireW / 2}px`,
    top: `${fireBaseTop}px`,
    width: `${fireW}px`,
    height: `${fireH}px`,
    pointerEvents: 'none',
    zIndex: 5
  }
  
  const sideFireW = fireW * 0.9 // Side fires slightly smaller
  const sideFireH = fireH * 0.9
  const sideOffset = fireW * 0.85 // Shift left and right
  
  fireStyleLeft.value = {
    position: 'absolute',
    left: `${potX - sideFireW / 2 - sideOffset}px`,
    top: `${fireBaseTop - 2}px`,
    width: `${sideFireW}px`,
    height: `${sideFireH}px`,
    pointerEvents: 'none',
    zIndex: 5
  }
  
  fireStyleRight.value = {
    position: 'absolute',
    left: `${potX - sideFireW / 2 + sideOffset}px`,
    top: `${fireBaseTop - 2}px`,
    width: `${sideFireW}px`,
    height: `${sideFireH}px`,
    pointerEvents: 'none',
    zIndex: 5
  }
  
  // 1. Back Rim
  potBack.clear()
  potBack.ellipse(potX, potY, potHalfW, potHalfH)
  potBack.fill({ color: 0x444444 }) 

  // Back Rim thickness
  potBack.moveTo(potX - potHalfW, potY)
  potBack.bezierCurveTo(potX - potHalfW, potY - oy, potX - ox, potY - potHalfH, potX, potY - potHalfH)
  potBack.bezierCurveTo(potX + ox, potY - potHalfH, potX + potHalfW, potY - oy, potX + potHalfW, potY)
  potBack.stroke({ width: 6, color: 0xaaaaaa })
  
  // 2. Water
  const waterY = potY + potHalfH * 0.4 
  const waterHalfW = potHalfW * 0.95 
  const waterHalfH = potHalfH * 0.95
  
  water.clear()
  water.ellipse(potX, waterY, waterHalfW, waterHalfH)
  water.fill({ color: 0x0088cc, alpha: isXrayMode.value ? 0.6 : 1 }) 
  
  // 3. Front Body
  potFront.clear()
  potFront.moveTo(potX - potHalfW, potY)
  potFront.bezierCurveTo(potX - potHalfW, potY + oy, potX - ox, potY + potHalfH, potX, potY + potHalfH)
  potFront.bezierCurveTo(potX + ox, potY + potHalfH, potX + potHalfW, potY + oy, potX + potHalfW, potY)
  potFront.lineTo(potX + potHalfW, potY + potHeight)
  potFront.bezierCurveTo(potX + potHalfW, potY + potHeight + oy, potX + ox, potY + potHeight + potHalfH, potX, potY + potHeight + potHalfH)
  potFront.bezierCurveTo(potX - ox, potY + potHeight + potHalfH, potX - potHalfW, potY + potHeight + oy, potX - potHalfW, potY + potHeight)
  potFront.lineTo(potX - potHalfW, potY)
  potFront.fill({ color: 0x888888, alpha: isXrayMode.value ? 0.2 : 1 })
  if (isXrayMode.value) {
    potFront.stroke({ width: 2, color: 0xaaaaaa })
  }
  
  // Front Rim thickness
  potFront.moveTo(potX - potHalfW, potY)
  potFront.bezierCurveTo(potX - potHalfW, potY + oy, potX - ox, potY + potHalfH, potX, potY + potHalfH)
  potFront.bezierCurveTo(potX + ox, potY + potHalfH, potX + potHalfW, potY + oy, potX + potHalfW, potY)
  potFront.stroke({ width: 6, color: 0xaaaaaa })
}

function applyPhysicsConstraints(spriteObj, isDrag) {
  if (!spriteObj || !spriteObj.sprite || !app) return
  
  const sprite = spriteObj.sprite
  const velocity = spriteObj.velocity
  const halfWidth = sprite.width / 2
  const halfHeight = sprite.height / 2
  
  // 1. Screen bounds
  if (sprite.width < app.screen.width) {
    if (sprite.x - halfWidth < 0) {
      if (!isDrag) velocity.x *= -0.8 
      sprite.x = halfWidth 
    } else if (sprite.x + halfWidth > app.screen.width) {
      if (!isDrag) velocity.x *= -0.8 
      sprite.x = app.screen.width - halfWidth 
    }
  }
  
  // Prevent going above title bar
  if (sprite.y - halfHeight < 60) {
    if (!isDrag) velocity.y *= -0.8
    sprite.y = 60 + halfHeight
  }
  
  if (sprite.height < app.screen.height) {
    if (sprite.y + halfHeight > app.screen.height) {
      if (!isDrag) velocity.y *= -0.8
      sprite.y = app.screen.height - halfHeight
    }
  }
  
  // 2. Pot boundaries
  const inPotXRange = sprite.x + halfWidth > potX - potHalfW && sprite.x - halfWidth < potX + potHalfW
  
  if (spriteObj.isInPot) {
    if (sprite.x - halfWidth < potX - potHalfW) {
      sprite.x = potX - potHalfW + halfWidth
      if (!isDrag) velocity.x *= -0.8
    } else if (sprite.x + halfWidth > potX + potHalfW) {
      sprite.x = potX + potHalfW - halfWidth
      if (!isDrag) velocity.x *= -0.8
    }
    if (sprite.y + halfHeight > potY + potHeight) {
      sprite.y = potY + potHeight - halfHeight
      if (!isDrag) velocity.y *= -0.5 
    }
    if (sprite.y + halfHeight < potY) {
      spriteObj.isInPot = false
    }
  } else {
    const hitPotBody = sprite.y + halfHeight > potY && sprite.y - halfHeight < potY + potHeight + potHalfH
    if (inPotXRange && hitPotBody) {
      if (sprite.y < potY + potHalfH * 0.5) {
        spriteObj.isInPot = true
      } else {
        const distLeft = Math.abs((sprite.x + halfWidth) - (potX - potHalfW))
        const distRight = Math.abs((sprite.x - halfWidth) - (potX + potHalfW))
        const distBottom = Math.abs((sprite.y - halfHeight) - (potY + potHeight + potHalfH))
        const minDist = Math.min(distLeft, distRight, distBottom)
        if (minDist === distLeft) {
          sprite.x = potX - potHalfW - halfWidth
          if (!isDrag) velocity.x *= -0.8
        } else if (minDist === distRight) {
          sprite.x = potX + potHalfW + halfWidth
          if (!isDrag) velocity.x *= -0.8
        } else {
          sprite.y = potY + potHeight + potHalfH + halfHeight
          if (!isDrag) velocity.y *= -0.8
        }
      }
    }
  }
}

onMounted(async () => {
  PIXI = await import('pixi.js')

  // 1. --------- Canvas ---------
  app = new PIXI.Application()
  await app.init({
    resizeTo: pixiContainer.value,        
    backgroundAlpha: 0, // Make transparent so CSS background shows if not loaded
    resolution: window.devicePixelRatio || 1,
    autoDensity: true,
  })
  pixiContainer.value.appendChild(app.canvas)

  // Load and add background first
  try {
    const bgTexture = await PIXI.Assets.load('/wood_background.jpg')
    bgSprite = new PIXI.Sprite(bgTexture)
    app.stage.addChild(bgSprite)
    resizeBackground()
  } catch(e) {
    console.log('No background image found, using fallback')
  }

  // Create graphics layers
  stove = new PIXI.Graphics()
  potBack = new PIXI.Graphics()
  water = new PIXI.Graphics()
  potFront = new PIXI.Graphics()

  app.stage.addChild(stove)
  app.stage.addChild(potBack)
  app.stage.addChild(water)
  app.stage.addChild(potFront) // Front layer needs to be above sprites
  
  // Stove Interactions
  stove.eventMode = 'static'
  stove.cursor = 'pointer'
  stove.on('pointerdown', () => {
    isFireLit.value = !isFireLit.value
    if (isFireLit.value) {
      boilTimer = 0
      boilTargetMs = 5000 + Math.random() * 10000 // 5 to 15 seconds random delay
      isBoiling.value = false
    } else {
      isBoiling.value = false
    }
  })

  drawPot()

  // Load initial picture
  try {
    const texture = await PIXI.Assets.load('/placeholder.PNG')
    addSpriteToCanvas(texture)
  } catch (e) {
    console.log('No initial sprite found')
  }

  // 4. -------- Game Loop -------- 
  app.ticker.add((ticker) => {
    
    // Boiling logic
    if (isFireLit.value && !isBoiling.value) {
      boilTimer += ticker.deltaMS
      if (boilTimer >= boilTargetMs) {
        isBoiling.value = true
      }
    }

    const isHeating = isFireLit.value && !isBoiling.value

    // Particles system
    if (isBoiling.value || isHeating) {
      const bubbleChance = isBoiling.value ? 0.5 : 0.15
      
      // Spawn Bubble
      if (Math.random() < bubbleChance) { 
        const p = new PIXI.Graphics()
        const size = isBoiling.value ? (Math.random() * 4 + 2) : (Math.random() * 2 + 1)
        p.circle(0, 0, size)
        p.fill({ color: 0xffffff, alpha: isBoiling.value ? 0.6 : 0.4 })
        
        // Spawn across the whole pot width and depth
        p.x = potX + (Math.random() - 0.5) * potHalfW * 1.8 
        p.y = potY + potHalfH * 0.4 + Math.random() * (potHeight + potHalfH * 0.4)
        
        app.stage.addChildAt(p, app.stage.getChildIndex(water) + 1)
        
        const speed = isBoiling.value ? (Math.random() * 3 + 2) : (Math.random() * 1 + 0.5)
        particles.push({ sprite: p, type: 'bubble', vy: -speed, life: 100 })
      }

      // Spawn Smoke (Only when boiling)
      if (isBoiling.value && Math.random() < 0.1) {
        const p = new PIXI.Graphics()
        p.circle(0, 0, Math.random() * 20 + 10)
        p.fill({ color: 0xffffff, alpha: 0.25 })
        p.x = potX + (Math.random() - 0.5) * potWidth * 0.9
        p.y = potY
        app.stage.addChild(p) // On top of everything
        particles.push({ sprite: p, type: 'smoke', vy: - (Math.random() * 2 + 1), vx: (Math.random() - 0.5) * 1.5, life: 100, maxLife: 100 })
      }
    }

    // Update particles
    for (let i = particles.length - 1; i >= 0; i--) {
      const p = particles[i]
      p.sprite.y += p.vy
      if (p.type === 'smoke') {
        p.sprite.x += p.vx
        p.life -= ticker.deltaTime
        p.sprite.alpha = (p.life / p.maxLife) * 0.25
        if (p.life <= 0) {
          p.sprite.destroy()
          particles.splice(i, 1)
        }
      } else if (p.type === 'bubble') {
        // Pop bubble when reaching near water surface (varies slightly)
        const popY = potY + potHalfH * 0.4 + (Math.random() * 10 - 5)
        if (p.sprite.y < popY) {
          p.sprite.destroy()
          particles.splice(i, 1)
        }
      }
    }

    // Sprite physics
    sprites.forEach(spriteObj => {
      const { sprite, velocity, isInPot, isDragging } = spriteObj

      if (!isDragging) {
        const inWater = isInPot && sprite.y > potY - 20 // Consider slightly above water still 'in water'
        
        if ((isBoiling.value || isHeating) && inWater) {
          // Override gravity - Buoyancy floating logic
          const surfaceY = potY + potHalfH * 0.4
          const distToSurface = sprite.y - surfaceY
          
          if (isBoiling.value) {
            // Boiling: Stronger spring and intense bobbing
            velocity.y -= distToSurface * 0.015
            
            const time = Date.now() * 0.006
            velocity.y += Math.sin(time + sprite.x * 0.1) * 0.5
            velocity.x += (Math.random() - 0.5) * 0.6
          } else {
            // Heating: Gently float to the surface
            velocity.y -= distToSurface * 0.004
            
            const time = Date.now() * 0.003
            velocity.y += Math.sin(time + sprite.x * 0.1) * 0.3
          }
        } else {
          // Normal Gravity outside of boiling/heating water
          velocity.y += 0.6
        }
        
        sprite.x += velocity.x
        sprite.y += velocity.y

        // Friction
        velocity.x *= 0.95
        velocity.y *= 0.95

        // Water resistance (damps the bouncing significantly so it doesn't fly out)
        if (inWater) {
          velocity.x *= 0.85
          velocity.y *= 0.85
        }

        applyPhysicsConstraints(spriteObj, false)
      }
      
      // rotation
      const targetRotation = velocity.x * 0.03
      sprite.rotation += (targetRotation - sprite.rotation) * 0.1 
    })
  })

  handleResize = () => {
    if (!app) return
    resizeBackground()
    drawPot()
    sprites.forEach(s => {
      fitSpriteToScreen(s.sprite, s.sprite.scale.x / (Math.max(120, Math.min(220, Math.min(app.screen.width, app.screen.height) * 0.25)) / Math.max(s.sprite.texture.width, s.sprite.texture.height))) // Try to maintain scaleFactor during resize
      applyPhysicsConstraints(s, false)
    })
  }
  window.addEventListener('resize', handleResize)
})

onBeforeUnmount(() => {
  if (handleResize) {
    window.removeEventListener('resize', handleResize)
  }
  if (app) app.destroy(true)
})
</script>

<style scoped>
.fullscreen-container {
  position: absolute;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  touch-action: none;
  background-color: #f4dcb9; /* Fallback */
  user-select: none;
  -webkit-user-select: none;
}

.fire-gif {
  /* Dynamic styles injected via fireStyle */
  mix-blend-mode: screen; /* Makes black background disappear if there's any */
}

/* Title Bar */
.title-bar {
  position: absolute;
  top: 0; 
  left: 0; 
  right: 0;
  height: 60px;
  background-color: #9d754a;
  box-shadow: 0 4px 8px rgba(0,0,0,0.3);
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 20px;
  z-index: 20;
}

.title {
  color: #fff;
  font-size: 1.5rem;
  font-weight: bold;
  text-shadow: 2px 2px 0px #5c4125;
}

.title-actions {
  display: flex;
  gap: 12px;
}

.icon-btn {
  background: #cba279;
  border: 2px solid #5c4125;
  color: #5c4125;
  border-radius: 50%;
  width: 44px;
  height: 44px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: transform 0.1s, box-shadow 0.1s;
  box-shadow: 0 3px 0 #5c4125;
}

.icon-btn:active {
  transform: translateY(3px);
  box-shadow: 0 0 0 #5c4125;
}

.xray-toggle {
  position: absolute;
  top: 75px;
  right: 20px;
  z-index: 10;
  background: rgba(255, 255, 255, 0.9);
  border: 2px solid #5c4125;
  color: #5c4125;
  border-radius: 50%;
  width: 48px;
  height: 48px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  box-shadow: 0 4px 0 #5c4125;
  backdrop-filter: blur(4px);
  transition: transform 0.1s;
}

.xray-toggle:active {
  transform: translateY(4px);
  box-shadow: 0 0 0 #5c4125;
}

.ingredients-toggle {
  position: absolute;
  top: 135px;
  right: 20px;
  z-index: 10;
  background: rgba(255, 255, 255, 0.9);
  border: 2px solid #5c4125;
  color: #5c4125;
  border-radius: 50%;
  width: 48px;
  height: 48px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  box-shadow: 0 4px 0 #5c4125;
  backdrop-filter: blur(4px);
  transition: transform 0.1s;
}

.ingredients-toggle:active {
  transform: translateY(4px);
  box-shadow: 0 0 0 #5c4125;
}

.note-toggle, .help-toggle {
  position: absolute;
  left: 20px;
  z-index: 10;
  background: rgba(255, 255, 255, 0.9);
  border: 2px solid #5c4125;
  color: #5c4125;
  border-radius: 50%;
  width: 48px;
  height: 48px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  box-shadow: 0 4px 0 #5c4125;
  backdrop-filter: blur(4px);
  transition: transform 0.1s;
}

.note-toggle {
  top: 75px;
}

.help-toggle {
  top: 135px;
}

.note-toggle:active, .help-toggle:active {
  transform: translateY(4px);
  box-shadow: 0 0 0 #5c4125;
}

.clear-btn-icon {
  position: absolute;
  bottom: 25px;
  right: 25px;
  z-index: 10;
  background: #ff6b6b;
  color: white;
  border: 2px solid #a32a2a;
  width: 54px;
  height: 54px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  box-shadow: 0 4px 0 #a32a2a;
  transition: all 0.1s;
}

.clear-btn-icon:active {
  transform: translateY(4px);
  box-shadow: 0 0 0 #a32a2a;
}

.ingredients-sidebar {
  position: absolute;
  top: 60px; /* Below title bar */
  bottom: 0;
  right: -300px;
  width: 250px;
  background: rgba(250, 234, 204, 0.95);
  border-left: 4px solid #9d754a;
  box-shadow: -4px 0 8px rgba(0,0,0,0.2);
  z-index: 15;
  display: flex;
  flex-direction: column;
  transition: right 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

.ingredients-sidebar.is-open {
  right: 0;
}

.left-sidebar {
  position: absolute;
  top: 60px; /* Below title bar */
  bottom: 0;
  left: -320px;
  width: 280px;
  background: rgba(250, 234, 204, 0.95);
  border-right: 4px solid #9d754a;
  box-shadow: 4px 0 8px rgba(0,0,0,0.2);
  z-index: 15;
  display: flex;
  flex-direction: column;
  transition: left 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

.left-sidebar.is-open {
  left: 0;
}

.ingredients-sidebar h3, .left-sidebar h3 {
  margin: 0;
  color: #5c4125;
  font-weight: bold;
}

.sidebar-header {
  background: #f0d0a8;
  border-bottom: 3px solid #9d754a;
  padding: 15px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.close-sidebar-btn {
  background: transparent;
  border: none;
  color: #5c4125;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 4px;
  border-radius: 50%;
  transition: background 0.2s;
}

.close-sidebar-btn:hover {
  background: rgba(92, 65, 37, 0.1);
}

.herbs-list {
  flex-grow: 1;
  overflow-y: auto;
  padding: 15px;
  display: flex;
  flex-direction: column;
  gap: 15px;
}

/* Custom scrollbar for herbs list */
.herbs-list::-webkit-scrollbar {
  width: 8px;
}
.herbs-list::-webkit-scrollbar-track {
  background: rgba(157, 117, 74, 0.2);
}
.herbs-list::-webkit-scrollbar-thumb {
  background: rgba(157, 117, 74, 0.6);
  border-radius: 4px;
}

.herb-item {
  background: white;
  border: 3px solid #c09d73;
  border-radius: 12px;
  padding: 10px;
  display: flex;
  flex-direction: column;
  align-items: center;
  cursor: grab;
  transition: transform 0.1s, box-shadow 0.1s;
  box-shadow: 0 3px 0 #c09d73;
}

.herb-item:active {
  cursor: grabbing;
  transform: translateY(3px);
  box-shadow: 0 0 0 #c09d73;
}

.herb-item img {
  width: 100%;
  max-width: 120px;
  height: auto;
  object-fit: contain;
  margin-bottom: 8px;
}

.herb-item span {
  font-weight: bold;
  color: #5c4125;
  text-transform: capitalize;
}

.sidebar-content {
  flex-grow: 1;
  overflow-y: auto;
  padding: 15px;
  display: flex;
  flex-direction: column;
  gap: 15px;
  color: #5c4125;
}

/* Scrollbar styles for sidebar-content */
.sidebar-content::-webkit-scrollbar {
  width: 8px;
}
.sidebar-content::-webkit-scrollbar-track {
  background: rgba(157, 117, 74, 0.2);
}
.sidebar-content::-webkit-scrollbar-thumb {
  background: rgba(157, 117, 74, 0.6);
  border-radius: 4px;
}

.note-item, .help-item {
  background: white;
  border: 3px solid #c09d73;
  border-radius: 12px;
  padding: 15px;
  box-shadow: 0 3px 0 #c09d73;
}

.help-item h4 {
  margin: 0 0 8px 0;
  font-weight: bold;
  font-size: 1.1rem;
}

.note-item p, .help-item p {
  margin: 0;
  line-height: 1.4;
  white-space: pre-wrap; /* Preserve newlines in txt files */
}

.loading-text, .empty-text {
  text-align: center;
  font-style: italic;
  padding: 20px 0;
  color: #83674b;
}

.friendly-overlay {
  position: fixed;
  top: 0; left: 0; right: 0; bottom: 0;
  background: rgba(40, 25, 10, 0.85);
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20px;
  z-index: 100;
}

.adjust-panel {
  background: #fff8eb;
  border: 4px solid #9d754a;
  border-radius: 20px;
  width: 100%;
  max-width: 450px;
  display: flex;
  flex-direction: column;
  overflow: hidden;
  color: #5c4125;
}

.adjust-header {
  padding: 15px 20px;
  background: #f0d0a8;
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-bottom: 3px solid #9d754a;
}

.adjust-header h3 {
  margin: 0;
  font-size: 1.3rem;
  font-weight: bold;
}

.close-btn {
  background: none;
  border: none;
  font-size: 1.5rem;
  color: #a32a2a;
  cursor: pointer;
  font-weight: bold;
}

.controls-group {
  display: flex;
  flex-direction: column;
  gap: 12px;
  align-items: center;
  padding: 15px;
  background: #faeacc;
}

.stencil-toggle, .die-cut-toggle {
  display: flex;
  gap: 15px;
}

.stencil-toggle label, .die-cut-toggle label {
  display: flex;
  align-items: center;
  gap: 8px;
  cursor: pointer;
  font-size: 1rem;
  font-weight: bold;
  background: #e6c89c;
  padding: 8px 16px;
  border-radius: 20px;
  border: 2px solid #c09d73;
}

.cropper-container {
  height: 250px;
  background: #333;
}

.adjust-footer {
  padding: 15px;
  display: flex;
  justify-content: center;
  background: #f0d0a8;
  border-top: 3px solid #9d754a;
}

.friendly-btn {
  background: #73b364;
  color: white;
  border: 2px solid #3c7030;
  border-radius: 30px;
  padding: 12px 30px;
  font-size: 1.2rem;
  font-weight: bold;
  cursor: pointer;
  box-shadow: 0 4px 0 #3c7030;
  transition: transform 0.1s, box-shadow 0.1s;
}

.friendly-btn:active {
  transform: translateY(4px);
  box-shadow: 0 0 0 #3c7030;
}
.friendly-btn:disabled {
  background: #999;
  border-color: #666;
  box-shadow: 0 4px 0 #666;
  cursor: not-allowed;
}

@media (max-width: 480px) {
  .title {
    font-size: 1.2rem;
  }
  .icon-btn {
    width: 38px;
    height: 38px;
  }
  .icon-btn svg {
    width: 20px;
    height: 20px;
  }
  .xray-toggle {
    top: 70px;
    width: 42px;
    height: 42px;
  }
  .xray-toggle svg {
    width: 20px;
    height: 20px;
  }
  .ingredients-toggle {
    top: 122px;
    width: 42px;
    height: 42px;
  }
  .ingredients-toggle svg {
    width: 20px;
    height: 20px;
  }
  .note-toggle {
    top: 70px;
    width: 42px;
    height: 42px;
  }
  .note-toggle svg {
    width: 20px;
    height: 20px;
  }
  .help-toggle {
    top: 122px;
    width: 42px;
    height: 42px;
  }
  .help-toggle svg {
    width: 20px;
    height: 20px;
  }
  .cropper-container {
    height: 200px;
  }
}

@media (max-width: 600px) {
  .ingredients-sidebar {
    width: 100%;
    right: -100%;
  }
  .left-sidebar {
    width: 100%;
    left: -100%;
  }
  .clear-btn-icon {
    bottom: 90px;
    right: 20px;
  }
}
</style>