<template>
  <form @submit.prevent="checkForm" action="#" method="POST" class="form modal__form">
    <input type="text"
           aria-label="Ваше имя"
           v-model="name"
           placeholder="Ваше имя"
           class="form__input"
           v-bind:class="{ 'error': attemptSubmit && missingName }"/>
    <input type="text"
           aria-label="Ваш комментарий"
           v-model="comment"
           placeholder="Ваш комментарий"
           class="form__input"
           v-bind:class="{ 'error': attemptSubmit && missingComment }"/>
    <button type="submit" class="button button_blue">Оставить комментарий</button>
  </form>
</template>

<script>
    export default {
        name: "Form",
        data() {
            return {
                attemptSubmit: false,
                name: '',
                comment: ''
            }
        },
        computed: {
            missingName() {
                return this.name === '';
            },
            missingComment() {
                return this.comment === '';
            },
        },
        methods: {
            checkForm() {
                this.attemptSubmit = true;
                if (!this.missingName || !this.missingComment) {
                    this.$emit('sendComment', {
                        name: this.name,
                        comment: this.comment
                    })
                }
            }
        }
    }
</script>