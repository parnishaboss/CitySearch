<template>
  <header class="sticky top-0 bg-weather-primary shadow-lg">
    <nav class="container flex flex-col sm:flex-row items-center gap-4 text-white py-6">
      <RouterLink :to="{ name: 'home'}">
        <div class="flex items-center gap-3 ">
          <i class="fa-solid fa-sun text-2xl"></i>
          <p class="text-2xl">The Local Weather</p>
        </div>
      </RouterLink>
      <div class="flex gap-3 flex-1 justify-end items-center">
        <i class="fa-solid fa-circle-info text-2xl hover:text-weather-secondary duration-150 cursor-pointer"
           @click="toggleModal"></i>
        <i class="fa-solid fa-plus text-xl hover:text-weather-secondary duration-150 cursor-pointer"></i>
      </div>
      <BaseModal :modalActive="modalActive" @close-modal="toggleModal">
        <div class="text-black">
          <h1 class="text-2xl mb-1">О приложении:</h1>
          <p class="mb-4">
            Локальная Погода позволяет отслеживать текущую и будущую погоду в городах, которые вы выберете.
          </p>
          <h2 class="text-2xl">Как это работает:</h2>
          <ol class="list-decimal list-inside mb-4">
            <li>
              Найдите свой город, введя его название в строку поиска.
            </li>
            <li>
              Выберите город из результатов, чтобы перейти к текущей погоде для этого города.
            </li>
            <li>
              Отслеживайте город, нажав на значок "+" в правом верхнем углу. Это сохранит город для просмотра позже на
              главной странице.
            </li>
          </ol>

          <h2 class="text-2xl">Удаление города:</h2>
          <p>
            Если вы больше не хотите отслеживать город, просто выберите его на главной странице. Внизу страницы будет
            опция для удаления города.
          </p>
        </div>

      </BaseModal>
    </nav>
  </header>
</template>

<script setup>
import {RouterLink} from "vue-router"
import BaseModal from "@/components/BaseModal.vue";
import {ref} from "vue";

const modalActive = ref(null)
const toggleModal = () => {
  modalActive.value = !modalActive.value
}
</script>

