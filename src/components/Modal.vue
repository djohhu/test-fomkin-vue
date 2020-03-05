<template>
  <transition name="modal-transition">
    <div class="modal" v-if="modalOpen">
      <div class="modal-overlay" @click.prevent="emitCLose"></div>
      <div class="modal__content">
        <div class="modal__close" @click.prevent="emitCLose">
          <svg width="18" height="18">
            <g stroke="#000000" fill="none" stroke-width="2">
              <line x1="0" y1="0" x2="18" y2="18"></line>
              <line x1="0" y1="18" x2="18" y2="0"></line>
            </g>
          </svg>
        </div>
        <div class="modal__body">
          <div class="modal__img">
            <div class="modal__img-item"
                 :style="{ backgroundImage: `url('${infoPhoto.url}')` }">
            </div>
          </div>
          <div class="modal__comment">
            <div v-if="infoPhoto.comments.length === 0">
              Нет комментариев
            </div>
            <div v-else=""
                 v-for="comment in infoPhoto.comments"
                 :key="comment.id"
                 class="modal__comment-item">
              <div class="modal__comment-time">{{formatDate(comment.date)}}</div>
              <div class="modal__comment-text">{{comment.text}}</div>
            </div>
          </div>
          <Form @sendComment="sendComment"/>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
    import Form from "./Form";

    export default {
        name: "Modal",
        components: {Form},
        props: {
            modalOpen: {
                type: Boolean
            },
            infoPhoto: {
                type: Object
            }
        },
        methods: {
            emitCLose() {
                this.$emit('close')
            },
            formatDate(date) {
                return new Date(date).toLocaleDateString()
            },
            sendComment(data) {
                fetch(`https://boiling-refuge-66454.herokuapp.com/images/${this.infoPhoto.id}/comments`,
                    {
                        method: "POST",
                        headers: {'Content-Type': 'application/json;charset=UTF-8'},
                        body: JSON.stringify(data)
                    })
                    .then(res => {
                        console.log('Отправлено', res);
                        const comment = {
                            date: new Date().toLocaleDateString(),
                            text: data.comment
                        };
                        this.infoPhoto.comments.push(comment)
                    })
                    .catch(error => console.error('Ошибка', error));
            }
        },
    };

</script>

<style lang="scss" scoped>
  @import "../assets/scss/variables";

  .modal, .modal-overlay {
    position: fixed;
    left: 0;
    top: 0;
    right: 0;
    bottom: 0;
    z-index: 1000;
  }

  .modal-overlay {
    background: rgba(0, 0, 0, 0.7);
  }

  .modal {
    display: flex;
    align-items: center;
    justify-content: center;
    transition: all .3s;

    &__content {
      background: #fff;
      color: $color-main;
      height: 100%;
      position: relative;
      width: 100%;
      z-index: 1000;
      padding: 0 22px 48px;
      line-height: 15px;
      @media (min-width: $container-desktop) {
        width: 619px;
        height: auto;
        padding: 30px 30px 32px;
      }
    }

    &__close {
      position: absolute;
      right: 21px;
      top: 21px;
      z-index: 101;
      cursor: pointer;
    }

    &__body {
      @media (min-width: $container-desktop) {
        display: grid;
        grid-template-columns: 331px 214px;
        grid-gap: 15px;
      }
    }

    &__img {
      margin: 0 -22px 25px;
      @media (min-width: $container-desktop) {
        margin: 0 0 14px;
      }

      &-item {
        height: 205px;
        width: 100%;
        background-size: cover;
        background-position: center;
        background-repeat: no-repeat;
        margin: 0 auto;
      }
    }

    &__comment {

      padding-bottom: 20px;
      @media (min-width: $container-desktop) {
        grid-row: span 2;
      }

      &-item {
        margin-bottom: 20px;
      }

      &-time {
        color: $color-grey;
        margin-bottom: 5px;

      }
    }
  }

  .modal-transition-enter-active {
    opacity: 1;
  }

  .modal-transition-enter {
    opacity: 0;
    transform: scale(1.5, 1.5);
  }

  .modal-transition-leave-active {
    opacity: 0;
  }

  .modal-transition-leave {
    opacity: 1;
  }
</style>