<template>
    <form class="user-form" @submit.prevent="submit">
        <div class="form-group" :class="{'form-group--error': $v.surname.$error}">
            <label class="form__label">фамилия</label>
            <input class="form__input" v-model.trim="$v.surname.$model" placeholder="Ivanov"/>
        </div>
        <div class="error" v-if="!$v.surname.required">Обязательное поле</div>
        <div class="error" v-else-if="!$v.surname.alpha">Только латинские буквы</div>
        <div class="error" v-else-if="!$v.surname.minLength">фамилия должна состоять как минимум с {{$v.surname.$params.minLength.min}} символов.</div>

        
        <div class="form-group" :class="{'form-group--error': $v.name.$error}">
            <label class="form__label">Имя</label>
            <input class="form__input" v-model.trim="$v.name.$model" placeholder="Ivan"/>
        </div>
        <div class="error" v-if="!$v.name.required">Обязательное поле</div>
        <div class="error" v-else-if="!$v.name.alpha">Только латинские буквы</div>
        <div class="error" v-if="!$v.name.minLength">Имя должно состоять как минимум с {{$v.name.$params.minLength.min}} символов.</div>
        
        <div class="client-patronymic form-group" :class="{'form-group--error': $v.patronymic.$error}">
            <label class="form__label" for="patronymic">Отчество</label>
            <input class="form__input" type="text" name="patronymic" id="patronymic" v-model.trim="patronymic" placeholder="Ivanovich"/>
        </div>
        <div class="error" v-if="!$v.patronymic.minLength">Отчество должно иметь как минимум {{$v.patronymic.$params.minLength.min}} символов.</div>
        <div class="error" v-else-if="!$v.patronymic.alpha">Только латинские буквы</div>

        <div class="form-group form-group--birthData">
            <div class="form-group form-group-day" :class="{'form-group--error': $v.birthDay.$error}">
                <label class="form__label" for="birthDay">День</label>
                <input class="form__input" type="text" id="birthDay" name="birthDay" :value="birthDay" @change="setDay($event.target.value)" placeholder="dd">
            </div>
            <div class="form-group form-group-month" :class="{'form-group--error': $v.birthMonth.$error}">
                <label class="form__label" for="birthMonth">Месяц</label>
                <input class="form__input" type="text" id="birthMonth" name="birthMonth" :value="birthMonth" @change="setMonth($event.target.value)" placeholder="mm">
            </div>
            <div class="form-group form-group-year" :class="{'form-group--error': $v.birthYear.$error}">
                <label class="form__label" for="birthDate">Год</label>
                <input class="form__input" type="text" id="birthYear" name="birthYear" :value="birthYear" @change="setYear($event.target.value)" placeholder="year">
            </div>
            <div class="error" v-if="!$v.birthYear.between || !$v.birthMonth.between || !$v.birthDay.between">Не корректная дата</div>
            <div class="error" v-else-if="!$v.birthYear.required || !$v.birthMonth.required || !$v.birthDay.required">Обязательное поле</div>
        </div>

        <div class="form-group form-group-phone" :class="{'form-group--error': $v.phone.$error}">
            <label class="form__label" for="phone">Номер телефона</label>
            <input class="form__input" type="tel" id="phone" name="phone" :value="phone" @change="setPhone($event.target.value)" placeholder="7 (123) 456-78-90">
        </div>
        <div class="error" v-if="!$v.phone.required">Обязательное поле</div>
        <div class="error" v-else-if="!$v.phone.firstNum">Введите в формате "7 (123) 456-78-90"</div>
        
        <div class="form-group form-group-gender" :class="{'form-group--error': $v.gender.$error}">
            <label class="form__label" for="gender">Пол</label>
            <input class="form__input" type="text" id="gender" name="gender" :value="gender" @change="setGender($event.target.value)" placeholder="male/female">
        </div>
        <div class="error" v-if="!$v.gender.genderValid">Укажите пол в формате: "male/female"</div>

        <button class="form__submit" type="submit" :disabled="submitStatus === 'PENDING'">Submit!</button>
        <p class="form__status form__status--success" v-if="submitStatus === 'OK'">Пользователь зарегистрирован!</p>
        <p class="form__status form__status--error" v-if="submitStatus === 'ERROR'">Заполните форму корректно.</p>
        <p class="form__status form__status--checking" v-if="submitStatus === 'PENDING'">Проверка...</p>
    </form>
</template>

<script>
    import {required, minLength, alpha, between, maxLength} from 'vuelidate/lib/validators'
    export default {
    data() {
        return {
        name: '',
        surname: '',
        patronymic: '',
        birthDay: '',
        birthMonth: '',
        birthYear: '',
        phone: '',
        gender: '',
        submitStatus: null
        }
    },
    validations: {
        surname: {
            required,
            alpha,
            minLength: minLength(4)
        },
        name: {
            required,
            alpha,
            minLength: minLength(4)
        },
        patronymic: {
            alpha,
            minLength: minLength(8)
        },
        birthDay: {
            required,
            maxLength: maxLength(2),
            between: between(1, 31)
        },
        birthMonth: {
            required,
            maxLength: maxLength(2),
            between: between(1, 12)
        },
        birthYear: {
            required,
            maxLength: maxLength(4),
            between: between(1940, 2020)
        },
        phone: {
            required,
            firstNum
        },
        gender: {
            genderValid
        }
    },

    methods: {
        setDay(value) {
            this.birthDay = value
            this.$v.birthDay.$touch()       
        },

        setMonth(value) {
            this.birthMonth = value
            this.$v.birthMonth.$touch()
        },
        setYear(value) {
            this.birthYear = value
            this.$v.birthYear.$touch()
        },
        
        setPhone(value) {
            this.phone = value
            this.$v.phone.$touch()
        },

        setGender(value) {
            this.gender = value
            this.$v.gender.$touch()
        },
        submit() {
            this.$v.$touch()
            if (this.$v.$invalid) {
                this.submitStatus = 'ERROR'
            } else {
                this.submitStatus = 'PENDING'
                setTimeout(() => {
                    this.submitStatus = 'OK'
                    }, 500)
                }  
            }
        }
    } 
    function firstNum() {
        let swich
        
        if(this.phone.charAt(0) === '7' && (this.phone.charAt(1) && this.phone.charAt(7) === ' ') && (this.phone.charAt(11) && this.phone.charAt(14) === '-')) {
            swich = true
            let phoneStr = this.phone
            let pattern = /[^\d)(-\s]/g;
            let phoneArr = phoneStr.match(pattern);
            if(phoneArr) {
                swich = false
            } else {
                swich = true
            }      
        } else {
            swich = false
        }
        return swich
    }
    function genderValid() {
        if(this.gender === 'male' || this.gender === 'female' || this.gender === '') {
            return true
        } else {
           return false 
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
  margin: 5px 0 0 0

.form-group--error
    input
        border: solid 2px rgba(#c22727, 0.8)
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

.form__status
    font-size: 12px

.form__status--error
    color: orange

.form__status--success
    color: #27c289 

.form__status--checking
    opacity: 0.3

.form-group--birthData
    display: flex
    flex-direction: row
    justify-content: space-around
    align-items: center
    align-content: center
    flex-wrap: wrap
    width: 230px
    height: auto
    input
        min-width: 20px
        min-height: 20px
        width: 45px
        height: 30px
        text-align: center
        padding: 0 0 0 0

.form-group-day,
.form-group-month,
.form-group-year
    display: flex
    flex-direction: column
    justify-content: center
    align-items: center

</style>