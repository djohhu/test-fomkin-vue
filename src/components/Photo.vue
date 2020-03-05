<template>
  <section class="photo">
    <div class="container">
      <div v-if="fetching" class="photo__loading">
        Загрузка
      </div>
      <div v-else="" class="photo__wrapper">
        <div v-for="photo in photos"
             class="photo__item"
             @click.prevent="openModal(photo.id)"
             :key="photo.id"
             :style="{ backgroundImage: `url('${photo.url}')` }">
        </div>
      </div>
    </div>
    <Modal :modalOpen="modalOpen" :infoPhoto="infoPhoto" @close="closeModal"/>
  </section>
</template>

<script>
    import Modal from "./Modal";

    export default {
        name: "Photo",
        components: {
            Modal
        },
        data() {
            return {
                modalOpen: false,
                photos: [],
                fetching: false,
                infoPhoto: {}
            }
        },
        methods: {
            loadPhotos() {
                this.fetching = true;
                fetch('https://boiling-refuge-66454.herokuapp.com/images')
                    .then(data => data.json())
                    .then(photos => {
                        this.photos = photos;
                        this.fetching = false
                    })
            },
            openModal(id) {
                fetch(`https://boiling-refuge-66454.herokuapp.com/images/${id}`)
                    .then(data => data.json())
                    .then(photo => {
                        this.infoPhoto = photo;
                        this.modalOpen = true;
                    });
            },
            closeModal() {
                this.modalOpen = false
            }

        },
        mounted() {
            this.loadPhotos()
        }
    }
</script>

<style lang="scss" scoped>
  @import "../assets/scss/variables";

  .photo {
    &__loading {
      font-size: 20px;
      text-align: center;
      padding: 20px 0;
    }

    &__wrapper {
      display: grid;
      padding-bottom: 88px;
      grid-template-columns: 280px;
      grid-auto-rows: 171px;
      justify-content: center;
      grid-gap: 30px 20px;
      @media (min-width: $container-desktop) {
        grid-template-columns: repeat(3, 229px);
        grid-auto-rows: 142px;
      }
    }

    &__item {
      background-size: cover;
      background-position: center;
      background-repeat: no-repeat;
      cursor: pointer;
    }
  }

</style>
