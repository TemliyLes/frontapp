<template>
    <div class="pt-6">
        <div class="flex gap-2 mb-6">
            <div v-for="(btn, btnIndex) in menuBtns" :key="btnIndex">
                <UButton :icon="makeIcon(btn.icon)" variant="subtle" class="cursor-pointer"></UButton>
            </div>
        </div>
        <div class="flex gap-4 mb-6">
            <div class="shrink-0"><img :src="getRealImgURL(data?.photo)"></div>
            <div class="md:-mt-1.5">
                <h4 class="text-xl mb-2">{{ data?.name }}</h4>
                <p class="text-sm mb-2">{{ showedDescription }} <span class="font-medium cursor-pointer"
                        @click="fullDescription = true">Показать всё</span></p>
                <div class="flex gap-2">
                    <div class="pt-0.5">
                        <UIcon class="text-yellow-600" :name="makeIcon('phone')" />
                    </div>
                    <a class="text-sky-600 hover:underline" :href="`tel:${data?.phone}`">{{ data?.phone }}</a>
                </div>
                <div class="flex gap-2">
                    <div class="pt-1">
                        <UIcon class="text-yellow-600" :name="makeIcon('mail')" />
                    </div>
                    <a class="text-sky-600 hover:underline" :href="`mailto:${data?.email}`">{{ data?.email }}</a>
                </div>
            </div>
        </div>
        <div class="mb-6">
            <h2 class="text-xl font-medium mb-3">Дела:</h2>
            <div class="flex gap-4">
                <div v-for="(action, actionIndex) in userActions" :key="actionIndex" :inert="action.disabled"
                    @click="toggleActive(actionIndex)"
                    :class="[action.active ? 'bg-green-500 text-white' : 'bg-white text-blue-400', action.disabled ? 'opacity-60' : '']"
                    class="transition-all duration-300 h-12 text-sm w-36 cursor-pointer border rounded-sm border-2 font-medium !border-green-300 text-center leading-none grid place-items-center px-4">
                    <span>{{ action.text }}</span>
                </div>
            </div>
        </div>
        <!-- Тут для разнообразия сделаю компонент -->
        <h2 class="text-xl font-medium mb-3">Статус рассмотрения:</h2>
        <Status :status="data?.status" />
        <div></div>
    </div>
</template>

<script setup lang="ts">
import Status from '~/components/Status.vue';
const URL = 'https://dev.jobcart.ru/wp-json/test/v2/app';

const { data } = await useFetch(URL);

const fullDescription = ref(false);
const MAX_DESCRIPTION_SYMBOLS = 150;
const showedDescription = computed(() => !fullDescription.value ? `${data.value?.description?.slice(0, MAX_DESCRIPTION_SYMBOLS)}...` : data.value?.description);

const makeIcon = (name: string) => `i-lucide-${name}`;

// Картинка есть только на реальном серваке)
const getRealImgURL = (basic: string) => `https://dev.jobcart.ru/${basic}`

// Иммитирую какое-то меню действий
const menuBtns = [{
    href: '#',
    icon: 'sun'
}, {
    href: '#',
    icon: 'arrow-down'
}, {
    href: '#',
    icon: 'eye'
}, {
    href: '#',
    icon: 'hash'
}, {
    href: '#',
    icon: 'eye-off'
}, {
    href: '#',
    icon: 'menu'
}];

// Тут я из макета вообще ничё не понял, то ли это ховер-эффект, то ли какое-то активное состояние, хотя из контекста больше похоже, что статусы и кнопки вместе
const userActions = ref([{
    text: 'Собеседование запланировано',
    active: true,
    disabled: false
},
{
    text: 'Создать видеозвонок',
    active: false,
    disabled: false
},
{
    text: 'Запланировать событие',
    active: false,
    disabled: true
},
{
    text: 'Отправить запрос',
    active: false,
    disabled: false
},
]);

const toggleActive = (index: number) => {
    userActions.value[index].active = !userActions.value[index].active
};

definePageMeta({
    layout: 'default',
});
</script>