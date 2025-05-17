<template>
    <div class="flex gap-x-6 gap-y-2 flex-wrap">
        <div v-for="(status, index) in statusList" :key="index" class="basis-1/7 shrink-0">
            <div v-calc class="relative grid place-items-center h-12 px-3"
                :class="index <= activeIndex ? 'bg-gray-400' : 'bg-[#ddd]'">
                <span class="text-sm text-gray-950 text-center relative z-10 block">{{ status.text }}</span>
                <div class="triangle absolute right-0" :style="getTriangleStyles"></div>
            </div>
        </div>
    </div>
</template>

<script setup lang="ts">
const props = defineProps({
    status: {
        type: String,
        default: 'new'
    }
});
const statusList = [
    {
        text: 'Новое',
        code: 'new'
    },
    {
        text: 'Просмотрено',
        code: 'viewed'
    },
    {
        text: 'Отправлено приглашение',
        code: 'invited'
    },
    {
        text: 'Назначено собеседование',
        code: 'interview'
    },
    {
        text: 'Не дошёл',
        code: 'died'
    },
    {
        text: 'Проведено собеседование',
        code: 'completed'
    },
    {
        text: 'Ожидает ответа соискателя',
        code: 'waiting'
    },
    {
        text: 'Принятие решения',
        code: 'solution'
    },
    {
        text: 'Принят',
        code: 'accept'
    },
    {
        text: 'Отклонено/Отказ',
        code: 'denied'
    },
    {
        text: 'Архивировано',
        code: 'archive'
    },
];


const activeIndex = computed(() => statusList.indexOf(statusList.find((el) => props.status === el.code) || statusList[0]));

const triangleHeight = ref(0);

// Треугольник в макете тоже не крашеный
const triangleColor = '#ddd';

// Чтобы треугольник брал реальные размеры плашки
const vCalc = {
    mounted: (e: HTMLElement) => {
        triangleHeight.value = e.offsetHeight / 2;
    }
}
const getTriangleStyles = computed(() => `right:-${triangleHeight.value / 2}px ;border-top: ${triangleHeight.value}px solid transparent;border-bottom: ${triangleHeight.value}px solid transparent;border-left: ${triangleHeight.value / 2}px solid ${triangleColor};`)

</script>
