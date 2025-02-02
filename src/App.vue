<template>
  <!-- 
    ★ 메인 컨테이너: 플로럴 배경을 위해 
    bg-gradient + 배경 이미지(꽃무늬 패턴) + 불투명도 조절을 조합했습니다.
  -->
  <div
    class="relative min-h-screen flex items-center justify-center bg-gradient-to-b from-pink-100 to-pink-300 p-4"
    style="
      background-image: url('https://via.placeholder.com/800x800/FFC0CB/ffffff?text=Pink+Flower+Pattern');
      background-repeat: repeat;
      background-size: 300px 300px;
      opacity: 0.95;
    "
  >
    <!-- ★ 카드 영역 -->
    <div class="bg-white max-w-md w-full rounded-xl shadow-xl relative overflow-hidden">
      <!-- ★ 상단 이미지: 더 화사한 느낌을 위해 플로럴 이미지 -->
      <img
        class="w-full h-auto object-cover"
        src="https://via.placeholder.com/600x300/FFB6C1/000000?text=Pink+Flowers"
        alt="메인 이미지"
      />

      <!-- ★ 카드 내용 -->
      <div class="p-6 text-center">
        <h1 class="text-2xl font-bold text-pink-700 mb-2">소중한 날, 함께해주세요</h1>

        <p class="text-gray-700 mb-4">
          저희 두 사람이 시작하는 첫 걸음에, 귀한 걸음 하셔서 자리를 빛내 주세요.
        </p>

        <!-- 예식 정보 -->
        <div class="text-left bg-pink-50 p-4 rounded mb-4">
          <p class="mb-1">
            <span class="text-pink-600 font-semibold">일시:</span> {{ weddingDate }}
            {{ weddingTime }}
          </p>
          <p class="mb-1">
            <span class="text-pink-600 font-semibold">장소:</span> {{ weddingPlace }}
          </p>
          <p><span class="text-pink-600 font-semibold">혼주:</span> {{ hosts }}</p>
        </div>

        <!-- RSVP 버튼 -->
        <button
          @click="openRsvpModal"
          class="bg-pink-500 text-white px-4 py-2 rounded shadow hover:bg-pink-600 transition-colors"
        >
          참석 여부 전달하기
        </button>
      </div>
    </div>

    <!-- 
      ★ RSVP 모달(참석 확인 폼):
      - v-if로 표시 여부 제어
      - Tailwind + 트랜지션으로 팝업 느낌을 줍니다.
    -->
    <transition name="fade">
      <div
        v-if="showRsvpModal"
        class="fixed inset-0 flex items-center justify-center bg-black bg-opacity-50 z-50"
      >
        <div class="bg-white rounded-xl shadow-xl max-w-sm w-full p-6 relative">
          <!-- 닫기 버튼 -->
          <button
            @click="closeRsvpModal"
            class="absolute top-2 right-2 text-gray-400 hover:text-gray-600"
          >
            &times;
          </button>

          <h2 class="text-xl font-bold text-pink-700 mb-4 text-center">RSVP</h2>

          <!-- 이름 입력 -->
          <div class="mb-4">
            <label class="block mb-1 text-gray-700">이름</label>
            <input
              v-model="rsvpName"
              type="text"
              class="w-full border rounded px-3 py-2 focus:outline-none focus:ring-2 focus:ring-pink-300"
              placeholder="성함을 입력해주세요"
            />
          </div>

          <!-- 연락처 입력 -->
          <div class="mb-4">
            <label class="block mb-1 text-gray-700">연락처</label>
            <input
              v-model="rsvpPhone"
              type="tel"
              class="w-full border rounded px-3 py-2 focus:outline-none focus:ring-2 focus:ring-pink-300"
              placeholder="예) 010-1234-5678"
            />
          </div>

          <!-- 메모 -->
          <div class="mb-4">
            <label class="block mb-1 text-gray-700">메모</label>
            <textarea
              v-model="rsvpMessage"
              rows="3"
              class="w-full border rounded px-3 py-2 focus:outline-none focus:ring-2 focus:ring-pink-300"
              placeholder="축하 메시지나 식사 여부 등을 적어주세요."
            />
          </div>

          <!-- 제출 버튼 -->
          <button
            @click="submitRsvp"
            class="bg-pink-500 w-full text-white px-4 py-2 rounded shadow hover:bg-pink-600 transition-colors"
          >
            전송
          </button>
        </div>
      </div>
    </transition>
  </div>
</template>

<script setup lang="ts">
// ★ Composition API 기능
import { ref } from 'vue'

// ★ 예식 정보
const weddingDate = ref('2025년 5월 14일')
const weddingTime = ref('오후 2시')
const weddingPlace = ref('서울 ○○호텔 웨딩홀 3층')
const hosts = ref('신랑 김프론트 • 신부 이프론트')

// ★ RSVP 모달 제어
const showRsvpModal = ref(false)

// ★ 모달에서 입력받을 데이터
const rsvpName = ref('')
const rsvpPhone = ref('')
const rsvpMessage = ref('')

// ★ 모달 열기
function openRsvpModal() {
  showRsvpModal.value = true
}

// ★ 모달 닫기
function closeRsvpModal() {
  showRsvpModal.value = false
  // 폼 초기화
  rsvpName.value = ''
  rsvpPhone.value = ''
  rsvpMessage.value = ''
}

// ★ RSVP 제출
function submitRsvp() {
  if (!rsvpName.value || !rsvpPhone.value) {
    alert('이름과 연락처를 모두 입력해주세요.')
    return
  }

  // 실제로는 서버 연동 혹은 이메일 전송 로직이 들어갈 수 있습니다.
  alert(
    `이름: ${rsvpName.value}\n` +
      `연락처: ${rsvpPhone.value}\n` +
      `메모: ${rsvpMessage.value}\n\n` +
      '참석 정보가 전송되었습니다. 감사합니다!',
  )

  // 제출 후 모달 닫기 + 입력값 초기화
  closeRsvpModal()
}
</script>

<!-- 
  ★ 모달의 트랜지션을 위한 Tailwind 기본 Fade 예시:
  - .fade-enter-active, .fade-leave-active: transition 속성
  - .fade-enter, .fade-leave-to: 시작/끝 상태
-->
<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.2s ease;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>
