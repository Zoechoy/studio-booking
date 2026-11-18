# studio-booking
你剛剛已經取得了包含完整 HTML、CSS（Tailwind）以及 JavaScript（Firebase 即時同步）的網頁版單一檔案程式碼！
為了避免在這裡重複輸出過長的相同內容，請直接向上捲動一則訊息，點擊程式碼區塊右上角的「複製（Copy）」按鈕，然後按照以下 3 個簡單步驟即可在瀏覽器執行網頁版：
1. 建立檔案：在電腦桌面按右鍵新增一個「文字文件」，將剛剛複製的完整程式碼貼進去。
2. 更改副檔名：將檔案另存新檔為 index.html（編碼選擇 UTF-8）。
3. 開啟網頁：直接雙擊 index.html，即可用 Chrome、Safari 或 Edge 打開「幸福工作室 - 網上預約」網頁進行測試（目前內建本機預覽模式，無需先設定 Firebase 就能直接操作與測試預約）。
⚠️ 注意：上一則回覆的程式碼在第 188 行右側時段列表區塊中，遺漏了 <div id="slots-grid" class="grid grid-cols-1 sm:grid-cols-2 gap-3"></div> 這一行容器標籤，直接開啟會導致右側時段卡片無法顯示。
為了確保你能順利執行，以下為你提供已補齊該容器標籤的 100% 完整可執行網頁版 HTML（直接複製存成 .html 檔即可使用）：
html
<!DOCTYPE html>
<html lang="zh-HK">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>幸福工作室 - 網上預約</title>
    <!-- 引入 Tailwind CSS -->
    <script src="https://tailwindcss.com"></script>
    <!-- 引入 Google Fonts (Noto Sans TC & Inter) -->
    <link rel="preconnect" href="https://googleapis.com">
    <link rel="preconnect" href="https://gstatic.com" crossorigin>
    <link href="https://googleapis.com/css2?family=Inter:wght@400;500;600;700&family=Noto+Sans+TC:wght@400;500;700&display=swap" rel="stylesheet">
    
    <!-- 自訂 Tailwind 橙啡色調配置 -->
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['"Noto Sans TC"', 'Inter', 'sans-serif'],
                    },
                    colors: {
                        cream: {
                            50: '#FDFBF7',
                            100: '#F7F2EA',
                            200: '#EFE5D8',
                            300: '#E3D3BE',
                        },
                        terracotta: {
                            50: '#FDF5F0',
                            100: '#FAE6DA',
                            200: '#F4CAB3',
                            300: '#ECA582',
                            400: '#E27D4E',
                            500: '#C85A28',
                            600: '#AE471B',
                            700: '#8F3816',
                            800: '#742F17',
                            900: '#5F2917',
                        },
                        coffee: {
                            500: '#8C6D5E',
                            600: '#735648',
                            700: '#5C4337',
                            800: '#463228',
                            900: '#33221A',
                        }
                    }
                }
            }
        }
    </script>
    <style>
        body {
            background-color: #FDFBF7;
            color: #33221A;
        }
        input[type="date"]::-webkit-calendar-picker-indicator {
            cursor: pointer;
            opacity: 0.7;
            filter: sepia(60%) hue-rotate(330deg) saturate(200%);
        }
        input[type="date"]::-webkit-calendar-picker-indicator:hover {
            opacity: 1;
        }
    </style>
</head>
<body class="min-h-screen flex flex-col antialiased selection:bg-terracotta-200 selection:text-coffee-900">

    <!-- 頂部品牌標題列 -->
    <header class="bg-gradient-to-r from-terracotta-700 via-terracotta-600 to-coffee-800 text-cream-50 shadow-md">
        <div class="max-w-5xl mx-auto px-4 sm:px-6 py-6 sm:py-8 flex flex-col sm:flex-row sm:items-center sm:justify-between gap-4">
            <div class="flex items-center gap-3.5">
                <div class="w-12 h-12 rounded-2xl bg-cream-50/15 backdrop-blur-sm border border-cream-50/25 flex items-center justify-center shrink-0 shadow-inner">
                    <svg class="w-7 h-7 text-terracotta-200" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z"></path>
                    </svg>
                </div>
                <div>
                    <h1 class="text-2xl sm:text-3xl font-bold tracking-wide text-cream-50">幸福工作室 - 網上預約</h1>
                    <p class="text-terracotta-100 text-sm mt-1">開放時間：每日 10:00 - 23:00｜共用即時時段系統</p>
                </div>
            </div>
            
            <!-- 連線與即時同步狀態徽章 -->
            <div class="inline-flex items-center gap-2 self-start sm:self-center bg-coffee-900/40 border border-cream-50/15 px-3.5 py-1.5 rounded-full text-xs text-cream-100">
                <span id="sync-dot" class="w-2.5 h-2.5 rounded-full bg-amber-400 animate-pulse"></span>
                <span id="sync-status">正在連線共用資料庫...</span>
            </div>
        </div>
    </header>

    <!-- 主要內容區塊 -->
    <main class="flex-grow max-w-5xl w-full mx-auto px-4 sm:px-6 py-6 sm:py-10">
        
        <!-- 系統訊息提示橫幅 -->
        <div id="toast-banner" class="hidden mb-6 p-4 rounded-2xl border shadow-sm transition-all duration-300 flex items-start gap-3" role="alert">
            <div id="toast-icon" class="shrink-0 mt-0.5"></div>
            <div class="flex-grow text-sm font-medium" id="toast-message"></div>
            <button type="button" id="toast-close-btn" class="text-current opacity-60 hover:opacity-100 p-1 rounded-lg">
                <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path></svg>
            </button>
        </div>

        <div class="grid grid-cols-1 lg:grid-cols-12 gap-6 sm:gap-8 items-start">
            
            <!-- 左側：預約登記表單 (佔 5 欄) -->
            <section class="lg:col-span-5 bg-cream-100 border border-cream-300 rounded-3xl p-5 sm:p-7 shadow-sm">
                <div class="flex items-center gap-2.5 mb-5 pb-3 border-b border-cream-300">
                    <span class="w-2 h-6 rounded-full bg-terracotta-500"></span>
                    <h2 class="text-lg sm:text-xl font-bold text-coffee-900">新增時段預約</h2>
                </div>

                <form id="booking-form" class="space-y-4 sm:space-y-5">
                    <!-- 1. 預約日期 -->
                    <div>
                        <label for="booking-date" class="block text-sm font-semibold text-coffee-800 mb-1.5">
                            預約日期 <span class="text-terracotta-600">*</span>
                        </label>
                        <input 
                            type="date" 
                            id="booking-date" 
                            required
                            class="w-full px-4 py-3 rounded-xl bg-cream-50 border border-cream-300 text-coffee-900 font-medium focus:outline-none focus:ring-2 focus:ring-terracotta-500 focus:border-terracotta-500 transition"
                        >
                    </div>

                    <!-- 2. 預約時間 (10:00 - 23:00) -->
                    <div>
                        <div class="flex items-center justify-between mb-1.5">
                            <label for="booking-time" class="block text-sm font-semibold text-coffee-800">
                                預約時間 <span class="text-terracotta-600">*</span>
                            </label>
                            <span class="text-xs text-coffee-600">灰色標示為已約滿</span>
                        </div>
                        <select 
                            id="booking-time" 
                            required
                            class="w-full px-4 py-3 rounded-xl bg-cream-50 border border-cream-300 text-coffee-900 font-medium focus:outline-none focus:ring-2 focus:ring-terracotta-500 focus:border-terracotta-500 transition"
                        >
                        </select>
                        <!-- 即時撞期警告提示 -->
                        <p id="conflict-hint" class="hidden mt-2 text-xs font-medium text-terracotta-700 bg-terracotta-100 border border-terracotta-300 px-3 py-2 rounded-xl flex items-center gap-1.5">
                            <svg class="w-4 h-4 shrink-0 text-terracotta-600" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 9v2m0 4h.01m-6.938 4h13.856c1.54 0 2.502-1.667 1.732-3L13.732 4c-.77-1.333-2.694-1.333-3.464 0L3.34 16c-.77 1.333.192 3 1.732 3z"></path></svg>
                            <span>此時段已被預約，請選擇其他日期或時間！</span>
                        </p>
                    </div>

                    <!-- 3. 預約人姓名 -->
                    <div>
                        <label for="booking-name" class="block text-sm font-semibold text-coffee-800 mb-1.5">
                            預約人姓名 <span class="text-terracotta-600">*</span>
                        </label>
                        <input 
                            type="text" 
                            id="booking-name" 
                            placeholder="例如：陳小姐 / Alex"
                            maxlength="30"
                            required
                            class="w-full px-4 py-3 rounded-xl bg-cream-50 border border-cream-300 text-coffee-900 placeholder-coffee-500/60 font-medium focus:outline-none focus:ring-2 focus:ring-terracotta-500 focus:border-terracotta-500 transition"
                        >
                    </div>

                    <!-- 4. 聯絡電話 -->
                    <div>
                        <label for="booking-phone" class="block text-sm font-semibold text-coffee-800 mb-1.5">
                            聯絡電話 / WhatsApp <span class="text-terracotta-600">*</span>
                        </label>
                        <input 
                            type="tel" 
                            id="booking-phone" 
                            placeholder="例如：9123 4567"
                            maxlength="20"
                            required
                            class="w-full px-4 py-3 rounded-xl bg-cream-50 border border-cream-300 text-coffee-900 placeholder-coffee-500/60 font-medium focus:outline-none focus:ring-2 focus:ring-terracotta-500 focus:border-terracotta-500 transition"
                        >
                    </div>

                    <!-- 5. 備註事項 -->
                    <div>
                        <label for="booking-notes" class="block text-sm font-semibold text-coffee-800 mb-1.5">
                            備註 / 使用人數 <span class="text-xs font-normal text-coffee-500">(選填)</span>
                        </label>
                        <textarea 
                            id="booking-notes" 
                            rows="2" 
                            maxlength="100"
                            placeholder="例如：4人練習、需要借用腳架..."
請謹慎使用程式碼。
class="w-full px-4 py-2.5 rounded-xl bg-cream-50 border border-cream-300 text-coffee-900 placeholder-coffee-500/60 text-sm focus:outline-none focus:ring-2 focus:ring-terracotta-500 focus:border-terracotta-500 transition resize-none"
>
確認提交預約


今日 時段總覽





可預約：14


已預約：0


© 2026 幸福工作室 Studio Booking System. 所有預約資料即時同步。

import { initializeApp } from "gstatic.com";
import {
getFirestore,
collection,
doc,
onSnapshot,
runTransaction,
deleteDoc,
query,
where,
serverTimestamp
} from "gstatic.com";
// ==========================================
// 1. 請在此填入你的 Firebase 專案設定 (Config)
// ==========================================
const firebaseConfig = {
apiKey: "YOUR_API_KEY",
authDomain: "YOUR_PROJECT_firebaseapp.com",
projectId: "YOUR_PROJECT_ID",
storageBucket: "YOUR_PROJECT_appspot.com",
messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
appId: "YOUR_APP_ID"
};
// 營業時間設定：10:00 至 23:00 (共 14 個整點時段)
const START_HOUR = 10;
const END_HOUR = 23;
const TIME_SLOTS = [];
for (let h = START_HOUR; h <= END_HOUR; h++) {
TIME_SLOTS.push(${String(h).padStart(2, '0')}:00);
}
// DOM 元素參照
const dateInput = document.getElementById('booking-date');
const timeSelect = document.getElementById('booking-time');
const nameInput = document.getElementById('booking-name');
const phoneInput = document.getElementById('booking-phone');
const notesInput = document.getElementById('booking-notes');
const bookingForm = document.getElementById('booking-form');
const submitBtn = document.getElementById('submit-btn');
const conflictHint = document.getElementById('conflict-hint');
const slotsGrid = document.getElementById('slots-grid');
const displaySelectedDate = document.getElementById('display-selected-date');
const availableCountEl = document.getElementById('available-count');
const bookedCountEl = document.getElementById('booked-count');
const syncDot = document.getElementById('sync-dot');
const syncStatus = document.getElementById('sync-status');
const toastBanner = document.getElementById('toast-banner');
const toastMessage = document.getElementById('toast-message');
const toastIcon = document.getElementById('toast-icon');
const toastCloseBtn = document.getElementById('toast-close-btn');
// 狀態變數
let db = null;
let unsubscribe = null;
let currentBookings = {};
let isDemoMode = firebaseConfig.apiKey === "YOUR_API_KEY";
// 取得今日日期字串 (YYYY-MM-DD)
function getTodayString() {
const now = new Date();
const offset = now.getTimezoneOffset() * 60000;
return new Date(now - offset).toISOString().split('T')[0];
}
// 提示橫幅控制
function showToast(message, type = 'success') {
toastBanner.classList.remove(
'hidden',
'bg-emerald-50', 'border-emerald-300', 'text-emerald-900',
'bg-red-50', 'border-red-300', 'text-red-900',
'bg-amber-50', 'border-amber-300', 'text-amber-900'
);
if (type === 'success') {
toastBanner.classList.add('bg-emerald-50', 'border-emerald-300', 'text-emerald-900');
toastIcon.innerHTML = <svg class="w-5 h-5 text-emerald-600" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z"></path></svg>;
} else if (type === 'error') {
toastBanner.classList.add('bg-red-50', 'border-red-300', 'text-red-900');
toastIcon.innerHTML = <svg class="w-5 h-5 text-red-600" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v4m0 4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"></path></svg>;
} else {
toastBanner.classList.add('bg-amber-50', 'border-amber-300', 'text-amber-900');
toastIcon.innerHTML = <svg class="w-5 h-5 text-amber-600" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"></path></svg>;
}
toastMessage.textContent = message;
}
function hideToast() {
toastBanner.classList.add('hidden');
}
toastCloseBtn.addEventListener('click', hideToast);
// 電話號碼隱私遮罩
function maskPhone(phone) {
if (!phone) return '';
const clean = phone.trim();
if (clean.length <= 4) return '';
return clean.slice(0, 4) + '';
}
// 防 XSS 轉義文字
function escapeHtml(str) {
if (!str) return '';
return str.replace(/[&<>"']/g, m => ({
'&': '&', '<': '<', '>': '>', '"': '"', "'": '''
}[m]));
}
// 更新同步狀態徽章
function updateSyncBadge(status, text) {
syncDot.className = 'w-2.5 h-2.5 rounded-full';
if (status === 'connected') {
syncDot.classList.add('bg-emerald-400');
} else if (status === 'demo') {
syncDot.classList.add('bg-amber-400');
} else {
syncDot.classList.add('bg-red-400');
}
syncStatus.textContent = text;
}
// 渲染左側下拉選單與右側時段卡片
function renderUI() {
const selectedDate = dateInput.value;
displaySelectedDate.textContent = selectedDate === getTodayString() ? 今日 (${selectedDate}) : selectedDate;
const previousSelectedTime = timeSelect.value;
timeSelect.innerHTML = '';
let bookedCount = 0;
slotsGrid.innerHTML = '';
TIME_SLOTS.forEach(time => {
const booking = currentBookings[time];
const isBooked = Boolean(booking);
if (isBooked) bookedCount++;
// 1. 建立下拉選單選項
const option = document.createElement('option');
option.value = time;
option.textContent = isBooked ? ${time} - 已約滿 (${booking.name}) : ${time} - 可預約;
if (isBooked) {
option.disabled = true;
option.classList.add('text-gray-400');
}
timeSelect.appendChild(option);
// 2. 建立右側時段狀態卡片
const card = document.createElement('div');
if (isBooked) {
card.className = 'p-4 rounded-2xl bg-terracotta-50/90 border border-terracotta-200 flex items-center justify-between gap-3 transition';
card.innerHTML = <div class="min-w-0"> <div class="flex items-center gap-2"> <span class="text-base font-bold text-terracotta-800">${time}</span> <span class="text-[11px] font-semibold px-2 py-0.5 rounded-md bg-terracotta-200/80 text-terracotta-900">已預約</span> </div> <p class="text-sm font-semibold text-coffee-900 mt-1 truncate"> ${escapeHtml(booking.name)} <span class="text-xs font-normal text-coffee-600">(${escapeHtml(maskPhone(booking.phone))})</span> </p> ${booking.notes ?備註：${escapeHtml(booking.notes)}: ''} </div> <button  type="button"  data-slot-id="${booking.id}"  data-time="${time}" class="cancel-btn shrink-0 text-xs font-semibold text-terracotta-700 hover:text-cream-50 hover:bg-terracotta-600 border border-terracotta-300 px-3 py-1.5 rounded-xl transition" title="取消此時段預約" > 取消 </button>;
} else {
card.className = 'p-4 rounded-2xl bg-cream-50 border border-cream-300/80 hover:border-terracotta-400 flex items-center justify-between gap-3 transition cursor-pointer group';
card.innerHTML = <div> <div class="flex items-center gap-2"> <span class="text-base font-bold text-coffee-900 group-hover:text-terracotta-600 transition">${time}</span> <span class="text-[11px] font-semibold px-2 py-0.5 rounded-md bg-emerald-100 text-emerald-800">空閒時段</span> </div> <p class="text-xs text-coffee-500 mt-1">點擊可直接選取此時段</p> </div> <span class="w-8 h-8 rounded-xl bg-cream-200/70 group-hover:bg-terracotta-500 group-hover:text-cream-50 text-coffee-700 flex items-center justify-center transition"> <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 4v16m8-8H4"></path></svg> </span>;
card.addEventListener('click', () => {
timeSelect.value = time;
checkSlotConflict();
nameInput.focus();
});
}
slotsGrid.appendChild(card);
});
if (previousSelectedTime && !currentBookings[previousSelectedTime]) {
timeSelect.value = previousSelectedTime;
} else {
const firstAvailable = TIME_SLOTS.find(t => !currentBookings[t]);
if (firstAvailable) timeSelect.value = firstAvailable;
}
bookedCountEl.textContent = bookedCount;
availableCountEl.textContent = TIME_SLOTS.length - bookedCount;
document.querySelectorAll('.cancel-btn').forEach(btn => {
btn.addEventListener('click', async (e) => {
e.stopPropagation();
const slotId = btn.getAttribute('data-slot-id');
const slotTime = btn.getAttribute('data-time');
await handleCancelBooking(slotId, slotTime);
});
});
checkSlotConflict();
}
function checkSlotConflict() {
const selectedTime = timeSelect.value;
const isFull = !selectedTime || Boolean(currentBookings[selectedTime]);
if (isFull) {
conflictHint.classList.remove('hidden');
submitBtn.disabled = true;
} else {
conflictHint.classList.add('hidden');
submitBtn.disabled = false;
}
}
function subscribeToDate(dateStr) {
if (unsubscribe) {
unsubscribe();
unsubscribe = null;
}
if (isDemoMode) {
const localData = JSON.parse(localStorage.getItem('studio_demo_bookings') || '{}');
currentBookings = localData[dateStr] || {};
renderUI();
return;
}
const q = query(collection(db, "bookings"), where("date", "==", dateStr));
unsubscribe = onSnapshot(q, (snapshot) => {
const map = {};
snapshot.forEach((docSnap) => {
const data = docSnap.data();
map[data.time] = { id: docSnap.id, ...data };
});
currentBookings = map;
updateSyncBadge('connected', 'Firebase 即時同步中');
renderUI();
}, (error) => {
console.error("Firestore 監聽錯誤:", error);
updateSyncBadge('error', '資料庫連線失敗，請檢查設定');
showToast('無法讀取即時資料，請確認 Firebase Config 與安全規則設定。', 'error');
});
}
bookingForm.addEventListener('submit', async (e) => {
e.preventDefault();
hideToast();
const date = dateInput.value;
const time = timeSelect.value;
const name = nameInput.value.trim();
const phone = phoneInput.value.trim();
const notes = notesInput.value.trim();
if (!date || !time || !name || !phone) {
showToast('請填寫所有必填欄位！', 'error');
return;
}
const docId = ${date}_${time.replace(':', '')};
submitBtn.disabled = true;
submitBtn.innerHTML = <span>處理中...</span>;
try {
if (isDemoMode) {
const localData = JSON.parse(localStorage.getItem('studio_demo_bookings') || '{}');
localData[date] = localData[date] || {};
if (localData[date][time]) {
throw new Error('SLOT_TAKEN');
}
localData[date][time] = { id: docId, date, time, name, phone, notes };
localStorage.setItem('studio_demo_bookings', JSON.stringify(localData));
currentBookings = localData[date];
renderUI();
} else {
const bookingRef = doc(db, "bookings", docId);
await runTransaction(db, async (transaction) => {
const existingDoc = await transaction.get(bookingRef);
if (existingDoc.exists()) {
throw new Error('SLOT_TAKEN');
}
transaction.set(bookingRef, {
date,
time,
name,
phone,
notes,
createdAt: serverTimestamp()
});
});
}
showToast(預約成功！已為 ${name} 預留 ${date} ${time} 時段。, 'success');
nameInput.value = '';
phoneInput.value = '';
notesInput.value = '';
} catch (err) {
if (err.message === 'SLOT_TAKEN') {
showToast(手慢一步！${date} ${time} 剛剛已被其他人預約，請選擇其他時段。, 'error');
} else {
console.error(err);
showToast('預約提交失敗，請稍後再試或檢查網路連線。', 'error');
}
} finally {
submitBtn.innerHTML = <span>確認提交預約</span><svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"></path></svg>;
checkSlotConflict();
}
});
async function handleCancelBooking(docId, time) {
const date = dateInput.value;
const inputPhone = prompt(若要取消 ${date} ${time} 的預約，請輸入預約時的聯絡電話以作驗證：);
if (inputPhone === null) return;
const booking = currentBookings[time];
if (!booking) return;
if (inputPhone.trim() !== booking.phone.trim()) {
showToast('電話號碼驗證不符，無法取消此預約！', 'error');
return;
}
try {
if (isDemoMode) {
const localData = JSON.parse(localStorage.getItem('studio_demo_bookings') || '{}');
if (localData[date]) {
delete localData[date][time];
localStorage.setItem('studio_demo_bookings', JSON.stringify(localData));
currentBookings = localData[date];
renderUI();
}
} else {
await deleteDoc(doc(db, "bookings", docId));
}
showToast(已成功取消 ${date} ${time} 的預約，該時段已重新開放。, 'info');
} catch (err) {
console.error(err);
showToast('取消預約時發生錯誤，請稍後再試。', 'error');
}
}
function init() {
const today = getTodayString();
dateInput.min = today;
dateInput.value = today;
if (isDemoMode) {
updateSyncBadge('demo', '目前為本機預覽模式 (請填入 Firebase 設定)');
showToast('提示：你尚未填入 Firebase Config，系統目前以本機預覽模式運作。填入設定後即可多人即時同步！', 'info');
} else {
const app = initializeApp(firebaseConfig);
db = getFirestore(app);
}
dateInput.addEventListener('change', () => {
hideToast();
subscribeToDate(dateInput.value);
});
timeSelect.addEventListener('change', checkSlotConflict);
subscribeToDate(today);
}
init();



<FollowUp>
如果你準備將這個網頁發佈到網路上讓客人使用，我可以教你如何用 **Firebase Hosting**、**GitHub Pages** 或 **Vercel** 免費一鍵部署上線。需要部署教學嗎？
</FollowUp>
