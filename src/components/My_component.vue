<template>
    <div class="root">
        <div class="people">
            <img  v-bind:src="people.avatar" alt="Фотокарточка почему-то не загрузилась, попробуйте обновить или нажать серую кнопку">
             <div class="people_info_right">
                <p>Имя {{people.first_name}}</p>
                <p>{{peopleAge}}</p>
                <p> Должность {{people.employment.title}}</p>
                <button 
                 class="more_people" 
                 v-on:mousemove = "updateCoordinates"
                 v-on:click = "newPeople">Следующий коллега</button>
             </div>
             
             
            <ul v-if="errors && errors.lenght">
                <li v-for = "(index, error) of errors" :key='index'>
                     {{error.message}}
                </li>
            </ul>
        </div> 
            <button  
            v-on:mousemove = "updateCoordinates" 
            class="drink_beer"
            v-if = 'activeButton'
            v-on:click = "showBeer">
            Выпьем пивка ?
            </button>
        <div class="some_beer" v-if="isDisplay">
            <p>Ура, сегодня пьем:</p>
            <p>Бренд: {{beer.brand}}</p>
            <p>Название: {{beer.name}}</p>
            <p>Вид: {{beer.style}}</p>
            <p>Оборотов: {{beer.alcohol}}</p>
            <button 
            class="more_beer" 
            v-on:mousemove="updateCoordinates"
            v-on:click="newBeer">
            Хочу другое
            </button>
        </div>
    </div>
    
</template>

<script>
import axios from 'axios'

export default {
    name: 'My_component',
    data() {
        return {
            people: {
                avatar: ' ',
                first_name: ' ',
                peopleAge: ' ',
                employment: ' '
            },
            errors: [],
            beer: {
                brand: ' ',
                name: ' ',
                style: ' ',
                alcohol: ' '
            },
            isDisplay: false,
            activeButton: true,       }
    },
    created() {
        axios.get('https://random-data-api.com/api/users/random_user')
        .then(response => {
            this.people = response.data;
        })
        .catch(e => {
            this.errors.push(e)
        })

         axios.get('https://random-data-api.com/api/beer/random_beer')
        .then(response => {
            this.beer = response.data;
        })
        .catch(e => {
            this.errors.push(e)
        })
    },
    computed: {
        peopleAge(){
            let date_of_birth = this.people.date_of_birth;
            if (!date_of_birth) return null;
            let age = this.getAge(date_of_birth);
            return age + ' ' + this.text(age);   
        },
    },
    methods: {
        getAge (dateString) {
            let today = new Date ();
            let birthDate = new Date (dateString);
            let age = today.getFullYear() - birthDate.getFullYear();
            let m = today.getMonth() - birthDate.getMonth();
            if (m<0 || (m === 0 && today.getDate()<birthDate.getDate())){
                age--;
            }
            return age;
        },
        text(some_date) {
            let txt;
            let count = Number(some_date) % 100;
            if (count >= 5 && count <= 20) {
              txt = 'лет';
            } else {
                count = count % 10;
                if (count == 1) {
                txt = 'год';
                } else if (count >= 2 && count <= 4) {
                txt = 'года';
                } else {
                txt = 'лет';
            }
        }
            return txt;
        },
        updateCoordinates: function(event) {
            const posX = event.pageX - event.target.offsetLeft;
            const posY = event.pageY - event.target.offsetTop;
  
            event.target.style.setProperty("--x", `${posX}px`);
            event.target.style.setProperty("--y", `${posY}px`);
        },
        showBeer() {
            this.isDisplay = true;
            this.activeButton = false;
        },
        newBeer() {
            axios.get('https://random-data-api.com/api/beer/random_beer')
        .then(response => {
            this.beer = response.data;
        })
        .catch(e => {
            this.errors.push(e)
        })
        },
        newPeople() {
             axios.get('https://random-data-api.com/api/users/random_user')
        .then(response => {
            this.people = response.data;
        })
        .catch(e => {
            this.errors.push(e)
            
        })
        },
    }
}
</script>
<style>
html, body, div, span, applet, object, iframe,
h1, h2, h3, h4, h5, h6, p, blockquote, pre,
a, abbr, acronym, address, big, cite, code,
del, dfn, em, img, ins, kbd, q, s, samp,
small, strike, strong, sub, sup, tt, var,
b, u, i, center,
dl, dt, dd, ol, ul, li,
fieldset, form, label, legend,
table, caption, tbody, tfoot, thead, tr, th, td,
article, aside, canvas, details, embed, 
figure, figcaption, footer, header, hgroup, 
menu, nav, output, ruby, section, summary,
time, mark, audio, video {
	margin: 0;
	padding: 0;
	border: 0;
	font-size: 100%;
	font: inherit;
	vertical-align: baseline;
    font-family: 'Roboto', sans-serif;
    font-weight: 400;
}
.root{
    background-image: url(/img/futurama.jpg);
    height: 1000px;
    background-repeat: no-repeat;
    background-size: cover;
}
.people{
    display: flex;
    flex-wrap: wrap;
    padding: 50px 0 0 50px;
}
.people_info_right{
    display: flex;
    flex-direction: column;
    font-size: 25px;
    line-height: 50px;
    margin: 50px 0 0 10px;
}
img{
    width: 300px;
    height: 300px;
    background: #fff;
    font-family: Arial, Helvetica, sans-serif;
    border: 2px solid rgb(66, 66, 66);
    -moz-box-shadow: 0 0 5px #888;
    -webkit-box-shadow: 0 0 5px#888;
    box-shadow: 0 0 5px #888;
    border-radius: 10px;
}
.people_info_right>p,
.some_beer>p{
    border-bottom: 3px solid #f9dd94;
}
button{
    width: 300px;
    margin: 20px 0 0 50px;
    cursor: pointer;
    background-color: #b01201;
    padding: 20px 40px;
    color: white;
    border-radius: 10px;
    font-size: 20px;
    position: relative;
    overflow:hidden;
    z-index: 1;
}
button:before{
    content: '';
    position:absolute;
    left: var(--x);
    top: var(--y);
    width: 0px;
    height: 0px;
    background: radial-gradient(circle closest-side, #FF0000, #FF000000);
    border-radius: 50%;
    z-index: -1;
    transform: translate(-50%, -50%);
    transition: width 0.3s linear, height 0.3s linear
}

button:hover:before{
    width: 100px;
    height: 100px;
}

.some_beer{
    display: inline-block;
    font-size: 25px;
    line-height: 30px;
    padding: 50px 0 0 50px;
}
.more_beer{
    margin: 20px 0 0 0;
}
.more_people {
    margin: 28px 0 0 5px;
    font-size: 20px;
    background-color: grey;
}
@media (min-width: 300px) and (max-width: 500px){
 .people{
    padding: 25px 0 0 25px;
 }
 img{
     width: 250px;
     height: 250px;
 }
 .more_people{
    margin: 20px 0 0 0;
 }
 .people_info_right{
    font-size: 20px;
    line-height: 30px;
    margin: 50px 0 0 10px;
 }
 .some_beer{
    font-size: 20px;
    line-height: 30px;
    padding: 50px 0 0 40px;
 }
 .drink_beer{
     margin: 20px 0 0 35px;
 }
  }

</style>