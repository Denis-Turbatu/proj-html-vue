<script>
export default {
    props: {
        title: "String",
        subTitle: "String",
    },
    data (){
        return{
            activeIndex: 0,
            start: "",
            imgUrl: "../src/assets/img/shop-bg.png",
            isShown: false,
            cards_pos: 0,
            portFolio: [
                {
                    name: "Sony PS5",
                    img: "s1",
                    score: "",
                    originalPrice: 599,
                    discountedPrice: 399,
                    moreImg: ["more1", "more2"]
                },

                {
                    name: "Asus Rog Strix A53",
                    img: "s2",
                    score: "",
                    originalPrice: 599,
                    discountedPrice: 399
                },

                {
                    name: "Samsung Gear VR",
                    img: "s3",
                    score: "",
                    originalPrice: 599,
                    discountedPrice: 399,
                    moreImg: ["morevr2"]
                },

                {
                    name: "Beats Pro",
                    img: "s4",
                    score: "",
                    originalPrice: 599,
                    discountedPrice: 399
                },

            ]
        }
    },
    computed: {
        cardsToDisplay() {
            return [...this.portFolio, ...this.portFolio].slice(this.cards_pos, this.cards_pos + 4)
        },
    },
    
    methods: {
        getPic(name) {
            return new URL(`../assets/img/${name}.png`, import.meta.url).href
        },
        prev() {
            this.cards_pos = (this.cards_pos + this.portFolio.length - 1) % this.portFolio.length;
        },
        next() {
            this.cards_pos = (this.cards_pos + 1) % this.portFolio.length;
        },

        stopInterval () {
            clearInterval(this.start)
            return this.start = null
        },
        startInterval () {
            return this.start = setInterval(this.next,2500);
        }
    },

    mounted(){
        this.start = setInterval(this.next,2500);
    },
}
</script>

<template>
    <div class="container-fluid" :style='{ backgroundImage: `url("${imgUrl}")`, backgroundSize: "cover" }'>
        <div class="container">
            <!-- HEADER -->
            <div class="row">
                <div class="col mt-5">
                    <div class="d-flex justify-content-between">

                        <!-- TEXT -->
                        <div class="w-50 ms-mg">
                            <h5 class="ms-green">{{ subTitle }}</h5>
                            <span class="ms-fs fw-bold text-white">{{ title }}</span>
                        </div>
                        <!-- TEXT -->

                        <!-- BUTTONS -->
                        <div class="w-50 ms-mg text-white d-flex gap-2 justify-content-end align-items-end">
                            <!-- PREV -->
                            <button class="ms-btn rounded-circle" @click="prev">
                                <i class="fa-solid fa-arrow-left"></i>
                            </button>
                            <!-- PREV -->

                            <!-- NEXT -->
                            <button class="ms-btn rounded-circle ms-bg-green text-white" @click="next">
                                <i class="fa-solid fa-arrow-right"></i>
                            </button>
                            <!-- NEXT -->
                        </div>
                        <!-- /BUTTONS -->

                    </div>
                </div>
            </div>
            <!-- /HEADER -->

            <!-- MAIN CARDS -->
            <div class="row mt-5 pb-5 justify-content-center">
                <div class="col-3" @mouseenter="stopInterval" @mouseleave="startInterval"
                    v-for="(element, index) in cardsToDisplay">

                    <!-- IMG -->
                    <div class="img-cont mb-2 d-flex justify-content-center">
                        <img :src="getPic(element.img)" :alt="element">
                        <div class="buttons">
                            <button class="card-btn m-1" @click="activeIndex = index" data-bs-toggle="modal"
                                data-bs-target="#exampleModal"><i class="fa-solid fa-eye"></i></button>
                            <button class="card-btn m-1"><i class="fa-solid fa-cart-flatbed"></i></button>
                            <button class="card-btn m-1"><i class="fa-solid fa-heart"></i></button>
                        </div>
                    </div>
                    <!-- /IMG -->

                    <!-- INFO -->
                    <div class="row align-items-center">
                        <div class="col">
                            <div class="txt-cont">
                                <div class="stars mt-3">
                                    <i class="fa-solid fa-star-half-stroke fs-5 ms-green"></i>
                                    <i class="fa-solid fa-star-half-stroke fs-5 ms-green"></i>
                                    <i class="fa-solid fa-star-half-stroke fs-5 ms-green"></i>
                                    <i class="fa-solid fa-star-half-stroke fs-5"></i>
                                    <i class="fa-solid fa-star-half-stroke fs-5"></i>
                                </div>
                                <h5 class="mt-2 mb-0">{{ element.name }}</h5>
                                <div class="d-flex gap-3">
                                    <p class="discounted-price">${{ element.discountedPrice }}.00</p>
                                    <p class="original-price">${{ element.originalPrice }}.00</p>
                                </div>
                            </div>
                        </div>
                    </div>
                    <!-- /INFO -->
                </div>
            </div>
            <!-- /MAIN CARDS-->
        </div>
    </div>


    <!-- MODAL IMG -->
    <div class="modal modal-lg fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel"
        aria-hidden="true">
        <div class="modal-dialog modal-dialog-centered">
            <div class="modal-content">
                <div class="modal-body m-auto">
                    <img style="width: 150%;" :src="getPic(portFolio[this.activeIndex].img)">
                </div>
            </div>
        </div>
    </div>

</template>

<style scoped lang="scss">
    @use "../style/partials/variables" as *;
    // CUSTOM
    .ms-green{
        color: $green
    }

    .ms-bg-green{
        background-color: $green
    }

    .ms-fs{
        font-size: 3rem;
    }

    .ms-mg {
        margin-top: 0px;
    }

    .ms-btn {
        height: 40px;
        width: 40px;
        border: hidden;
    }
    
    .card-btn {
        height: 50px;
        width: 50px;
        padding: 15px;
        border-radius: 50%;
        border: hidden;
        background-color: $light-violet;
        color: white;
    }
    
    // CONTAINER INDEX
    .container{
        position: relative;
        z-index: 2;
    }
    // BG
    .container-fluid {
        position: relative;
    }

    // BG BLACK LAYER
    .container-fluid::before {
        content: '';
        position: absolute;
        left: 0px;
        top: 0px;
        width: 100%;
        height: 100%;
        background: rgba(32, 32, 70, 0.7);
        z-index: 0;
    }   

    // HOVER
    .col-3:hover .buttons {
        cursor: pointer;
        display: block;
    }

    .card-btn:hover {
        background-color: $light-green
    }

    .img-cont{
        background-color: $purple;
        padding: 30px;
        border-radius: 15px;
        position: relative;
        min-height: 260px;
        img {
            max-width: 100%;
            object-fit: contain;
        }
        .buttons{
            display: none;
            position: absolute;
            bottom: 50px;

        }
    }

    .txt-cont{
        color: white;

        h5 {
            font-size: 1.5rem;
            font-weight: bold;
        }

        p {
            font-size: 1.2rem;
            font-weight: 400;
        }

        .discounted-price {
            color: $light-green;
        }

        .original-price {
            text-decoration: line-through;
        }

    // MODAL 
}

</style>