<template>
    <form class="user-form" @submit.prevent="submit">
        <div class="user-form__main-info">
            <h3 class="user-form__hedline">Основная информация</h3>
            <div class="user-form__surname">
                <div class="form-group" :class="{'form-group--error': $v.surname.$error}">
                    <label class="form__label">фамилия</label>
                    <input class="form__input" v-model.trim="$v.surname.$model" placeholder="Ivanov"/>
                </div>
                <p class="error" v-if="!$v.surname.required">Обязательное поле</p>
                <p class="error" v-else-if="!$v.surname.alpha">Только латинские символы</p>
                <p class="error" v-else-if="!$v.surname.minLength">фамилия должна состоять как минимум с {{$v.surname.$params.minLength.min}} символов.</p>
                <p class="error" v-else-if="!$v.surname.surnameValid">Не корректный ввод</p>
            </div>
            <div class="user-form__name">
                <div class="form-group" :class="{'form-group--error': $v.name.$error}">
                    <label class="form__label">Имя</label>
                    <input class="form__input" v-model.trim="$v.name.$model" placeholder="Ivan"/>
                </div>
                <p class="error" v-if="!$v.name.required">Обязательное поле</p>
                <p class="error" v-else-if="!$v.name.nameValid">Не корректный ввод</p>
                <p class="error" v-else-if="!$v.name.alpha">Только латинские символы</p>
                <p class="error" v-else-if="!$v.name.minLength">Имя должно состоять как минимум с {{$v.name.$params.minLength.min}} символов.</p>
            </div>
            <div class="user-form__patronymic">
                <div class="client-patronymic form-group" :class="{'form-group--error': $v.patronymic.$error}">
                    <label class="form__label" for="patronymic">Отчество</label>
                    <input class="form__input" type="text" name="patronymic" id="patronymic" v-model.trim="patronymic" placeholder="Ivanovich"/>
                </div>
                <p class="error" v-if="!$v.patronymic.minLength">Отчество должно иметь как минимум {{$v.patronymic.$params.minLength.min}} символов.</p>
                <p class="error" v-else-if="!$v.patronymic.alpha">Только латинские символы</p>
                <p class="error" v-else-if="!$v.patronymic.patronymicValid">Не корректный ввод</p>
            </div>
            <div class="user-form__birthData">
                <div class="user-form__day" :class="{'form-group--error': $v.birthDay.$error}">
                    <label class="form__label" for="birthDay">День</label>
                    <input class="form__input" type="text" id="birthDay" name="birthDay" :value="birthDay" @change="setDay($event.target.value)" placeholder="dd">
                </div>
                <div class="user-form__month" :class="{'form-group--error': $v.birthMonth.$error}">
                    <label class="form__label" for="birthMonth">Месяц</label>
                    <input class="form__input" type="text" id="birthMonth" name="birthMonth" :value="birthMonth" @change="setMonth($event.target.value)" placeholder="mm">
                </div>
                <div class="user-form__year" :class="{'form-group--error': $v.birthYear.$error}">
                    <label class="form__label" for="birthDate">Год</label>
                    <input class="form__input" type="text" id="birthYear" name="birthYear" :value="birthYear" @change="setYear($event.target.value)" placeholder="year">
                </div>
                <p class="error" v-if="!$v.birthYear.between || !$v.birthMonth.between || !$v.birthDay.between">Не корректная дата</p>
                <p class="error" v-else-if="!$v.birthYear.required || !$v.birthMonth.required || !$v.birthDay.required">Обязательное поле</p>
            </div>
            <div class="user-form__phone" :class="{'form-group--error': $v.phone.$error}">
                <label class="form__label" for="phone">Номер телефона</label>
                <input class="form__input" type="tel" id="phone" name="phone" :value="phone" @change="setPhone($event.target.value)" placeholder="7 (123) 456-78-90">
                <p class="error" v-if="!$v.phone.required">Обязательное поле</p>
                <p class="error" v-else-if="!$v.phone.numValid">Введите в формате "7 (123) 456-78-90"</p>
            </div>
            <div class="user-form__gender" :class="{'form-group--error': $v.gender.$error}">
                <label class="form__label" for="gender">Пол</label>
                <input class="form__input" type="text" id="gender" name="gender" :value="gender" @change="setGender($event.target.value)" placeholder="male/female">
                <p class="error" v-if="!$v.gender.genderValid">Укажите в формате: "male/female"</p>  
            </div>
            <div class="user-form__client-info">
                <div class="user-form__clientGroup" :class="{'form-group--error': $v.clientGroupInfo.$error}">
                    <label class="form__label" for="clientsList">Группа клиентов</label>
                    <select name="clientsList" id="clientsList" multiple :client="client" v-model="groupSelected">
                        <option v-for="client of clientGroup" :value="client.title">{{client.title}}</option>
                    </select>
                    <p class="error" v-if="!$v.clientGroupInfo.clientGroupValid">Обязательное поле</p>
                </div>
                <div class="user-form__doctorsList">
                    <label class="form__label" for="doctorsList">Лечащий врач</label>
                    <select name="doctorsList" id="doctorsList" :doctor="doctor" v-model="doctorSelected">
                        <option v-for="doctor of doctorGroup" :value="doctor.title">{{doctor.title}}</option>
                    </select>
                </div>
                <div class="user-form__checkbox">
                    <input type="checkbox" v-model="sendSms" true-value="Не отправлять СМС" false-value="Отправить СМС">
                    <p class="error">{{sendSms}}</p>
                </div>
            </div>
        </div>
        <div class="user-form__adress">
            <h3 class="user-form__hedline">Адрес</h3>
            <div class="form-index" :class="{'form-group--error': $v.countryIndex.$error}">
                <label class="form__label">Индекс</label>
                <input class="form__input" v-model.trim="$v.countryIndex.$model" placeholder="123456"/>
                <p class="error" v-if="!$v.countryIndex.numeric || !$v.countryIndex.minLength || !$v.countryIndex.maxLength">Индекс должен составлять 6 цифр</p>
            </div>
            <div class="form-country" :class="{'form-group--error': $v.country.$error}">
                <label class="form__label">Страна</label>
                <input class="form__input" v-model.trim="$v.country.$model" placeholder="Russia"/>
                <p class="error" v-if="!$v.country.alpha && !$v.country.minLength">Введите в формате: "Country"</p>
                <p class="error" v-if="!$v.country.minLength">Название должно состоять как минимум с {{$v.country.$params.minLength.min}} символов.</p>
            </div>
            <div class="form-region" :class="{'form-group--error': $v.region.$error}">
                <label class="form__label">Область</label>
                <input class="form__input" v-model.trim="$v.region.$model" placeholder="Moscow region"/>
                <p class="error" v-if="!$v.region.alpha && !$v.region.minLength">Введите в формате: "Область"</p>
                <p class="error" v-if="!$v.region.minLength">Название должно состоять как минимум с {{$v.region.$params.minLength.min}} символов.</p>
            </div>
            <div class="form-city" :class="{'form-group--error': $v.city.$error}">
                <label class="form__label">Город</label>
                <input class="form__input" v-model.trim="$v.city.$model" placeholder="Moscow"/>
                <p class="error" v-if="!$v.city.required">Обязательное поле</p>
                <p class="error" v-if="!$v.city.alpha">Введите в формате: "Город"</p>
                <p class="error" v-if="!$v.city.minLength">Название должно состоять как минимум с {{$v.city.$params.minLength.min}} символов.</p>
            </div>
            <div class="form-street" :class="{'form-group--error': $v.street.$error}">
                <label class="form__label">Улица</label>
                <input class="form__input" v-model.trim="$v.street.$model" placeholder="Akademika Koroleva"/>
                <p class="error" v-if="!$v.street.alpha">Введите в формате: "Улица"</p>
                <p class="error" v-if="!$v.street.minLength">Название должно состоять как минимум с {{$v.street.$params.minLength.min}} символов.</p>
            </div>
            <div class="form-houseNum" :class="{'form-group--error': $v.houseNum.$error}">
                <label class="form__label">Дом</label>
                <input class="form__input" v-model.trim="$v.houseNum.$model" placeholder="7a"/>
                <p class="error" v-if="!$v.houseNum.alphaNum">Только латинские символы</p>
                <p class="error" v-if="!$v.houseNum.minLength">Название должно состоять как минимум с {{$v.houseNum.$params.minLength.min}} символов.</p>
                <p class="error" v-if="!$v.houseNum.maxLength">Максимальная длина {{$v.houseNum.$params.maxLength.max}} символа.</p>
            </div>
        </div>

        <div class="user-form__passport">
            <h3 class="user-form__hedline">Паспорт</h3>
            <div class="user-form__documentsList" :class="{'form-group--error': $v.documentSelected.$error}">
                <label class="form__label" for="documentsList">Тип документа</label>
                <select name="documentsList" id="documentsList" :document="document" v-model="documentSelected">
                    <option v-for="document of documentGroup" :value="document.title">{{document.title}}</option>
                </select>
                <p class="error" v-if="!$v.documentSelected.required">Обязательное поле</p>
            </div>
            <div class="user-form__passSeries" :class="{'form-group--error': $v.passSeries.$error}">
                <label class="form__label">Серия</label>
                <input class="form__input" v-model.trim="$v.passSeries.$model" placeholder="1234"/>
                <p class="error" v-if="!$v.passSeries.minLength || !$v.passSeries.maxLength">Допускается только {{$v.passSeries.$params.minLength.min}} цифры.</p>
            </div>
            <div class="user-form__passNum" :class="{'form-group--error': $v.passNum.$error}">
                <label class="form__label">Номер</label>
                <input class="form__input" v-model.trim="$v.passNum.$model" placeholder="123456"/>
                <p class="error" v-if="!$v.passNum.minLength || !$v.passNum.maxLength">Допускается только {{$v.passNum.$params.minLength.min}} цифр.</p>
            </div>
            <div class="user-form__passInstitution" :class="{'form-group--error': $v.passInstitution.$error}">
                <label class="form__label">Кем выдан</label>
                <input class="form__input" v-model.trim="$v.passInstitution.$model" placeholder="gu mvd rossii po g. moskve"/>
                <p class="error" v-if="!$v.passInstitution.alpha">Допускается только латинские символы.</p>
            </div>
            <div class="user-form__passDate" :class="{'form-group--error': $v.passDateDay.$error}">
                <div class="user-form__passDay">
                    <label class="form__label" for="passDateDay">День</label>
                    <input class="form__input" type="text" id="passDateDay" name="passDateDay" :value="passDateDay" @change="setPassDateDay($event.target.value)" placeholder="dd">
                </div>
                <div class="user-form__passMonth" :class="{'form-group--error': $v.passDateMonth.$error}">
                    <label class="form__label" for="passDateMonth">Месяц</label>
                    <input class="form__input" type="text" id="passDateMonth" name="passDateMonth" :value="passDateMonth" @change="setPassDateMonth($event.target.value)" placeholder="mm">
                </div>
                <div class="user-form__passYear" :class="{'form-group--error': $v.passDateYear.$error}">
                    <label class="form__label" for="passDateYear">Год</label>
                    <input class="form__input" type="text" id="passDateYear" name="passDateYear" :value="passDateYear" @change="setPassDateYear($event.target.value)" placeholder="year">
                </div>
                <p class="error" v-if="!$v.passDateYear.between || !$v.passDateMonth.between || !$v.passDateDay.between">Не корректная дата</p>
                <p class="error" v-else-if="!$v.passDateYear.required || !$v.passDateMonth.required || !$v.passDateDay.required">Обязательное поле</p>
            </div>
        </div>
        <div class="form__submit-block">
            <button class="form__submit" type="submit" :disabled="submitStatus === 'PENDING'">Submit!</button>
            <p class="form__status form__status--success" v-if="submitStatus === 'OK'">Пользователь зарегистрирован!</p>
            <p class="form__status form__status--error" v-if="submitStatus === 'ERROR'">Заполните форму корректно.</p>
            <p class="form__status form__status--checking" v-if="submitStatus === 'PENDING'">Проверка...</p>       
        </div>
    </form>
</template>

<script>
    import {required, minLength, alpha, alphaNum, between, maxLength, numeric} from 'vuelidate/lib/validators'
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
            clientGroupInfo: '',
            groupSelected: '',
            doctorSelected: '',
            documentSelected: '',
            countryIndex: '',
            country: '',
            region: '',
            city: '',
            street: '',
            houseNum: '',
            passSeries: '',
            passNum: '',
            passInstitution: '',
            passDate: '',
            passDateDay: '',
            passDateMonth: '',
            passDateYear: '',
            sendSms: 'Отправить СМС',
            submitStatus: null
            }
        },

        validations: {
            surname: {
                required,
                alpha,
                minLength: minLength(4),
                surnameValid
            },
            name: {
                required,
                alpha,
                minLength: minLength(4),
                nameValid
            },
            patronymic: {
                alpha,
                minLength: minLength(8),
                patronymicValid
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
                numValid
            },
            gender: {
                genderValid
            },
            clientGroupInfo: {
                clientGroupValid
            },
            countryIndex: {
                maxLength: maxLength(6),
                minLength: minLength(6),
                numeric
            },
            country: {
                alpha,
                minLength: minLength(4)
            },
            region: {
                alpha,
                minLength: minLength(4)
            },
            city: {
                alpha,
                minLength: minLength(4),
                required
            },
            street: {
                alpha,
                minLength: minLength(4),
            },
            houseNum: {
                alphaNum,
                minLength: minLength(1),
                maxLength: maxLength(4)
            },
            documentSelected: {
                required
            },
            passSeries: {
                numeric,
                minLength: minLength(4),
                maxLength: maxLength(4)
            },
            passNum: {
                numeric,
                minLength: minLength(6),
                maxLength: maxLength(6)
            },
            passInstitution: {
                alpha
            },
            passDateDay: {
                required,
                maxLength: maxLength(2),
                between: between(1, 31)
            },
            passDateMonth: {
                required,
                maxLength: maxLength(2),
                between: between(1, 12)
            },
            passDateYear: {
                required,
                maxLength: maxLength(4),
                between: between(1920, 2020)
            },
        },

        props: {
            clientGroup: {
                type: Array,
                required: true
            },
            doctorGroup: {
                type: Array,
                required: true
            },
            documentGroup: {
                type: Array,
                required: true
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
            setPassDateDay(value) {
                this.passDateDay = value
                this.$v.passDateDay.$touch()       
            },

            setPassDateMonth(value) {
                this.passDateMonth = value
                this.$v.passDateMonth.$touch()
            },
            setPassDateYear(value) {
                this.passDateYear = value
                this.$v.passDateYear.$touch()
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
    function numValid(phone) {
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
    function genderValid(gender) {
        if(this.gender === 'male' || this.gender === 'Male' || this.gender === 'female' || this.gender === 'Female' || this.gender === '') {
            return true
        } else {
           return false 
        }
    }
    function clientGroupValid(groupSelected) {
        if(this.groupSelected) {
            return true
        } else {
            return false
        }
    }
    function nameValid(name) {  
        let upName = this.name.toUpperCase()
        let nameArr = upName.split('')
        let swich = true
        if(nameArr.length > 3) {
            let j = 1
            let i = 0
            let k = 2
            while (j < nameArr.length) {
                if(nameArr[i] === nameArr[j] && nameArr[j] === nameArr[k]) {
                    swich = false
                } 
                j++
                i++
                k++
            }
        }
        return swich
    }
    function surnameValid(surname) {
        let upsurname = this.surname.toUpperCase()
        let surnameArr = upsurname.split('')
        let swich = true
        if(surnameArr.length > 3) {
            let j = 1
            let i = 0
            let k = 2
            while (j < surnameArr.length) {
                if(surnameArr[i] === surnameArr[j] && surnameArr[j] === surnameArr[k]) {
                    swich = false
                } 
                j++
                i++
                k++
            }
        }
        return swich
    }
    function patronymicValid(patronymic) {
        let uppatronymic = this.patronymic.toUpperCase()
        let patronymicArr = uppatronymic.split('')
        let swich = true
        if(patronymicArr.length > 8) {
            let j = 1
            let i = 0
            let k = 2
            while (j < patronymicArr.length) {
                if(patronymicArr[i] === patronymicArr[j] && patronymicArr[j] === patronymicArr[k]) {
                    swich = false
                } 
                j++
                i++
                k++
            }
        }
        return swich
    }

</script>

<style scoped lang="sass">
p,
label,
select
    text-align: center

.user-form
    display: flex
    flex-direction: column
    justify-content: center
    align-items: center

.user-form__hedline
    font-size: 20px
    background-color: rgba(#00d5ff, 0.1)
    @media screen and (min-width: 768px)
        width: 100%
        order: 1

.user-form__main-info,
.user-form__adress,
.user-form__passport,
.form__submit-block
    width: 90%
    max-width: 1600px

.user-form__main-info 
    @media screen and (min-width: 768px)
        display: flex
        flex-direction: row
        justify-content: space-between
        align-items: flex-start
        align-content: flex-start
        flex-wrap: wrap
    
.user-form__birthData
    p
        width: 100%
    @media screen and (min-width: 768px)
        width: 40%
        display: flex
        flex-direction: row
        justify-content: space-between
        align-items: center
        align-content: center
        flex-wrap: wrap

.user-form__day,
.user-form__month,
.user-form__year,
.user-form__gender
    @media screen and (min-width: 768px)
        display: flex
        flex-direction: column

.user-form__day,
.user-form__month,
.user-form__year,
.user-form__gender,
.user-form__phone
    @media screen and (min-width: 768px)
        width: 30%
        input
            text-align: center


.user-form__surname,
.user-form__name,
.user-form__patronymic,
.user-form__birthData
.user-form__phone,
.user-form__gender,
.user-form__client-info,
.form__submit-block
    display: flex
    flex-direction: column
    justify-content: center
    align-items: center

.user-form__surname,
.user-form__name,
.user-form__patronymic,
    @media screen and (min-width: 768px)
        width: 30%
        margin: 0 0 0 0

.user-form__surname
    @media screen and (min-width: 768px)
        order: 2

.user-form__name
    @media screen and (min-width: 768px)
        order: 3

.user-form__patronymic
    @media screen and (min-width: 768px)
        order: 4

.user-form__birthData
    @media screen and (min-width: 768px)
        order: 5

.user-form__gender
    @media screen and (min-width: 768px)
        width: 10%
        align-self: flex-start
        order: 6

.user-form__phone
    @media screen and (min-width: 768px)
        width: 40%
        order: 7

.user-form__client-info
    @media screen and (min-width: 768px)
        width: 100%
        display: flex
        flex-direction: row
        justify-content: space-between
        align-items: flex-start
        flex-wrap: wrap
        padding: 15px 0 0 0
        margin: 15px 0 0 0
        border-top: 1px solid #e5e5e5
        order: 8

.user-form__clientGroup
    height: 140px
    select
        height: 70px
        max-height: none
        display: flex
        flex-direction: column
        option
            margin: 15px 0

.user-form__clientGroup,
.user-form__doctorsList
    display: flex
    flex-direction: column
    width: 100%
    @media screen and (min-width: 768px)
        width: 48%
        select
            height: 60px
            border-top: none
            border-bottom: none
            border-radius: 0px

.user-form__doctorsList
    @media screen and (min-width: 768px)
        order: 2
        select
            height: 60px
            max-height: none

    @media screen and (min-width: 768px)
        order: 1

.user-form__checkbox
    display: flex
    flex-direction: column
    justify-content: center
    align-items: center
    width: 95px
    margin: 0.3rem 0 0 0
    input
        max-width: 25px
        max-height: 25px
        min-width: 25px
        min-height: 25px
    @media screen and (min-width: 768px)
        order: 3
        width: 100%
        min-height: 25px

.user-form__adress,
.user-form__passport,
.user-form__passDate
    @media screen and (min-width: 768px)
        display: flex
        flex-direction: row
        justify-content: space-between
        align-items: flex-start
        flex-wrap: wrap

.form-index,
.form-country,
.form-region,
.form-city,
.form-street,
.form-houseNum
    @media screen and (min-width: 768px)
        width: 30%
        margin: 5px 0

.form-index
    @media screen and (min-width: 768px)
        order: 4

.form-country
    @media screen and (min-width: 768px)
        order: 2

.form-region
    @media screen and (min-width: 768px)
        order: 3

.form-city
    @media screen and (min-width: 768px)
        order: 5

.form-street
    @media screen and (min-width: 768px)
         order: 6

.form-houseNum
    @media screen and (min-width: 768px)
         order: 7

.user-form__documentsList,
.user-form__passSeries,
.user-form__passNum,
.user-form__passInstitution,
    @media screen and (min-width: 768px)
        width: 23%

.user-form__documentsList,
    @media screen and (min-width: 768px)
        order: 2

.user-form__passSeries
    @media screen and (min-width: 768px)
        order: 3

.user-form__passNum
    @media screen and (min-width: 768px)
        order: 4

.user-form__passInstitution
    @media screen and (min-width: 768px)
        order: 5

.user-form__passDate
    @media screen and (min-width: 768px)
        width: 100%
        order: 6
        p
            width: 100%

.user-form__passDay
    @media screen and (min-width: 768px)
        width: 30%

.user-form__passMonth
    @media screen and (min-width: 768px)
        width: 30%

.user-form__passYear
    @media screen and (min-width: 768px)
        width: 30%

label
    font-size: 16px
    color: rgba(31,63,104, 0.8)
    
input,
select
    display: flex
    width: 100%
    height: 9.376vw
    min-height: 30px
    max-height: 45px
    border: 1px solid rgba(#356EAD, 0.4)
    box-sizing: border-box
    border-radius: 5px
    outline: none
    padding: 0 0.8rem
    margin: 0.3rem 0 0 0

.error
  font-size: 15px
  color: orange
  margin: 5px 0 0 0

.form-group--error
    input,
    select
        border: solid 2px rgba(#c22727, 0.8)
        margin: 0.3rem 0 0 0

span
  font-size: 15px
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
</style>