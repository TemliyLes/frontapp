<template>
    <div class="pt-6 pb-24">
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
                    <NuxtLink class="text-sky-600 hover:underline" :to="`tel:${data?.phone}`">{{ data?.phone }}
                    </NuxtLink>
                    <div class="flex gap-1">
                        <NuxtLink v-for="(social, socialIndex) in socials" :to="social.to" :key="socialIndex" external>
                            <img :src="social.img" class="w-6 h-6 object-cover" alt="">
                        </NuxtLink>
                    </div>
                </div>
                <div class="flex gap-2">
                    <div class="pt-1">
                        <UIcon class="text-yellow-600" :name="makeIcon('mail')" />
                    </div>
                    <NuxtLink class="text-sky-600 hover:underline" :to="`mailto:${data?.email}`">{{ data?.email }}
                    </NuxtLink>
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
        <div class="mb-6">
            <h2 class="text-xl font-medium mb-3">Статус рассмотрения:</h2>
            <Status :status="data?.status" />
        </div>
        <h2 class="text-3xl font-medium mb-3">Pikabu отклик:</h2>
        <div class="mb-6">
            <NuxtLink to="https://pikabu.ru/" external>???????????????</NuxtLink>
        </div>
        <div class="mb-12">
            <p class="text-xl"><b>Дата рождения:</b> {{ dateToShow }}</p>

            <!-- Поля гражданство нет в апи -->
            <p class="text-xl"><b>Город:</b> {{ data?.town }}</p>
        </div>

        <div class="bg-sky-600 h-36 w-full flex gap-12 p-9">
            <div class="h-full aspect-square grid place-items-center">
                <UIcon size="72" :name="makeIcon('info')"></UIcon>
            </div>
            <div>
                <h2 class="text-xl font-medium mb-3">Файлы портфолио:</h2>
                <div class="flex gap-3">
                    <div v-for="(file, fileIndex) in portfolios" :key="fileIndex">
                        <NuxtLink :to="file.href">{{ file.text }}</NuxtLink>
                    </div>
                </div>
            </div>
        </div>
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

const socials = [{
    to: 'https://www.whatsapp.com/?lang=ru_RU',
    img: 'data:image/png;base64,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'
}, {
    to: 'https://www.viber.com/ru/',
    img: 'https://cdn-icons-png.flaticon.com/512/3938/3938039.png'
},
{
    to: 'https://web.telegram.org/',
    img: 'data:image/png;base64,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',

}
];

const toggleActive = (index: number) => {
    userActions.value[index].active = !userActions.value[index].active
};

const dateToShow = computed(() => data.value?.date.split(' ')?.[0])


// К сожалению , тут ничего не приходит (
const portfolios = computed(() => data.value?.portfolios || [{
    text: 'Ну хоть что-то',
    href: 'https://pikabu.ru/'
}])

definePageMeta({
    layout: 'default',
});
</script>