<template>
    <form class="user-form" @submit.prevent="formHandler">
        <div class="client-surname form-group">
            <label class="form__label" for="surname">Фамилия</label>
            <input class="form__input" type="text" name="surname" id="surname" v-model.trim="surname" :class="{invalid: ($v.surname.$dirty && !$v.surname.required) || ($v.surname.$dirty && !$v.surname.minLength) || ($v.surname.$dirty && $v.surname.numeric)}" placeholder="Иванов"/>
        </div>
        <div class="error" v-if="$v.surname.$dirty && !$v.surname.required">Обязательное поле</div>
        <div class="error" v-else-if="$v.surname.$dirty && $v.surname.numeric">Фамилия не должна состоять с цифр</div>
        <div class="error" v-else-if="$v.surname.$dirty && !$v.surname.minLength">Фамилия должна иметь как минимум {{$v.surname.$params.minLength.min}} символа.</div>


        <div class="client-name form-group">
            <label class="form__label" for="name">Имя</label>
            <input class="form__input" type="text" name="name" id="name" v-model.trim="name" :class="{invalid: ($v.name.$dirty && !$v.name.required) || ($v.name.$dirty && !$v.name.minLength) || ($v.name.$dirty && $v.name.numeric)}" placeholder="Иван"/>
        </div>
        <div class="error" v-if="$v.name.$dirty && !$v.name.required">Обязательное поле</div>
        <div class="error" v-else-if="$v.name.$dirty && $v.name.numeric">Имя не должно состоять с цифр</div>
        <div class="error" v-else-if="$v.name.$dirty && !$v.name.minLength">Имя должно иметь как минимум {{$v.name.$params.minLength.min}} символа.</div>

        <div class="client-patronymic form-group">
            <label class="form__label" for="patronymic">Отчество</label>
            <input class="form__input" type="text" name="patronymic" id="patronymic" v-model.trim="patronymic" :class="{invalid: ($v.patronymic.$dirty && !$v.patronymic.minLength) || ($v.patronymic.$dirty && $v.patronymic.numeric && $v.patronymic.required)}" placeholder="Иванович"/>
        </div>
        
        <div class="error" v-if="$v.patronymic.$dirty && !$v.patronymic.minLength">Отчество должно иметь как минимум {{$v.patronymic.$params.minLength.min}} символов.</div>
        <div class="error" v-else-if="($v.patronymic.$dirty && $v.patronymic.numeric && $v.patronymic.required)">Отчество не должно состоять с цифр</div>
        
        <button class="form__submit" type="submit">Зарегистрировать</button>
    </form>
</template>

<script>
    import {required, numeric, minLength} from 'vuelidate/lib/validators'

    export default {
        data() {
            return {
                name: '',
                surname: '',
                patronymic: ''
            }
        },
        validations: {
            surname: {
                required,
                numeric,
                minLength: minLength(4)
            },
            name: {
                required,
                numeric,
                minLength: minLength(4)
            },
            patronymic: {
                required,
                numeric,
                minLength: minLength(8)
            }
        },

        methods: {
            formHandler() {
                if(this.$v.$invalid) {
                    let patronymicArr = []
                    patronymic.value
                    patronymicArr = patronymic.value.split('')

                    removeSpaces(patronymicArr)
                    console.log(patronymicArr)
                    
                    this.$v.$touch()
                    return
                }

                function removeSpaces(array) {
                    for (var i = 0; i < array.length; i++) {
                        for (var j = i + 1; j < array.length;) {
                        if (array[i] === ' ') {
                            array.splice(i, 1);
                        } else {
                            j++;
                        }
                        }
                    }
                }

                function alphaNumValid(array) {

                }
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
        margin: 0.3rem 0 19px 0
        &.invalid
            border: solid 2px rgba(#c22727, 0.8)
            margin: 0.3rem 0 0 0

.error
  font-size: 12px
  color: orange
  margin: 5px 0 0 0

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