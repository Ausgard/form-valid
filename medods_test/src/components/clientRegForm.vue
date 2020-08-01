<template>
    <form class="user-form" @submit.prevent="formHandler">
        <div class="client-surname form-group" :class="{ 'form-group--error': $v.surname.$error }">
            <label class="form__label">Фамилия</label>
            <input class="form__input" v-model.trim="$v.surname.$model" placeholder="Ivanov"/>
        </div>
        <div class="error" v-if="!$v.surname.required">Обязательное поле</div>
        <div class="error" v-else-if="$v.surname.numeric">Фамилия не должна состоять с цифр</div>
        <div class="error" v-else-if="!$v.surname.minLength">Фамилия должна состоять как минимум с {{$v.surname.$params.minLength.min}} символов.</div>
        
        <div class="client-name form-group" :class="{ 'form-group--error': $v.name.$error }">
            <label class="form__label">Имя</label>
            <input class="form__input" v-model.trim="$v.name.$model" placeholder="Ivan"/>
        </div>
        <div class="error" v-if="!$v.name.required">Обязательное поле</div>
        <div class="error" v-else-if="$v.name.numeric">Имя не должно состоять с цифр</div>
        <div class="error" v-if="!$v.name.minLength">Имя должно иметь как минимум {{$v.name.$params.minLength.min}} символа.</div>

        <div class="client-patronymic form-group" :class="{ 'form-group--error': $v.patronymic.$error }">
            <label class="form__label">Отчество</label>
            <input class="form__input" v-model.trim="$v.patronymic.$model" placeholder="Ivanovich"/>
        </div>
        <div class="error" v-if="!$v.patronymic.alpha">Отчество должно состоять только с букв</div>
        <div class="error" v-else-if="$v.patronymic.numeric">Отчество не должно состоять с цифр</div>
        <div class="error" v-if="!$v.patronymic.minLength">Отчество должно иметь как минимум {{$v.patronymic.$params.minLength.min}} символов.</div>
        <button class="form__submit" type="submit">Зарегистрировать</button>
    </form>
</template>

<script>
    import {required, numeric, alpha, minLength, alphaNum} from 'vuelidate/lib/validators'

    export default {
        data() {
            return {
                name: '',
                surname: '',
                patronymic: ''
            }
        },
        validations: {
            name: {
                required,
                numeric,
                alphaNum,
                minLength: minLength(4)
            },
            surname: {
                required,
                numeric,
                alphaNum,
                minLength: minLength(4)
            },
            patronymic: {
                alpha,
                numeric,
                alphaNum,
                minLength: minLength(8)
            }
        }
    }
</script>

<style scoped lang="sass">
.user-form
    display: flex
    flex-direction: column
    justify-content: space-around
    align-items: center
    font-family: Avenir, Helvetica, Arial, sans-serif
    width: auto
    height: auto
    min-width: 255px
    max-width: 428px
    text-align: center
    color: #2c3e50
    margin: auto

.form-group
    display: flex
    flex-direction: column
    align-items: flex-start
    label
        font-size: 16px
        color: rgba(31,63,104, 0.8);
    input
        display: flex
        width: 79.688vw
        height: 9.376vw
        min-width: 255px
        min-height: 30px
        max-width: 428px
        max-height: 45px
        border: 1px solid rgba(#356EAD, 0.4)
        box-sizing: border-box
        border-radius: 5px
        outline: none
        padding: 0 0.8rem
        margin: 0.3rem 0 0 0

.error
  font-size: 12px
  color: orange
  margin: 0.3rem 0 0 0

span
  font-size: 12px
  color: red

.form__submit
    width: 255px
    height: 40px
    color: white
    font-size: 16px
    background-color: #27c289
    border: solid 2px #27c289
    border-radius: 5px
    transition: 0.2s
    margin: 1rem 0 0 0

    &:hover
        background-color: white
        color: #27c289
        transition: 0.2s
</style>