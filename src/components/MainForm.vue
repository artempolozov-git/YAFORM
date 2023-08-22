<template>
  <div class="container">
      <div class="logotype-wrapper">
        <img src="@/assets/kronosgr.png" class="img-logo">
      </div>
      <div class="form__head">
        <h2 class="form__title">
          Заявка на оплату частями Яндекс Сплит
        </h2>
        <p class="form__subtitle text--large">
          Заполните поля данными, чтобы перейти на оплату частями
        </p>
      </div>

      <form class="form" id="my-form" action="https://formspree.io/f/xeqbganb" method="POST">
        <div class="form__body">
          <input v-model="fullName" name="ФИО" class="form__element form__input" type="text" placeholder="ФИО" required>
          <input v-model="insurance" name="Вид страхования" class="form__element form__input" type="text" placeholder="Вид страхования" required>
          <input v-model="company" name="Страховая компания" class="form__element form__input" type="text" placeholder="Страховая компания" required>
          <input v-model="number" name="Номер полиса" class="form__element form__input" type="text" placeholder="Номер полиса" required>
          <input v-model="date" name="Дата оформления полиса" class="form__element form__input" type="text" onfocus="(type='date')" placeholder="Дата оформления полиса" required>
          <input v-model="prices" name="Cтоимость полиса" class="form__element form__input" type="number" min="1" step="any" placeholder="Cтоимость полиса с копейками" required>
          <masked-input v-model="phone" name="Номер телефона" class="form__element form__input" placeholder="Номер телефона" mask="\+\7 (111) 111-1111" type="tel" required />

          <button @click="getUrl" class="form__button" id="my-form-button">
            Сплит — оплатить частями
          </button>
        </div>
      </form>
  </div>
</template>

<script>
    import MaskedInput from 'vue-masked-input'
    import axios from 'axios'
    export default {
        name: "MainForm",
        components: {
            MaskedInput,
        },
        data () {
            return {
              fullName: '',
              insurance: '',
              company: '',
              number: '',
              date: '',
              prices: '',
              phone: '',
            };
        },
        methods: {
            getUrl() {
                let data = {
                    products: this.fullName + '. ' + this.insurance + '. ' + this.company + '. ' + this.number + '. ' + this.date + '. ' + this.phone,
                    prices: this.prices,
                };
                axios.post ('https://payserver.kronosgr.com/users/split', data).then((response) => {
                    document.location.href = response.data.data.paymentUrl;
                    //console.log('Ссылка на оплату: ' + response.data.data.paymentUrl);
                });
                //console.log (data)
            },
        },
        mounted() {
            var form = document.getElementById("my-form");

            async function handleSubmit(event) {
                event.preventDefault();
                var data = new FormData(event.target);
                fetch(event.target.action, {
                    method: form.method,
                    body: data,
                    headers: {
                        'Accept': 'application/json'
                    }
                }).then(response => {
                    if (response.ok) {
                        form.reset()
                    } else {
                        response.json().then(data => {
                            if (Object.hasOwn(data, 'errors')) {
                                status.innerHTML = data["errors"].map(error => error["message"]).join(", ")
                            } else {
                                status.innerHTML = "Oops! There was a problem submitting your form"
                            }
                        })
                    }
                });
            }
            form.addEventListener("submit", handleSubmit)
        }
    }
</script>

<style scoped>
  .container {
    width: 100%;
    max-width: 550px;
    margin: 0 auto;
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  .form {
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  .form__head {
    text-align: center;
    margin-bottom: 20px;
  }
  .form__title {
    font-weight: bold;
    margin-bottom: 30px;
  }
  .logotype-wrapper {
    width: auto;
    height: 100px;
    margin-bottom: 20px;
  }
  .logotype-wrapper .img-logo {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }
  .form__subtitle {
    max-width: 560px;
    font-weight: 500;
  }
  .form__body {
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    max-width: 620px;
    text-align: left;
  }
  .form__input {
    color: #030104;
    border: 2px solid #f5f4f3;
    background-color: #f5f4f3;
    margin: 0;
    height: 60px;
    padding: 0 20px;
    font-size: 16px;
    line-height: 1.33;
    width: 100%;
    box-sizing: border-box;
    outline: 0;
    border-radius: 0;
    font-family: Montserrat;
  }
  .form__element {
    margin-bottom: 25px;
    width: 100%;
  }
  .form__element p {
    padding-bottom: 5px;
  }
  .form__button {
    display: flex;
    align-items: center;
    justify-content: center;
    font-family: Montserrat;
    color: #ffffff;
    background: linear-gradient(90deg, #3bc5fe 0%, #008ee0 100%);
    border-radius: 10px;
    font-weight: normal;
    text-align: center;
    height: 60px;
    border: 0 none;
    font-size: 16px;
    padding-left: 60px;
    padding-right: 60px;
    cursor: pointer;
    margin: 20px 0 0 0;
    box-sizing: border-box;
    outline: 0;
    max-width: 100%;
  }
  @media screen and (max-width: 768px) {
    .form__button {
      padding-left: 20px;
      padding-right: 20px;
      width: 100%;
    }
  }
  .form__button:hover {
    background: #008ee0;
  }
  .form__button-wrapper {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-top: 5px;
    margin-bottom: 10px;
  }
  .form__credetials a {
    color: rgb(39, 39, 38);
  }
  .text--large {
    font-size: 16px;
  }
  .text--default {
    font-size: 18px;
  }

  #url {
    max-width: 100%;
    overflow-wrap: break-word;
    margin-top: 30px;
  }
  #url a {
    color: rgb(39, 39, 38);
    text-decoration: underline;
  }
  #url a:hover {
    color: blue;
    text-decoration: none;
  }

  input[type="date"] {
    position: relative;
  }

  input[type="date"]::-webkit-calendar-picker-indicator {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    width: auto;
    height: auto;
    color: transparent;
    background: transparent;
  }

  input[type="date"]::-webkit-inner-spin-button,
  input[type="date"]::-webkit-clear-button {
    z-index: 1;
  }

  input[type="number"]::-webkit-outer-spin-button,
  input[type="number"]::-webkit-inner-spin-button {
    -webkit-appearance: none;
  }

  input[type='number'],
  input[type="number"]:hover,
  input[type="number"]:focus {
    appearance: none;
    -moz-appearance: textfield;
  }
</style>