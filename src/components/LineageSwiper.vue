<template>
    <div>
        <swiper
            :slides-per-view="1"
            :loop="true"
            class="mySwiper"
            ref="mySwiper"
            @swiper="onSwiper"
            >
            <swiper-slide v-for="(item, key) in bosses" :key="key" class="swiper-slide">
                <div class="row">
                    <div class="col">
                        <div class="container">
                            <div class="bossName">{{ item.name }}</div>
                            <vue-countdown :time="item.nearestRespawnDateTime" v-slot="{ hours, minutes, seconds }" class="countdown">
                                {{ hours }} hours, {{ minutes }} minutes, {{ seconds }} seconds.
                            </vue-countdown>
                            <div class="percent">Вероятность: {{ item.percent }}</div>
                            <NextSwiperButton :id="item.id" @click="defeatBoss($event)" />
                        </div>
                    </div>
                </div>
                <div class="row">
                    <div class="col">
                    </div>
                </div>
            </swiper-slide>
        </swiper>
    </div>
</template>

<script lang="ts">
    import { ref, defineComponent } from 'vue';
    import { Swiper, SwiperSlide } from 'swiper/vue';
    import VueCountdown from '@chenfengyuan/vue-countdown';
    import NextSwiperButton from './NextSwiperButton.vue';

    import 'swiper/css';
    import 'swiper/css/navigation';
    import 'swiper/css/pagination';
    import 'swiper/css/scrollbar';

    interface Boss {
        id: number,
        name: string,
        resetHours: number,
        percent: number,
        pictureName: string,
        nearestRespawnDateTime: number
    }

    export default defineComponent({
        components: {
            Swiper,
            SwiperSlide,
            VueCountdown,
            NextSwiperButton
        },
        setup() {
            return {
            }
        },
        data() {
            const bosses = ref<Boss[]>([]);

            return {
                loading: false,
                bosses: bosses.value
            };
        },
        created() {
            this.fetchData();
        },
        watch: {
            '$route': 'fetchData'
        },
        methods: {
            fetchData(): void {
                this.bosses = [];
                this.loading = true;

                fetch('https://localhost:5001/LineageBossesApi/LineageBossesApi/GetBosses')
                    .then(r => r.json())
                    .then(json => {
                        this.bosses = json as Boss[];
                        this.loading = false;
                        return;
                    });
            },
            getPicturePath(pictureName: string): string {
                return require('../assets/bosses/' + pictureName + '.png');
            },
            defeatBoss(id: number): void {
                this.bosses = [];
                this.loading = true;

                fetch('https://localhost:5001/LineageBossesApi/LineageBossesApi/DefeatBoss/' + id)
                    .then(r => r.json())
                    .then(json => {
                        this.bosses = json as Boss[];
                        this.loading = false;
                        return;
                    });

            }
        },
    });
</script>

<style scoped>
    .swiper-slide {
        text-align: center;
        background: #fff;
        display: -webkit-box;
        display: -ms-flexbox;
        display: -webkit-flex;
        display: flex;
        -webkit-box-pack: center;
        -ms-flex-pack: center;
        -webkit-justify-content: center;
        justify-content: center;
        -webkit-box-align: center;
        -ms-flex-align: center;
        -webkit-align-items: center;
        align-items: center;
    }
</style>