<template>
    <section class="search">
        <input
            type="number"
            v-model="inputnomor"
            class="input"
            placeholder="Nomor"
        />
        <button @click="view" class="btn">view</button>
    </section>

    <section class="surah">
        <div class="title1">
            <h1 v-if="namaSurah" class="title">{{ namaSurah.name_simple }}</h1>
        </div>

        <div class="murotal">
            <p v-if="audio">
                <audio controls class="murotal1">
                    <source :src="audio.audio_url" type="audio/mpeg" />
                </audio>
            </p>
        </div>

        <div class="bis">
            <p class="basmillah">بسم الله الرحمن الرحيم</p>
        </div>
        <div class="bis">
            <p class="artibasmillah">"dengan menyebut nama Allah yang maha pengasih lagi maha penyayang" </p>
            
        </div>
    </section>

    <section class="hasil">
        <div class="hasill">
            <ul class="lista">
                <li class="ayat" v-for="ayat in ayat" :key="ayat.id">
                    {{ ayat.text_uthmani }} {{ ayat.text }}
                </li>
            </ul>
        </div>
    </section>
</template>
<script>
    import axios from "axios";

    export default {
        data() {
            return {
                ayat: [],
                audio: null,
                namaSurah: null,
                inputnomor: "",
            };
        },

        methods: {
            async view() {
                let nomor = this.inputnomor;
                let title = "https://api.quran.com/api/v4/chapters?language=en";
                let murotal = "https://api.quran.com/api/v4/chapter_recitations/2?language=en";
                 let ayat = `https://api.quran.com/api/v4/quran/verses/uthmani?chapter_number=${nomor}`;
                let arti = "https://api.quran.com/api/v4/quran/translations/134?chapter_number=" + nomor;

                if (nomor > 114 || nomor <= 0 ) {
                    alert("Surah Tidak ditemukan");
                } else {
                    const reqAyat = axios.get(ayat);
                    const reqArti = axios.get(arti);
                    const reqJudul = axios.get(title);
                    const reqSuara = axios.get(murotal);

                    axios.all([reqJudul, reqSuara, reqAyat, reqArti, ]).then(
                        axios.spread((...response) => {
                            const responseJudul = response[0];
                            const responseSuara = response[1];
                            const responseAyat = response[2];
                            const responseArti = response[3];

                            const a = responseAyat.data.verses;
                            const b = responseArti.data.translations;

                            const mix = (a, b) => {
                                const res = [];

                                for (let i = 0; i < a.length + b.length; i++) {
                                    if (i % 2 === 0) {
                                        res.push(a[i / 2]);
                                    } else {
                                        res.push(b[(i - 1) / 2]);
                                    }
                                }
                                return res;
                            };

                            this.ayat = mix(a, b);
                            this.audio =
                                responseSuara.data.audio_files[nomor - 1];
                            this.namaSurah =
                                responseJudul.data.chapters[nomor - 1];
                        })
                    );
                }
            },
        },
    };
</script>
<style>
    .search {
        display: flex;
        margin: 50px 0 0 0;
        flex-direction: row;
        justify-content: center;
    }

    .input {
        height: 30px;
        border: 2px solid #323232;
        color: #163317;
    }

    .input:hover {
        border: 1px solid #6d9581;
    }

    .btn {
        background-color: #163317;
        border: 1px solid #323232;
        height: 30px;
        font-size: 12px;
        color: white;
        border-radius: 5%;
    }

    .btn:hover {
        color: #c2c2b4;
    }

    .murotal1 {
        width: 70%;
        height: 30px;
    }

    .murotal {
        margin: 50px 0;
        text-align: center;
    }

    .title {
        text-align: center;
        font-size: 70px;
        color: #705725;
        margin: 60px 0 0 30px;
    }

    .basmillah {
        text-align: center;
        font-size: 50px;
        color: #163317;
    }

      .artibasmillah {
        text-align: center;
        font-size: 20px;
        color: #163317;
        margin-bottom: 20px;
    }

    .ayat {
        margin-top: 20px;
        color: #163317;
        list-style: none;
        margin: 0 200px 50px 200px;
    }

    .ayat:nth-child(odd) {
        text-align: right;
        font-size: 40px;
    }
    .ayat:nth-child(even) {
        text-align: left;
        font-size: 15px;
        color: #163317;
    }

    @media screen and (max-width: 400px) {
        .ayat:nth-child(odd) {
            font-size: 20px;
            margin-bottom: 20px;
            margin-right: 10px;
        }
        .ayat:nth-child(even) {
            font-size: 10px;
            margin: 20px;
        }
        .ayat {
            margin: 0;
        }
    }
</style>