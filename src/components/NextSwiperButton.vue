<template>
    <div class="row">
        <div class="col">
            <button @click="onNext()" class="actionButton">Внести убийство</button>
        </div>
    </div>
    <div class="row">
        <div class="col">
            <button @click="onNext()" class="actionButton">Не появился</button>
        </div>
    </div>
</template>

<script lang="ts">
    import { useSwiper } from 'swiper/vue';

    export default {
        props: {
            id: Number
        },
        setup(props: any, { emit } : any) {
            const swiper = useSwiper();

            const onNext = () => {
                swiper.value.slideNext();

                emit('click', props.id);
            };

            return {
                onNext,
                swiper,
            };
        },
    };
</script>

<style scoped>
    .actionButton {
        width: 200px;
    }
</style>