<template>
  <div class="w-full h-screen flex flex-col overflow-hidden" id="demo_wrap">
    <div
      class="flex flex-row justify-center w-full m-auto relative z-10"
      style="height: 90%;"
    >
      <div class="flex flex-col w-full md:w-5/12 rounded-lg">
        <div class="m-auto w-4/5 mt-4 mb-0">
          <div
            v-for="(audio, indexo) in audios.slice(index, index + 1)"
            :key="indexo"
            class="mb-4"
          >
            <h3 class="text-xl text-white font-semibold">
              {{ audio.name }}
            </h3>
            <p class="text-sm text-grey mt-1">{{ audio.artist }}</p>
          </div>
          <div class="m-auto relative" style="width: 300px; height: 300px;">
            <img
              class="w-full rounded-full block m-auto h-full"
              src="../assets/quran.jpg"
              alt="Album Pic"
            />
            <div class="cd-center"></div>
          </div>
        </div>
        <div class="flex w-4/5 m-auto justify-between items-center mt-8 mb-4">
          <div class="text-grey-darker hover:bg-gray-300 rounded-full p-1">
            <svg
              @click="random = !random"
              :class="random ? 'text-red-500' : ''"
              class="w-8 h-8 cursor-pointer"
              fill="currentColor"
              xmlns="http://www.w3.org/2000/svg"
              viewBox="0 0 20 20"
            >
              <path
                d="M6.59 12.83L4.4 15c-.58.58-1.59 1-2.4 1H0v-2h2c.29 0 .8-.2 1-.41l2.17-2.18 1.42 1.42zM16 4V1l4 4-4 4V6h-2c-.29 0-.8.2-1 .41l-2.17 2.18L9.4 7.17 11.6 5c.58-.58 1.59-1 2.41-1h2zm0 10v-3l4 4-4 4v-3h-2c-.82 0-1.83-.42-2.41-1l-8.6-8.59C2.8 6.21 2.3 6 2 6H0V4h2c.82 0 1.83.42 2.41 1l8.6 8.59c.2.2.7.41.99.41h2z"
              />
            </svg>
          </div>
          <div class="text-grey-darker hover:bg-gray-300 rounded-full p-1">
            <svg
              @click="prevButton ? previous() : ''"
              class="w-8 h-8 cursor-pointer"
              fill="currentColor"
              xmlns="http://www.w3.org/2000/svg"
              viewBox="0 0 20 20"
            >
              <path d="M4 5h3v10H4V5zm12 0v10l-9-5 9-5z" />
            </svg>
          </div>
          <div
            class="text-white p-4 rounded-full bg-gradient-to-r from-red-500 via-red-600 to-red-700 shadow-lg"
          >
            <svg
              v-if="!pauseTrack"
              @click="play()"
              class="w-8 h-8 cursor-pointer"
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M14.752 11.168l-3.197-2.132A1 1 0 0010 9.87v4.263a1 1 0 001.555.832l3.197-2.132a1 1 0 000-1.664z"
              />
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M21 12a9 9 0 11-18 0 9 9 0 0118 0z"
              />
            </svg>
            <svg
              v-else
              @click="pause()"
              class="w-8 h-8 cursor-pointer"
              fill="currentColor"
              xmlns="http://www.w3.org/2000/svg"
              viewBox="0 0 20 20"
            >
              <path d="M5 4h3v12H5V4zm7 0h3v12h-3V4z" />
            </svg>
          </div>
          <div class="text-grey-darker hover:bg-gray-300 rounded-full p-1">
            <svg
              @click="nextButton ? next() : ''"
              class="w-8 h-8 cursor-pointer"
              fill="currentColor"
              xmlns="http://www.w3.org/2000/svg"
              viewBox="0 0 20 20"
            >
              <path d="M13 5h3v10h-3V5zM4 5l9 5-9 5V5z" />
            </svg>
          </div>
          <div class="text-grey-darker hover:bg-gray-300 rounded-full p-1">
            <svg
              @click="repeat = !repeat"
              :class="repeat ? 'text-red-500' : ''"
              class="w-8 h-8 cursor-pointer"
              fill="currentColor"
              xmlns="http://www.w3.org/2000/svg"
              viewBox="0 0 20 20"
            >
              <path
                d="M5 4a2 2 0 0 0-2 2v6H0l4 4 4-4H5V6h7l2-2H5zm10 4h-3l4-4 4 4h-3v6a2 2 0 0 1-2 2H6l2-2h7V8z"
              />
            </svg>
          </div>
        </div>
      </div>
      <div class="w-7/12 hidden md:block">
        <ul
          class="w-full overflow-auto m-auto mb-2 pt-2"
          style="max-height: 100%; background: rgba(0, 0, 0, 0.5);"
          id="journal-scroll"
        >
          <li
            @click="selectSound(indexo)"
            :style="indexo == index ? '' : ''"
            :class="
              indexo == index
                ? 'bg-gradient-to-r from-red-500 via-red-600 to-red-700 text-gray'
                : ''
            "
            class="flex py-1 rounded cursor-pointer w-11/12 m-auto"
            v-for="(audio, indexo) in audios"
            :key="indexo"
          >
            <div class="w-1/5 font-semibold m-auto text-white">
              {{ indexo + 1 }}
            </div>
            <div class="w-3/5 font-semibold text-left m-auto">
              <div class="font-semibold text-sm">
                <p class="text-white">{{ audio.name }}</p>
                <p class="text-xs text-white">{{ audio.artist }}</p>
              </div>
            </div>
            <div class="w-1/5 m-auto">
              <svg
                v-if="state.audioPlaying[indexo]"
                class="w-6 h-6 m-auto"
                fill="currentColor"
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 20 20"
              >
                <path d="M5 4h3v12H5V4zm7 0h3v12h-3V4z" />
              </svg>
              <svg
                v-else
                class="w-6 h-6 m-auto"
                xmlns="http://www.w3.org/2000/svg"
                fill="none"
                viewBox="0 0 24 24"
                stroke="currentColor"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="2"
                  d="M14.752 11.168l-3.197-2.132A1 1 0 0010 9.87v4.263a1 1 0 001.555.832l3.197-2.132a1 1 0 000-1.664z"
                />
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="2"
                  d="M21 12a9 9 0 11-18 0 9 9 0 0118 0z"
                />
              </svg>
            </div>
          </li>
        </ul>
      </div>
    </div>
    <div
      class="w-full bg-gradient-to-r from-gray-300 to-gray-400 relative z-10"
      style="height: 10%;"
    >
      <div class="flex w-11/12 h-16 items-center justify-around m-auto">
        <div
          class="w-2/12 md:flex items-center hidden"
          v-for="(audio, indexo) in audios.slice(index, index + 1)"
          :key="indexo"
        >
          <img
            class="w-10 h-10 rounded-full"
            src="https://tailwindcss.com/img/card-top.jpg"
          />
          <div class="flex flex-col ml-2 font-semibold">
            <p>{{ audio.name }}</p>
            <p class="text-xs text-gray-600">{{ audio.artist }}</p>
          </div>
        </div>
        <div class="w-4/5 flex md:w-8/12 items-center">
          <div class="text-sm text-grey-darker w-2/12 md:w-1/12 font-semibold">
            <p>{{ timer }}</p>
          </div>
          <div class="mt-1 relative w-8/12 md:w-10/12">
            <div
              @click="seek($event)"
              ref="progress"
              class="h-1 bg-grey-dark cursor-pointer rounded-full bg-gray-500"
            >
              <div
                class="flex w-full justify-end h-1 bg-gradient-to-r from-red-500 to-red-700 rounded-full relative"
                :style="{ width: step + '%' }"
              ></div>
            </div>
            <div
              class="flex w-full justify-end h-1 rounded-full relative"
              :style="{ width: step + '%' }"
            >
              <span
                id="progressButtonTimer"
                class="w-3 h-3 md:w-4 md:h-4 bg-gradient-to-r from-red-500 to-red-700 absolute pin-r pin-b -mb-1 rounded-full shadow"
              ></span>
            </div>
          </div>
          <div class="text-sm text-grey-darker w-2/12 md:w-1/12 font-semibold">
            <p>{{ duration }}</p>
          </div>
        </div>

        <div class="w-1/5 flex md:w-2/12 m-auto items-center">
          <div
            class="w-3/12 md:w-2/12 hover:bg-gray-500 rounded-full md:p-1"
            @click="mute()"
          >
            <svg
              v-if="mutePlayer"
              class="w-6 h-6 m-auto text-red-500 cursor-pointer"
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M5.586 15H4a1 1 0 01-1-1v-4a1 1 0 011-1h1.586l4.707-4.707C10.923 3.663 12 4.109 12 5v14c0 .891-1.077 1.337-1.707.707L5.586 15z"
                clip-rule="evenodd"
              />
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M17 14l2-2m0 0l2-2m-2 2l-2-2m2 2l2 2"
              />
            </svg>
            <svg
              v-else
              class="w-6 h-6 m-auto cursor-pointer"
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M15.536 8.464a5 5 0 010 7.072m2.828-9.9a9 9 0 010 12.728M5.586 15H4a1 1 0 01-1-1v-4a1 1 0 011-1h1.586l4.707-4.707C10.923 3.663 12 4.109 12 5v14c0 .891-1.077 1.337-1.707.707L5.586 15z"
              />
            </svg>
          </div>
          <div class="w-9/12 md:w-10/12 m-auto relative">
            <div
              @click="volume($event)"
              ref="volBar"
              class="h-1 bg-grey-dark cursor-pointer rounded-full bg-gray-500 m-auto relative"
              style="width: 100%;"
            >
              <div
                class="flex justify-end h-1 bg-gradient-to-r from-teal-400 to-blue-500 rounded-full relative"
                :style="{ width: volumeProgress + '%' }"
              ></div>
            </div>
            <div
              class="flex justify-end h-1 rounded-full relative"
              :style="{ width: volumeProgress + '%' }"
            >
              <span
                id="progressButtonVolume"
                class="w-3 h-3 md:w-4 md:h-4 bg-gradient-to-r from-teal-400 to-blue-500 absolute pin-r pin-b -mb-1 rounded-full shadow"
              ></span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, reactive, computed } from 'vue'
import A1 from '../assets/quran/1.mp3'
import A2 from '../assets/quran/2.mp3'
import A3 from '../assets/quran/3.mp3'
import A4 from '../assets/quran/4.mp3'
import A5 from '../assets/quran/5.mp3'
import A6 from '../assets/quran/6.mp3'
import A7 from '../assets/quran/7.mp3'
import A8 from '../assets/quran/8.mp3'
import A9 from '../assets/quran/9.mp3'
import A10 from '../assets/quran/10.mp3'
import A11 from '../assets/quran/11.mp3'
import A12 from '../assets/quran/12.mp3'
import A13 from '../assets/quran/13.mp3'
import A14 from '../assets/quran/14.mp3'
import A15 from '../assets/quran/15.mp3'
import A16 from '../assets/quran/16.mp3'
import A17 from '../assets/quran/17.mp3'
import A18 from '../assets/quran/18.mp3'
import A19 from '../assets/quran/19.mp3'
import A20 from '../assets/quran/20.mp3'
import A21 from '../assets/quran/21.mp3'
import A22 from '../assets/quran/22.mp3'
import A23 from '../assets/quran/23.mp3'
import A24 from '../assets/quran/24.mp3'
import A25 from '../assets/quran/25.mp3'
import A26 from '../assets/quran/26.mp3'
import A27 from '../assets/quran/27.mp3'
import A28 from '../assets/quran/28.mp3'
import A29 from '../assets/quran/29.mp3'
import A30 from '../assets/quran/30.mp3'
import A31 from '../assets/quran/31.mp3'
import A32 from '../assets/quran/32.mp3'
import A33 from '../assets/quran/33.mp3'
import A34 from '../assets/quran/34.mp3'
import A35 from '../assets/quran/35.mp3'
import A36 from '../assets/quran/36.mp3'
import A37 from '../assets/quran/37.mp3'
import A38 from '../assets/quran/38.mp3'
import A39 from '../assets/quran/39.mp3'
import A40 from '../assets/quran/40.mp3'
import A41 from '../assets/quran/41.mp3'
import A42 from '../assets/quran/42.mp3'
import A43 from '../assets/quran/43.mp3'
import A44 from '../assets/quran/44.mp3'
import A45 from '../assets/quran/45.mp3'
import A46 from '../assets/quran/46.mp3'
import A47 from '../assets/quran/47.mp3'
import A48 from '../assets/quran/48.mp3'
import A49 from '../assets/quran/49.mp3'
import A50 from '../assets/quran/50.mp3'
import A51 from '../assets/quran/51.mp3'
import A52 from '../assets/quran/52.mp3'
import A53 from '../assets/quran/53.mp3'
import A54 from '../assets/quran/54.mp3'
import A55 from '../assets/quran/55.mp3'
import A56 from '../assets/quran/56.mp3'
import A57 from '../assets/quran/57.mp3'
import A58 from '../assets/quran/58.mp3'
import A59 from '../assets/quran/59.mp3'
import A60 from '../assets/quran/60.mp3'
import A61 from '../assets/quran/61.mp3'
import A62 from '../assets/quran/62.mp3'
import A63 from '../assets/quran/63.mp3'
import A64 from '../assets/quran/64.mp3'
import A65 from '../assets/quran/65.mp3'
import A66 from '../assets/quran/66.mp3'
import A67 from '../assets/quran/67.mp3'
import A68 from '../assets/quran/68.mp3'
import A69 from '../assets/quran/69.mp3'
import A70 from '../assets/quran/70.mp3'
import A71 from '../assets/quran/71.mp3'
import A72 from '../assets/quran/72.mp3'
import A73 from '../assets/quran/73.mp3'
import A74 from '../assets/quran/74.mp3'
import A75 from '../assets/quran/75.mp3'
import A76 from '../assets/quran/76.mp3'
import A77 from '../assets/quran/77.mp3'
import A78 from '../assets/quran/78.mp3'
import A79 from '../assets/quran/79.mp3'
import A80 from '../assets/quran/80.mp3'
import A81 from '../assets/quran/81.mp3'
import A82 from '../assets/quran/82.mp3'
import A83 from '../assets/quran/83.mp3'
import A84 from '../assets/quran/84.mp3'
import A85 from '../assets/quran/85.mp3'
import A86 from '../assets/quran/86.mp3'
import A87 from '../assets/quran/87.mp3'
import A88 from '../assets/quran/88.mp3'
import A89 from '../assets/quran/89.mp3'
import A90 from '../assets/quran/90.mp3'
import A91 from '../assets/quran/91.mp3'
import A92 from '../assets/quran/92.mp3'
import A93 from '../assets/quran/93.mp3'
import A94 from '../assets/quran/94.mp3'
import A95 from '../assets/quran/95.mp3'
import A96 from '../assets/quran/96.mp3'
import A97 from '../assets/quran/97.mp3'
import A98 from '../assets/quran/98.mp3'
import A99 from '../assets/quran/99.mp3'
import A100 from '../assets/quran/100.mp3'
import A101 from '../assets/quran/101.mp3'
import A102 from '../assets/quran/102.mp3'
import A103 from '../assets/quran/103.mp3'
import A104 from '../assets/quran/104.mp3'
import A105 from '../assets/quran/105.mp3'
import A106 from '../assets/quran/106.mp3'
import A107 from '../assets/quran/107.mp3'
import A108 from '../assets/quran/108.mp3'
import A109 from '../assets/quran/109.mp3'
import A110 from '../assets/quran/110.mp3'
import A111 from '../assets/quran/111.mp3'
import A112 from '../assets/quran/112.mp3'
import A113 from '../assets/quran/113.mp3'
import A114 from '../assets/quran/114.mp3'
import axios from 'axios'
import { Howl, Howler } from 'howler'
export default {
  mounted() {
    let sound = this.audios[this.index].howl
    this.fetchSurah()
    let barWidth = (0.9 * 100) / 100
    this.sliderBtnVol =
      this.volBar.offsetWidth * barWidth + this.volBar.offsetWidth * 0.05 - 25
  },
  data() {
    return {
      surras: [
        {
          surah: '../assets/quran/001_1306160345.mp3',
        },
      ],
    }
  },

  setup() {
    const audios = ref([
      { name: 'Al-Fatihah', file: A1, artist: 'Alafasy', howl: null },
      { name: 'Al-Baqarah', file: A2, artist: 'Alafasy', howl: null },
      { name: 'Al-Imran', file: A3, artist: 'Alafasy', howl: null },
      { name: 'Al-Nisa', file: A4, artist: 'Alafasy', howl: null },
      { name: 'Al-Maidah', file: A5, artist: 'Alafasy', howl: null },
      { name: 'Al-An`am', file: A6, artist: 'Alafasy', howl: null },
      { name: 'Al-A`rof', file: A7, artist: 'Alafasy', howl: null },
      { name: 'Al-Anfal', file: A8, artist: 'Alafasy', howl: null },
      { name: 'At-Taubah', file: A9, artist: 'Alafasy', howl: null },
      { name: 'Yunus', file: A10, artist: 'Alafasy', howl: null },
      { name: 'Hood', file: A11, artist: 'Alafasy', howl: null },
      { name: 'Yusuf', file: A12, artist: 'Alafasy', howl: null },
      { name: 'Ar-Ra`d', file: A13, artist: 'Alafasy', howl: null },
      { name: 'Ibrahim', file: A14, artist: 'Alafasy', howl: null },
      { name: 'Al-Hijr', file: A15, artist: 'Alafasy', howl: null },
      { name: 'An-Nahl', file: A16, artist: 'Alafasy', howl: null },
      { name: 'Al-Isra', file: A17, artist: 'Alafasy', howl: null },
      { name: 'Al-Kahf', file: A18, artist: 'Alafasy', howl: null },
      { name: 'Maryam', file: A19, artist: 'Alafasy', howl: null },
      { name: 'Taha', file: A20, artist: 'Alafasy', howl: null },
      { name: 'Al-Anbiya', file: A21, artist: 'Alafasy', howl: null },
      { name: 'Al-Haj', file: A22, artist: 'Alafasy', howl: null },
      { name: ' Al-Mu’minun', file: A23, artist: 'Alafasy', howl: null },
      { name: 'An-Nur', file: A24, artist: 'Alafasy', howl: null },
      { name: 'Al-Furqan', file: A25, artist: 'Alafasy', howl: null },
      { name: ' Ash-Shu’ara', file: A26, artist: 'Alafasy', howl: null },
      { name: 'An-Naml', file: A27, artist: 'Alafasy', howl: null },
      { name: 'Al-Qasas', file: A28, artist: 'Alafasy', howl: null },
      { name: 'Al-Ankabut', file: A29, artist: 'Alafasy', howl: null },
      { name: 'Ar-Rum', file: A30, artist: 'Alafasy', howl: null },
      { name: 'Luqman  ', file: A31, artist: 'Alafasy', howl: null },
      { name: ' As-Sajdah', file: A32, artist: 'Alafasy', howl: null },
      { name: 'Al-Ahzab', file: A33, artist: 'Alafasy', howl: null },
      { name: 'Saba', file: A34, artist: 'Alafasy', howl: null },
      { name: 'Al-Fatir', file: A35, artist: 'Alafasy', howl: null },
      { name: 'Ya-Sin', file: A36, artist: 'Alafasy', howl: null },
      { name: 'As-Saffah', file: A37, artist: 'Alafasy', howl: null },
      { name: 'Sad ', file: A38, artist: 'Alafasy', howl: null },
      { name: 'Az-Zumar  ', file: A39, artist: 'Alafasy', howl: null },
      { name: 'Ghafar  ', file: A40, artist: 'Alafasy', howl: null },
      { name: 'Fusilat ', file: A41, artist: 'Alafasy', howl: null },
      { name: 'Ash-Shura', file: A42, artist: 'Alafasy', howl: null },
      { name: 'Az-Zukhruf', file: A43, artist: 'Alafasy', howl: null },
      { name: 'Ad-Dukhan', file: A44, artist: 'Alafasy', howl: null },
      { name: 'Al-Jathiyah', file: A45, artist: 'Alafasy', howl: null },
      { name: ' Al-Ahqaf ', file: A46, artist: 'Alafasy', howl: null },
      { name: 'Muhammad ', file: A47, artist: 'Alafasy', howl: null },
      { name: 'Al-Fat’h', file: A48, artist: 'Alafasy', howl: null },
      { name: 'Al-Hujurat ', file: A49, artist: 'Alafasy', howl: null },
      { name: 'Qaf ', file: A50, artist: 'Alafasy', howl: null },
      { name: 'Adz-Dzariyah', file: A51, artist: 'Alafasy', howl: null },
      { name: 'At-Tur', file: A52, artist: 'Alafasy', howl: null },
      { name: 'An-Najm', file: A53, artist: 'Alafasy', howl: null },
      { name: 'Al-Qamar', file: A54, artist: 'Alafasy', howl: null },
      { name: ' Ar-Rahman ', file: A55, artist: 'Alafasy', howl: null },
      { name: 'Al-Waqi’ah', file: A56, artist: 'Alafasy', howl: null },
      { name: 'Al-Hadid', file: A57, artist: 'Alafasy', howl: null },
      { name: 'Al-Mujadilah', file: A58, artist: 'Alafasy', howl: null },
      { name: ' Al-Hashr', file: A59, artist: 'Alafasy', howl: null },
      { name: 'Al-Mumtahanah', file: A60, artist: 'Alafasy', howl: null },
      { name: 'As-Saf ', file: A61, artist: 'Alafasy', howl: null },
      { name: 'Al-Jum’ah', file: A62, artist: 'Alafasy', howl: null },
      { name: 'Al-Munafiqun', file: A63, artist: 'Alafasy', howl: null },
      { name: 'At-Taghabun', file: A64, artist: 'Alafasy', howl: null },
      { name: 'At-Talaq', file: A65, artist: 'Alafasy', howl: null },
      { name: 'At-Tahrim', file: A66, artist: 'Alafasy', howl: null },
      { name: 'Al-Mulk ', file: A67, artist: 'Alafasy', howl: null },
      { name: 'Al-Qalam', file: A68, artist: 'Alafasy', howl: null },
      { name: ' Al-Haqqah', file: A69, artist: 'Alafasy', howl: null },
      { name: 'Al-Ma’arij ', file: A70, artist: 'Alafasy', howl: null },
      { name: 'Nuh ', file: A71, artist: 'Alafasy', howl: null },
      { name: 'Al-Jinn ', file: A72, artist: 'Alafasy', howl: null },
      { name: ' Al-Muzammil', file: A73, artist: 'Alafasy', howl: null },
      { name: 'Al-Mudaththir', file: A74, artist: 'Alafasy', howl: null },
      { name: 'Al-Qiyamah', file: A75, artist: 'Alafasy', howl: null },
      { name: 'Al-Insan', file: A76, artist: 'Alafasy', howl: null },
      { name: 'Al-Mursalat', file: A77, artist: 'Alafasy', howl: null },
      { name: 'An-Naba', file: A78, artist: 'Alafasy', howl: null },
      { name: ' An-Nazi’at', file: A79, artist: 'Alafasy', howl: null },
      { name: 'Abasa ', file: A80, artist: 'Alafasy', howl: null },
      { name: 'At-Takwir', file: A81, artist: 'Alafasy', howl: null },
      { name: ' Al-Infitar ', file: A82, artist: 'Alafasy', howl: null },
      { name: 'Al-Mutaffifin', file: A83, artist: 'Alafasy', howl: null },
      { name: 'Al-Inshiqaq ', file: A84, artist: 'Alafasy', howl: null },
      { name: ' Al-Buruj', file: A85, artist: 'Alafasy', howl: null },
      { name: 'At-Tariq', file: A86, artist: 'Alafasy', howl: null },
      { name: ' Al-A’la', file: A87, artist: 'Alafasy', howl: null },
      { name: 'Al-Ghashiyah', file: A88, artist: 'Alafasy', howl: null },
      { name: ' Al-Fajr', file: A89, artist: 'Alafasy', howl: null },
      { name: 'Al-Balad', file: A90, artist: 'Alafasy', howl: null },
      { name: 'Ash-Shams', file: A91, artist: 'Alafasy', howl: null },
      { name: 'Al-Layl ', file: A92, artist: 'Alafasy', howl: null },
      { name: 'Adh-Dhuha', file: A93, artist: 'Alafasy', howl: null },
      { name: 'Al-Inshirah', file: A94, artist: 'Alafasy', howl: null },
      { name: 'At-Tin', file: A95, artist: 'Alafasy', howl: null },
      { name: 'Al-‘Alaq', file: A96, artist: 'Alafasy', howl: null },
      { name: 'Al-Qadar', file: A97, artist: 'Alafasy', howl: null },
      { name: 'Al-Bayinah', file: A98, artist: 'Alafasy', howl: null },
      { name: 'Az-Zalzalah', file: A99, artist: 'Alafasy', howl: null },
      { name: 'Al-‘Adiyah', file: A100, artist: 'Alafasy', howl: null },
      { name: ' Al-Qari’ah', file: A101, artist: 'Alafasy', howl: null },
      { name: 'At-Takathur', file: A102, artist: 'Alafasy', howl: null },
      { name: 'Al-‘Asr', file: A103, artist: 'Alafasy', howl: null },
      { name: 'Al-Humazah', file: A104, artist: 'Alafasy', howl: null },
      { name: 'Al-Fil ', file: A105, artist: 'Alafasy', howl: null },
      { name: 'Quraish ', file: A106, artist: 'Alafasy', howl: null },
      { name: 'Al-Ma’un', file: A107, artist: 'Alafasy', howl: null },
      { name: 'Al-Kauthar', file: A108, artist: 'Alafasy', howl: null },
      { name: 'Al-Kafirun', file: A109, artist: 'Alafasy', howl: null },
      { name: 'An-Nasr', file: A110, artist: 'Alafasy', howl: null },
      { name: 'Al-Masad', file: A111, artist: 'Alafasy', howl: null },
      { name: 'Al-Ikhlas', file: A112, artist: 'Alafasy', howl: null },
      { name: 'Al-Falaq', file: A113, artist: 'Alafasy', howl: null },
      { name: ' An-Nas', file: A114, artist: 'Alafasy', howl: null },
    ])
    const step = ref(0)
    const nextButton = ref(true)
    const prevButton = ref(true)
    const random = ref(false)
    const repeat = ref(false)
    const index = ref(0)
    const duration = ref('00:00')
    const timer = ref('00:00')
    const pauseTrack = ref(false)
    const progress = ref(null)
    const volBar = ref(null)
    const sliderBtn = ref(0)
    const sliderBtnVol = ref(null)
    const volumeProgress = ref(90)
    const mutePlayer = ref(false)
    const state = reactive({
      audioPlaying: [],
    })
    function formatTime(secs) {
      let minutes = Math.floor(secs / 60) || 0
      let seconds = Math.floor(secs - minutes * 60) || 0

      return (
        (minutes < 10 ? '0' : '') +
        minutes +
        ':' +
        (seconds < 10 ? '0' : '') +
        seconds
      )
    }
    function play() {
      let sound

      let audio = audios.value[index.value]
      if (audio.howl) {
        sound = audio.howl
      } else {
        state.audioPlaying[index.value] = false
        sound = audio.howl = new Howl({
          src: [audio.file],
          html5: true, // A live stream can only be played through HTML5 Audio.
          format: ['mp3', 'aac'],
          onplay: function () {
            pauseTrack.value = true
            nextButton.value = true
            prevButton.value = true
            duration.value = formatTime(sound.duration())
            requestAnimationFrame(stepFunction.bind(this))
          },
          onpause: function () {
            pauseTrack.value = false
          },
          onend: function () {
            next()
          },
          onseek: function () {
            window.requestAnimationFrame(stepFunction.bind(this))
          },
        })
      }

      sound.play()

      state.audioPlaying[index.value] = true
    }
    function pause(indexo) {
      let audio = audios.value[index.value].howl

      if (audio) {
        audio.pause()
        pauseTrack.value = false
        state.audioPlaying[index.value] = false
      }
    }

    function stepFunction() {
      let sound = audios.value[index.value].howl
      let seek = sound.seek()
      timer.value = formatTime(Math.round(seek))
      step.value = (seek * 100) / sound.duration()

      sliderBtn.value =
        progress.value.offsetWidth * (step.value / 100) +
        progress.value.offsetWidth * 0.05 -
        25

      if (sound.playing()) {
        window.requestAnimationFrame(stepFunction.bind(this))
      }
    }

    function seek(event) {
      let per = event.offsetX / progress.value.clientWidth

      let sound = audios.value[index.value].howl

      if (sound) {
        if (sound.playing()) {
          sound.pause()
          sound.seek(sound.duration() * per)
          let barWidth = (per * 100) / 100
          sliderBtn.value =
            progress.value.offsetWidth * barWidth +
            progress.value.offsetWidth * 0.05 -
            25
          sound.play()
        } else {
          sound.seek(sound.duration() * per)
          let barWidth = (per * 100) / 100
          sliderBtn.value =
            progress.value.offsetWidth * barWidth +
            progress.value.offsetWidth * 0.05 -
            25
        }
      }
    }

    function next() {
      nextButton.value = false
      let audio = audios.value[index.value].howl

      state.audioPlaying[index.value] = false

      mutePlayer.value ? (mutePlayer.value = false) : ''
      audio && audio.mute(true) ? audio.mute(false) : ''

      if (audio && audios.value.length - 1 == index.value) {
        audio.stop()

        repeat.value
          ? (index.value = index.value)
          : random.value
          ? (index.value = Math.floor(Math.random() * audios.value.length))
          : (index.value = 0)
      } else {
        if (audio) {
          audio.stop()
        }

        repeat.value
          ? (index.value = index.value)
          : random.value
          ? (index.value = Math.floor(Math.random() * audios.value.length))
          : index.value++
      }

      play()
    }

    function previous() {
      let audio = audios.value[index.value].howl
      prevButton.value = false
      state.audioPlaying[index.value] = false

      mutePlayer.value ? (mutePlayer.value = false) : ''
      audio && audio.mute(true) ? audio.mute(false) : ''

      if (!audio) {
        index.value = audios.value.length - 1
      } else if (audio && index.value == 0) {
        audio.stop()

        repeat.value
          ? (index.value = index.value)
          : random.value
          ? (index.value = Math.floor(Math.random() * audios.value.length))
          : (index.value = audios.value.length - 1)
      } else if (audio) {
        audio.stop()

        repeat.value
          ? (index.value = index.value)
          : random.value
          ? (index.value = Math.floor(Math.random() * audios.value.length))
          : index.value--
      }

      play()
    }
    function selectSound(indexSelected) {
      let audio = audios.value[index.value].howl

      if (audio) {
        audio.stop()
        state.audioPlaying[index.value] = false
      }

      index.value = indexSelected

      play()
    }

    function volume(event) {
      let per = event.layerX / parseFloat(volBar.value.scrollWidth)
      let barWidth = (per * 100) / 100
      volumeProgress.value = barWidth * 100
      sliderBtnVol.value =
        volBar.value.offsetWidth * barWidth +
        volBar.value.offsetWidth * 0.05 -
        25
      Howler.volume(per)
    }

    function mute() {
      let audio = audios.value[index.value].howl

      if (audio) {
        mutePlayer.value = !mutePlayer.value

        mutePlayer.value ? audio.mute(true) : audio.mute(false)
      }
    }

    return {
      play,
      pause,
      duration,
      formatTime,
      audios,
      pauseTrack,
      next,
      previous,
      index,
      timer,
      step,
      stepFunction,
      seek,
      selectSound,
      state,
      random,
      repeat,
      progress,
      volume,
      volBar,
      volumeProgress,
      sliderBtn,
      mute,
      mutePlayer,
      sliderBtnVol,
      nextButton,
      prevButton,
    }
  },
  methods: {
    fetchSurah() {
      axios
        .get('http://api.alquran.cloud/v1/quran/ar.alafasy')
        .then((res) => console.log(res.data.data.surahs))
    },
  },
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&display=swap');
* {
  font-family: 'Poppins', sans-serif;
}
#journal-scroll::-webkit-scrollbar {
  width: 4px;
  cursor: pointer;
}
#journal-scroll::-webkit-scrollbar-track {
  background-color: rgba(229, 231, 235, let(--bg-opacity));
  cursor: pointer;
}
#journal-scroll::-webkit-scrollbar-thumb {
  cursor: pointer;
  background-color: #a0aec0;
}
.cd-center {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 60px;
  height: 60px;
  border-radius: 50%;
  background-color: #f7fafc;
}

#progressButtonTimer,
#progressButtonVolume {
  margin-top: -9px;
  right: -8px;
}

@media screen and (max-width: 768px) {
  #progressButtonTimer,
  #progressButtonVolume {
    margin-top: -8px;
    right: -7px;
  }
}
</style>

<style scoped></style>
